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

### Porque usar a ferramenta Hive?
O Hive é uma ferramenta que irá facilitar a leitura, escrita e gerenciamento da grande quantidades de dados contida na Warehouse.

## Detalhando a Arquitetura

### Serviços
