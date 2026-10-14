# Aprendizado supervisionado e não supervisionado aplicados ao projeto de churn

Lucas Farias Epitácio de Almeida · Curso de Ciência de Dados, EBAC

## 1. Diferença entre aprendizado supervisionado e não supervisionado

No aprendizado supervisionado, o modelo aprende a partir de exemplos que já vêm com a resposta. Cada registro da base de treino tem as variáveis explicativas e também o rótulo, que é o valor que queremos prever no futuro (por exemplo, se o cliente cancelou ou não). O algoritmo procura a relação entre as variáveis e esse rótulo para depois aplicá-la a casos novos, cuja resposta ainda não conhecemos. Quando o rótulo é uma categoria, o problema é de classificação; quando é um número contínuo, é de regressão.

No aprendizado não supervisionado não existe rótulo. O modelo recebe só as variáveis e tenta encontrar alguma estrutura nos dados por conta própria, como grupos de registros parecidos entre si (agrupamento ou clusterização), combinações de variáveis que resumem a informação (redução de dimensionalidade) ou registros que fogem muito do padrão (detecção de anomalias). Como não há uma resposta certa para comparar, a interpretação do resultado depende bastante de quem analisa e do conhecimento sobre o negócio.

Na prática, a escolha entre os dois depende de duas perguntas. A primeira é sobre os dados: existe um histórico em que a resposta que me interessa foi registrada? A segunda é sobre o negócio: eu quero prever um resultado que já sei qual é (vai pagar ou não, vai cancelar ou não, quanto vai vender) ou quero descobrir padrões que ainda não conheço? Se as duas respostas apontam para "tenho a resposta e quero prevê-la", o caminho é o supervisionado. Se não existe rótulo, ou se o objetivo é justamente explorar e organizar os dados, o caminho é o não supervisionado. Os exemplos a seguir foram escolhidos para mostrar essa relação entre dado disponível, objetivo e método.

## 2. Exemplos de problemas de negócio com aprendizado supervisionado

Concessão de crédito. Os bancos têm um histórico de contratos já encerrados em que se sabe se o cliente pagou ou ficou inadimplente, além de dados como renda, idade, tempo de emprego, dívidas e histórico de pagamentos. O objetivo do negócio é decidir, no momento do pedido, se o crédito deve ser aprovado, ou seja, prever um desfecho que só vai acontecer meses depois. Como o desfecho do passado está registrado e o que se quer é antecipar esse mesmo desfecho para clientes novos, o problema é supervisionado, do tipo classificação. O modelo devolve uma probabilidade de inadimplência, e o banco escolhe o ponto de corte de acordo com o risco que aceita correr. A regressão logística é bastante usada nesse caso porque os coeficientes mostram o peso de cada variável, o que ajuda a justificar a decisão para o cliente e para a área de risco.

Detecção de fraude em cartão de crédito. As operadoras guardam o histórico de transações, e as fraudes ficam marcadas quando o cliente contesta a compra. Com isso, cada transação antiga tem um rótulo: fraude ou legítima. O objetivo é decidir em segundos se uma compra nova deve ser aprovada, bloqueada ou confirmada com o cliente. Como existe o rótulo e o que se quer é aplicar o mesmo julgamento a transações futuras, o problema é de classificação supervisionada. A escolha do método e da métrica tem que levar em conta que fraude é rara (bem menos de 1% das transações), então a acurácia sozinha engana. O que importa é equilibrar o recall (quantas fraudes são pegas) com a precisão (quantos clientes honestos são bloqueados à toa). Modelos de árvores, como random forest e gradient boosting, costumam ser usados porque captam combinações de sinais, como valor alto, horário incomum e local diferente do habitual.

Previsão de vendas ou de demanda. O varejo tem o registro de quanto vendeu de cada produto em cada dia ou semana, junto com informações de preço, promoções, feriados e época do ano. O objetivo é saber quanto vai vender no próximo período para planejar compras, estoque e escala de funcionários. A resposta já está registrada no histórico (a quantidade vendida), então o problema é supervisionado; como essa resposta é um número, e não uma categoria, o método é de regressão. O erro do modelo é medido na mesma unidade do negócio, por exemplo em unidades vendidas, o que facilita traduzir o erro em sobra ou falta de estoque.

## 3. Exemplo de problema de negócio com aprendizado não supervisionado

Segmentação de clientes. Uma empresa de varejo ou de telecom tem os dados de cadastro e de consumo dos clientes: frequência de compra, valor gasto, tempo desde a última compra, serviços contratados. O que ela não tem é uma coluna dizendo a que grupo cada cliente pertence, porque esses grupos ainda não foram definidos. O objetivo do negócio também é diferente dos casos anteriores: em vez de prever um evento, a empresa quer entender que perfis de clientes existem para direcionar campanhas, produtos e atendimento. Sem rótulo e com objetivo de descoberta, o problema é de aprendizado não supervisionado, do tipo agrupamento.

O K-means é uma escolha comum porque trabalha bem com variáveis numéricas de consumo e gera grupos fáceis de descrever pelo centro de cada um (por exemplo, "clientes antigos, de alto gasto e que compram toda semana"). Como ele se baseia em distâncias, as variáveis precisam ser padronizadas antes, senão a de maior escala domina o resultado. O número de grupos é definido com apoio de métodos como o cotovelo e a silhueta, mas também pelo uso que o marketing consegue fazer deles. Aqui não existe gabarito para conferir: o resultado é bom quando os grupos fazem sentido e levam a ações diferentes, como fidelização para os clientes de alto valor e ofertas de entrada para os novos.

Outro uso comum é a detecção de anomalias, em transações ou em leituras de sensores industriais. Ela se aplica quando não existe um histórico rotulado do que é um evento anormal, ou quando surgem padrões novos que o modelo supervisionado nunca viu. Por isso, em fraude, as empresas costumam combinar as duas abordagens.

