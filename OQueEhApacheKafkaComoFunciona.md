![A imagem mostra um notebook com o logo do kafka. Mas você sabe o que é Apache Kafka? Leia o artigo para descobrir](https://luby.com.br/wp-content/uploads/2021/05/o-que-e-apache-kafka-1-770x400.png)

[INFRAESTRUTURA](https://luby.com.br/category/infraestrutura/)

# O que é Apache Kafka e como funciona?

[![img](https://secure.gravatar.com/avatar/a1761e84c9831726b11f2b4e1bb5b6fb?s=96&d=mm&r=g) José Thomaz](https://luby.com.br/author/jose-thomaz/)

 

 18 de maio de 2021

 

 841 views

 

 [0 Comment](https://luby.com.br/infraestrutura/o-que-e-apache-kafka/#comments)

Processar fluxos de dados de diversas fontes e movimentar grandes volumes de informações é uma tarefa bem complexa presente na rotina dos devs. O Apache Kafka é uma plataforma pode facilitar que vem facilitado muito o [desenvolvimento de software](https://luby.com.br/solucoes/desenvolvimento-de-produto-de-software/). Mas, afinal, você sabe **o que é o Apache Kafka**?

Neste artigo, vamos abordar tudo sobre essa ferramenta. Então vamos lá?

## O que é Apache Kafka?

O Apache Kafka é uma plataforma *open-source* para realizar transmissão de dados em um fluxo contínuo. Ou seja, é um sistema de mensageria de alto desempenho e em tempo real. Essa plataforma se popularizou muito nos últimos anos com a “ascensão dos microsserviços”. O Kafka se destaca por ser extremamente veloz, escalável e versátil.

Inicialmente, o Kafka era um sistema interno desenvolvido pelo [LinkedIn](https://www.linkedin.com/company/luby-software). Então, sua principal aplicação era no processamento em tempo real das mensagens, comentários e postagens na rede social.

### **Explicando alguns termos**

Nesse artigo, serão utilizados alguns termos estritamente técnicos e isso pode assustar quem tá começando na área ou pessoas que não tenham estudado a fundo sobre esse assunto. Portanto, listei aqui definições de alguns termos técnicos para facilitar a sua leitura.

- ***Broker:\*** em computação, um *brocke*r é o servidor de envio de mensagens. É ele que irá disparar e receber essas mensagens. O *broker* também possui os tópicos e as partições;

- **Tópicos**: um tópico é um identificador. Por meio desse identificador, será possível se conectar ao *broker* e disparar e receber as mensagens;

- **Microsserviços**: microsserviços são um modelo de sistema da informação. Esse modelo visa “quebrar” um sistema em vários sistemas menores, a fim de melhorar a performance e a disponibilidade;
- **Partição:** subdivisão de um tópico, a partição é um recurso para ajudar a balancear a carga;

- 

- **Cluster:** resumidamente, um *cluster* é um conjunto de *brokers,* nele serão contidos os servidores e a instância principal do Kafka.

## **Como o Kafka funciona?**

O Kafka foi escrito nas linguagens de programação [Java](https://luby.com.br/reactjs/componentes-funcionais-em-reactjs/) e Scala. Ele realiza as suas transmissões de maneira assíncrona e pode ser considerado como um sistema distribuído. Em sua estrutura, o Kafka possui *consumers* e *producers*. Para definir esses termos, basta traduzir: os *producers* são os produtores das mensagens (quem envia e distribui os dados); já os *consumers* são os consumidores das mensagens (eles se inscrevem em um tópico e ficam ouvindo as mensagem que chegarão).

![A imagem mostra o diagrama que representa visualmente o que é apache kafka](https://luby.com.br/wp-content/uploads/2021/05/o-que-e-apache-kafka.png)

A arquitetura do Kafka descrita acima permite a consistência do envio de mensagens. Assim, os *brokers* são projetados para suportarem quedas e falhas. Quando um *broker* do Kafka cai e fica fora do ar por algum motivo, um *broker* reserva assume as atividades para manter a entrega e alta disponibilidade, sem perder nenhuma informação. A mesma coisa acontece com os tópicos, que possuem múltiplas partições, que são réplicas da partição principal.

Ao inicializar uma nova instância do Kafka, é possível definir um número personalizado de *brokers* e partições nos tópicos. O mais comum é três de cada um, mas use conforme a necessidade da sua aplicação.

## **Vantagens do Kafka**

- Rápido e versátil;
- Utiliza o disco rígido ao invés da memória para armazenar os dados;
- Alta disponibilidade;
- Transmissões assíncronas;
- Sua funcionalidade de mensageria já possui filas integradas por padrão;
- Garantia de entrega das mensagens ao *consumer*. O Kafka armazena os dados no disco. Então, se ocorre algum problema de conexão, as mensagens não serão perdidas, pois estão escritas no disco rígido. Assim, ao restabelecer a conexão, o disco será lido e as informações serão enviadas.

## **Desvantagens do Kafka**

- As mensagens não são entregues de maneira ordenada;
- Exige uma conexão de internet forte. Isso porque pequenas oscilações podem afetar a comunicação. Portanto, em casos de conexão via satélite, é mais viável implementar o sistema de mensageria com outras tecnologias (como por exemplo o MQTT);
- Curva de aprendizado e complexidade nas configurações.

## **Quais empresas adotaram o Apache Kafka?**

Por causa de suas qualidades, ampla comunidade e documentação completa, o Kafka foi adotado por diversas empresas, inclusive as famosas *big techs.* Alguns exemplos de grandes empresas que utilizam o Apache Kafka são: Uber; Netflix; RedHat; Spotify; Twitter; LinkedIn; AirBnb; Nubank; e PayPal.

## **Conclusão**

Chegamos ao final deste artigo e podemos concluir que o Apache Kafka é uma ótima tecnologia, que está em ascensão e que pode ser utilizada em vários casos de uso (como [IOT](https://luby.com.br/tips/como-criar-um-bot-para-testes/), E-commerces, chats e em sistemas bancários). Portanto, vale a pena tirar um tempo para estudar essa ferramenta, pois ela pode ajudar muito nos seus futuros projetos, tornando-os mais escaláveis e blindados.

A comunidade do Kafka é muito ampla e sua documentação é completa. Ou seja, isso facilita muito no processo de aprendizado e nas dúvidas e *bugs* que eventualmente possam aparecer.