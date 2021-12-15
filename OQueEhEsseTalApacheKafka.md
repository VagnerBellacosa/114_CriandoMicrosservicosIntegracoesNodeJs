

![img](https://miro.medium.com/max/1400/1*vuYl6F8MmB5fMyBMXHToHQ.png)

# O que é esse tal de Apache Kafka?

[![Gabriel Queiroz](https://miro.medium.com/fit/c/56/56/1*E9VgOIN_kbTmfKDkzXcIZQ.png)](https://medium.com/@gabrielqueiroz?source=post_page-----a8f447cac028-----------------------------------) - [Gabriel Queiroz](https://medium.com/@gabrielqueiroz?source=post_page-----a8f447cac028-----------------------------------) - [Feb 20, 2017·5 min read](https://medium.com/@gabrielqueiroz/o-que-é-esse-tal-de-apache-kafka-a8f447cac028?source=post_page-----a8f447cac028-----------------------------------)



Quando ouvi falar sobre Apache Kafka pela primeira vez, logo pensei “O que é esse tal de Apache Kafka e para o que posso usar então?”. Pesquisando no [site oficial do Apache Kafka](https://kafka.apache.org/) encontramos a seguinte definição:

> “Apache Kafka é uma plataforma distribuída de mensagens e streaming”.

Ta… Legal, mas a pergunta continua. O que é Apache Kafka? Para entender um pouquinho melhor sobre esse universo do Apache Kafka, podemos começar entendendo o fluxo básico do Kafka, que pode ser resumido em três ações bem simples:

- Você **produz** uma mensagem.
- Essa mensagem é **anexada** em um tópico.
- Você então **consome** essa mensagem.

![img](https://miro.medium.com/max/60/1*q2jYvDNJMS72HgWsOG1f8g.png?q=20)

![img](https://miro.medium.com/max/630/1*q2jYvDNJMS72HgWsOG1f8g.png)

[Matt Kirwan. Outubro, 2016.](http://www.mattkirwan.com/apache/kafka/2016/10/17/apache-kafka-series-1-an-introduction.html)

## Para o que posso usar o Kafka então?

Vendo o fluxo básico do Kafka você deve estar se perguntando, “mas para que isso pode ser útil?”. Entre suas diversas finalidades, Matt Kirwan diz que:

> “Se você quer mover e transformar um grande volume de dados em tempo real entre diferentes sistemas, então Apache Kafka pode ser exatamente o que você precisa”.

Desde leitura e escrita direta entre diversos bancos e mecanismos de pesquisas (SQL, MongoDB, RethinkDB, ElasticSearch) até materialização de dados on streaming com KTable por exemplo, é possível utilizar o Kafka de diversas maneiras para mover e transformar grande volume de dados.

O [site oficial do Apache Kafka](https://kafka.apache.org/uses) também disponibiliza uma lista com alguns casos de uso do Kafka.

## Conceitos

Bom agora que sabemos um pouquinho mais sobre Apache Kafka e para o que podemos usa-lo podemos nos aprofundar um pouco mais nos conceitos por trás do fluxo básico do Kafka.

- **Mensagens**

Mensagem é o principal recurso do Kafka. Todos os eventos do Kafka podem ser resumidos em mensagens, sendo consumidas e produzidas através de tópicos. Uma mensagem pode ser desde uma simples String com “Hello World!” ou até mesmo um JSON contendo um objeto do seu domínio.

O Kafka permite definir Schemas para mensagens, como por exemplo utilizando o Avro. Como num exemplo de um JSON contendo um objeto do seu domínio, o Schema pode auxiliar impedindo que mensagens contendo conteúdos inválidos sejam trafegadas no tópico.

Uma mensagem pode também ser composta por uma chave (key/value), que é utilizada para sharding e compactação dentro do Kafka. Assim em um ambiente distribuído, é garantido a ordem das mensagens uma vez que mensagens com a mesma chaves são direcionadas para uma única partição do Kafka.

- **Tópicos**

Um tópico é como categorizamos grupos de mensagens dentro do Kafka. Todas as mensagens enviadas para o Kafka permanecem em um tópico. Como comentado sobre Event Sourcing, mensagens são imutáveis e ordenadas.

Para manter a ordenação em um ecossistema de Kafka, os tópicos possuem partições e fatores de replicação. Um tópico pode possuir n partições, mas ao receber uma nova mensagem o Kafka automaticamente direciona aquela mensagem para uma partição específica dependendo de sua chave (key). Assim mensagens de uma mesma chave estarão apenas em uma única partição, garantindo assim a leitura ordenada de todas as mensagens de um tópico.

- **Producer**

Um Kafka Producer é responsável por enviar uma mensagem para um tópico específico. De forma simples, você pode produzir uma mensagem em um tópico.

Uma vez que uma mensagem é produzida em um tópico o próprio Kafka organiza a mensagem em uma partição, garantindo sempre a ordem das mensagens produzidas, como citado anteriormente.

- **Consumer**

Temos os tópicos, e as mensagens dentro dos tópicos. Com o Kafka Consumer é possível ler essas mensagens de volta. Importante entender que, ao ler uma mensagem com o consumer, a mensagem não é retirada do tópico.

Você pode ter vários Kafka Consumers conectados em um mesmo tópico, e cada um terá a posição onde parou de ler. Assim você pode ter um tópico produzindo como no exemplo acima, pontuações de um jogador, e ter diversos consumers em pontos diferentes do tópico realizando ações diferentes. Você também pode escolher ter vários consumers lendo o mesmo tópico e na mesma partição, para escalar sua aplicação por exemplo, neste caso estes consumers fariam parte de um Consumer Group, e compartilharão sempre a posição final de leitura entre eles (offsets).

- **Apache Zookeeper**

O Zookeeper é um serviço centralizado para, entre outras coisas, coordenação de sistemas distribuídos. O Kafka é um sistema distribuído, e consequentemente delega diversas funções de gerenciamento e coordenação para o Zookeeper.

Eles possuem uma dependência muito forte, mas isso não é tão ruim. O Kafka pode fazer o que ele intencionalmente tem que saber fazer de melhor, delegando essas demais funcionalidades para quem sabe fazer isso bem, sem precisar reinventar a roda.

- **Kafka Brokers | Kafka Clusters**

![img](https://miro.medium.com/max/60/1*KeK4Ftz3jtRr3pEE1V3oMw.png?q=20)

![img](https://miro.medium.com/max/630/1*KeK4Ftz3jtRr3pEE1V3oMw.png)

O Broker é o coração do ecossistema do Kafka. Um Kafka Broker é executado em uma única instância em sua máquina. Um conjunto de Brokers entre diversas máquinas formam um Kafka Cluster.

Uma das principais características do Kafka é a escalabilidade e resiliência que ele oferece. Você pode rodar o Kafka local na sua máquina onde sua própria máquina teria um Kafka Broker formando um Kafka Cluster, como pode subir n instâncias de Kafka Brokers e todas estarem no mesmo Kafka Cluster. Com isso é possível escalar sua aplicação, e replicar os dados entre os Brokers.

# Streams for life?

Gostou do Kafka ou quer entender mais sobre o assunto? Junte-se ao grupo no [Slack da Confluent](https://slackpass.io/confluentcommunity) (Uma das principais mantenedoras do Apache Kafka) e ajude a desenvolver novas soluções!

# Referências

[Event SourcingThis is part of the Further Enterprise Application Architecture development writing that I was doing in the mid 2000's…martinfowler.com](https://martinfowler.com/eaaDev/EventSourcing.html)

[Stream processing, Event sourcing, Reactive, CEP... and making sense of it all - ConfluentSome people call it stream processing. Others call it Event Sourcing or CQRS. Some even call it Complex Event…www.confluent.io](https://www.confluent.io/blog/making-sense-of-stream-processing/)

[Apache Kafka - An IntroductionIt's a common occurrence in our industry to have heard of a technology, glean a vague idea of "what it is" but never…www.mattkirwan.com](http://www.mattkirwan.com/apache/kafka/2016/10/17/apache-kafka-series-1-an-introduction.html)

[Quickstart - Confluent Platform 3.0.1 documentationIn this section, we provide a simple guide for running a Kafka cluster along with all of the other Confluent Platform…docs.confluent.io](http://docs.confluent.io/3.0.1/cp-docker-images/docs/quickstart.html)

[Gabriel Queiroz](https://medium.com/@gabrielqueiroz?source=post_sidebar--------------------------post_sidebar--------------)

https://policy.medium.com/medium-terms-of-service-9db0094a1e0f?source=post_page-----a8f447cac028-----------------------------------)