## 4. Por que a base de teste não deve conter os rótulos a serem previstos

A base de teste serve para simular a situação real de uso do modelo, em que ele vai receber clientes novos e não vai saber a resposta. Por isso, no momento da previsão, o modelo só pode ver as variáveis explicativas do teste. Se o rótulo estiver entre as variáveis de entrada, o modelo simplesmente usa a própria resposta para "prever" a resposta, e o desempenho medido fica artificialmente perfeito ou quase perfeito. Esse problema é chamado de vazamento de dados (data leakage).

Os rótulos verdadeiros do teste não são jogados fora. Eles ficam guardados separadamente, como um gabarito, e só são usados depois que o modelo já fez suas previsões, para comparar o que foi previsto com o que de fato aconteceu e calcular métricas como acurácia, precisão, recall e F1. Também é por isso que o teste não deve participar do treino: um modelo avaliado em dados que ele já viu tende a parecer melhor do que realmente é, e esse otimismo some quando ele é usado com dados novos.

## 5. Tipo de aprendizado ideal para prever o churn da base trabalhada desde o módulo 14

O tipo ideal é o aprendizado supervisionado, mais especificamente um problema de classificação binária. A justificativa aparece quando se olha para as mesmas três coisas usadas nos exemplos anteriores.

Os dados disponíveis. A base de clientes de telecom que pré-processei no módulo 14 e analisei no módulo 15 tem 2.495 registros e uma coluna Churn que informa, para cada cliente, se ele cancelou ou não o serviço. Esse rótulo foi registrado depois que o desfecho aconteceu, então a base é um histórico com a resposta conhecida. Junto dele estão as variáveis que descrevem o cliente antes do cancelamento: tipo de contrato, tempo como cliente, tipo de internet, forma de pagamento, serviços de suporte e segurança, mensalidade, entre outras.

O objetivo do negócio. A empresa quer saber, entre os clientes que ainda estão ativos, quais têm mais chance de cancelar, para agir antes que isso aconteça com ofertas de retenção, migração para contratos mais longos ou contato da equipe de suporte. É o mesmo desfecho que está registrado na base, só que para clientes cujo futuro ainda não se conhece. Prever um desfecho conhecido a partir de um histórico rotulado é exatamente o caso do aprendizado supervisionado, e como o desfecho tem só duas categorias (cancelou ou não cancelou), o método é de classificação binária.

A escolha do método. Um bom ponto de partida é a regressão logística, porque ela devolve a probabilidade de churn de cada cliente, o que permite ordenar a carteira por risco, e os coeficientes mostram o peso de cada variável, o que facilita explicar o resultado para a área de negócio. Em seguida vale testar árvores de decisão e random forest, que captam combinações de fatores. A análise exploratória sugere que essas combinações existem: clientes com contrato mensal cancelaram 42,4%, contra 1,5% entre os de contrato de dois anos; no primeiro ano de casa o churn chegou a 45,8%; quem não tinha suporte técnico cancelou 42%, contra 12,6% de quem tinha. Um cliente novo, de contrato mensal e sem suporte junta vários desses fatores ao mesmo tempo.

Os próprios dados também definem alguns cuidados. Como só cerca de 26% dos clientes cancelaram, a variável alvo é desbalanceada, então a divisão entre treino e teste deve ser estratificada. A avaliação deve dar atenção ao recall e ao F1 da classe de churn, e não só à acurácia, já que deixar passar um cliente que vai cancelar costuma custar mais caro do que oferecer retenção a quem ficaria de qualquer jeito. Além disso, Tempo_como_Cliente e Total_Pago têm correlação de 0,83, e uma delas deve ser removida ou tratada antes da modelagem.

O aprendizado não supervisionado não seria adequado como método principal. Um agrupamento formaria grupos pela semelhança geral entre os clientes, sem usar a informação de quem cancelou, e nada garante que esses grupos separem quem sai de quem fica. Ele também não entrega uma probabilidade de churn para cada cliente, que é o que a área de retenção precisa. Pode servir como complemento, por exemplo para segmentar os clientes de alto risco apontados pelo modelo e montar ações diferentes para cada perfil.

## Quadro-resumo: dado disponível, objetivo e método

| Problema | Dados disponíveis | Objetivo do negócio | Tipo e método |
|---|---|---|---|
| Concessão de crédito | Contratos antigos com rótulo pagou/inadimplente | Aprovar ou negar crédito antes do desfecho | Supervisionado, classificação (ex.: regressão logística) |
| Fraude em cartão | Transações marcadas como fraude ou legítima | Bloquear compras suspeitas em tempo real | Supervisionado, classificação (ex.: gradient boosting) |
| Previsão de demanda | Vendas passadas em quantidade | Planejar compras e estoque | Supervisionado, regressão |
| Segmentação de clientes | Dados de consumo, sem grupos definidos | Descobrir perfis para campanhas | Não supervisionado, agrupamento (ex.: K-means) |
| Churn (base do módulo 14) | 2.495 clientes com coluna Churn (sim/não) | Identificar clientes ativos com risco de cancelar | Supervisionado, classificação binária |

## Referências

- GÉRON, Aurélien. Mãos à obra: aprendizado de máquina com Scikit-Learn, Keras & TensorFlow. 2. ed. Rio de Janeiro: Alta Books, 2021.
- JAMES, Gareth; WITTEN, Daniela; HASTIE, Trevor; TIBSHIRANI, Robert. An Introduction to Statistical Learning. 2. ed. New York: Springer, 2021.
- Material de aula do curso de Ciência de Dados da EBAC e análises próprias dos módulos 14 e 15.
