# Como fazer a integração de dados com Apache Kafka

- outubro 1, 2020 

**É para fazer a transferência dos dados entre as aplicações que usamos um sistema de mensagens como o Apache Kafka**.

Podemos justificar essa necessidade já que o mundo tem gerado um volume cada vez maior de informações. Um cenário comum é que estes dados, uma vez gerados, precisam ser transportados para diversas aplicações.

Por exemplo, quando fazemos uma compra com cartão de crédito a operadora precisa avisar várias aplicações diferentes para contabilizar a compra, os benefícios e até a análise contra fraudes.

Neste caso, é desejável que cada aplicação seja responsável pela implementação das regras do negócio. E apenas isso. Uma aplicação não deveria se preocupar com a entrega ou captura dos dados.

Este é um problema complexo envolvendo **data streaming**.

> 10 [skills de um desenvolvedor back-end](https://blog.geekhunter.com.br/desenvolvedor-back-end/) de alto nível

**Conteúdo** [ocultar](https://blog.geekhunter.com.br/apache-kafka/#) 

[1 O que é Data streaming](https://blog.geekhunter.com.br/apache-kafka/#O_que_e_Data_streaming)

[2 Sistema de Mensagem](https://blog.geekhunter.com.br/apache-kafka/#Sistema_de_Mensagem)

[3 O que é Apache Kafka](https://blog.geekhunter.com.br/apache-kafka/#O_que_e_Apache_Kafka)

[4 Características do Apache Kafka](https://blog.geekhunter.com.br/apache-kafka/#Caracteristicas_do_Apache_Kafka)

[5 Ferramentas similares ao Apache Kafka](https://blog.geekhunter.com.br/apache-kafka/#Ferramentas_similares_ao_Apache_Kafka)

[6 Arquitetura do Apache Kafka](https://blog.geekhunter.com.br/apache-kafka/#Arquitetura_do_Apache_Kafka)

[6.1 Acesso sequencial no disco](https://blog.geekhunter.com.br/apache-kafka/#Acesso_sequencial_no_disco)

[7 Caso de Uso do Apache Kafka](https://blog.geekhunter.com.br/apache-kafka/#Caso_de_Uso_do_Apache_Kafka)

[7.1 TOPICO_NOVA_VENDA](https://blog.geekhunter.com.br/apache-kafka/#TOPICO_NOVA_VENDA)

[7.2 TOPICO_NOVO_USUARIO](https://blog.geekhunter.com.br/apache-kafka/#TOPICO_NOVO_USUARIO)

[7.3 TOPICO_RECLAMACAO](https://blog.geekhunter.com.br/apache-kafka/#TOPICO_RECLAMACAO)

[7.4 TOPICO_DEVOLUCAO](https://blog.geekhunter.com.br/apache-kafka/#TOPICO_DEVOLUCAO)

[7.5 TOPICO_NOTIFICA_USUARIO](https://blog.geekhunter.com.br/apache-kafka/#TOPICO_NOTIFICA_USUARIO)

[8 Ferramentas complementares](https://blog.geekhunter.com.br/apache-kafka/#Ferramentas_complementares)

[9 Apache Kafka e microservices](https://blog.geekhunter.com.br/apache-kafka/#Apache_Kafka_e_microservices)

[10 Conclusão sobre o Apache Kafka](https://blog.geekhunter.com.br/apache-kafka/#Conclusao_sobre_o_Apache_Kafka)

## O que é Data streaming

Um fluxo de dados constante e sem controle é chamada de *data streaming*.

Sua principal característica é que os dados não tem limites definidos, assim, não é possível dizer quando começa ou acaba o fluxo (*stream*).

Os dados são processados à medida em que chegam no destino. Alguns autores chamam de processamento em tempo real, ou on-line.

Uma abordagem diferente é o processamento em bloco, *batch*, ou *off-line*, na qual blocos de dados são processados em janelas de tempo de horas ou dias.

Muitas vezes o *batch* é um processo que roda a noite, consolidando os dados daquele dia. Há casos de janelas de tempo de uma semana ou mesmo de um mês, gerando relatórios desatualizados.

## Sistema de Mensagem

O sistema de mensagens permite desacoplar a geração dos dados do seu processamento de forma assíncrona, assim, uma aplicação pode continuar funcionando sem esperar a outra terminar seu processamento.

Outra vantagem é que essa técnica tem o potencial de diminuir a necessidade de leitura, por exemplo, no banco de dados.

Sistemas de mensagem podem ser usados em qualquer área. Viagem, finanças, logística, mapas, transporte público e varejo.

Neste artigo veremos o que é o **Apache Kafka** e como podemos nos beneficiar com um sistema de mensagens para criar soluções de *streaming*.

> 10 [livros de Python](https://blog.geekhunter.com.br/10-livros-de-python-que-todo-dev-especialista-deve-ler/) que todo dev especialista deve ler

## O que é Apache Kafka

O Apache Kafka é um sistema de mensagens usado para criar aplicações de *streaming*, ou seja, aquelas com fluxo de dados contínuo.

Criado originalmente pelo LinkedIn, o Kafka é baseado em logs, algumas vez chamado *de write-ahead logs*, *commit logs* ou até mesmo *transaction logs*.

Um log é uma forma básica de armazenamento, porque cada nova informação é adicionada no final do arquivo. Este é o princípio de funcionamento do Kafka. Este assunto está muito bem detalhado no [artigo](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying) do próprio criador.

O Kafka é adequado para soluções com grande volume de dados (*big data*) porque uma das suas características é a alta taxa de transferência.

O Apache Kafka tem 3 funcionalidades:

- Sistema de mensagem do tipo *publish/subscribe*;
- Sistema de armazenamento: as mensagens ficam armazenadas por um período de tempo pré-definido. Por padrão, as mensagens duram 7 dias, mas podem até mesmo ficar indefinidamente;
- Processamento de *stream*: é possível transformar a mensagem imediatamente após o seu recebimento.

Nas próximas seções veremos a arquitetura do Kafka, seus principais componentes e os casos de uso mais comuns.

Basicamente, o Kafka é um intermediário que coleta os dados da fonte e entrega para uma aplicação que consumirá esses dados, como visto na imagem:

![apache kafka coleta](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20541%20350'%3E%3C/svg%3E)Figura 1: Visão geral da arquitetura

Um uso mais recente do Kafka é o processo de ETL (*Extract, Transform and Load*), que copia os registros de um banco de dados para outro, geralmente de uma base transacional (OLTP) para uma base analítica (OLAP).

Alguns autores acreditam que o [futuro do ETL passa pelo Kafka](https://www.infoq.com/articles/batch-etl-streams-kafka), porque as ferramentas de ETL tendem a ser complexas, e o Kafka pode ajudar a diminuir a dificuldade.

Com o Apache Kafka é possível até mesmo transformar um processamento *batch* em tempo real, como será mostrado no fim deste texto.

O ponto central do sistema de mensagem é, naturalmente, a mensagem, que pode ser chamada também de registro ou evento, e é composta por:

- **Nome do tópico:** fila na qual a mensagem será gravada. Pode ser comparado a uma tabela do banco de dados;
- **Partição:** subdivisão do tópico, a partição é um recurso para ajudar a balancear a carga;
- **Timestamp:** os registros são ordenados pela hora de gravação;
- **Chave:** opcional, pode ser usada em cenários avançados;
- **Valor:** a informação que se pretende transferir. O ideal é que os dados usem um formato conhecido, como JSON ou XML.

## Características do Apache Kafka

Os sistemas de mensagem podem ser de dois tipos: point-to-point e publish-subscribe.

O Apache Kafka é do segundo tipo, no qual uma aplicação que publica (*publish*) e uma aplicação que se inscreve (*subscribe*) para receber as mensagens.

O tempo entre publicar e receber a mensagem pode ser de milissegundos, assim, uma solução Kafka tem baixa latência.

A arquitetura do Apache Kafka, vista com mais detalhe na seção a seguir, combina diversas outras características, como:

- **Escalabilidade:** o cluster pode ser facilmente redimensionado para atender ao aumento ou diminuição das cargas de trabalho;
- **Distribuído:** o cluster Kafka opera com vários nós, dividindo o processamento;
- **Replicado, particionado e ordenado:** as mensagens são replicadas em partições nos nós do cluster na ordem em que chegam para garantir segurança e velocidade de entrega;
- **Alta disponibilidade:** o cluster tem diversos nós (*brokers*) e várias cópias dos dados, assim, .

## Ferramentas similares ao Apache Kafka

Há outras ferramentas similares ao Kafka, são exemplos:

- [ActiveMQ](https://activemq.apache.org/);
- [RabbitMQ](https://www.rabbitmq.com/).

Eles têm diferenças e semelhanças e cada um tem sido usado em cenários distintos, com suas vantagens e desvantagens.

## Arquitetura do Apache Kafka

O Apache Kafka tem sido usado por empresas como Netflix, Spotify, Uber, LinkedIn e Twitter. E sua arquitetura é composta por *producers*, *consumers* e o próprio *cluster*.

O *producer* é qualquer aplicação que publica mensagens no cluster. O *consumer* é qualquer aplicação que recebe as mensagens do Kafka. O cluster Kafka é um conjunto de nós que funcionam como uma instância única do serviço de mensagem.

Um cluster Kafka é composto por vários *brokers*. Um *broker* é um servidor Kafka que recebe mensagens dos *producers* e as grava no disco. Cada *broker* gerencia uma lista de tópicos e cada tópico é dividido em diversas partições.

Depois de receber as mensagens, o broker as envia para os consumidores que estão registrados para cada tópico. Veja a imagem:

![apache kafka arquitetura](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20783%20235'%3E%3C/svg%3E)Figura 2: Visão geral dos produtores, *broker* e consumidores.

As configurações do Apache Kafka são gerenciadas pelo [Apache Zookeeper](https://zookeeper.apache.org/), que armazena os metadados do cluster, como localização das partições, lista de nomes, lista de tópicos e nós disponíveis. Assim, o Zookeeper mantém a sincronização entre os diversos elementos do cluster.

Isso é importante porque o Kafka é um sistema distribuído, ou seja, as gravações e leituras são feitas por diversos clientes simultaneamente. Quando há uma falha, o Zookeeper elege um substituto e recupera a operação.

### Acesso sequencial no disco

Uma curiosidade é que o Apache Kafka persiste a mensagem no disco e não na memória, como se imagina ser mais rápido.

De fato, o acesso à memória é mais rápido na maioria das situações, principalmente quando consideramos acesso a dados que estão em posições aleatórias na memória.

Contudo, **o Kafka faz acesso sequencial** e, neste caso, o disco é mais eficiente. Há outras ferramenta de big data que trabalham da mesma forma, como por exemplo o [HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html).

No acesso sequencial o disco sabe onde começa e termina o bloco de dados, enquanto que no acesso aleatório o disco precisa procurar e se mover para a posição dos blocos de dados.

É como se os dados estivessem ordenados. Pensando assim, **o Kafka lê os dados como se fosse um livro, da primeira para a última página**.

Quando temos leitura/gravação aleatória seria como ler o mesmo livro, mas com as páginas misturadas. Antes de ler, você teria de procurar a próxima página.

#### Exemplo de como funciona o acesso

![apache kafka producer](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20587%20441'%3E%3C/svg%3E)Apache Kafka producer

##### Producer

Um *producer* envia mensagens simultaneamente para vários tópicos. Da mesma forma, um *consumer* lê mensagens de vários tópicos simultaneamente.

Isso garante a baixa latência citada anteriormente, porque tudo acontece ao mesmo tempo. Aqui, o Zookeeper é o responsável por indicar os *brokers* para cada *producer/consumer*.

##### Consumer

Um *consumer* se registra para receber as mensagens de pelo menos um tópico. A partir daí, as novas mensagens daquele tópico são recebidas automaticamente pelo *consumer*.

Para aumentar a velocidade de leitura das mensagens, é possível registrar um conjunto de *consumers*, aumentando o paralelismo.

O *consumer* mantém o controle da última mensagem lida por meio de uma propriedade chamada *offset*. Assim, é possível reprocessar um fluxo de dados, se ele ainda estiver disponível. Esse controle é flexível, mas ao mesmo tempo traz um certo nível de complexidade. 

A configuração padrão do Apache Kafka tem ótima performance, mesmo com hardware limitado.

Ainda assim é necessário otimizar o cluster quando temos grandes cargas de dados. Para escalar usamos várias estratégias, geralmente testando as combinações de configuração, por exemplo, [alterando o número de produtores, consumidores e tópicos](https://www.infoq.com/articles/building-scalable-iot-application-with-apache-kafka/).

[![img](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/04/24190734/CTA-Emprego-dos-sonhos-5-1024x209-1.jpg)](https://bit.ly/36gFXXY)

## Caso de Uso do Apache Kafka

![apache kafka apps](https://blog-geek-midia.s3.amazonaws.com/wp-content/uploads/2020/09/30225549/apache-kafka-dados-1024x457.png)Possibilidades com Apache Kafka

O caso de uso selecionado para mostrar o funcionamento de uma solução Kafka será a integração entre um sistema de vendas, o log de navegação do usuário no site, o *data lake*, o *analytics*, o BI (*Business Intelligence*) e a ferramenta de visualização de dados.

A ideia é explorar várias possibilidades, mesmo que nem todas sejam usadas na sua realidade. Entre cada um dos sistemas está o cluster do Apache Kafka, que funciona como o integrador dos dados em tempo real.

Vamos criar diversos tópicos para alimentar cada uma das fontes de dados da solução, registrando desde a navegação no site até a conclusão da venda. Os tópicos do Kafka para atender a esta demanda são:

- **TOPICO_NAVEGACAO_USUARIO:** o servidor web notifica cada nova navegação do usuário;
- **TOPICO_NOVA_VENDA:** o e-commerce notifica cada nova venda;
- **TOPICO_NOVO_USUARIO:** o e-commerce notifica cada novo usuário;
- **TOPICO_RECLAMACAO:** o e-commerce notifica cada nova reclamação;
- **TOPICO_DEVOLUCAO:** o e-commerce notifica cada nova devolução de produto;
- **TOPICO_NOTIFICA_USUARIO:** o *analytics* notifica o usuário;

Os logs de navegação no site serão enviados para TOPICO_NAVEGACAO_USUARIO e serão posteriormente usados no *analytics* para entender o comportamento dos usuários, por exemplo, o que eles estão procurando.

O analytics pode ser o [Apache Spark](https://spark.apache.org/) ou famoso [Apache Hadoop](https://hadoop.apache.org/).

Estes logs são armazenados em um sistema de arquivos, como o [Amazon S3](https://aws.amazon.com/s3) ou o HDFS. Os registros serão posteriormente processados com ferramentas de análise de dados ou *machine learning*. Existem muitas ferramentas para captura de log com conectores Kafka, como o [Apache Flume](https://flume.apache.org/).

### TOPICO_NOVA_VENDA

O TOPICO_NOVA_VENDA receberá uma mensagem cada vez que uma vez for realizada. Com o suporte do Kafka Streams esta mensagem será transformada em um registro de banco de dados para ser gravado no BI. Isso garante atualização em tempo real do BI.

Além do BI, pode-se gravar os registros consolidados de vendas na a ferramenta de visualização de dados utiliza uma versão consolidada deste conjunto de dados, como o [Tableau](https://www.tableau.com/) ou [Qlik](https://www.qlik.com/us). Para coletar os dados diretamente do banco podemos usar um CDC (visto na próxima seção), ou alterar a aplicação para enviar a mensagem quando gravar a venda.

### TOPICO_NOVO_USUARIO

O TOPICO_NOVO_USUARIO receberá uma mensagem para cada novo usuário cadastrado. Essa informação será usada futuramente pelo *analytics*, por exemplo, para receber ofertas personalizadas a partir do seu histórico de navegação.

### TOPICO_RECLAMACAO

O TOPICO_RECLAMACAO receberá uma mensagem para cada nova reclamação. Essa informação será usada também pelo *analytics*, para minimizar o *customer churn*, que é o percentual de clientes que param de usar seus produtos. É sempre bom saber o motivo de insatisfação dos clientes.

### TOPICO_DEVOLUCAO

O TOPICO_DEVOLUCAO receberá uma mensagem para cada devolução de produto, uma informação importante para avaliar a qualidade dos produtos e a satisfação do cliente.

### TOPICO_NOTIFICA_USUARIO

O TOPICO_NOTIFICA_USUARIO receberá uma mensagem para cada notificação a ser enviada para os clientes. Essa notificação é criada pelo *analytics*, no módulo de *machine learning* que identifica as ofertas mais adequadas àquele perfil.

Opcionalmente, todos os tópicos podem gravar os dados no S3, criando assim um *data lake*. Esta é uma solução mais complexa que envolve sistemas de *big data* e *machine learning*. Algumas empresas optam por essa abordagem de gravar os dados originais para análise futura.

## Ferramentas complementares

O Apache Kafka é responsável pela transferência dos dados entre aplicações, mas não é suficiente para construir uma solução completa. Por isso, são necessárias outras ferramentas para as tarefas que o Kafka não faz, como o CDC.

O CDC (*Change Data Capture*), como o nome sugere, é uma categoria de software que captura as mudanças em um banco de dados e pode ser usado em conjunto com o Kafka para entrega de dados em tempo real, como uma alternativa ao *batch*.

O CDC monitora as alterações no banco, que são entregues ao Kafka continuamente, o que minimiza o impacto na rede e na performance do banco. Outras ferramentas que valem menção:

- [Kafka Streams](https://kafka.apache.org/documentation/streams/): biblioteca do Kafka que pode ser usada no lugar da API de *producer/consumer* para processamento das mensagens em tempo real;
- [KSQL](https://www.confluent.io/product/ksql/): mecanismo de SQL para o Kafka. Permite fazer consultas no stream de dados;
- [Kafka Connect](https://docs.confluent.io/current/connect/index.html): componente do Kafka para conexão com sistemas externos como bancos de dados (SQL ou NoSQL) ou sistema de arquivo;
- [Spark Streaming](https://spark.apache.org/docs/latest/streaming-programming-guide.html): extensão do Apache Spark para processamento de *streaming*;
- Apache Flume: serviço de coleta e agregação de dados;
- [Spring for Apache Kafka](https://spring.io/projects/spring-kafka): permite o desenvolvimento de aplicações Kafka com as facilidades do Spring;
- [Debezium](https://debezium.io/): plataforma de CDC escalável, moderna e distribuída com suporte a diversos bancos de dados.

## Apache Kafka e microservices

Os microservices tem usado cada vez mais o Apache Kafka. Tradicionalmente, a comunicação entre microservices é feita pela própria API REST.

Entretanto, à medida em que a quantidade de microservices aumenta, aumenta a complexidade para integração. Novamente, um bom cenário para o Kafka pelo motivos já vistos acima:

- **Desacoplamento entre o produtor e o consumidor da mensagem:** isso aumenta até a velocidade de desenvolvimento;
- **Alta disponibilidade:** mesmo quando um microservice fica indisponível, o sistema continua funcionando;
- **Baixa latência:** a transferência é imediata.

O projeto de novas arquiteturas costuma levar em consideração o uso de sistemas de mensagens, especialmente com o envio de mensagens assíncronas, centralizando a integração de dados no Apache Kafka.

Um sistema de mensagem é recomendado inclusive quando temos a migração de uma aplicação legada (monólito) para microservices.

## Conclusão sobre o Apache Kafka

O Apache Kafka é uma plataforma de streaming distribuída, que pode ser usada como um sistema de mensagem no estilo *publish/subscribe*.

Tem sido usado principalmente para integração de sistemas em tempo real e, em função de sua flexibilidade, pode ser usado para outras tarefas como ETL.

Os dados são enviados na forma de mensagens da aplicação de origem para a aplicação de destino.

O Apache Kafka centraliza o recebimento e a entrega das mensagens ao consumidor final, garantindo uma alta taxa de transferência e confiabilidade. Por isso tem sido usado para integração de sistemas com grande volume de dados.

Existem diversas ferramentas integradas ao Kafka com funcionalidades complementares, como o Kafka Streams, Spark, Flume e Debezium. Com estas ferramentas é possível criar sistemas em tempo real, ou até mesmo transformar processamento *batch* em tempo real.

Com todas estas vantagens, o Apache Kafka desponta como uma ferramenta importante nesta nova era da análise de dados para ajudar a resolver o problema da complexidade da transferência de dados entre aplicações.