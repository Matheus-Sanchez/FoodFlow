# Seção 3 — Modelagem Comportamental — Fatia 2

> **Trabalho 3 — FoodFlow | Modelagem de Software**  
> **Fatia 2:** Ciclo de vida da entrega (do aceite à confirmação de recebimento)  
> **Tipo de diagrama:** Diagrama de Estados (UML State Machine)

---

## 3.1 Justificativa da Escolha do Tipo

Escolhemos o **Diagrama de Estados** para a Fatia 2 porque o objeto central — a **entidade `Entrega`** — possui um ciclo de vida com estados bem definidos, transições disparadas por eventos externos (ações do entregador, do cliente, ou do sistema por timeout), e ações automáticas associadas a cada transição.

O diagrama de sequência não seria a melhor escolha aqui: embora a entrega envolva múltiplos atores, o que queremos capturar não é *a troca de mensagens* passo a passo, mas *as regras de transição de estado* da entidade — inclusive as que ocorrem automaticamente (sem interação humana direta), como timeouts de aceite. O diagrama de atividades seria mais adequado se o foco fosse o processo *do entregador*, mas o foco aqui é a *máquina de estados do objeto `Entrega`* que governa todo o fluxo.

Os **estados compostos** e ações `entry/do/exit` permitem expressar as regras de timeout e as notificações automáticas de forma concisa.

---

## 3.2 Diagrama de Estados — Fatia 2: Ciclo de Vida da Entrega

```mermaid
stateDiagram-v2
    [*] --> AguardandoAceite : pedido confirmado e pago\n/ criar Entrega, notificar entregadores próximos

    %% ─────────────────────────────────────────
    %% ESTADO: AGUARDANDO ACEITE
    %% ─────────────────────────────────────────
    state AguardandoAceite {
        [*] --> OfertaEnviada
        OfertaEnviada --> TimeoutEspera : após 5 minutos sem aceite
        TimeoutEspera --> OfertaEnviada : [tentativas < 3]\n/ reenviar oferta a outro entregador
        TimeoutEspera --> FalhaSemEntregador : [tentativas >= 3]
    }

    AguardandoAceite --> Aceita : entregador aceita corrida\n/ registrar aceite_em, notificar cliente e restaurante

    AguardandoAceite --> Cancelada : pedido cancelado pelo cliente\n[status=AGUARDANDO_ACEITE]\n/ notificar entregadores que receberam oferta

    FalhaSemEntregador --> Cancelada : [nenhum entregador disponível]\n/ notificar cliente, iniciar estorno do pagamento

    %% ─────────────────────────────────────────
    %% ESTADO: ACEITA
    %% ─────────────────────────────────────────
    state Aceita {
        note right of Aceita
            entry: notificar cliente "Entregador a caminho do restaurante"
            do: rastrear localização do entregador em tempo real (a cada 15s)
            timeout: 20 minutos para chegar ao restaurante
        end note
    }

    Aceita --> EmColeta : entregador registra "Cheguei ao restaurante"\n/ registrar coleta_em, notificar cliente e restaurante

    Aceita --> Cancelada : [timeout: entregador não chegou ao restaurante em 20 min]\n/ liberar corrida para novo entregador (recolocar em AGUARDANDO_ACEITE)

    Aceita --> Cancelada : cliente cancela pedido\n[somente se entregador ainda não coletou]\n/ notificar entregador, pagar taxa de cancelamento parcial

    %% ─────────────────────────────────────────
    %% ESTADO: EM COLETA
    %% ─────────────────────────────────────────
    state EmColeta {
        note right of EmColeta
            entry: notificar restaurante para entregar o pedido ao entregador
            do: rastrear localização em tempo real
        end note
    }

    EmColeta --> EmTransito : entregador registra "A caminho do cliente"\n/ notificar cliente com ETA estimado

    EmColeta --> Cancelada : restaurante relata que pedido não estava disponível\n/ notificar cliente, iniciar estorno, penalizar restaurante

    %% ─────────────────────────────────────────
    %% ESTADO: EM TRÂNSITO
    %% ─────────────────────────────────────────
    state EmTransito {
        note right of EmTransito
            entry: notificar cliente "Pedido saiu para entrega"
            do: rastrear localização em tempo real (a cada 10s)
            exit: registrar entrega_em
        end note
    }

    EmTransito --> Entregue : entregador registra "Entrega concluída"\n/ registrar entrega_em, notificar cliente para confirmar

    EmTransito --> Cancelada : [não permitido — cancelamento bloqueado em trânsito]

    %% ─────────────────────────────────────────
    %% ESTADO: ENTREGUE
    %% ─────────────────────────────────────────
    state Entregue {
        note right of Entregue
            entry: notificar cliente "Pedido entregue! Como foi?"
            do: aguardar avaliação por até 48h
            exit: creditar valor ao entregador
        end note
    }

    Entregue --> Avaliada : cliente confirma recebimento e avalia entregador\n/ criar Avaliacao, creditar valor ao entregador, atualizar avaliacao_media

    Entregue --> Avaliada : [timeout: 48h sem avaliação]\n/ confirmar automaticamente, não criar Avaliacao

    %% ─────────────────────────────────────────
    %% ESTADOS FINAIS
    %% ─────────────────────────────────────────
    Avaliada --> [*]
    Cancelada --> [*] : / executar ações de cancelamento\n(estorno, notificações, logs)
```

