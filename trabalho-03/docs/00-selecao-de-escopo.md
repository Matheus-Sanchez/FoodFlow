# Seção 0 — Seleção de Escopo

> **Trabalho 3 — FoodFlow | Modelagem de Software**

---

## 0.1 Recapitulação: Subsistemas e Atores do FoodFlow

Com base nos Trabalhos 1 e 2, o FoodFlow é estruturado em três subsistemas principais, cada um responsável por um domínio distinto da plataforma:

| Subsistema | Atores Principais |
|---|---|
| **Catálogo & Pedidos** | Cliente, Admin do Restaurante |
| **Logística & Rastreamento** | Entregador, Cliente, Admin do Restaurante |
| **Pagamentos & Avaliações** | Cliente, Gateway de Pagamento (externo), Admin da Plataforma |

Os quatro atores do sistema são:

- **Cliente (USER-001):** busca restaurantes, monta carrinho, realiza pedido e acompanha entrega.
- **Admin do Restaurante (USER-002):** gerencia cardápio, recebe notificações de novos pedidos e confirma preparo.
- **Entregador (USER-003):** recebe ofertas de corrida, aceita/recusa, realiza coleta e entrega.
- **Admin da Plataforma (USER-004):** modera cadastros, configura comissões e monitora o sistema.

---

## 0.2 Fatias Selecionadas

Este trabalho modela **3 fatias verticais** escolhidas por representarem os fluxos mais críticos e de maior complexidade do FoodFlow, cobrindo os três critérios obrigatórios.

---

### Fatia 1 — Cliente realiza pedido e conclui pagamento

**Histórias de usuário cobertas:**

- **US-CLI-003:** *"Como cliente, quero adicionar itens de um restaurante ao carrinho e visualizar o total antes de confirmar."*
- **US-CLI-004:** *"Como cliente, quero finalizar o pedido com pagamento por cartão de crédito de forma segura."*
- **US-CLI-005:** *"Como cliente, quero receber confirmação imediata quando o pagamento for aprovado, com o número do pedido."*

**Por que é representativa:**
É o fluxo de monetização central da plataforma — sem ele, o FoodFlow não existe como negócio. Atravessa três subsistemas (Catálogo para validar disponibilidade de itens, Pagamentos para autorizar a transação com gateway externo, e Logística para criar a solicitação de entrega logo após a confirmação). Inclui caminhos de erro críticos: item esgotado entre carrinho e checkout, cartão recusado, gateway indisponível.

**O que esperamos aprender:**
Como modelar um **fluxo síncrono distribuído** envolvendo ator humano, sistema interno e serviço externo (gateway de pagamento). Como expressar alternativas e exceções com fragmentos `alt` e `opt` em diagrama de sequência UML. Como registrar o efeito transacional em múltiplas entidades (carrinho esvaziado, pedido criado, pagamento registrado, entrega solicitada).

---

### Fatia 2 — Ciclo de vida da entrega (do aceite à confirmação de recebimento)

**Histórias de usuário cobertas:**

- **US-ENT-001:** *"Como entregador, quero visualizar corridas disponíveis próximas a mim e escolher aceitar ou recusar."*
- **US-CLI-006:** *"Como cliente, quero acompanhar em tempo real a localização do entregador após o pedido ser aceito."*
- **US-CLI-007:** *"Como cliente, quero confirmar o recebimento e avaliar o entregador após a entrega."*
- **US-ENT-003:** *"Como entregador, quero registrar as etapas da entrega (cheguei ao restaurante, a caminho do cliente, entrega concluída)."*

**Por que é representativa:**
Envolve **três atores distintos** em interação: Admin do Restaurante (origina a solicitação de entrega), Entregador (executa), Cliente (recebe). Possui um **ciclo de vida com estados bem definidos e transições condicionais** — incluindo timeouts (ex.: entregador não aceita em 5 minutos → reoferta para outro). É o fluxo que entrega valor concreto ao usuário final e onde a maioria dos problemas operacionais se manifesta.

