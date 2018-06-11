# Arquitetura
Uma visão geral da arquitetura escolhida está ilustrada na imagem abaixo.

![Imagem da Arquitetura do Sistema](https://github.com/tuliocpbs/odesafio/blob/master/imagens/arquitetura.png)

Passos para o tráfego de dados na arquitetura:
1. As bases A, B e C enviaram os dados para o Kafka.
2. Os serviços 1, 2 e 3 irão consumir os conteúdos necessários do Kafka.
3. Cada serviço irá armazenar seus dados na Warehouse D.
4. A aplicação Web irá ler os dados necessários da Warehouse D com o auxílio da ferramenta Hive.

## Explicações sobre as tecnologias adotadas

### Porque usar o Kafka?
O Kafka irá funcionar como um intermediário entre as bases externas e a Warehouse D. A utilização do Kafka traz as seguintes vantagens:
* Organiza em filas bem estruturadas o dado que será consumido.
* Evita sobrecarga na base.
* Uma melhor abordagem para inserir dados em D, que é através de uma fila de mensagens, ao invés de escrita direto na base.

### Porque usar uma Warehouse?
O uso de uma Warehouse é principalmente pelas diferentes necessidades dos 3 serviços descritos. Outras vantagens adicionadas pelo uso de uma Warehouse é:
* Aumenta perfomance do sistema
* Fácil e rápido acesso aos dados
* Aumenta a qualidade e consistência dos dados
* Resolve o problema de redundância de dados
* Facilita a composição de dados provindos de diferentes tipos de bases

### Porque usar a ferramenta Hive?
O Hive é uma ferramenta que irá facilitar a leitura, escrita e gerenciamento da grande quantidades de dados contida na Warehouse.

## Detalhando a Arquitetura

### Serviços
Os serviços 1, 2 e 3 serão "dockerizados". Com esses serviços dentro de containers a escalabilidade se necessário ser mais fácil, assim como outras vantagens são adicionadas com a ferramenta Docker, como por exemplo:
* Mais fácil de manter o sistema
* Maior compatibilidade
* Padronização
* Melhora na produtividade, por conta de possíveis rollbacks para imagens anteriores do Docker

O serviço 2, que irá utilizar os dados da Base B, tem uma peculiaridade por ser utilizado para o  cálculo do Score de Crédito. Para esse caso a ferramenta Lambda:

> O AWS Lambda é um serviço de computação sem servidor que executa seu código em resposta a eventos e gerencia automaticamente os recursos computacionais adjacentes para você. [Amazon]

### Comunicação
Alguns exemplos de comunições do sistema.

#### Histórico de movimentações financeiras de um cliente
Para disponibilizar um histórico de movimentações financeiras de um cliente será necessário utilizar o Serviço 1 e o Serviço 3. O resultado final será um json com o nome do cliente e seu histórico de movimentações financeiras.

Esse json será montado da seguinte maneira:
1. O cliente escolhido será através do CPF
2. Com o CPF será feita uma busca (Busca 1) na Base C, retornando as movimentações financeiras daquele cliente
3. O json final será montado com o nome do cliente da Base A e o retorno da Busca 1

Esse json será enviado para o serviço final (Web).

#### Ranking dos Top 10 clientes com melhores Score de Crédito
Para disponibilizar o ranking será necessário acionar o Serviço 1 e o Serviço 2. O resultado final será uma lista jsons com o nome do cliente, e o Score de Crédito.

A comunicação será efutuada da seguinte maneira:
1. O Serviço 2 será acionado para calcular os Scores de Crédito de cada cliente
2. Os Scores juntamente com o CPF do cliente serão armazenados em D
3. Com esse CPF armazenado em D, será feita a busca no Serviço 1 pelo nome do cliente
4. Cada json da lista terá o nome do cliente, Score de Crédito e posicionamento no rank (de 1 a 10)

### Melhorando o Score de Crédito com aprendizagem de máquina online
Com os dados disponíveis nos Serviço 2 e Serviço 3, é possível treinar modelos online utilizando a ferramenta Lambda (aprendizagem de máquina online) para prever futuras dívidas que um cliente pode vir a adquirir. Esse modelo será disparado sempre que uma nova transação do cliente for feita, com o passar do tempo esse modelo estará bem treinado e poderá ser utilizado para a previsão. Os dados que serão utilizados para treinar o modelo são lista de bens e fonte de renda provenientes do Serviço 2 e movimentação financeira, dados relacionados a última compra do  do Serviço 3. O modelo treinado será armazenado com dado no Warehouse D.

Com o modelo treinado, uma nova variável poderá ser adicionada no cálculo do Score de Crédito. E o mesmo Ranking dos Top 10 clientes poderá ser feito, só que agora com mais informação.
