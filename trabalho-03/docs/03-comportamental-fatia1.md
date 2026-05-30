# Seção 3 — Modelagem Comportamental — Fatia 1

> **Trabalho 3 — FoodFlow | Modelagem de Software**  
> **Fatia 1:** Cliente realiza pedido e conclui pagamento  
> **Tipo de diagrama:** Diagrama de Sequência (UML)

---

## 3.1 Justificativa da Escolha do Tipo

Escolhemos o **Diagrama de Sequência** para a Fatia 1 porque o fluxo envolve **coordenação temporal precisa entre múltiplos componentes**: o cliente (ator humano), o front-end da aplicação, o back-end (API), e o gateway de pagamento externo (Stripe/Pagar.me). Cada componente age em resposta ao anterior e o tempo de resposta é crítico — especialmente na autorização do pagamento.

O diagrama de estados não seria adequado aqui: o objeto de interesse não é o ciclo de vida de uma entidade isolada, mas a **orquestração de uma transação distribuída**. O diagrama de atividades capturaria o fluxo, mas perderia a clareza sobre *quem* faz o quê e *quando*.

Os fragmentos `alt` são essenciais para expressar os três caminhos de erro principais: item esgotado no checkout, cartão recusado, e gateway indisponível.

---

## 3.2 Diagrama de Sequência — Fatia 1: Checkout com Pagamento

```mermaid
sequenceDiagram
    autonumber

    actor CLI as Cliente
    participant APP as App (Front-end)
    participant API as API (Back-end)
    participant DB as Banco de Dados
    participant GW as Gateway de Pagamento

    %% ── FASE 1: MONTAGEM DO CARRINHO ──────────────────────────────
    note over CLI,DB: FASE 1 — Montagem e revisão do carrinho

    CLI->>APP: Seleciona item do cardápio
    APP->>API: POST /carrinho/itens {itemId, quantidade, personalizacoes}
    API->>DB: Verificar disponibilidade do item
    DB-->>API: Item disponível / indisponível

    alt Item disponível
        API->>DB: Inserir/atualizar item_carrinho
        DB-->>API: OK
        API-->>APP: 200 OK — carrinho atualizado (subtotal, itens)
        APP-->>CLI: Exibe carrinho atualizado
    else Item indisponível
        API-->>APP: 409 Conflict — "Item indisponível no momento"
        APP-->>CLI: Exibe aviso "Item fora de estoque"
    end

    %% ── FASE 2: REVISÃO DO PEDIDO ─────────────────────────────────
    note over CLI,DB: FASE 2 — Revisão e seleção de endereço/pagamento

    CLI->>APP: Clica em "Revisar Pedido"
    APP->>API: GET /carrinho/{clienteId}
    API->>DB: Consultar carrinho completo + calcular taxa de entrega
    DB-->>API: Carrinho com itens, subtotal, taxa estimada
    API-->>APP: Carrinho + total calculado
    APP-->>CLI: Exibe tela de revisão (itens, endereço, forma de pagamento)

    CLI->>APP: Confirma endereço e seleciona forma de pagamento

    %% ── FASE 3: CHECKOUT ──────────────────────────────────────────
    note over CLI,API: FASE 3 — Finalização e validação

    CLI->>APP: Clica em "Confirmar Pedido"
    APP->>API: POST /pedidos {carrinhoId, enderecoId, metodoPagamento}

    API->>DB: Revalidar disponibilidade de TODOS os itens do carrinho
    DB-->>API: Resultado da revalidação

    alt Algum item ficou indisponível entre carrinho e checkout
        API-->>APP: 409 Conflict — {itensIndisponiveis: [...]}
        APP-->>CLI: "Os seguintes itens não estão mais disponíveis: [lista]. Revise seu carrinho."
    else Todos os itens disponíveis
        API->>DB: Criar registro Pedido (status=AGUARDANDO_CONFIRMACAO)
        API->>DB: Criar registros ItemPedido (snapshot de preços)
        API->>DB: Criar registro Pagamento (status=PENDENTE)
        API->>DB: Copiar endereço para endereco_entrega
        DB-->>API: Pedido criado (pedidoId, numero)

        %% ── FASE 4: PROCESSAMENTO DO PAGAMENTO ───────────────────
        note over API,GW: FASE 4 — Autorização do pagamento

        API->>GW: POST /charges {valor, metodoPagamento, pedidoId}

        alt Gateway indisponível (timeout > 10s)
            GW-->>API: Timeout / Connection Error
            API->>DB: Atualizar Pagamento (status=PENDENTE, codigoErro=GATEWAY_TIMEOUT)
            API->>DB: Atualizar Pedido (status=CANCELADO, motivo=FALHA_GATEWAY)
            API-->>APP: 503 Service Unavailable — "Serviço de pagamento temporariamente indisponível. Tente novamente."
            APP-->>CLI: Exibe mensagem de erro + opção de tentar novamente
        else Gateway responde
            GW-->>API: Resultado {aprovado, transacaoId, codigoErro}

            alt Pagamento aprovado
                API->>DB: Atualizar Pagamento (status=APROVADO, gatewayTransacaoId)
                API->>DB: Atualizar Pedido (status=CONFIRMADO, dataConfirmacao=NOW())
                API->>DB: Limpar Carrinho do cliente
                API->>DB: Criar Notificacao para Cliente (PAGAMENTO_APROVADO)
                API->>DB: Criar Notificacao para AdminRestaurante (NOVO_PEDIDO)
                DB-->>API: OK
                API-->>APP: 201 Created — {pedidoId, numero, status=CONFIRMADO}
                APP-->>CLI: "Pedido #FFXXXX confirmado! Acompanhe em tempo real."

            else Pagamento recusado (ex.: saldo insuficiente, cartão bloqueado)
                API->>DB: Atualizar Pagamento (status=RECUSADO, codigoErro, mensagemErro)
                API->>DB: Excluir Pedido criado (rollback — pedido não confirmado)
                DB-->>API: OK
                API-->>APP: 402 Payment Required — {codigoErro, mensagemUsuario}
                APP-->>CLI: "Pagamento recusado: [motivo]. Tente outro cartão ou forma de pagamento."
            end
        end
    end
```

