# Seção 4 — Casos de Teste das Fatias Modeladas

> **Trabalho 3 — FoodFlow | Modelagem de Software**  
> Padrão: **IEEE Std 829-2008**  
> Total: **6 casos de teste** (2 por fatia)

---

## Fatia 1 — Cliente realiza pedido e conclui pagamento

### TC-FATIA1-01 — Caminho feliz: pagamento aprovado com sucesso

| Campo | Conteúdo |
|---|---|
| **ID** | TC-FATIA1-01 |
| **Fatia / História** | Fatia 1 — US-CLI-004 (pagamento com cartão) + US-CLI-005 (confirmação imediata) |
| **Pré-condições** | (1) Cliente autenticado com sessão válida. (2) Carrinho contém 1 item: "Pizza Margherita" do restaurante "Pizza Palace" — R$ 38,00; todos os itens estão disponíveis no cardápio. (3) Taxa de entrega calculada: R$ 7,00. (4) Gateway de pagamento configurado em modo de homologação com cartão de teste de aprovação garantida. (5) Restaurante "Pizza Palace" está ativo e aberto. |
| **Dados de entrada** | Cartão de crédito de teste (número: `4242 4242 4242 4242`, CVV: `123`, validade: `12/28`). Endereço de entrega: "Rua das Flores, 123, São Paulo-SP, CEP 01001-000". Forma de pagamento: `CARTAO_CREDITO`. |
| **Passos** | 1. Cliente acessa tela do cardápio e adiciona "Pizza Margherita" ao carrinho. 2. Cliente navega para o carrinho e revisa o pedido. 3. Cliente clica em "Finalizar Pedido". 4. Cliente seleciona o endereço de entrega e a forma de pagamento (cartão cadastrado). 5. Cliente clica em "Confirmar e Pagar". 6. Sistema chama o gateway com os dados do cartão de teste. 7. Gateway retorna `status=APPROVED, transacaoId=TXN-TEST-001`. |
| **Resultado esperado** | (a) Pedido é criado com número único (ex.: `#FF00001`) e status `CONFIRMADO`. (b) Pagamento é registrado com status `APROVADO` e `gatewayTransacaoId=TXN-TEST-001`. (c) Carrinho do cliente é limpo. (d) Cliente recebe notificação push "Pedido #FF00001 confirmado! 🎉". (e) AdminRestaurante recebe notificação "Novo pedido #FF00001 recebido". (f) Tela de acompanhamento é exibida com status "Pedido confirmado". |
| **Critério de aprovação** | (a) Registro em `pedido` com `status='CONFIRMADO'` e `data_confirmacao` não nula. (b) Registro em `pagamento` com `status='APROVADO'`. (c) `carrinho` do cliente está vazio (`item_carrinho` zerado). (d) Duas notificações criadas: uma para o cliente, uma para o admin do restaurante. (e) Tempo total entre clique em "Pagar" e exibição de confirmação ≤ 3 segundos. |
| **Severidade em caso de falha** | **Crítica** — falha no fluxo de monetização principal. Nenhuma receita é gerada sem este fluxo funcionando. |

---

### TC-FATIA1-02 — Fronteira: pagamento recusado por saldo insuficiente

| Campo | Conteúdo |
|---|---|
| **ID** | TC-FATIA1-02 |
| **Fatia / História** | Fatia 1 — US-CLI-004 (pagamento com cartão) — caminho de erro crítico |
| **Pré-condições** | (1) Cliente autenticado. (2) Carrinho contém "Combo Burguer" — R$ 45,00 + taxa de entrega R$ 8,00 = total R$ 53,00. (3) Gateway configurado em modo de homologação com cartão de teste que simula saldo insuficiente. |
| **Dados de entrada** | Cartão de crédito de teste com resposta forçada `INSUFFICIENT_FUNDS` (número de teste: `4000 0000 0000 9995`). Valor total: R$ 53,00. |
| **Passos** | 1. Cliente revisa o carrinho. 2. Cliente clica em "Confirmar e Pagar". 3. Sistema cria registro provisório de `Pedido` (status=`AGUARDANDO_CONFIRMACAO`) e `Pagamento` (status=`PENDENTE`). 4. Sistema envia requisição ao gateway. 5. Gateway retorna `{aprovado: false, codigoErro: "INSUFFICIENT_FUNDS", mensagemErro: "Saldo insuficiente"}`. 6. Sistema processa resposta negativa. |
| **Resultado esperado** | (a) Mensagem de erro exibida: "Pagamento recusado: saldo insuficiente. Tente outro cartão ou forma de pagamento." (b) Pedido provisório é **excluído** do banco de dados (rollback). (c) Estoque e disponibilidade dos itens permanecem inalterados. (d) Carrinho do cliente permanece intacto com todos os itens. (e) Nenhuma notificação enviada ao restaurante. (f) Nenhum valor debitado do cliente. |
| **Critério de aprovação** | (a) Nenhum registro em `pedido` com status diferente de `CANCELADO` após a tentativa. (b) Nenhum registro em `pagamento` com status `APROVADO`. (c) `item_carrinho` do cliente inalterado. (d) Mensagem de erro clara e específica exibida na interface (não mensagem genérica). (e) Log de erro registrado com `codigoErro='INSUFFICIENT_FUNDS'`. |
| **Severidade em caso de falha** | **Crítica** — falha aqui significa criação de pedido sem pagamento confirmado, causando prejuízo financeiro direto ao restaurante e à plataforma. |

