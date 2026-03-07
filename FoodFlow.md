# Documento de Visão do Produto — FoodFlow

> Plataforma de Delivery de Alimentos
Integrantes:
- Giovanna Paiva Alves
- Matheus Sanchez Duda
- Phelipe Pereira 

---

## Sumário

1. [Visão Geral do Produto](#1-visão-geral-do-produto)
   - 1.1 [Oportunidade de Negócio e Declaração do Problema](#11-oportunidade-de-negócio-e-declaração-do-problema)
   - 1.2 [Perspectiva do Produto](#12-perspectiva-do-produto)
   - 1.3 [Capacidades do Produto](#13-capacidades-do-produto)
2. [Descrição dos Usuários](#2-descrição-dos-usuários)
3. [Restrições do Projeto e do Produto](#3-restrições-do-projeto-e-do-produto)
   - 3.1 [Tecnologia e Padrões](#31-tecnologia-e-padrões)
   - 3.2 [Legislação e Regulamentações](#32-legislação-e-regulamentações)
4. [Análise de Riscos e Mitigação](#4-análise-de-riscos-e-mitigação)
5. [Referências](#5-referências)

---

## 1. Visão Geral do Produto

### 1.1 Oportunidade de Negócio e Declaração do Problema

#### Declaração do Problema

O crescimento acelerado do consumo de refeições fora do lar evidenciou uma lacuna significativa no mercado: a falta de uma plataforma integrada e acessível que conecte, de forma eficiente, clientes que desejam conveniência, restaurantes que buscam ampliar seu alcance e entregadores que necessitam de trabalho flexível.

Segundo a Associação Brasileira de Bares e Restaurantes (ABRASEL), o mercado de food service no Brasil movimenta mais de R$ 200 bilhões por ano, e o segmento de delivery cresceu mais de 30% nos anos pós-pandemia, tornando-se parte essencial do comportamento de consumo alimentar urbano [1]. Ainda assim, muitos restaurantes de pequeno e médio porte enfrentam dificuldades para ingressar nas grandes plataformas por conta de comissões elevadas ou requisitos técnicos complexos [2].

#### Causas do Problema

- **Do lado do consumidor:** a experiência fragmentada entre aplicativos diferentes, falta de rastreamento confiável em tempo real e opções limitadas por localização.
- **Do lado dos restaurantes:** comissões abusivas de plataformas dominantes (que chegam a 30% por pedido [3]), pouca visibilidade para estabelecimentos menores e ausência de controle sobre dados dos próprios clientes.
- **Do lado dos entregadores:** ausência de transparência nos valores de corrida, baixa previsibilidade de demanda e ausência de ferramentas de gestão de ganhos.

#### Oportunidade de Negócio

O FoodFlow se posiciona como uma alternativa viável e justa para os três elos da cadeia. A plataforma oferece comissões competitivas para restaurantes, experiência de usuário superior para clientes e maior transparência para entregadores. O potencial de mercado endereçável (SAM) somente na região sudeste do Brasil é estimado em R$ 4,5 bilhões em pedidos digitais por ano [1][4].

#### Impacto Potencial

- Redução de custos operacionais para restaurantes locais.
- Geração de renda acessível para trabalhadores autônomos.
- Conveniência e rastreabilidade para consumidores finais.
- Estímulo à economia local por meio da valorização de restaurantes independentes.

---

### 1.2 Perspectiva do Produto

#### Contexto no Ecossistema Tecnológico

O FoodFlow integra-se a um ecossistema composto por sistemas de mapas (como Google Maps ou OpenStreetMap), gateways de pagamento (como Stripe ou Pagar.me) e provedores de notificações push (como Firebase Cloud Messaging). Em relação a produtos similares, o FoodFlow compete diretamente com iFood, Rappi e Uber Eats, diferenciando-se pela proposta de tarifas reduzidas para parceiros e experiência simplificada.

#### Público-alvo

| Perfil | Descrição |
|--------|-----------|
| **Consumidor final** | Pessoas físicas, 18–45 anos, que residem ou trabalham em centros urbanos e buscam praticidade para refeições. |
| **Administrador de restaurante** | Proprietários ou gerentes de restaurantes pequenos e médios que desejam ampliar seu alcance digital. |
| **Entregador** | Trabalhadores autônomos com veículo próprio (moto ou bicicleta) que buscam fonte de renda flexível. |
| **Administrador da plataforma** | Equipe interna responsável por suporte, moderação e configurações gerais do sistema. |

#### Proposta de Valor

- **Para clientes:** facilidade de descoberta de restaurantes próximos, acompanhamento de pedidos em tempo real e histórico centralizado.
- **Para restaurantes:** acesso a uma base de clientes digital com comissões justas e controle total do próprio cardápio.
- **Para entregadores:** interface clara de aceite/recusa de corridas, visibilidade de ganhos e liberdade de horário.

---

### 1.3 Capacidades do Produto

#### Principais Funcionalidades

| Funcionalidade | Benefício ao Usuário |
|----------------|----------------------|
| Busca de restaurantes por localização | Cliente encontra opções próximas de forma rápida |
| Visualização de cardápio com preços | Transparência antes do pedido |
| Realização de pedidos com múltiplos itens | Compra completa em uma única sessão |
| Cálculo automático de total + taxa de entrega | Sem surpresas no valor final |
| Rastreamento em tempo real do entregador | Previsibilidade e segurança para o cliente |
| Gerenciamento de cardápio pelo restaurante | Autonomia e agilidade para o parceiro |
| Aceitação/recusa de corridas pelo entregador | Flexibilidade de trabalho |
| Sistema de avaliações pós-entrega | Melhoria contínua da qualidade |
| Histórico de pedidos | Fidelização e recompra facilitada |

#### Wireframes (Baixíssima Resolução)

**Fluxo 1 — Cliente realizando um pedido**

```
┌─────────────────────────────┐
│    Tela Inicial (Home)       │
│  [Buscar restaurantes...]    │
│  🍕 Pizza Palace  ⭐4.8      │
│  🍔 BurgerZone    ⭐4.5      │
│  🍣 SushiTime     ⭐4.9      │
└────────────┬────────────────┘
             │ Seleciona restaurante
             ▼
┌─────────────────────────────┐
│       Cardápio               │
│  Margherita ........R$38,00  │
│  Calabresa ........R$42,00   │
│  [+ Adicionar ao carrinho]   │
└────────────┬────────────────┘
             │ Confirma itens
             ▼
┌─────────────────────────────┐
│         Checkout             │
│  Subtotal: R$ 80,00          │
│  Taxa de entrega: R$ 7,00    │
│  Total: R$ 87,00             │
│  Endereço: [Rua das Flores]  │
│  Pagamento: [Cartão de créd] │
│  [Confirmar Pedido]          │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│   Acompanhamento do Pedido   │
│  ✅ Pedido confirmado         │
│  🍳 Em preparo...             │
│  🛵 Entregador a caminho      │
│  📍 [Mapa com localização]    │
└─────────────────────────────┘
```

**Fluxo 2 — Entregador aceitando uma corrida**

```
┌─────────────────────────────┐
│   App do Entregador          │
│   Status: 🟢 Disponível       │
│                              │
│  ⚡ NOVA ENTREGA DISPONÍVEL   │
│  Restaurante: Pizza Palace   │
│  Distância: 1,2 km           │
│  Valor: R$ 9,50              │
│  [✅ Aceitar]  [❌ Recusar]   │
└────────────┬────────────────┘
             │ Aceita
             ▼
┌─────────────────────────────┐
│   Detalhes da Entrega        │
│  📍 Retirada: Rua das Rosas  │
│  🏠 Entrega: Rua das Flores  │
│  Itens: 1x Margherita        │
│  [Cheguei ao restaurante]    │
│  [A caminho do cliente]      │
│  [Entrega concluída ✅]       │
└─────────────────────────────┘
```

#### Características de Qualidade

- **Usabilidade:** Interface intuitiva, fluxo de pedido concluível em até 4 telas.
- **Segurança:** Autenticação por login e senha; dados de pagamento não expostos a restaurantes ou entregadores; dados pessoais de entregadores não visíveis ao cliente.
- **Confiabilidade:** Alta disponibilidade nos horários de pico (almoço: 11h–14h / jantar: 18h–21h).
- **Manutenibilidade:** Arquitetura modular que permita atualização independente de cardápios, rotas e pagamentos.

---

## 2. Descrição dos Usuários

### USER-001 — Cliente

| **Atributo** | **Descrição** |
|---|---|
| **Usuário ID** | USER-001 |
| **Nome do Perfil** | Cliente |
| **Descrição** | Pessoa física que utiliza a plataforma para buscar restaurantes, realizar pedidos e acompanhar entregas. |
| **Experiência Técnica** | Baixa a média; habituado ao uso de smartphones e aplicativos de e-commerce. |
| **Frequência de Uso** | Regular; estimativa de 4–10 pedidos mensais. |
| **Principais Objetivos** | Encontrar restaurantes próximos, fazer pedidos com agilidade e acompanhar a entrega em tempo real. |
| **Desafios** | Dificuldade em identificar tempo real de espera; desconfiança em formas de pagamento online. |
| **Restrições** | Acessa apenas restaurantes disponíveis em sua região; visualiza somente seus próprios pedidos. |
| **Requisitos Principais** | Busca por localização, rastreamento em tempo real, histórico de pedidos, avaliações. |

---

### USER-002 — Administrador do Restaurante

| **Atributo** | **Descrição** |
|---|---|
| **Usuário ID** | USER-002 |
| **Nome do Perfil** | Administrador do Restaurante |
| **Descrição** | Proprietário ou gerente responsável por cadastrar e manter as informações do restaurante na plataforma. |
| **Experiência Técnica** | Baixa a média; familiaridade com sistemas de ponto de venda (PDV) e aplicativos de gestão. |
| **Frequência de Uso** | Diária durante o horário de funcionamento do restaurante. |
| **Principais Objetivos** | Manter cardápio atualizado, receber e gerenciar pedidos, visualizar avaliações dos clientes. |
| **Desafios** | Atualização rápida de itens esgotados; gerenciamento simultâneo de múltiplos pedidos em horários de pico. |
| **Restrições** | Gerencia apenas seu próprio restaurante; não tem acesso a dados de pagamento dos clientes. |
| **Requisitos Principais** | Painel de pedidos em tempo real, edição ágil de cardápio, notificações de novos pedidos. |

---

### USER-003 — Entregador

| **Atributo** | **Descrição** |
|---|---|
| **Usuário ID** | USER-003 |
| **Nome do Perfil** | Entregador |
| **Descrição** | Trabalhador autônomo que utiliza a plataforma para aceitar e realizar entregas de pedidos. |
| **Experiência Técnica** | Baixa; uso principalmente de smartphones e aplicativos de navegação. |
| **Frequência de Uso** | Variável; conforme disponibilidade e jornada de trabalho escolhida. |
| **Principais Objetivos** | Aceitar entregas rentáveis, navegar até o destino com eficiência e acompanhar seus ganhos. |
| **Desafios** | Localização imprecisa de clientes; concorrência por corridas em horários de alta demanda. |
| **Restrições** | Acessa apenas informações necessárias para a entrega (endereço, itens); não vê dados pessoais do cliente além do endereço. |
| **Requisitos Principais** | Notificação de novas corridas, integração com mapa, painel de ganhos. |

---

### USER-004 — Administrador da Plataforma

| **Atributo** | **Descrição** |
|---|---|
| **Usuário ID** | USER-004 |
| **Nome do Perfil** | Administrador da Plataforma |
| **Descrição** | Membro da equipe interna do FoodFlow responsável por configurações gerais, suporte e moderação. |
| **Experiência Técnica** | Alta; conhecimento em ferramentas de administração de sistemas e análise de dados. |
| **Frequência de Uso** | Regular; acesso diário para monitoramento e suporte. |
| **Principais Objetivos** | Garantir o funcionamento da plataforma, moderar cadastros e resolver disputas entre usuários. |
| **Desafios** | Escalar suporte em períodos de alta demanda; identificar fraudes em pedidos ou cadastros. |
| **Restrições** | Acesso amplo ao sistema, exigindo autenticação reforçada e logs de auditoria. |
| **Requisitos Principais** | Painel administrativo completo, logs de auditoria, ferramentas de gestão de usuários. |

---

## 3. Restrições do Projeto e do Produto

### 3.1 Tecnologia e Padrões

| **Campo** | **Descrição** |
|---|---|
| **Restrição ID** | NF-CONST-001 |
| **Título** | Restrições Tecnológicas |
| **Descrição** | O sistema deve ser desenvolvido como uma aplicação web e/ou mobile, com back-end acessível via API REST. A integração com mapas deve usar Google Maps API ou OpenStreetMap. O processamento de pagamentos deve utilizar gateway homologado (ex: Stripe, Pagar.me). |
| **Origem** | Equipe de Arquitetura |
| **Critérios de Verificação e Validação** | (a) A arquitetura do sistema deve seguir o padrão REST; (b) O sistema deve integrar corretamente ao gateway de pagamento em ambiente de homologação antes do lançamento. |
| **Relacionamento com outros requisitos** | RF03 (realizar pedidos), RF08 (rastreamento em tempo real) |

---

| **Campo** | **Descrição** |
|---|---|
| **Restrição ID** | NF-CONST-002 |
| **Título** | Restrições de Prazo |
| **Descrição** | O MVP (Produto Mínimo Viável) deve ser entregue dentro do prazo semestral definido pela disciplina. A ordem de prioridade de entregas segue: cadastro de usuários → cardápio → pedidos → rastreamento → avaliações. |
| **Origem** | Coordenação do Projeto |
| **Critérios de Verificação e Validação** | (a) Cada milestone deve ser validado ao final de cada sprint; (b) O MVP deve conter ao menos os fluxos de pedido e entrega funcionando de ponta a ponta. |
| **Relacionamento com outros requisitos** | Todos os RF de RF01 a RF10 |

---

| **Campo** | **Descrição** |
|---|---|
| **Restrição ID** | NF-CONST-003 |
| **Título** | Restrições de Recursos |
| **Descrição** | A equipe de desenvolvimento é composta por estudantes em regime de dedicação parcial. A infraestrutura de cloud deve utilizar planos gratuitos ou de baixo custo (ex: Render, Railway, Supabase, Firebase) durante a fase de desenvolvimento. |
| **Origem** | Equipe de Projeto |
| **Critérios de Verificação e Validação** | (a) Orçamento de infraestrutura deve ser aprovado antes de contratação de qualquer serviço pago; (b) O escopo do MVP deve respeitar a capacidade de entrega da equipe. |
| **Relacionamento com outros requisitos** | NF-CONST-001 | 

### 3.2 Legislação e Regulamentações

| **Campo** | **Descrição** |
|---|---|
| **Restrição ID** | NF-CONST-004 |
| **Título** | Conformidade com a LGPD |
| **Descrição** | O sistema deve estar em conformidade com a Lei Geral de Proteção de Dados (Lei nº 13.709/2018). Dados pessoais de clientes, entregadores e restaurantes devem ser coletados somente com consentimento explícito, armazenados de forma segura e não compartilhados com terceiros sem autorização. |
| **Origem** | Legislação Federal Brasileira |
| **Critérios de Verificação e Validação** | (a) Existência de política de privacidade clara e acessível; (b) Mecanismo de consentimento implementado no cadastro; (c) Dados de pagamento não armazenados diretamente no sistema (delegados ao gateway). |
| **Relacionamento com outros requisitos** | RF-Segurança (autenticação), NF-CONST-001 |

---

## 4. Análise de Riscos e Mitigação

| **Campo** | **Descrição** |
|---|---|
| **ID do Risco** | RISCO-001 |
| **Descrição** | Sincronização de estoque — um item pode estar esgotado no restaurante enquanto o cliente conclui o pedido. |
| **Categoria** | Técnico |
| **Probabilidade** | Alta |
| **Impacto** | Alto |
| **Ação de Mitigação** | Implementar atualização em tempo real da disponibilidade de itens pelo administrador do restaurante; exibir aviso de "item limitado" no cardápio. |
| **Plano de Contingência** | Notificar o cliente e permitir substituição ou cancelamento parcial do pedido sem cobrança de taxa. |

---

| **Campo** | **Descrição** |
|---|---|
| **ID do Risco** | RISCO-002 |
| **Descrição** | Sobrecarga do sistema nos horários de pico (almoço e jantar), causando lentidão ou falhas. |
| **Categoria** | Técnico |
| **Probabilidade** | Alta |
| **Impacto** | Alto |
| **Ação de Mitigação** | Utilizar infraestrutura com auto-scaling (ex: serviços cloud com escalabilidade horizontal); realizar testes de carga antes do lançamento. |
| **Plano de Contingência** | Implementar fila de pedidos com mensagem ao usuário sobre tempo estimado elevado; acionar suporte técnico imediatamente. |

---

| **Campo** | **Descrição** |
|---|---|
| **ID do Risco** | RISCO-003 |
| **Descrição** | Localização imprecisa do GPS em áreas com sinal fraco, prejudicando rastreamento do entregador e busca do endereço do cliente. |
| **Categoria** | Técnico / Externo |
| **Probabilidade** | Média |
| **Impacto** | Médio |
| **Ação de Mitigação** | Integrar com API de mapas de alta confiabilidade; permitir que o entregador confirme ou corrija manualmente o endereço de entrega. |
| **Plano de Contingência** | Disponibilizar campo de observações no pedido para que o cliente informe pontos de referência; habilitar contato direto entre entregador e cliente via chat anonimizado. |

---

| **Campo** | **Descrição** |
|---|---|
| **ID do Risco** | RISCO-004 |
| **Descrição** | Demora na alocação de entregadores disponíveis próximos ao restaurante, resultando em comida fria e insatisfação do cliente. |
| **Categoria** | Operacional |
| **Probabilidade** | Média |
| **Impacto** | Alto |
| **Ação de Mitigação** | Implementar algoritmo de alocação por proximidade geográfica em tempo real; ampliar base de entregadores cadastrados. |
| **Plano de Contingência** | Notificar o cliente sobre o atraso com previsão atualizada; oferecer cupom de desconto em caso de atraso significativo. |

---

| **Campo** | **Descrição** |
|---|---|
| **ID do Risco** | RISCO-005 |
| **Descrição** | Baixa adesão inicial de restaurantes parceiros, tornando a plataforma pouco atrativa para os clientes. |
| **Categoria** | Negócio / Externo |
| **Probabilidade** | Média |
| **Impacto** | Alto |
| **Ação de Mitigação** | Realizar campanha de captação de parceiros com isenção de comissão nos primeiros meses; focar o lançamento em uma região geográfica limitada para garantir densidade de oferta. |
| **Plano de Contingência** | Reduzir área de cobertura inicial para garantir experiência de qualidade com os parceiros disponíveis. |

---

| **Campo** | **Descrição** |
|---|---|
| **ID do Risco** | RISCO-006 |
| **Descrição** | Não conformidade com a LGPD, expondo a plataforma a penalidades legais e perda de confiança dos usuários. |
| **Categoria** | Legal / Regulatório |
| **Probabilidade** | Baixa |
| **Impacto** | Alto |
| **Ação de Mitigação** | Revisar fluxos de coleta de dados desde o início do desenvolvimento; nunca armazenar dados de cartão diretamente; implementar política de privacidade acessível. |
| **Plano de Contingência** | Consultar jurídico especializado em privacidade de dados; suspender coleta de dados específicos até regularização. |

---

## 5. Referências

[1] ASSOCIAÇÃO BRASILEIRA DE BARES E RESTAURANTES (ABRASEL). **O setor de food service no Brasil em 2023**. Disponível em: https://abrasel.com.br. Acesso em: março de 2026.

[2] SEBRAE. **Delivery: como as pequenas empresas podem crescer com o digital**. Disponível em: https://www.sebrae.com.br. Acesso em: março de 2026.

[3] PROCON-SP. **Delivery de alimentos: taxas, comissões e direitos do consumidor**. Disponível em: https://www.procon.sp.gov.br. Acesso em: março de 2026.

[4] STATISTA. **Online Food Delivery — Brazil**. Disponível em: https://www.statista.com/outlook/dmo/ecommerce/online-food-delivery/brazil. Acesso em: março de 2026.

[5] IEEE Std 830-1998. **IEEE Recommended Practice for Software Requirements Specifications**. Institute of Electrical and Electronics Engineers, 1998. Disponível em: https://professor.pucgoias.edu.br/sitedocente/admin/arquivosUpload/17785/material/IEEE830.pdf. Acesso em: março 2026.

[6] ISO/IEC/IEEE 29148:2018. **Systems and software engineering — Life cycle processes — Requirements engineering**. International Organization for Standardization, 2018. Disponível em: https://www.iso.org/obp/ui/en/#iso:std:iso-iec-ieee:29148:ed-2:v1:en. Acesso em: março de 2026.

[7] BRASIL. **Lei nº 13.709, de 14 de agosto de 2018 — Lei Geral de Proteção de Dados Pessoais (LGPD)**. Disponível em: https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm. Acesso em: março de 2026.# Documento de Visão do Produto — FoodFlow
