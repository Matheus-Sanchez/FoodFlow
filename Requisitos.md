# Requisitos do FoodFlow

**Instituição:** [Centro Universitario SENAC Nações Unidas](https://www.sp.senac.br)  
**Disciplina:** Engenharia de Software  
**Integrantes:**
- Giovanna Paiva Alves
- Matheus Sanchez Duda
- Phelipe Pereira

**Data:** 2026  
**Versão:** 2.0

---

## Sumário

1. [Requisitos Funcionais](#1-requisitos-funcionais)
   - 1.1 [Diagrama de Caso de Uso - Visão Geral](#11-diagrama-de-caso-de-uso---visão-geral)
   - 1.2 [Histórias de Usuário por Subsistema](#12-histórias-de-usuário-por-subsistema)
   - 1.3 [Estimativa de Tamanho](#13-estimativa-de-tamanho)
   - 1.4 [Priorização com MoSCoW](#14-priorização-com-moscow)
2. [Requisitos Não Funcionais](#2-requisitos-não-funcionais)
3. [Referências](#3-referências)

---

## 1. Requisitos Funcionais

### 1.1 Diagrama de Caso de Uso - Visão Geral

```
Autenticar-se                        ───────────────────────────────────────────────── Cliente
Buscar Restaurante
Realizar Pedido
   | <<include>> ──> Processar Pagamento
Acompanhar Entrega em Tempo Real
Avaliar Restaurante / Entregador
Gerenciar Histórico de Pedidos

Cadastrar e Configurar Restaurante   ──────────────────────────────────────────────── Admin. Restaurante
Gerenciar Cardápio
Marcar Item como Indisponível
Gerenciar Pedidos Recebidos
Configurar Horários de Funcionamento
Visualizar Avaliações

Cadastrar-se como Entregador         ──────────────────────────────────────────────── Entregador
Definir Disponibilidade
Aceitar / Recusar Entrega
Atualizar Status da Entrega
Visualizar Ganhos

Gerenciar Usuários e Restaurantes    ──────────────────────────────────────────────── Admin. Plataforma
Visualizar Métricas e Logs
Gerenciar Disputas

 --- APIs externas ---
Gateway de Pagamento                 ──── Processar Pagamento
API de Mapas                         ──── Acompanhar Entrega em Tempo Real
Serviço de Notificações              ──── Notificar Usuários
```

---

**Cliente**
![Cliente](assets/cliente.png)

---

**Restaurante**
![Restaurante](assets/restaurante.png)

---

**Entregador**
![Entregador](assets/entregador.png)

---

**Admin da Plataforma**
![Admin](assets/admin.png)

---

**APIs Externas**
![Externo](assets/externo.png)

---

### 1.2 Histórias de Usuário por Subsistema

Abaixo estão descritas as histórias de usuário mapeadas para os subsistemas do projeto FoodFlow, no formato ágil exigido.

#### 1.2.1 Subsistema Cliente (B2C)

| ID | História | Critérios de Aceitação | Dependências | Fonte |
|:---|:---|:---|:---|:---|
| **US-CLI-001** | **Como** cliente, **eu quero** me cadastrar e autenticar na plataforma, **para que** eu possa acessar os restaurantes e salvar meus dados de forma segura. | (a) O sistema deve validar e-mail e senha. (b) Impedir cadastros duplicados. (c) Login concluído em até 2 segundos. | Nenhuma | Cliente |
| **US-CLI-002** | **Como** cliente, **eu quero** buscar restaurantes por localização ou endereço, **para que** eu encontre opções de entrega disponíveis na minha região. | (a) Exibir restaurantes em um raio configurável. (b) Ocultar restaurantes fechados no momento da busca. | US-CLI-001 | Cliente |
| **US-CLI-003** | **Como** cliente, **eu quero** visualizar o cardápio detalhado, **para que** eu possa ver fotos, descrições, preços e escolher meus pratos. | (a) Refletir a versão mais atualizada do cardápio. (b) Bloquear itens marcados como indisponíveis. | US-CLI-002 | Cliente |
| **US-CLI-004** | **Como** cliente, **eu quero** adicionar itens ao carrinho e realizar o pedido, **para que** o sistema calcule o valor total e a taxa de entrega automaticamente. | (a) Atualizar valores do carrinho em tempo real. (b) Bloquear a confirmação se o carrinho estiver vazio. | US-CLI-003 | Cliente |
| **US-CLI-005** | **Como** cliente, **eu quero** pagar online via gateway (cartão ou PIX), **para que** eu tenha uma experiência rápida e segura sem usar dinheiro físico. | (a) Aprovação ou recusa do gateway em até 5s. (b) O sistema não deve armazenar dados do cartão. | US-CLI-004 | Cliente |
| **US-CLI-006** | **Como** cliente, **eu quero** acompanhar o status do pedido e a localização do entregador em tempo real, **para que** eu saiba exatamente quando minha comida chegará. | (a) Atualizar posição do entregador a cada 15s no mapa. (b) Exibir mudanças de status sem recarregar a tela. | US-CLI-004 | Cliente |
| **US-CLI-007** | **Como** cliente, **eu quero** avaliar o restaurante e o entregador após a entrega, **para que** eu possa compartilhar minha experiência e ajudar a manter a qualidade. | (a) Liberar avaliação apenas com status "Entregue". (b) Atualizar a nota média do restaurante imediatamente. | US-CLI-006 | Cliente |
| **US-CLI-008** | **Como** cliente, **eu quero** acessar meu histórico de pedidos anteriores, **para que** eu possa repetir um pedido favorito rapidamente com um clique. | (a) Exibir pedidos dos últimos 12 meses. (b) Pré-preencher o carrinho ao clicar em "repetir pedido". | US-CLI-004 | Cliente |
| **US-CLI-009** | **Como** cliente, **eu quero** salvar meus endereços favoritos, **para que** eu possa fazer pedidos mais rapidamente sem digitar tudo de novo. | (a) Permitir apelidos como "Casa" ou "Trabalho". | Nenhuma | Cliente |
| **US-CLI-010** | **Como** cliente, **eu quero** contatar o suporte da plataforma, **para que** eu possa relatar problemas não resolvidos com o restaurante ou entregador. | (a) Abertura de chamado direto pelo app. | US-CLI-008 | Cliente |


#### 1.2.2 Subsistema Restaurante (B2B)

| ID | História | Critérios de Aceitação | Dependências | Fonte |
|:---|:---|:---|:---|:---|
| **US-RES-001** | **Como** administrador do restaurante, **eu quero** cadastrar e configurar meu perfil, **para que** o estabelecimento fique visível na plataforma. | (a) Validação de CNPJ obrigatória. (b) Aprovação em até 48h. (c) Dados bancários devem ser criptografados. | Nenhuma | Restaurante |
| **US-RES-002** | **Como** administrador do restaurante, **eu quero** gerenciar o cardápio, **para que** eu possa adicionar, editar ou remover pratos e preços. | (a) Alterações visíveis ao cliente em até 30s. (b) Exigir nome e preço para salvar. | US-RES-001 | Restaurante |
| **US-RES-003** | **Como** administrador do restaurante, **eu quero** marcar itens como indisponíveis, **para que** clientes não comprem produtos que estão sem estoque. | (a) Atualização em 30s para o cliente. (b) Indicação visual de bloqueio no app. | US-RES-002 | Restaurante |
| **US-RES-004** | **Como** administrador do restaurante, **eu quero** configurar horários de funcionamento, **para que** eu receba pedidos apenas quando estiver aberto. | (a) Ocultar restaurante fora do horário. (b) Alterações devem entrar em vigor imediatamente. | US-RES-001 | Restaurante |
| **US-RES-005** | **Como** administrador do restaurante, **eu quero** receber notificações e gerenciar pedidos, **para que** eu possa prepará-los e iniciar o fluxo de entrega. | (a) Notificação em até 10s do pagamento. (b) Cancelar pedido automaticamente após 3 min sem aceite. | US-CLI-005 | Restaurante |
| **US-RES-006** | **Como** administrador do restaurante, **eu quero** visualizar as avaliações recebidas, **para que** eu possa monitorar a qualidade do serviço. | (a) Ordem cronológica decrescente. (b) Recálculo da média em tempo real. | US-CLI-007 | Restaurante |
| **US-RES-007** | **Como** administrador do restaurante, **eu quero** pausar o recebimento de pedidos temporariamente, **para que** a cozinha não fique sobrecarregada nos horários de pico. | (a) Ocultar restaurante da busca instantaneamente. | US-RES-004 | Restaurante |
| **US-RES-008** | **Como** administrador do restaurante, **eu quero** criar cupons de desconto, **para que** eu possa atrair e fidelizar mais clientes. | (a) Definir % de desconto, data de validade e limite de uso. | US-RES-002 | Restaurante |
| **US-RES-009** | **Como** administrador do restaurante, **eu quero** visualizar um extrato de repasses financeiros, **para que** eu possa fazer a conciliação bancária do meu negócio. | (a) Exibir valores líquidos após dedução da taxa da plataforma. | US-RES-005 | Restaurante |
| **US-RES-010** | **Como** administrador do restaurante, **eu quero** contatar o entregador alocado, **para que** eu possa avisar sobre detalhes da coleta ou atrasos. | (a) Chat anonimizado disponível apenas enquanto o pedido não for coletado. | US-RES-005 | Restaurante |

#### 1.2.3 Subsistema Entregador (Logística)

| ID | História | Critérios de Aceitação | Dependências | Fonte |
|:---|:---|:---|:---|:---|
| **US-ENT-001** | **Como** entregador, **eu quero** me cadastrar enviando dados e documentos, **para que** eu seja aprovado para trabalhar na plataforma. | (a) Upload de foto legível da CNH/CRLV. (b) Análise em até 3 dias úteis. | Nenhuma | Entregador |
| **US-ENT-002** | **Como** entregador, **eu quero** me autenticar no aplicativo, **para que** eu possa acessar minha conta de forma segura. | (a) Login em até 2s. (b) Bloqueio de 15 min após 5 tentativas falhas. | US-ENT-001 | Entregador |
| **US-ENT-003** | **Como** entregador, **eu quero** definir minha disponibilidade (online/offline), **para que** eu receba ofertas baseadas no meu GPS. | (a) Iniciar GPS ao ficar online. (b) Bloquear ação se houver pedido em andamento. | US-ENT-002 | Entregador |
| **US-ENT-004** | **Como** entregador, **eu quero** receber ofertas de entrega detalhadas, **para que** eu analise a distância e o valor antes de aceitar. | (a) Oferta expira em 30s. (b) Priorizar entregadores próximos ao restaurante. | US-ENT-003 | Entregador |
| **US-ENT-005** | **Como** entregador, **eu quero** aceitar ou recusar uma entrega, **para que** eu tenha flexibilidade nas rotas. | (a) Aceite bloqueia a oferta para outros. (b) Repassar oferta se recusada. | US-ENT-004 | Entregador |
| **US-ENT-006** | **Como** entregador, **eu quero** atualizar o status da entrega, **para que** o cliente e a plataforma acompanhem o progresso. | (a) Registrar GPS a cada mudança. (b) Status "Entregue" aciona o repasse de valor. | US-ENT-005 | Entregador |
| **US-ENT-007** | **Como** entregador, **eu quero** visualizar um painel com meus ganhos, **para que** eu controle minhas finanças. | (a) Atualização em tempo real após entrega. (b) Exibir saldo disponível para saque. | US-ENT-006 | Entregador |
| **US-ENT-008** | **Como** entregador, **eu quero** reportar um problema com a entrega (ex: pneu furado, acidente), **para que** a plataforma possa realocar outro entregador. | (a) Pausar corrida e alertar suporte imediatamente. | US-ENT-005 | Entregador |
| **US-ENT-009** | **Como** entregador, **eu quero** contatar o cliente via chat anonimizado, **para que** eu possa tirar dúvidas sobre o endereço ou avisar que cheguei. | (a) Chat liberado apenas com o status "A caminho do cliente". | US-ENT-006 | Entregador |
| **US-ENT-010** | **Como** entregador, **eu quero** visualizar o histórico de rotas realizadas, **para que** eu possa analisar quais regiões são mais lucrativas. | (a) Mapa de calor com o histórico de corridas do próprio usuário. | US-ENT-007 | Entregador |

#### 1.2.4 Subsistema Admin da Plataforma (Backoffice)

| ID | História | Critérios de Aceitação | Dependências | Fonte |
|:---|:---|:---|:---|:---|
| **US-ADM-001** | **Como** admin, **eu quero** me autenticar com 2FA, **para que** o painel de gestão fique protegido contra acessos indevidos. | (a) 2FA obrigatório. (b) Sessão expira em 30 min de inatividade. | Nenhuma | Admin |
| **US-ADM-002** | **Como** admin, **eu quero** gerenciar usuários, **para que** eu possa suspender contas que violem os termos de uso. | (a) Bloqueio imediato ao suspender. (b) Anonimização de dados ao excluir (LGPD). | US-ADM-001 | Admin |
| **US-ADM-003** | **Como** admin, **eu quero** aprovar e gerenciar restaurantes, **para que** apenas parceiros verificados operem no sistema. | (a) Restaurante aprovado fica visível em até 5 min. (b) Acesso ao histórico para auditoria. | US-ADM-001 | Admin |
| **US-ADM-004** | **Como** admin, **eu quero** visualizar métricas e logs, **para que** eu monitore a saúde do negócio e a estabilidade técnica. | (a) Dashboard atualizado a cada 60s. (b) Retenção de logs por 90 dias. | US-ADM-001 | Admin |
| **US-ADM-005** | **Como** admin, **eu quero** gerenciar disputas, **para que** eu possa reembolsar clientes ou advertir parceiros. | (a) Alerta automático de nova disputa. (b) Prazo de SLA de 5 dias úteis. | US-ADM-001 | Admin |
| **US-ADM-006** | **Como** admin, **eu quero** configurar o raio máximo de entrega dinamicamente, **para que** eu possa limitar pedidos em dias de chuva forte ou falta de entregadores. | (a) Alteração global aplicada a todos os restaurantes. | US-ADM-001 | Admin |
| **US-ADM-007** | **Como** admin, **eu quero** gerenciar cupons de desconto globais da plataforma, **para que** o time de marketing possa criar campanhas sazonais (ex: Black Friday). | (a) Criação de cupons que a plataforma subsidia. | US-ADM-001 | Admin |
| **US-ADM-008** | **Como** admin, **eu quero** processar os repasses financeiros para os restaurantes, **para que** eles recebam os pagamentos conforme os prazos de liquidação. | (a) Integração automática com a subadquirente de pagamentos. | US-ADM-003 | Admin |
| **US-ADM-009** | **Como** admin, **eu quero** processar os repasses financeiros para os entregadores, **para que** eles possam sacar os valores de suas corridas. | (a) Disparo de transferência via PIX. | US-ADM-002 | Admin |
| **US-ADM-010** | **Como** admin, **eu quero** visualizar um mapa de calor de pedidos em tempo real, **para que** eu possa direcionar incentivos para entregadores irem para áreas de alta demanda. | (a) Mapa exibindo concentração de pedidos pendentes por bairro. | US-ADM-004 | Admin |

---

### 1.3 Estimativa de Tamanho

Para estimar o tamanho de cada história de usuário, utilizamos a metodologia de **Story Points (escala Fibonacci: 1, 2, 3, 5, 8, 13, 21)**. 

**Justificativa:** Esta metodologia é a mais adequada para o escopo do FoodFlow pois permite mensurar o esforço baseado na complexidade técnica (ex: integrações com APIs externas de pagamento e mapas) e nas incertezas da arquitetura, sendo mais eficaz do que estimativas baseadas em horas absolutas para equipes ágeis em formação.

---

### 1.4 Priorização com MoSCoW

Abaixo apresentamos a tabela consolidada com todas as 40 histórias de usuário do projeto, estimadas (Story Points) e priorizadas pelo framework MoSCoW para a definição do MVP.

| ID | Subsistema | História de Usuário | Tam. (SP) | Prioridade | Justificativa |
|:---|:---|:---|:---:|:---|:---|
| US-CLI-001 | Cliente | Cadastro e Autenticação | 3 | **Must** | Essencial para identificação e segurança transacional. |
| US-CLI-002 | Cliente | Busca por Localização | 5 | **Must** | Core do negócio; conecta a demanda à oferta local. |
| US-CLI-003 | Cliente | Visualizar Cardápio | 3 | **Must** | Necessário para a tomada de decisão do cliente. |
| US-CLI-004 | Cliente | Realizar Pedido (Carrinho) | 8 | **Must** | Fluxo principal de conversão da plataforma. |
| US-CLI-005 | Cliente | Pagamento Online (Gateway) | 13 | **Must** | Viabiliza a transação financeira de forma segura. |
| US-CLI-006 | Cliente | Rastreio em Tempo Real | 13 | **Should** | Relevante, mas o sistema base funciona com simples notificações de status no MVP. |
| US-CLI-007 | Cliente | Avaliações | 3 | **Could** | Agrega valor e confiança, mas pode ser lançado pós-MVP. |
| US-CLI-008 | Cliente | Histórico de Pedidos | 3 | **Could** | Útil para retenção, mas secundário no primeiro lançamento. |
| US-CLI-009 | Cliente | Endereços Favoritos | 2 | **Could** | Funcionalidade de conveniência, não bloqueia o uso principal. |
| US-CLI-010 | Cliente | Contatar Suporte | 5 | **Should** | Importante para resolução de problemas operacionais logo no início. |
| US-RES-001 | Restaurante | Cadastrar Perfil | 5 | **Must** | Necessário para a existência de oferta na plataforma. |
| US-RES-002 | Restaurante | Gerenciar Cardápio | 5 | **Must** | Permite manter a lista de produtos atualizada. |
| US-RES-003 | Restaurante | Indisponibilizar Itens | 3 | **Must** | Evita a venda de produtos sem estoque, reduzindo cancelamentos. |
| US-RES-004 | Restaurante | Configurar Horários | 3 | **Must** | Evita que pedidos cheguem quando o local está fechado. |
| US-RES-005 | Restaurante | Gerenciar Pedidos | 8 | **Must** | Essencial para o restaurante iniciar o preparo e chamar o entregador. |
| US-RES-006 | Restaurante | Visualizar Avaliações | 3 | **Could** | Depende do sistema de avaliações do cliente estar pronto. |
| US-RES-007 | Restaurante | Pausar Pedidos | 3 | **Should** | Protege a saúde operacional do restaurante em horários de pico. |
| US-RES-008 | Restaurante | Cupons de Desconto | 5 | **Could** | Estratégia de marketing que pode ser lançada na versão 2.0. |
| US-RES-009 | Restaurante | Extrato de Repasses | 5 | **Should** | Fundamental para a transparência financeira com o parceiro. |
| US-RES-010 | Restaurante | Chat com Entregador | 3 | **Could** | Ajuda na logística fina, mas não impede a entrega padrão. |
| US-ENT-001 | Entregador | Cadastro de Entregador | 5 | **Must** | Necessário para compor a frota de logística. |
| US-ENT-002 | Entregador | Autenticação | 3 | **Must** | Segurança e acesso ao perfil do trabalhador. |
| US-ENT-003 | Entregador | Definir Disponibilidade | 3 | **Must** | Controla quando o entregador deseja começar a trabalhar. |
| US-ENT-004 | Entregador | Receber Oferta | 5 | **Must** | O motor de alocação de corridas é o coração logístico do app. |
| US-ENT-005 | Entregador | Aceitar/Recusar Entrega | 8 | **Must** | Fluxo vital para conectar o pedido à entrega física. |
| US-ENT-006 | Entregador | Atualizar Status | 5 | **Must** | Gatilho para avisar o cliente e liberar o pagamento da corrida. |
| US-ENT-007 | Entregador | Painel de Ganhos | 5 | **Should** | Importante para a retenção de entregadores no app. |
| US-ENT-008 | Entregador | Reportar Problema | 5 | **Should** | Necessário para tratamento de exceções (ex: pneu furado). |
| US-ENT-009 | Entregador | Chat com Cliente | 3 | **Could** | Facilita encontrar o endereço, mas rotas GPS costumam ser suficientes no MVP. |
| US-ENT-010 | Entregador | Histórico de Rotas | 5 | **Won't** | Feature analítica avançada, fora do escopo inicial. |
| US-ADM-001 | Admin | Autenticação 2FA | 5 | **Must** | Segurança obrigatória para acessar dados sensíveis do sistema. |
| US-ADM-002 | Admin | Gerenciar Usuários | 5 | **Must** | Controle de qualidade e bloqueio de usuários mal-intencionados. |
| US-ADM-003 | Admin | Aprovar Restaurantes | 5 | **Should** | No MVP, a aprovação poderia ser manual direto no banco de dados, mas é muito recomendada. |
| US-ADM-004 | Admin | Dashboard de Métricas | 8 | **Won't** | No início, relatórios podem ser extraídos via scripts de banco de dados. |
| US-ADM-005 | Admin | Gerenciar Disputas | 5 | **Should** | Essencial para o atendimento ao cliente (SAC) e reembolsos. |
| US-ADM-006 | Admin | Raio de Entrega Dinâmico | 8 | **Won't** | Funcionalidade avançada de logística, será feita futuramente. |
| US-ADM-007 | Admin | Cupons Globais | 5 | **Won't** | Campanhas da plataforma não são o foco do lançamento inicial. |
| US-ADM-008 | Admin | Repasse Restaurante | 8 | **Must** | Automatizar o pagamento do parceiro é requisito legal e de negócio. |
| US-ADM-009 | Admin | Repasse Entregador | 8 | **Must** | Garantir que o entregador receba sua taxa de entrega. |
| US-ADM-010 | Admin | Mapa de Calor Logístico | 13 | **Won't** | Funcionalidade de Big Data muito complexa para a versão inicial. |
---

## 2. Requisitos Não Funcionais

Abaixo estão detalhados os requisitos não funcionais do sistema FoodFlow, seguindo o padrão IEEE-830 e com justificativas embasadas nas normas de qualidade de software (ISO/IEC 25010) e literatura técnica.

---

#### NF-FF-001: Tempo de Resposta

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-001 |
| **Título** | Tempo de Resposta |
| **Descrição** | O sistema deve responder a todas as solicitações dos usuários em até 2 segundos em condições normais de uso e em até 4 segundos nos horários de pico. |
| **Entrada** | Qualquer solicitação do usuário (busca de restaurantes, carregamento de cardápio, confirmação de pedido, atualização de status). |
| **Processamento** | O sistema processa a solicitação, consulta o banco de dados e/ou serviços externos e retorna o resultado ao cliente. |
| **Saída** | Resposta renderizada na interface do usuário (listagem, confirmação, atualização). |
| **Restrições** | Aplica-se a todas as rotas da API pública. Operações de terceiros (gateway de pagamento, API de mapas) estão sujeitas aos SLAs dos respectivos fornecedores. |
| **Critérios de Aceitação** | (a) 95% das requisições devem ser atendidas em até 2 segundos em carga normal. (b) Em testes de carga simulando horário de pico, o percentil 99 não deve exceder 4 segundos. (c) A experiência do usuário não deve ser prejudicada por lentidão perceptível. |
| **Justificativa** | **ISO/IEC 25010 (Eficiência de Desempenho):** Segundo Sommerville (2019), o comportamento do sistema em relação ao tempo afeta diretamente a retenção de usuários. Tempos acima de 4 segundos aumentam drasticamente a taxa de abandono do carrinho de compras. |

---

#### NF-FF-002: Disponibilidade e Uptime

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-002 |
| **Título** | Disponibilidade e Uptime |
| **Descrição** | O sistema deve estar disponível 99,5% do tempo mensal, com janelas de manutenção programadas fora dos horários de pico (almoço: 11h–14h / jantar: 18h–21h). |
| **Entrada** | Qualquer tentativa de acesso à plataforma por qualquer ator. |
| **Processamento** | O servidor processa a requisição e entrega a resposta. |
| **Saída** | Interface funcional ou mensagem de manutenção programada clara ao usuário. |
| **Restrições** | Manutenções emergenciais podem ocorrer fora da janela programada, mas devem ser comunicadas aos parceiros com antecedência mínima de 1 hora. |
| **Critérios de Aceitação** | (a) O sistema não deve ficar indisponível mais de 3,6 horas por mês. (b) Falhas inesperadas devem acionar alertas automáticos à equipe de operações. (c) Nenhuma manutenção programada deve ocorrer entre 11h e 22h. |
| **Justificativa** | **ISO/IEC 25010 (Confiabilidade - Disponibilidade):** A indisponibilidade do sistema durante horários de refeição gera falha no processo de negócio principal, resultando em perda de receita imediata e quebra de confiança por parte dos restaurantes e clientes. |

---

#### NF-FF-003: Escalabilidade e Capacidade

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-003 |
| **Título** | Escalabilidade e Capacidade |
| **Descrição** | O sistema deve suportar entre 2.000 e 10.000 usuários ativos mensais na fase inicial, com capacidade de escalar horizontalmente para até 50.000 usuários sem redesenho arquitetural. |
| **Entrada** | Acessos simultâneos de clientes, entregadores e administradores de restaurantes, especialmente nos horários de pico. |
| **Processamento** | O sistema deve distribuir a carga entre instâncias e realizar auto-scaling conforme demanda. |
| **Saída** | Tempo de resposta dentro dos limites do NF-FF-001 mesmo em carga elevada. |
| **Restrições** | Estimativa de 20.000 pedidos processados no primeiro ano. Picos esperados de 500 pedidos/hora nos horários de almoço e jantar. |
| **Critérios de Aceitação** | (a) Testes de carga devem simular 500 usuários simultâneos sem degradação de desempenho. (b) A arquitetura deve permitir adição de instâncias sem downtime (zero-downtime deploy). (c) O banco de dados deve suportar ao menos 10.000 transações por hora. |
| **Justificativa** | **Arquitetura de Software e ISO/IEC 25010 (Eficiência de Desempenho - Capacidade):** Plataformas de delivery possuem sazonalidade extrema. O provisionamento elástico (auto-scaling) é mandatório para suportar os picos de tráfego sem ociosidade de infraestrutura nos períodos de baixa demanda. |

---

#### NF-FF-004: Segurança e Controle de Acesso

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-004 |
| **Título** | Segurança e Controle de Acesso |
| **Descrição** | O sistema deve garantir que cada ator acesse somente os recursos autorizados ao seu perfil, utilizando autenticação baseada em tokens (JWT) e comunicação criptografada via HTTPS/TLS. |
| **Entrada** | Credenciais de login (e-mail e senha) ou token de sessão válido em requisições autenticadas. |
| **Processamento** | O sistema valida as credenciais, emite um token JWT com tempo de expiração e aplica controle de acesso baseado em papéis (RBAC) em todas as rotas protegidas. |
| **Saída** | Acesso concedido ou negado com mensagem de erro padronizada (HTTP 401/403). |
| **Restrições** | Senhas devem ser armazenadas com hash (bcrypt, mínimo custo 12). Dados de cartão não devem ser persistidos nos servidores do FoodFlow. Tokens JWT devem expirar em 24 horas. |
| **Critérios de Aceitação** | (a) Um cliente não deve conseguir acessar endpoints de administração de restaurante. (b) Todas as requisições à API devem trafegar exclusivamente via HTTPS. (c) Tentativas de brute-force devem ser bloqueadas após 5 tentativas falhas consecutivas (bloqueio de 15 minutos). |
| **Justificativa** | **ISO/IEC 25010 (Segurança - Confidencialidade e Autenticidade):** O tráfego de dados financeiros e pessoais exige a mitigação de vulnerabilidades (ex: injeções, interceptação de dados). O uso de JWT e HTTPS assegura que informações sensíveis não sejam expostas. |

---

#### NF-FF-005: Conformidade com a LGPD

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-005 |
| **Título** | Conformidade com a LGPD (Lei nº 13.709/2018) |
| **Descrição** | O sistema deve coletar, armazenar e processar dados pessoais de acordo com os princípios da Lei Geral de Proteção de Dados, garantindo consentimento, finalidade e minimização de dados. |
| **Entrada** | Dados pessoais fornecidos pelos usuários no momento do cadastro e durante o uso da plataforma (nome, e-mail, CPF, endereço, localização GPS). |
| **Processamento** | O sistema deve registrar o consentimento explícito do usuário e permitir que ele solicite a exclusão ou portabilidade dos seus dados. |
| **Saída** | Confirmação de consentimento registrado; relatório de dados pessoais disponível para exportação pelo usuário. |
| **Restrições** | Dados de localização GPS do entregador devem ser utilizados exclusivamente para o rastreamento ativo de entregas e descartados após a conclusão. Nenhum dado pessoal pode ser compartilhado com terceiros sem consentimento. |
| **Critérios de Aceitação** | (a) O fluxo de cadastro deve exibir e exigir aceite da Política de Privacidade. (b) O usuário deve poder solicitar exclusão de conta e dados a qualquer momento. (c) A plataforma não deve armazenar dados de localização após a conclusão do pedido. |
| **Justificativa** | **Engenharia de Requisitos (Restrições Legais):** Requisito obrigatório para garantir a adequação legal do software. O descumprimento dos princípios de *Privacy by Design* previstos na lei brasileira pode acarretar bloqueio do serviço e sanções financeiras severas. |

---

#### NF-FF-006: Usabilidade e Acessibilidade

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-006 |
| **Título** | Usabilidade e Acessibilidade |
| **Descrição** | A interface deve ser intuitiva o suficiente para que um novo usuário consiga concluir um pedido pela primeira vez sem assistência, em até 4 minutos, em dispositivos móveis (iOS e Android) e navegadores web modernos. |
| **Entrada** | Interação do usuário com a interface (toques, cliques, digitação). |
| **Processamento** | O sistema responde às ações do usuário com feedback visual imediato (loading states, mensagens de sucesso/erro). |
| **Saída** | Fluxo de pedido concluído com confirmação clara ao usuário. |
| **Restrições** | O design responsivo deve suportar telas a partir de 320px de largura. O contraste das cores deve atender ao nível AA das diretrizes WCAG 2.1. |
| **Critérios de Aceitação** | (a) Teste de usabilidade com 5 usuários novatos deve demonstrar conclusão do primeiro pedido em até 4 minutos. (b) A interface deve ser funcional nos navegadores Chrome, Firefox e Safari (últimas 2 versões). (c) O app mobile deve ser compatível com Android 10+ e iOS 14+. |
| **Justificativa** | **ISO/IEC 25010 (Usabilidade - Apreensibilidade e Acessibilidade):** Um design centrado no utilizador é vital para plataformas de consumo de massa. Atender às diretrizes WCAG garante que a plataforma seja acessível e tenha uma curva de aprendizagem mínima. |

---

#### NF-FF-007: Rastreamento em Tempo Real do Entregador

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-007 |
| **Título** | Rastreamento em Tempo Real do Entregador |
| **Descrição** | O sistema deve atualizar a posição do entregador no mapa exibido ao cliente a cada 15 segundos durante o fluxo de entrega ativa, utilizando WebSockets ou tecnologia equivalente. |
| **Entrada** | Coordenadas GPS enviadas pelo dispositivo do entregador durante a entrega. |
| **Processamento** | O servidor recebe as coordenadas, atualiza o estado da entrega e transmite em tempo real para o cliente conectado via canal persistente (WebSocket). |
| **Saída** | Marcador de posição atualizado no mapa da tela de acompanhamento do cliente. |
| **Restrições** | A atualização de posição não deve consumir mais de 5% de bateria do dispositivo do entregador por hora. Dados de localização só devem ser transmitidos durante entregas ativas. |
| **Critérios de Aceitação** | (a) A posição do entregador deve ser atualizada com atraso máximo de 20 segundos na interface do cliente. (b) Em caso de perda de sinal do entregador, o sistema deve exibir a última posição conhecida com indicação de tempo. (c) A conexão WebSocket deve ser restabelecida automaticamente em até 5 segundos após queda. |
| **Justificativa** | **ISO/IEC 25010 (Adequação Funcional - Completude funcional):** O rastreamento em tempo real é uma exigência do mercado atual (estado da arte) para aplicativos de delivery, reduzindo a ansiedade do cliente e diminuindo chamadas para o suporte. |

---

#### NF-FF-008: Compatibilidade de Plataforma

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-008 |
| **Título** | Compatibilidade de Plataforma (Web e Mobile) |
| **Descrição** | O FoodFlow deve disponibilizar experiências otimizadas tanto para navegadores web modernos quanto para dispositivos móveis, garantindo consistência funcional entre as plataformas. |
| **Entrada** | Acesso à plataforma via navegador desktop/mobile ou aplicativo nativo (Android/iOS). |
| **Processamento** | O sistema detecta o tipo de dispositivo e serve a interface adequada (responsiva no web; nativa no mobile). |
| **Saída** | Interface funcional e otimizada para o dispositivo utilizado. |
| **Restrições** | Funcionalidades de GPS e notificações push têm dependência de permissões do sistema operacional do dispositivo. O app mobile deve pesar menos de 30 MB no download inicial. |
| **Critérios de Aceitação** | (a) Todas as funcionalidades críticas (pedido, pagamento, rastreamento) devem funcionar tanto no web quanto no mobile. (b) O app deve passar na revisão das lojas Google Play e Apple App Store. (c) A versão web deve ser totalmente funcional sem instalação de plugins adicionais. |
| **Justificativa** | **ISO/IEC 25010 (Portabilidade - Adaptabilidade):** O software precisa se comportar adequadamente nos diversos ecossistemas dos utilizadores finais (dispositivos móveis e browsers), garantindo assim um maior alcance de mercado. |

---

#### NF-FF-009: Sistema de Notificações

| **Campo** | **Descrição** |
|---|---|
| **Requisito ID** | NF-FF-009 |
| **Título** | Sistema de Notificações |
| **Descrição** | O sistema deve enviar notificações em tempo real para clientes, restaurantes e entregadores em resposta a eventos relevantes, utilizando notificações push (mobile), in-app e e-mail como canais de comunicação. |
| **Entrada** | Eventos do sistema: novo pedido, pedido confirmado, entregador a caminho, entrega concluída, nova oferta de corrida. |
| **Processamento** | O serviço de notificações (ex.: Firebase Cloud Messaging) recebe o evento e despacha a mensagem para os dispositivos registrados dos atores relevantes. |
| **Saída** | Notificação recebida no dispositivo do destinatário em até 10 segundos após o evento. |
| **Restrições** | O usuário deve poder configurar quais notificações deseja receber. Notificações de marketing devem respeitar o horário preferencial do usuário. |
| **Critérios de Aceitação** | (a) Notificações críticas (novo pedido para restaurante, entrega aceita para cliente) devem ser entregues em até 10 segundos. (b) O sistema deve registrar log de todas as notificações enviadas com status de entrega. (c) O usuário deve conseguir desativar notificações não críticas sem perder as operacionais. |
| **Justificativa** | **ISO/IEC 25010 (Usabilidade - Proteção contra erros de operação e feedback):** As notificações servem como o principal meio de feedback assíncrono do sistema, informando os usuários sobre mudanças de estado sem que precisem atualizar a aplicação constantemente. |

---

## 3. Referências

[1] IEEE Std 830-1998. **IEEE Recommended Practice for Software Requirements Specifications**. Institute of Electrical and Electronics Engineers, 1998.

[2] ISO/IEC/IEEE 29148:2011. **Systems and software engineering – Life cycle processes – Requirements engineering**. International Organization for Standardization, 2011.

[3] ABNT NBR ISO/IEC 9126-1:2003. **Engenharia de software – Qualidade de produto**. Associação Brasileira de Normas Técnicas, 2003.

[4] BRASIL. **Lei nº 13.709, de 14 de agosto de 2018 – Lei Geral de Proteção de Dados Pessoais (LGPD)**. Disponível em: https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm. Acesso em: abril de 2026.

[5] PRESSMAN, R. S.; MAXIM, B. R. **Engenharia de Software: Uma Abordagem Profissional**. 8. ed. Porto Alegre: AMGH, 2016.

[6] SOMMERVILLE, I. **Engenharia de Software**. 10. ed. São Paulo: Pearson, 2019.

[7] W3C. **Web Content Accessibility Guidelines (WCAG) 2.1**. Disponível em: https://www.w3.org/TR/WCAG21/. Acesso em: abril de 2026.
