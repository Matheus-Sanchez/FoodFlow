# FoodFlow — Plataforma de Delivery de Alimentos

> **Trabalho 3 — Modelagem de Software**  
> Disciplina: Engenharia de Software  

---

## Sobre o Projeto

O **FoodFlow** é uma plataforma de delivery de alimentos que conecta clientes que desejam conveniência, restaurantes que buscam ampliar seu alcance digital e entregadores que necessitam de trabalho flexível.

Este repositório contém os artefatos de modelagem do **Trabalho 3**, construídos sobre a Visão do Produto (T1) e os Requisitos (T2).

---

## Documentação — Trabalho 3

| Seção | Arquivo | Descrição |
|-------|---------|-----------|
| **0. Seleção de Escopo** | [00-selecao-de-escopo.md](docs/00-selecao-de-escopo.md) | Justificativa das 3 fatias verticais selecionadas |
| **1. Diagrama de Classes** | [01-diagrama-de-classes.md](docs/01-diagrama-de-classes.md) | Classes, atributos, métodos e relações UML |
| **2. MER** | [02-mer.md](docs/02-mer.md) | Modelo Entidade-Relacionamento |
| **3. Comportamental — Fatia 1** | [03-comportamental-fatia1.md](docs/03-comportamental-fatia1.md) | Diagrama de Sequência — Checkout com pagamento |
| **3. Comportamental — Fatia 2** | [03-comportamental-fatia2.md](docs/03-comportamental-fatia2.md) | Diagrama de Estados — Ciclo de vida da entrega |
| **3. Comportamental — Fatia 3** | [03-comportamental-fatia3.md](docs/03-comportamental-fatia3.md) | Diagrama de Atividades — Restaurante recebe e prepara pedido |
| **4. Casos de Teste** | [04-casos-de-teste.md](docs/04-casos-de-teste.md) | 6 casos de teste IEEE (2 por fatia) |
| **5. Rastreabilidade** | [05-rastreabilidade.md](docs/05-rastreabilidade.md) | Tabela de rastreabilidade entre artefatos |
| **Referências** | [references.md](references.md) | Bibliografia ABNT |

---

## Visão do Produto

O documento de visão completo (acumulativo T1 + T2) está em:  
[foodflow-visao-produto.md](foodflow-visao-produto.md)

---

## Atores do Sistema

| Ator | Papel |
|------|-------|
| **Cliente** | Busca restaurantes, faz pedidos e acompanha entregas |
| **Admin do Restaurante** | Gerencia cardápio e aceita/processa pedidos |
| **Entregador** | Aceita corridas e realiza a entrega física |
| **Admin da Plataforma** | Modera cadastros, configura comissões, monitora o sistema |

---

## Fatias Verticais Modeladas

| Fatia | Fluxo | Prioridade MoSCoW |
|-------|-------|-------------------|
| **Fatia 1** | Cliente realiza pedido e conclui pagamento | Must Have |
| **Fatia 2** | Ciclo de vida da entrega (aceite → confirmação) | Must Have |
| **Fatia 3** | Restaurante recebe pedido, processa e aciona entrega | Must Have + regras de negócio |
