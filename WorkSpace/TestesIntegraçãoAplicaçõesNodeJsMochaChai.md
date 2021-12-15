# Testes de integração para aplicações Node.js com Mocha e Chai

[![Giuliana Bezerra](https://miro.medium.com/fit/c/43/43/0*oSao5jCNzJcSBYwv.jpg)](https://giuliana-bezerra.medium.com/?source=post_page-----610a1ba15e1b-----------------------------------) - [Giuliana Bezerra](https://giuliana-bezerra.medium.com/?source=post_page-----610a1ba15e1b-----------------------------------) -  [May 31, 2019](https://medium.com/desenvolvimento-com-node-js/testes-de-integração-para-aplicações-node-js-com-mocha-e-chai-610a1ba15e1b?source=post_page-----610a1ba15e1b-----------------------------------) · 5 min read



![img](https://miro.medium.com/max/313/1*TPgqiC-pFMY_ig-hBoMamw.png)

No ciclo de vida de uma aplicação existem diversas tarefas. Uma das mais importantes é o teste. Afinal, de que adianta programar um sistema se não é possível atestar o seu funcionamento? A melhor forma de checar se um sistema funciona é testando-o, preferencialmente de forma automatizada. Essa automação é possível através da implementação de **Testes de Unidade**. O problema é que [esse tipo de teste nem sempre é eficaz](https://www.toptal.com/nodejs/nodejs-guide-integration-tests), muitas vezes os erros ocorrem durante a integração do sistema com componentes externos. É aí que entram os [**Testes de Integração**](https://blog.caelum.com.br/unidade-integracao-ou-sistema-qual-teste-fazer/). Então, como podemos criar esses testes para aplicações Node.js? Para isso vamos utilizar dois módulos: [Mocha](https://mochajs.org/) e [Chai](https://www.chaijs.com/).

> 💡 Se você ainda acha que testes não são importantes, recomendo a leitura [desse artigo](https://blog.umbler.com/br/qualidade-de-software-1-7-motivos-para-considerar-o-teste-de-software-indispensavel/)*.*

# Pré requisitos

O leitor precisa estar familiarizado com a linguagem Javascript e o [Node.js](https://medium.com/@giu.drawer/começando-a-desenvolver-com-o-node-js-74b70af01a0d). Para seguir o tutorial será necessário baixar [esse projeto](https://github.com/giuliana-bezerra/servico-nodejs) e iniciá-lo com o comando *npm start*.

# Estrutura do Projeto

Vamos começar abrindo o [nosso projeto](https://github.com/giuliana-bezerra/servico-nodejs) no [Visual Studio Code](https://code.visualstudio.com/):

![img](https://miro.medium.com/max/27/1*gdOmHuKVYnIUKOgKWrSkjw.png?q=20)

![img](https://miro.medium.com/max/278/1*gdOmHuKVYnIUKOgKWrSkjw.png)

Iremos criar o diretório *test/integration* para adicionar os testes de integração:

![img](https://miro.medium.com/max/24/1*6cExlwV5fhIINX14NDc9dA.png?q=20)

![img](https://miro.medium.com/max/277/1*6cExlwV5fhIINX14NDc9dA.png)

Essa é a estrutura necessária para começarmos a implementar os testes de integração no nosso projeto Node.js. Simples, não é mesmo?

# Mocha como framework de testes

O Mocha é um framework de testes que fornece a estrutura necessária para rodar um teste automatizado. Então, como podemos utilizá-lo? Primeiro, precisamos adicioná-lo como dependência do projeto:

```
$ npm install mocha@6.1.4 --save-dev --save-exact
```

> 💡 O mocha é um módulo para testes utilizado apenas durante o desenvolvimento. Por isso, foi utilizada a opção *save-dev*. A opção save-exact é para garantir que a versão descrita nesse tutorial será a instalada na sua máquina.

Feito isso, precisamos implementar o teste de fato. Iremos assumir aqui que o escopo dos testes será o serviço de criação de usuário. Então, vamos criar um arquivo chamado *usuario.js* dentro do diretório *test/integration*:

![img](https://miro.medium.com/max/22/1*GqKgkiYVtNjS4LSkMndkeQ.png?q=20)

![img](https://miro.medium.com/max/274/1*GqKgkiYVtNjS4LSkMndkeQ.png)

Precisamos descrever quem será testado, e cada cenário de teste. Para isso, podemos utilizar o código abaixo como template:

<iframe src="https://medium.com/media/21735c3e6e8eebab87d5c4a3d8636d47" allowfullscreen="" frameborder="0" height="392" width="680" title="usuario.js" class="t u v jt aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 391.997px;"></iframe>

O *describe* descreve a API testada e cada um dos serviços. Já o *it* vai descrever cada cenário de teste do serviço descrito. Por exemplo, na nossa API nós definimos que existem os seguintes cenários de execução do serviço de criação de usuário:

1. O usuário é cadastrado com sucesso
2. O usuário já existe e por isso o sistema irá obtê-lo a partir da sua localização existente
3. Os dados de criação do usuário não foram passados corretamente ou são inválidos

O *it* recebe como argumento uma função com o parâmetro *done*. Esse objeto é uma função utilizada para informar ao Mocha quando o teste será concluído. Por isso, foi necessário chamar o *done()* dentro de cada cenário de teste*.*

Para finalizar a configuração do Mocha, vamos criar um arquivo *mocha.opts* dentro do diretório *test* para informar que existem arquivos de teste dentro de subdiretórios do *test*:

![img](https://miro.medium.com/max/27/1*N7ttJqyKb3GXTjc9zvZTNg.png?q=20)

![img](https://miro.medium.com/max/491/1*N7ttJqyKb3GXTjc9zvZTNg.png)

Será possível executar o teste criado com o comando abaixo (**lembre de iniciar o projeto com o npm start antes de rodar os testes!**):

![img](https://miro.medium.com/max/27/1*Q6gVj92gohjHiHlOtg2ZlQ.png?q=20)

![img](https://miro.medium.com/max/630/1*Q6gVj92gohjHiHlOtg2ZlQ.png)

Podemos adicionar esse comando no *package.json* e executar os testes apenas com o comando *npm test*:

![img](https://miro.medium.com/max/27/1*PFBXHNra7tXKiDc_j5S7jw.png?q=20)

![img](https://miro.medium.com/max/589/1*PFBXHNra7tXKiDc_j5S7jw.png)

Os testes passaram pois nenhuma lógica está sendo avaliada. Precisaremos fazer requisições à API e validar as respostas obtidas. Para isso, vamos utilizar o [Chai](https://www.chaijs.com/).

# Chai como biblioteca de asserção

O Chai é um módulo que possui diversas funções de asserção que permitem validar se os retornos do teste estão de acordo com o esperado. Para utilizá-lo no nosso projeto, vamos primeiro instalá-lo:

```
$ npm install chai@4.2.0 --save-dev --save-exact
```

Agora, vamos começar a implementar os cenários de teste utilizando o Chai. O primeiro cenário descreve uma situação em que o usuário é criado com sucesso. Isso significa que o serviço é chamado com todos os dados necessários para criar um usuário válido. Vamos então descrever esses dados válidos num arquivo [JSON](https://www.json.org/json-pt.html) chamado *data.json*, criado no diretório *test/integration*:

<iframe src="https://medium.com/media/0784913b330df9ffd3902ebf97e2bcba" allowfullscreen="" frameborder="0" height="106" width="680" title="data.json" class="t u v jt aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 105.99px;"></iframe>

Vamos importar esse arquivo no teste para que seja possível utilizar esse dado:

<iframe src="https://medium.com/media/987cbf2e5f5f9e4f7c3344a6e9f9b344" allowfullscreen="" frameborder="0" height="436" width="680" title="usuario.js" class="t u v jt aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 435.99px;"></iframe>

Para fazer uma chamada POST à API, vamos precisar de um cliente HTTP. O Chai também provê um cliente através do módulo [chai-http](https://www.chaijs.com/plugins/chai-http/):

```
$ npm install chai-http@4.3.0 --save-dev --save-exact
```

Agora vamos fazer a chamada à API utilizando esse módulo:

<iframe src="https://medium.com/media/f9f01f9be57af4b4100622cd1c1534f9" allowfullscreen="" frameborder="0" height="740" width="680" title="usuario.js" class="t u v jt aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 740px;"></iframe>

A primeira coisa que fizemos foi importar os módulos *chai* e *chai-http*. Para utilizar o *chai-http*, é necessário utilizar a função *use* do objeto *chai*. Também utilizamos a função *should* para ter acesso as operações desse estilo de assertiva. Feitas as importações, a chamada à API é bastante simples. A partir do objeto *chai* é feita uma *request* com o método *post* enviando como corpo da requisição um *USUARIO_VALIDO*. A resposta é tratada pela função *end*, que recebe como argumento um objeto de erro e a resposta. O que declaramos dentro dessa função são as assertivas de como esperamos que a nossa API funcione. O que dissemos no teste desse cenário foi:

1. Não deve haver erros
2. A resposta deve ter um corpo
3. O código retornado deve ser 201
4. O corpo deve ter uma propriedade *error* igual a *0*
5. O corpo da resposta deve estar no seguinte formato:

```
{"error":0,"payload":{"comments":{"nome":"Rachel Green","_id":"<Alguma coisa>"}}}
```

A implementação desse cenário foi concluída. Vamos executar os testes e verificar se estão passando:

![img](https://miro.medium.com/max/27/1*_lwdFZEhHiz_lZVstNmxqQ.png?q=20)

![img](https://miro.medium.com/max/607/1*_lwdFZEhHiz_lZVstNmxqQ.png)

Para visualizar o cenário em que o teste não passa, vamos modificar o serviço de criação do usuário (localizado em controllers/usuario/create.js) para retornar o *comments* vazio:

<iframe src="https://medium.com/media/0e60973cfd5694c06776ff9a062cf019" allowfullscreen="" frameborder="0" height="344" width="680" title="create.js" class="t u v jt aj" scrolling="auto" style="box-sizing: inherit; position: absolute; top: 0px; left: 0px; width: 680px; height: 343.993px;"></iframe>

Salve o arquivo e execute os testes novamente:

![img](https://miro.medium.com/max/27/1*A3eF5OfcudhmvTnMmyx8yw.png?q=20)

![img](https://miro.medium.com/max/614/1*A3eF5OfcudhmvTnMmyx8yw.png)

O log informa claramente que o teste 1 falhou pois era esperado que um objeto vazio tivesse a propriedade *_id*.

# Conclusão

Nesse post vimos como elaborar testes de integração para projetos Node.js utilizando os módulos Mocha e Chai. A documentação de ambos é excelente e pode servir de guia durante a implementação dos testes.

Como exercício, eu deixo a implementação dos outros dois cenários de teste que não foram cobertos. Afinal, a prática leva a perfeição ;)

# Referências

- [Documentação do Mocha](https://mochajs.org/)
- [Documentação do Chai](https://www.chaijs.com/)

[Desenvolvimento com Node.js](https://medium.com/desenvolvimento-com-node-js?source=post_sidebar--------------------------post_sidebar--------------)

Uma série de posts que exploram de forma prática o…



- [Nodejs](https://medium.com/desenvolvimento-com-node-js/tagged/nodejs)  -  [Integration Testing](https://medium.com/desenvolvimento-com-node-js/tagged/integration-testing)  - [Mocha](https://medium.com/desenvolvimento-com-node-js/tagged/mocha) - [Chai](https://medium.com/desenvolvimento-com-node-js/tagged/chai)



e exploram de forma prática o desenvolvimento de aplicações com o Node.js.