Plataforma de Delivery de Alimentos (FoodFlow)
1. Escopo e Objetivos
Muitas pessoas desejam pedir comida de restaurantes sem precisar sair de casa ou do trabalho. Ao mesmo tempo, restaurantes querem alcançar mais clientes e entregadores buscam oportunidades de trabalho flexível.

A plataforma FoodFlow conecta clientes, restaurantes e entregadores. O objetivo principal é permitir que o cliente escolha um restaurante, faça seu pedido e acompanhe a entrega até a chegada da comida.

2. Descrição do Sistema
O sistema é organizado por Restaurantes Parceiros. Cada restaurante gerencia seu próprio cardápio, preços e horários de funcionamento. Os clientes podem buscar restaurantes próximos, fazer pedidos e acompanhar a entrega em tempo real.

O fluxo básico de um pedido:

Cliente escolhe restaurante e itens do cardápio
Cliente confirma o pedido e realiza o pagamento
Restaurante recebe o pedido e prepara a comida
Entregador coleta o pedido no restaurante
Entregador leva o pedido até o cliente
Cliente recebe a comida e avalia o serviço
3. Atores e Fluxos Principais
Administrador do Restaurante
Cadastrar o restaurante na plataforma
Gerenciar o cardápio (criar, editar, remover itens)
Definir preços e disponibilidade dos itens
Definir horários de funcionamento
Receber e gerenciar pedidos
Visualizar avaliações dos clientes
Permissões: Gerencia apenas seu próprio restaurante e pedidos recebidos.

Entregador
Cadastrar-se como entregador na plataforma
Definir disponibilidade para entregas
Receber ofertas de entrega baseadas na localização
Aceitar ou recusar entregas
Informar status da entrega (coletou, em trânsito, entregue)
Visualizar seus ganhos
Permissões: Vê apenas informações necessárias para a entrega (endereço, itens do pedido).

Cliente
Buscar restaurantes por localização e tipo de comida
Visualizar cardápios e preços
Fazer pedidos e escolher forma de pagamento
Acompanhar o status do pedido e localização do entregador
Avaliar restaurante e entregador após a entrega
Salvar endereços favoritos
Permissões: Acessa restaurantes disponíveis na sua região e seus próprios pedidos.

4. Requisitos Funcionais
RF01: O sistema deve permitir cadastrar restaurantes com cardápio
RF02: O sistema deve permitir buscar restaurantes por localização
RF03: O sistema deve permitir fazer pedidos com itens do cardápio
RF04: O sistema deve calcular o valor total do pedido incluindo taxa de entrega
RF05: O sistema deve notificar o restaurante sobre novos pedidos
RF06: O sistema deve permitir que o restaurante confirme ou recuse pedidos
RF07: O sistema deve oferecer entregas para entregadores disponíveis
RF08: O sistema deve mostrar a localização do entregador em tempo real
RF09: O sistema deve permitir avaliação do restaurante e entregador
RF10: O sistema deve manter histórico de pedidos do cliente
5. Requisitos Não-Funcionais
Estimativa de usuários: 2.000-10.000 usuários ativos por mês
Volume de dados: ~20.000 pedidos no primeiro ano
Segurança básica: Login com usuário e senha; clientes não veem dados pessoais de entregadores; restaurantes não veem dados de pagamento
Disponibilidade esperada: Alta (especialmente nos horários de pico: almoço e jantar)
6. Integrações Externas
Sistema de mapas: Para mostrar restaurantes próximos e rastrear entregadores
Sistema de pagamentos: Para processar pagamentos de pedidos
Envio de notificações: Para alertar restaurantes, entregadores e clientes sobre atualizações
7. Riscos e Desafios Técnicos
Sincronização de estoque: Um item pode acabar no restaurante enquanto o cliente está fazendo o pedido. É importante manter o cardápio atualizado.

Demora na entrega: Se demorar muito para encontrar um entregador, a comida pode esfriar. O sistema precisa ser ágil na alocação.

Localização imprecisa: O GPS pode mostrar localização errada em alguns lugares. O entregador pode ter dificuldade em encontrar o endereço.

Horários de pico: Nos horários de almoço e jantar, muitos pedidos acontecem ao mesmo tempo. O sistema precisa aguentar a demanda.