---

## Fatia 2 — Ciclo de vida da entrega

### TC-FATIA2-01 — Caminho feliz: entrega concluída e avaliada pelo cliente

| Campo | Conteúdo |
|---|---|
| **ID** | TC-FATIA2-01 |
| **Fatia / História** | Fatia 2 — US-ENT-001 (aceite de corrida) + US-ENT-003 (etapas da entrega) + US-CLI-007 (confirmar recebimento e avaliar) |
| **Pré-condições** | (1) Pedido #FF00042 criado e pago. (2) Entrega #ENT-042 criada com status `AGUARDANDO_ACEITE`. (3) Entregador "Carlos" (ID: ENT-001) está disponível e a 0,8 km do restaurante. (4) Nenhum outro entregador mais próximo disponível. |
| **Dados de entrada** | EntregadorId: `ENT-001`. Ação: aceitar corrida. Etapas: "Cheguei ao restaurante" → "A caminho do cliente" → "Entrega concluída". Avaliação do cliente: nota 5, comentário "Entrega rápida e educada!". |
| **Passos** | 1. Sistema envia oferta de corrida ao entregador Carlos. 2. Carlos aceita a corrida em 1 minuto e 30 segundos. 3. Carlos chega ao restaurante e registra "Cheguei ao restaurante". 4. Carlos coleta o pedido e registra "A caminho do cliente". 5. Carlos chega ao endereço e registra "Entrega concluída". 6. Cliente recebe notificação e avalia com nota 5 e comentário. |
| **Resultado esperado** | (a) Entrega passa sequencialmente por: `AGUARDANDO_ACEITE → ACEITA → EM_COLETA → EM_TRANSITO → ENTREGUE`. (b) Avaliação criada com nota 5. (c) Valor da corrida creditado ao entregador. (d) `avaliacao_media` do entregador recalculada. (e) Cliente recebe notificações em cada transição de estado. (f) Pedido atualizado para status `ENTREGUE`. |
| **Critério de aprovação** | (a) Entrega com `status='ENTREGUE'` e `entrega_em` não nulo. (b) Registro em `avaliacao` com `nota=5` vinculado à entrega. (c) `ganhos_totais` do entregador incrementado com o valor da corrida. (d) 5 notificações distintas enviadas ao cliente durante o fluxo. |
| **Severidade em caso de falha** | **Alta** — fluxo de entrega é o core da proposta de valor. Falha impacta diretamente a experiência do cliente e a renda do entregador. |

---

### TC-FATIA2-02 — Fronteira: tentativa de cancelamento com entrega em trânsito (bloqueio esperado)

| Campo | Conteúdo |
|---|---|
| **ID** | TC-FATIA2-02 |
| **Fatia / História** | Fatia 2 — US-CLI-007 (acompanhamento de entrega) — transição de estado proibida |
| **Pré-condições** | (1) Entrega #ENT-055 com status `EM_TRANSITO` (entregador já coletou e está a caminho). (2) Cliente autenticado. (3) Pedido #FF00055 pago e confirmado. |
| **Dados de entrada** | ClienteId: `CLI-007`. Ação: `DELETE /pedidos/FF00055` (solicitação de cancelamento). Status atual da entrega: `EM_TRANSITO`. |
| **Passos** | 1. Cliente acessa tela de acompanhamento do pedido #FF00055. 2. Cliente clica em "Cancelar pedido" (botão disponível na interface). 3. Sistema verifica o status atual da entrega associada. 4. Sistema identifica que status é `EM_TRANSITO`. 5. Sistema rejeita a solicitação. |
| **Resultado esperado** | (a) Sistema retorna erro `422 Unprocessable Entity` com mensagem: "Não é possível cancelar: o entregador já está a caminho com seu pedido." (b) Status do pedido permanece `EM_ENTREGA` (inalterado). (c) Status da entrega permanece `EM_TRANSITO` (inalterado). (d) Nenhuma notificação de cancelamento enviada ao entregador. (e) Nenhum estorno iniciado. |
| **Critério de aprovação** | (a) HTTP response code `422`. (b) Mensagem de erro clara e específica retornada. (c) `entrega.status` permanece `EM_TRANSITO` após a tentativa. (d) `pedido.status` permanece `EM_ENTREGA`. (e) Zero registros novos em `notificacao` do tipo `CANCELAMENTO` para esta entrega. |
| **Severidade em caso de falha** | **Crítica** — cancelamento indevido durante trânsito prejudica o entregador (que já possui o pedido) e pode causar disputas financeiras e operacionais graves. |