**O que esperamos aprender:**
Como modelar **estados compostos com transições disparadas por eventos**, incluindo ações de entrada/saída e guardas. Como representar timeouts e transições automáticas (sem ação humana direta). A diferença entre o estado da **entrega** e o estado do **pedido** — que são entidades distintas, mas relacionadas.

---

### Fatia 3 — Restaurante recebe pedido, gerencia preparo e aciona entrega

**Histórias de usuário cobertas:**

- **US-REST-001:** *"Como admin do restaurante, quero receber notificação de novo pedido e confirmá-lo ou recusá-lo em até 2 minutos."*
- **US-REST-002:** *"Como admin do restaurante, quero atualizar o status do pedido conforme o preparo avança (em preparo, pronto para retirada)."*
- **US-REST-003:** *"Como admin do restaurante, quero que a plataforma acione automaticamente a busca por entregador quando o pedido estiver pronto."*
- **US-CLI-008:** *"Como cliente, quero ser notificado quando meu pedido for confirmado pelo restaurante e quando sair para entrega."*

**Por que é representativa:**
É a fatia que conecta os dois lados da plataforma: o lado da **demanda** (cliente fez o pedido) e o lado da **oferta** (restaurante e entregador). Possui **regras de negócio não-triviais**: prazo máximo para aceite (2 minutos, senão cancela automaticamente), lógica de cálculo da taxa de entrega (baseada em distância e horário de pico), e decisão de quando acionar a busca por entregador. Envolve **múltiplos subsistemas** (Pedidos, Notificações, Logística) e **dois tipos de usuário** (Admin do Restaurante e sistema automático).

**O que esperamos aprender:**
Como modelar **decisões compostas em sequência** com raias (swimlanes) por ator/subsistema, usando diagrama de atividades. Como representar **paralelismo** (notificar cliente e registrar log ao mesmo tempo). Como expressar **regras de tempo** e ações automáticas disparadas por inatividade.

---

## 0.3 Cobertura dos Critérios Obrigatórios

| Critério | Fatia 1 | Fatia 2 | Fatia 3 |
|---|---|---|---|
| **Must Have do MoSCoW** | ✅ Must Have absoluto | ✅ Must Have | ✅ Must Have |
| **Múltiplos subsistemas / atores** | ✅ 3 subsistemas + gateway externo | ✅ 3 atores distintos | ✅ 2 subsistemas + notificações automáticas |
| **Regras de negócio não-triviais** | ✅ Caminhos de erro + integridade transacional | ✅ Timeouts + transições condicionais | ✅ Prazo de aceite + cálculo de taxa + acionar entrega |

Os três critérios estão integralmente cobertos. Cada fatia expõe uma dimensão diferente do sistema, tornando o conjunto representativo da complexidade real do FoodFlow.

---

## 0.4 O que Fica de Fora (e Por Quê)

As seguintes funcionalidades **não serão modeladas** neste trabalho:

| Funcionalidade | Justificativa da Exclusão |
|---|---|
| **Cadastro e autenticação** | Fluxo padrão (OAuth / JWT) sem regra de domínio específica. Não agrega aprendizado de modelagem. |
| **Busca e filtros de restaurante** | Funcionalidade de exibição. Predominantemente CRUD com consulta. Não tem estados complexos nem múltiplos atores em interação. |
| **Gerenciamento de cardápio** | CRUD de itens. Semelhante em padrão ao exemplo do trabalho; não acrescenta nova dimensão de modelagem. |
| **Histórico de pedidos** | Consulta de dados já persistidos. Não há fluxo de negócio a modelar. |
| **Painel do Admin da Plataforma** | Conjunto de telas operacionais. Predominantemente CRUD administrativo. |
| **Sistema de avaliação do restaurante** | Parcialmente coberto pela Fatia 2 (avaliação do entregador segue o mesmo padrão). Modelar os dois seria redundante. |

> **Nota de rastreabilidade:** a exclusão não implica que essas funcionalidades sejam irrelevantes para o produto — elas são descritas no documento de requisitos e estão previstas no MVP. A exclusão é uma decisão deliberada de foco, conforme orientação da filosofia do trabalho: *profundidade sobre largura*.