---

## 3.3 Descrição dos Participantes (Lifelines)

| Participante | Papel no Fluxo |
|---|---|
| **Cliente** | Ator humano que inicia o fluxo e recebe o resultado final |
| **App (Front-end)** | Interface mobile/web; coleta ações do usuário e exibe feedback |
| **API (Back-end)** | Orquestrador principal; valida, persiste e chama serviços externos |
| **Banco de Dados** | Repositório de estado persistente; nunca inicia comunicação |
| **Gateway de Pagamento** | Sistema externo (ex.: Stripe); autoriza ou recusa transações financeiras |

---

## 3.4 Fragmentos Utilizados e Sua Lógica

| Fragmento | Condição | Resultado |
|---|---|---|
| `alt` Item no carrinho | Disponível / Indisponível | Adiciona ao carrinho ou exibe aviso |
| `alt` Revalidação no checkout | Algum item esgotou / Todos ok | Bloqueia checkout ou prossegue |
| `alt` Gateway de pagamento | Timeout / Responde | Cancela pedido com mensagem técnica |
| `alt` Resultado do pagamento | Aprovado / Recusado | Confirma pedido ou faz rollback |

---

## 3.5 Invariantes Garantidas pelo Fluxo

1. **Nunca há pedido sem pagamento aprovado:** o status `CONFIRMADO` em `Pedido` só é atribuído após `status=APROVADO` em `Pagamento`.
2. **Preços são snapshots:** os `ItemPedido` são criados com os preços do momento do checkout. Alterações futuras no cardápio não afetam pedidos históricos.
3. **Carrinho só é limpo após confirmação:** se o pagamento falhar, o carrinho permanece intacto para nova tentativa.
4. **Rollback explícito em falha:** se o pagamento for recusado, o pedido provisório criado é excluído — não fica em estado `AGUARDANDO_CONFIRMACAO` indefinidamente.