---

## 3.3 Tabela de Transições

| De → Para | Evento Disparador | Guarda | Ação Associada |
|---|---|---|---|
| `[*]` → `AguardandoAceite` | Pagamento aprovado no Pedido | — | Criar entrega; enviar oferta a entregadores próximos |
| `AguardandoAceite` → `Aceita` | Entregador aceita corrida | — | Registrar `aceite_em`; notificar cliente e restaurante |
| `AguardandoAceite` → `Cancelada` | Cliente cancela pedido | `status = AGUARDANDO_ACEITE` | Retirar oferta; iniciar estorno |
| `OfertaEnviada` → `TimeoutEspera` | Timer: 5 minutos | — | Incrementar `tentativas_aceite` |
| `TimeoutEspera` → `OfertaEnviada` | — | `tentativas < 3` | Reenviar oferta a próximo entregador disponível |
| `TimeoutEspera` → `FalhaSemEntregador` | — | `tentativas >= 3` | Cancelar; notificar cliente; iniciar estorno |
| `Aceita` → `EmColeta` | Entregador: "Cheguei ao restaurante" | — | Registrar `coleta_em`; notificar cliente e restaurante |
| `Aceita` → `Cancelada` | Timer: 20 minutos | Entregador não chegou | Recolocar em `AguardandoAceite` com novo entregador |
| `Aceita` → `Cancelada` | Cliente cancela | Coleta ainda não ocorreu | Notificar entregador; pagar taxa parcial |
| `EmColeta` → `EmTransito` | Entregador: "A caminho do cliente" | — | Notificar cliente com ETA |
| `EmColeta` → `Cancelada` | Restaurante: pedido indisponível | — | Notificar cliente; estorno; penalidade ao restaurante |
| `EmTransito` → `Entregue` | Entregador: "Entrega concluída" | — | Registrar `entrega_em`; notificar cliente |
| `Entregue` → `Avaliada` | Cliente avalia | — | Criar `Avaliacao`; creditar entregador; atualizar média |
| `Entregue` → `Avaliada` | Timer: 48h sem avaliação | — | Auto-confirmar; creditar entregador; sem `Avaliacao` |

---

## 3.4 Regras de Negócio Expressas no Diagrama

1. **Cancelamento é irreversível a partir de `EmTransito`:** uma vez que o entregador está a caminho do cliente com o pedido, não é possível cancelar. Isso protege o entregador e preserva a integridade da operação.

2. **Máximo de 3 tentativas de atribuição:** se nenhum entregador aceitar após 3 rounds de oferta (15 minutos no total), a entrega é cancelada automaticamente e o cliente recebe estorno. Isso evita que pedidos fiquem "presos" indefinidamente.

3. **Avaliação opcional mas creditação obrigatória:** o entregador recebe o valor da corrida independentemente da avaliação. O timer de 48h garante que o sistema não fique aguardando indefinidamente por ação do cliente.

4. **Rastreamento diferenciado por estado:** a frequência de atualização de localização varia por urgência — 15 segundos em `Aceita`, 10 segundos em `EmTransito`.
