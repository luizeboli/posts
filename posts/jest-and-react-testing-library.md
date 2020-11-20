![Language badge](https://img.shields.io/badge/Language-PT--BR-blue)

> This is a post about testing in React with Jest + RTL. It's written in pt-br to accomplish the mentorship program.

# React: Os primeiros passos para testar seus componentes

Você enquanto desenvolvedor, provavelmente já ouviu em algum momento sobre a importância que os testes tem na nossa rotina, sobre seus benefícios, e talvez já tenha até escrito algumas linhas de teste. Falando especificamente sobre frontend, é vital para a saúde da sua aplicação que existam testes, principalmente pelo cenário da componentização: reuso, composição, programação funcional e etc...

Esse post é pra você, que não tem muita experiência ou nunca mexeu com implementação de testes. Mas, caso já esteja em um nível mais intermediário/avançado, a leitura também é válida para relembrar alguns conceitos. Sempre bom refrescar a memória, né? 😁

## 1. O que é um teste? 🐛

Antes da prática, que tal abordar alguns conceitos? 

De forma resumida, o teste nada mais é um que processo, que irá avaliar se um software, ou parte dele, cumpre seu papel.

Em outras palavras: se funciona como deveria. Simples assim, não é? Bom, nem tanto!

Existem vários tipos de teste, como:

1. Unitário
2. Integração
3. Regressão
4. End-to-end
5. Funcional

E talvez mais alguns... Porém, pra não te confundir, e não encher sua cabeça logo no começo com muitos termos e definições, vamos focar nessa imagem:

![Troféu de testes](https://pbs.twimg.com/media/DVUoM94VQAAzuws.jpg)
_Imagem retirada [deste tweet](https://twitter.com/kentcdodds/status/960723172591992832)._

Se nunca viu ou ouviu falar dessa imagem, trata-se do troféu de testes. Esse modelo foi criado pois com as ferramentas atuais, já não faz mais sentido seguir a pirâmide de testes originalmente criada pelo Martin Fowler (considerando frontend).

O tamanho de cada bloco representa a importância que esse tipo de teste possui, mas não existe regra ou certo e errado. Nesse caso, a relevância pode variar de acordo com os princípios do seu time. A ideia aqui é apenas extrair o máximo de valor com o menor custo possível de cada teste. São eles:

**1. Estático**

Previne erros de tipagem, formatação e garante um padrão de código pela aplicação. Ferramentas como TypeScript, ESLint, Prettier e Flow cumprem bem esse papel. É o teste mais barato em termos de ambiente utilizado e tempo investido para escrever e manter. Ao mesmo tempo é o que menos passa confiabilidade.

**2. Unitário**

Garante que partes individuais e isoladas funcionam como deveriam: métodos auxiliares, métodos de API, componentes de UI (como um botão), etc...

**3. Integração**

O teste de integração nada mais é do que validar se várias unidades funcionam adequadamente quando integradas (por isso integração 🥁).

Um bom exemplo seria testar um formulário e se certificar de que os valores são enviados corretamente para a API e, se após isso os campos são limpos.

**4. End-to-end**

Esse é o teste mais completo entre todos, o mais caro, porém o que passa mais confiabilidade. Aqui, cobrimos o fluxo completo de determinada parte da aplicação, desde o frontend até o backend, verificando por exemplo se o retorno da API está correto, se o usuário é redirecionado para a página correta e etc...

Se quiser se aprofundar mais sobre o troféu de testes, recomendo a leitura do artigo [Static vs Unit vs Integration vs E2E Testing for Frontend Apps](https://kentcdodds.com/blog/unit-vs-integration-vs-e2e-tests).

## 2. Por que testar? ☢️

O principal motivo é a **confiabilidade**. Em poucas palavras, testar uma aplicação nos passa a segurança de que mudanças no código não irão quebrar a funcionalidade. Alguns outros motivos:

* Não teremos que testar o código manualmente
* O teste também é uma forma de documentar o código
* Podemos refatorar grande parte do código sem nos preocupar se a regra de negócio será afetada
* Nos certificamos de que o código funciona adequadamente
* Menos preocupações com o processo de deployment
* Mais segurança em alterar funcionalidades que foram escritas por outro desenvolvedor

Um artigo muito interessante sobre os motivos que nos levam a escrever testes, e qual tipo aplicar é o [Write tests](https://kentcdodds.com/blog/write-tests).

## 3. O que devo testar? ⚠️

Essa é uma dúvida bem comum nas comunidades que participo, e aqui vou te dar um exemplo prático:

![Imgur](https://imgur.com/6GMKyzo.png)

Temos um bloco simples de código, um tipo de contador. Nele existe um estado, um efeito colateral controlado pelo `useEffect` e dois botões, um para somar e outro para subtrair.

#### **Como você testaria esse componente?** 🤔

Algumas pessoas podem pensar em fazer um `mock` das funções do React, pra verificar por exemplo se o `useState` foi chamado com o valor `0`. Ou talvez testar de alguma forma diretamente a função `handleClick`.

Se você pensou em qualquer coisa parecida com isso, vamos refletir um pouco. Imagine que em certo momento a implementação desse componente mude, o estado não é mais com o `useState`, não existe mais a função `handleClick`, e agora o que o `useEffect` faz passou pra dentro do próprio botão. A regra de negócio, a funcionalidade em si, pode continuar a mesma, porém o teste estará completemente quebrado, certo? Isso aconteceu pelo fato desse teste ter sido escrito baseado totalmente na implementação do componente.

Uma excelente maneira de não cometer esse erro é olhar pra tela, pra aplicação final, e imaginar o fluxo de interação que o usuário irá fazer. No mesmo exemplo acima, eu visualizo três principais interações:

1. O usuário pode clicar no botão para incrementar o contador
2. O usuário pode clicar no botão para subtrair o contador
3. O usuário pode visualizar o valor atual do contador.

A partir desses pontos, posso começar a escrita do teste de uma forma mais genérica, sem me preocupar se quando a implementação do componente mudar sem alterar sua funcionalidade, eu terei problemas.

Uma outra forma é experimentar a metodologia TDD, pois você escreverá o teste antes mesmo do código e assim não tem como ficar enviesado.

**Resumindo: não se prenda à implementação do código, escreva o teste seguindo o mesmo fluxo de interação do seu usuário final**

Recomendo a leitura de um artigo um pouco mais aprofundado nesse assunto: [How to know what to test](https://kentcdodds.com/blog/how-to-know-what-to-test). Inclusive este, tem uma tradução oficial: [Como saber o que testar?](https://segredo.dev/como-saber-o-que-testar/?utm_source=kentcdodds&utm_medium=translate&utm_campaign=howknowtest)

E também outro específico sobre testar implementação de componentes: [Testing implementation details](https://kentcdodds.com/blog/testing-implementation-details)

### 4. Testando com Jest e Testing Library

As ferramentas mais conhecidas e utilizadas para testar aplicações frontend atualmente são: [Jest](https://jestjs.io/) e [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/).

O Jest é um framework de testes JavaScript e a React Testing Library é uma biblioteca com métodos utilitários focados em boas práticas. O Jest será usado para executar os testes, obter a cobertura, snapshots e mocks, enquanto a RTL será responsável por expor os métodos e obter os elementos do DOM.

Existem duas principais vantagens em usar a RTL, a primeira delas é que os principais métodos para selecionar os elementos são baseados em acessibilidade, dessa forma podemos expandir o público alvo da aplicação. A segunda, é que esses mesmos métodos nos fazem testar o componente simulando totalmente o comportamento do usuário, o que nos traz segurança de que a aplicação funcionará adequadamente quando utilizada por um usuário real.

#### A prática leva à perfeição ✨

Vamos relembrar do nosso último exemplo e escrever alguns testes:

![Imgur](https://imgur.com/6GMKyzo.png)

A regra de negócio que esse componente deve atender é: 

* Possuir um botão para subtrair do contador
* Possuir um botão para incrementar o contador
* Exibir o valor do contador atual

Como vimos anteriormente, é muito melhor que o teste reproduza as interações de um usuário, e é ai que o Jest + RTL entram em ação.

#### O primeiro teste... 🎉

![Imgur](https://imgur.com/5L7Oe1G.png)

##### describe()

O método `describe` é utilizado para representar um bloco de testes, dessa maneira o Jest agrupa todos os testes contidos nele. 

##### it() ou test()

Já o método `it`, ou `test` _(os dois tem a mesma função, o `it` é apenas um alias para o `test`)_ é onde os testes serão escritos e devem ser nomeados com uma sentença descritiva não muito longa. Uma convenção é nomear o teste com o comportamento que o componente deverá ter, como: `it('should render without crashing')`.

Tudo o que um arquivo de testes precisa para funcionar é de um bloco `it` ou `test`. O `describe` não é obrigatório, embora seja muito útil para a organização da suíte de testes.

Nesse exemplo, o nosso primeiro teste se certifica de que o componente é renderizado sem causar um erro na aplicação. A renderização ocorre através do método `render` da RTL. Em seguida utilizamos o matcher `toMatchSnapshot` do Jest. Essa linha criará um arquivo de snapshot com toda a marcação e estilização do componente, e sempre que este sofrer alteração, o teste irá quebrar. Isso assegura que não houve alterações indevidas no componente, pois podemos verificar facilmente o que mudou na marcação.

##### expect()

O método `expect` é o que nos permite testar o valor de determinado objeto, ou componente. [Ele possui](https://jestjs.io/docs/en/expect) uma série de matchers que podem ser utilizados para verificar as condições necessárias.

#### Melhorando os expects...

![Imgur](https://imgur.com/DziZd6Q.png)


Continuando com a cobertura do nosso código, o segundo teste que faremos é para assegurar de que dois botões são renderizados na tela, que afinal é um dos requisitos dessa aplicação. Usamos o método [screen](https://testing-library.com/docs/dom-testing-library/api-queries#screen) da RTL para obter os botões, de acordo com o seu atributo acessível `name`. 

E aí você pergunta: **Não vimos que era ruim testar baseado na implementação?** Aí que tá! Quando falamos de implementação, nos referimos a event listeners, métodos internos, estados e efeitos colaterais, pois de fato para obter um elemento precisamos de alguma informação da marcação HTML. O ponto aqui é: quanto melhor estruturarmos nosso código, menos o teste estará acoplado a ele.

Geralmente quando definimos as interfaces, seja com o UX ou não, sabemos a label e textos dos elementos, e isso é uma coisa que dificilmente muda durante ciclo de vida da aplicação, inclusive é muito relevante que um teste quebre nesse cenário pois nem sempre essa mudança é devida e necessária. E ainda assim caso venha a acontecer, não será um grande problema, apenas precisaremos atualizar a string no teste desse componente.

#### O último teste...

![Imgur](https://imgur.com/Vu5ypVe.png)

Aqui uma figura nova, o [userEvent](https://github.com/testing-library/user-event): uma biblioteca mantida também pela RTL, que simula as ações da mesma forma que um usuário real. Nesse caso usaremos para simular um clique nos botões com o método `click`. Perceba que usamos um matcher novo do Jest, o `toBe`, que valida se determinado elemento possui o valor esperado.

Se você ainda tinha alguma dúvida sobre testar a implementação, espero que a partir daqui estejam todas sanadas 😅

Perceba que eu testei tudo o que essa aplicação deveria fazer funcionalmente, sem acessar estado, useEffect, event listeners e etc... Tudo o que precisamos, foi obter o elemento relativo ao valor do contador, e os botões.

### A estrada é longa... 🛣

Espero que com esse pequeno post, você saia daqui com pelo menos o mínimo necessário para começar a escrever seus primeiros testes. Ainda existe muito a se estudar, não podemos parar de nos atualizar nunca, nosso mundo é frenético e não podemos ficar pra trás.

Como uma última recomendação, o blog do Kent C Dodds possui uma sessão específica para testes: [Testing Garden](https://kentcdodds.com/testing/), que possui mais alguns artigos além dos que eu já recomendei aqui.

Caso tenha ficado alguma dúvida, queira dar um feedback, crítica ou trocar ideia, estou a disposição. Só me chamar 🤜🏻 🤛🏻

