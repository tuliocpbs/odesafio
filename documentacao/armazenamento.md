# Armazenamento

Embora no desafio esteja explícito que não é necessário escrever sobre a implementação das bases de dados, acho interessante levantar alguns pontos. Outra observação a ser feita é que para simplificar a escolha dos tipos de cada base de dados (o motivo da simplificaço está explicada num tópico ao final do artigo) vou utilizar o Teorema de CAP, que diz:
> É impossível que o o armazenamento de dados distribuído forneça simultaneamente mais de duas das três garantias seguintes:
>* Consistência: Cada leitura recebe a escrita mais recente ou um erro.
>* Disponibilidade: Cada pedido recebe uma resposta (sem erro) - sem garantia de que contém a escrita mais recente.
>* Tolerância a falhas: O sistema continua a funcionar apesar de um número arbitrário de mensagens serem descartadas (ou atrasadas) pela rede entre nós.

![Imagem do Teorema de CAP](https://github.com/tuliocpbs/odesafio/blob/master/imagens/teorema_cap.png)

## *Base A*
Como a Base A não tem tanta necessidade na disponibilidade dos dados, e o foco seria em Consistência e Tolerância a falhas então para a Base A escolheria um MongoDB. Como será utilizado um grande volume de dados, poderá ser criado indexadores para acessar os dados mais rapidamente.

Uma observação a ser feita é que estou escolhendo a base de dados MongoDB também pela familiaridade com a ferramenta, mas outras possíveis base de dados que poderiam ser analisadas são Hbase e Redis. Essas bases possuem o mesmo foco de Consistência e Tolerância buscados.

Pelo alto nível de segurança que é necessário na Base A, listo alguns importantes pontos a serem considerados:
* Rigoroso controle de acesso, limitando a quantidade de pessoas que podem acessá-lo
* Informação contida no banco ser criptografada
* Criar versões de testes marcaradas/criptografadas, e garantir que esse processo seja irreversível
* Monitorar as atividades da base através de auditorias, armazenamentos de ações feitas sob a base e análises de ações suspeitas

Além desses fatores, acredito que a segurança na base de dados A pode ser melhor provida através de uma infraestrutura mais robusta com o uso de mais de uma base de dados.

<!---Dados da Base A: CPF, Nome, Endereço, Lista de dívidas--->

## *Base B*
A Base B tem como principais necessidades a Consistência (utilizar os dados corretos) e a Disponibilidade (acesso mais rápido aos dados), deixando em segundo plano a Tolerância a Falhas. Acrescentando o fato de já ter trabalhado com essa base de dados, para a Base B escolheria PostgreSQL. Mas outras bases que poderiam ser analisadas são Oracle, IBM DB2, todas RDBMS (Relational Database Management Systems) e possuem a mesma finalidade.

Alguns pontos tratados para a Base A com relação a segurança dos dados podem também ser utilizados na Base B.

Na hora de consultar a Base B é preciso levar em consideração também que para o serviço de cálculo do Score de Crédito o melhor fazer uma consulta por bloco. Isso agilizará o processo de consulta para esse serviço.

<!---Dados da Base B: Idade, lista de bens, Endereço, Fonte de renda--->

## *Base C*
Na Base C a busca é por Consistência e Disponibilidade como na Base B. A escolha de uma base de dados PostgreSQL será mantida pelo mesmo motivos.

Para agilizar o acesso a base de dados C é necessário tomar algumas decisões extras, como:
* Adicionar indexadores secundários
* Otimizar as queries de acesso ao banco
* Carregar apenas o que for usar

<!---Dados da Base C: Última consulta do CPF em um Bureau de crédito, Movimentação Financeira no CPF, Dados relacionados a última compra com o cartão de crédito vinculado ao CPF--->

## Porque simplificar com o Teorema de CAP
Caso não fosse feita a simplifição com o Teorema de CAP outros fatores gerais que deveriam ser analisados são:
* Arquitetura de banco de dados escolhida (isso pode variar muito por conta da possibilidade de utilizar mais de um tipo de base de dados num mesmo projeto - Persistência Poliglota).
* Limites técnicos de cada base de dados como por exemplo quantidade de acessos por unidade de tempo, tempo de resposta, entre outros.
* Variedade de bases de dados disponíveis no mercado.

Em resumo, é possível montar várias arquiteturas diferentes para dar suporte a um mesmo projeto. Para definir uma boa arquitetura é necessário mais tempo e uma análise mais detalhada. 

A imagem a seguir mostra algumas opções de bases de dados disponíveis no mercado, além de outras tecnologias.

![Imagem de Algumas Opções de Bases de Dados](https://github.com/tuliocpbs/odesafio/blob/master/imagens/datafloq.jpg)