---

## Fatia 3 — Restaurante recebe pedido e aciona entrega

### TC-FATIA3-01 — Caminho feliz: restaurante confirma pedido dentro do prazo e aciona entrega

| Campo | Conteúdo |
|---|---|
| **ID** | TC-FATIA3-01 |
| **Fatia / História** | Fatia 3 — US-REST-001 (aceite do pedido) + US-REST-002 (atualização de status) + US-REST-003 (acionar entregador automaticamente) |
| **Pré-condições** | (1) Pedido #FF00099 criado e pago às 12:05 (horário de pico — almoço). (2) Restaurante "Sushi Time" aberto e ativo. (3) Distância calculada: 2,3 km. (4) Tarifa base: R$ 3,00/km. Tarifa de pico: R$ 3,60/km. (5) Timer de 2 minutos para aceite iniciado em 12:05:00. |
| **Dados de entrada** | AdminRestauranteId: `REST-007`. Ação de aceite às 12:06:15 (1 minuto e 15 segundos dentro do prazo). Ação de início de preparo: 12:07:00. Ação de "pronto para retirada": 12:22:00. |
| **Resultado esperado** | (a) Pedido atualizado para `CONFIRMADO` às 12:06:15. (b) Pedido atualizado para `EM_PREPARO` às 12:07:00. (c) Taxa de entrega calculada: `2,3 km × R$ 3,60 = R$ 8,28` (tarifa de pico). (d) Entrega criada com `status=AGUARDANDO_ACEITE` e `valor_entregador` proporcional. (e) Cliente recebe notificações em cada mudança de status. (f) Módulo de logística inicia busca por entregadores. |
| **Critério de aprovação** | (a) `pedido.status='PRONTO_PARA_RETIRADA'` registrado às 12:22:00. (b) `entrega.status='AGUARDANDO_ACEITE'` criada imediatamente após. (c) `entrega.valor_entregador` calculado com tarifa de pico (validar com precisão de R$ 0,01). (d) 3 notificações enviadas ao cliente (confirmado, em preparo, pronto). (e) Timer de 2 minutos cancelado no momento do aceite. |
| **Severidade em caso de falha** | **Alta** — falha no aceite ou no cálculo da taxa impacta diretamente a remuneração do entregador e a experiência do cliente. |

---

### TC-FATIA3-02 — Fronteira: cancelamento automático por timeout de 2 minutos (restaurante não responde)

| Campo | Conteúdo |
|---|---|
| **ID** | TC-FATIA3-02 |
| **Fatia / História** | Fatia 3 — US-REST-001 (aceite do pedido) — caminho de timeout |
| **Pré-condições** | (1) Pedido #FF00100 criado e pago às 19:10:00. (2) Notificação enviada ao restaurante "BurgerZone" às 19:10:00. (3) Nenhuma ação do restaurante registrada. (4) Timer de 2 minutos configurado para expirar às 19:12:00. (5) Gateway de pagamento configurado para processar estorno em ambiente de homologação. |
| **Dados de entrada** | Nenhuma ação do AdminRestaurante durante 2 minutos. Disparo automático do timer às 19:12:00. |
| **Passos** | 1. Sistema envia notificação de novo pedido ao restaurante às 19:10:00. 2. Nenhuma ação do restaurante até 19:12:00. 3. Timer expira; sistema executa cancelamento automático. 4. Sistema inicia processo de estorno via gateway. 5. Sistema notifica o cliente. 6. Sistema registra penalidade ao restaurante no SLA. |
| **Resultado esperado** | (a) Pedido atualizado para `CANCELADO` às 19:12:00 (±5 segundos de tolerância de execução do job). (b) Estorno iniciado no gateway para o cliente. (c) Cliente recebe notificação: "Seu pedido foi cancelado. O restaurante não confirmou a tempo. Estorno em até 5 dias úteis." (d) Registro de penalidade criado no histórico do restaurante. (e) Nenhuma entrega criada. (f) Nenhum entregador acionado. |
| **Critério de aprovação** | (a) `pedido.status='CANCELADO'` com `motivo_cancelamento='TIMEOUT_ACEITE_RESTAURANTE'` registrado entre 19:12:00 e 19:12:05. (b) Solicitação de estorno enviada ao gateway (log de chamada registrado). (c) Notificação de cancelamento enviada ao cliente. (d) Registro em tabela de SLA/penalidades vinculado ao restaurante. (e) `entrega` **não criada** — zero registros em `entrega` para este pedido. |
| **Severidade em caso de falha** | **Alta** — timeout sem cancelamento resulta em pedido "preso" indefinidamente, bloqueando o carrinho do cliente e sem previsão de estorno. Falha no registro de penalidade prejudica o mecanismo de qualidade da plataforma. |
