# Dicionário de dados

Fonte: `dados/PalcoJa_Base_Ficticia.xlsx`. Dados inteiramente fictícios para estudo. Valores monetários em reais (BRL).

## Vendas — tbVendas

Cada linha representa um pedido. A quantidade pode ser maior que um, mas todos os ingressos do pedido pertencem ao mesmo evento, setor, lote e tipo. `id_pedido` é a chave única. As datas de compra pertencem a 2025 e antecedem a data do show.

| Campo | Tipo no Power BI | Definição |
| --- | --- | --- |
| id_pedido | Texto | Identificador único do pedido. |
| data_compra | Data | Data de criação do pedido. |
| id_evento | Texto | Referência à chave da tabela Eventos. |
| id_cliente | Texto | Identificador fictício, reutilizado em compras do mesmo cliente. |
| cidade_cliente | Texto | Cidade de residência fictícia do cliente. |
| uf_cliente | Texto | Estado de residência fictícia do cliente. |
| faixa_etaria | Texto | Faixa etária do comprador. |
| sistema_app | Texto | Android ou iOS. |
| origem_venda | Texto | Orgânico, Instagram Ads, Google Ads, Indicação ou Push do app. |
| setor | Texto | Pista, Pista premium ou Camarote. |
| lote | Texto | 1º, 2º ou 3º lote. |
| tipo_ingresso | Texto | Inteira ou Meia-entrada. |
| quantidade | Número inteiro | Quantidade solicitada, de 1 a 4. Inclui pedidos não pagos. |
| preco_unitario | Número decimal fixo | Preço por ingresso antes do cupom. Já considera setor, lote e meia-entrada. |
| cupom | Texto | Sem cupom, PALCO10 ou PRIMEIRACOMPRA. Ambos os cupons concedem 10%. |
| valor_desconto | Número decimal fixo | Desconto total do cupom no pedido. |
| valor_ingressos | Número decimal fixo | Quantidade multiplicada pelo preço unitário, antes do desconto e da taxa. |
| taxa_servico | Número decimal fixo | 10% do valor dos ingressos após desconto, arredondado a centavos. |
| valor_pedido | Número decimal fixo | Valor dos ingressos menos desconto mais taxa, mesmo quando não houve pagamento. |
| forma_pagamento | Texto | Pix, Cartão de crédito ou Boleto. |
| parcelas | Número inteiro | De 1 a 4 no cartão. Pix e boleto têm valor 1. |
| status_pedido | Texto | Pago, Reembolsado, Cancelado ou Recusado. Situação final. |
| valor_pago | Número decimal fixo | Total cobrado nos pedidos Pago ou Reembolsado. Zero nos demais. |
| valor_reembolsado | Número decimal fixo | Estorno integral do pedido Reembolsado. Zero nos demais. |
| ingressos_utilizados | Número inteiro | Check-ins simulados, de zero até a quantidade comprada. Zero fora do status Pago. |

### Regras de status

| Status | Houve cobrança? | Houve estorno? | Conta como venda mantida? |
| --- | --- | --- | --- |
| Pago | Sim | Não | Sim |
| Reembolsado | Sim | Sim, integral | Não |
| Cancelado | Não | Não | Não |
| Recusado | Não | Não | Não |

Não somar `valor_pedido` para representar recebimentos: ele inclui pedidos não pagos. Para o recebimento líquido, usar `valor_pago − valor_reembolsado`.

### Lotes

- **1º lote:** compra com mais de 60 dias de antecedência.
- **2º lote:** compra entre 26 e 60 dias antes do evento.
- **3º lote:** compra entre 1 e 25 dias antes do evento.

Esses intervalos são regras da simulação, não uma prática universal do setor.

## Eventos — tbEventos

Uma linha por show. `id_evento` é único e se relaciona com vários pedidos em Vendas.

| Campo | Tipo no Power BI | Definição |
| --- | --- | --- |
| id_evento | Texto | Identificador único do evento. |
| nome_evento | Texto | Nome fictício do show. |
| artista | Texto | Artista ou grupo fictício. |
| genero | Texto | Rock, Pop, Sertanejo, Samba, Eletrônica ou Rap. |
| cidade_evento | Texto | Cidade de realização do evento. |
| uf_evento | Texto | Estado de realização do evento. |
| local | Texto | Nome fictício do espaço. |
| data_evento | Data | Data do show, entre março e dezembro de 2025. |
| capacidade | Número inteiro | Quantidade máxima de ingressos do evento inteiro, sem divisão por setor. |

Somar a capacidade na tabela Eventos, evitando replicá-la em cada pedido. A ocupação por setor não pode ser calculada, pois não há capacidade específica de cada setor.

## Aba Guia

Documentação para leitura. Contém definições de indicadores, relacionamento e sugestões de páginas. Não é uma tabela de fatos ou dimensões e não precisa ser importada no modelo.
