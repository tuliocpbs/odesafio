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

<!---A segunda, é a Base B que também possui dados críticos, mas ao contrário da Base A, o acesso precisa ser um pouco mais rápido. Uma outra característica da Base B é que além de consultas ela é utilizada para extração de dados por meio de algoritmos de aprendizado de máquina.--->

## *Base B*


<!---A última base, é a Base C, que não possui nenhum tipo de dado crítico, mas precisa de um acesso extremamente rápido.--->

## *Base C*


## Porque simplificar com o Teorema de CAP

Caso não fosse feita a simplifição com o Teorema de CAP outros fatores gerais que deveriam ser analisados são:
* Arquitetura de banco de dados escolhida (isso pode variar muito por conta da possibilidade de utilizar mais de um tipo de base de dados).
* Limites técnicos de cada base de dados como por exemplo quantidade de acessos por unidade de tempo, tempo de resposta, entre outros.
* Variedade de bases de dados disponíveis no mercado.

Em resumo, é possível montar várias arquiteturas diferentes para dar suporte a um mesmo projeto. Para definir uma boa arquitetura é necessário mais tempo e uma análise mais detalhada. 

A imagem a seguir mostra algumas opções de bases de dados disponíveis no mercado, além de outras tecnologias.

![Imagem de Algumas Opções de Bases de Dados](https://github.com/tuliocpbs/odesafio/blob/master/imagens/datafloq.jpg)
