---
title: Tecnologias da Web comuns do lado do cliente
description: Arquitetar aplicativos Web modernos com o ASP.NET Core e o Azure | Tecnologias da Web comuns no lado do cliente
author: ardalis
ms.author: wiwagn
no-loc:
- Blazor
ms.date: 12/04/2019
ms.openlocfilehash: e8ea035c491fad39d2932572255a19c7c1493418
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174348"
---
# <a name="common-client-side-web-technologies"></a>Tecnologias da Web comuns do lado do cliente

> "Os sites devem ter uma bela aparência em todo lugar."  
> _- Paul Cookson_

Os aplicativos ASP.NET Core são aplicativos Web e normalmente se baseiam em tecnologias da Web do lado do cliente como HTML, CSS e JavaScript. Ao separar o conteúdo da página (o HTML) de seu layout e estilo (o CSS) e seu comportamento (por meio do JavaScript), os aplicativos Web complexos podem aproveitar o princípio da Separação de Interesses. As alterações futuras na estrutura, no design ou no comportamento do aplicativo podem ser feitas com mais facilidade quando esses interesses não são entrelaçados.

Embora o HTML e o CSS sejam relativamente estáveis, o JavaScript, por meio das estruturas do aplicativo e dos utilitários com os quais os desenvolvedores trabalham para criar aplicativos baseados na Web, está evoluindo em uma velocidade vertiginosa. Este capítulo analisa algumas maneiras em que o JavaScript é usado por desenvolvedores da Web e fornece uma visão geral de alto nível das bibliotecas angulares e reajam do lado do cliente.

> [!NOTE]
> Blazorfornece uma alternativa para estruturas JavaScript para criar interfaces de usuário de cliente avançadas e interativas. O suporte do lado do cliente Blazor ainda está em versão prévia, portanto, por enquanto, está fora do escopo deste capítulo.

## <a name="html"></a>HTML

HTML é a linguagem de marcação padrão usada para criar páginas da Web e aplicativos Web. Seus elementos formam os blocos de construção de páginas, representando o texto formatado, imagens, entradas de formulário e outras estruturas. Quando um navegador faz uma solicitação para uma URL, independentemente se ele está buscando uma página ou um aplicativo, a primeira coisa retornada é um documento HTML. Esse documento HTML pode referenciar ou incluir informações adicionais sobre sua aparência e o layout na forma de CSS ou sobre seu comportamento na forma de JavaScript.

## <a name="css"></a>CSS

O CSS (folhas de estilos em cascata) é usado para controlar a aparência e o layout de elementos HTML. Os estilos CSS podem ser aplicados diretamente a um elemento HTML, definidos separadamente na mesma página ou definidos em um arquivo separado e referenciados pela página. Os estilos são aplicados em cascata de acordo com a forma como são usados para selecionar determinado elemento HTML. Por exemplo, um estilo pode se aplicar a um documento inteiro, mas ser substituído por um estilo aplicado a um elemento específico. Da mesma forma, um estilo específico de elemento seria substituído por um estilo aplicado a uma classe CSS que foi aplicada ao elemento, que, por sua vez, seria substituído por um estilo direcionado a uma instância específica desse elemento (por meio de sua ID). Figura 6-1

![Regras de especificidade de CSS](./media/image6-1.png)

**Figura 6-1.** Regras de especificidade do CSS, na ordem.

É melhor manter os estilos em seus próprios arquivos de folha de estilos separados e usar a cascata baseada em seleção para implementar estilos consistentes e reutilizáveis dentro do aplicativo. A colocação das regras de estilo em HTML deve ser evitada e a aplicação de estilos a elementos individuais específicos (em vez de classes inteiras de elementos ou elementos que tiveram determinada classe CSS aplicada a eles) deve ser uma exceção, não a regra.

### <a name="css-preprocessors"></a>Pré-processadores de CSS

As folhas de estilos CSS não têm suporte para lógica condicional, variáveis e outros recursos de linguagem de programação. Assim, as folhas de estilo grandes geralmente incluem um pouco de repetição, pois a mesma cor, fonte ou outra configuração é aplicada a muitas variações diferentes de elementos HTML e classes CSS. Os pré-processadores do CSS podem ajudar as folhas de estilos a seguirem o [princípio DRY](https://deviq.com/don-t-repeat-yourself/), com a adição de suporte para variáveis e lógica.

Os pré-processadores do CSS mais populares são o Sass e LESS. Ambos estendem o CSS e são compatíveis com versões anteriores dele, o que significa que um arquivo CSS simples é um arquivo do Sass ou LESS válido. O Sass é baseado em Ruby e o LESS é baseado em JavaScript e ambos costumam ser executados como parte do processo de desenvolvimento local. Ambos têm ferramentas de linha de comando disponíveis, bem como suporte interno no Visual Studio para executá-las usando tarefas Gulp ou Grunt.

## <a name="javascript"></a>JavaScript

O JavaScript é uma linguagem de programação dinâmica e interpretada que foi padronizada na especificação da linguagem ECMAScript. É a linguagem de programação da Web. Assim como o CSS, o JavaScript pode ser definido como atributos em elementos HTML, como blocos de script em uma página ou em arquivos separados. Assim como o CSS, é recomendável organizar o JavaScript em arquivos separados, mantendo-o separado o máximo possível do HTML encontrado em páginas da Web individuais ou em exibições de aplicativos.

Ao trabalhar com o JavaScript no aplicativo Web, há algumas tarefas que você geralmente precisará executar:

- Selecionar um elemento HTML e recuperar e/ou atualizar seu valor.

- Consultar dados em uma API Web.

- Enviando um comando para uma API Web (e respondendo a um retorno de chamada com seu resultado).

- Executando a validação.

Você pode executar todas essas tarefas apenas com o JavaScript, mas muitas bibliotecas existem para facilitar essas tarefas. Uma das primeiras e mais bem-sucedidas dessas bibliotecas é o jQuery, que continua sendo uma opção popular para simplificar essas tarefas em páginas da Web. Para SPAs (Aplicativos de Página Única), o jQuery não fornece muitos dos recursos desejados oferecidos pelo Angular e React.

### <a name="legacy-web-apps-with-jquery"></a>Aplicativos Web herdados com jQuery

Embora o antigo dos padrões de estrutura do JavaScript, o jQuery continua sendo uma biblioteca comumente usada para trabalhar com HTML/CSS e criar aplicativos que fazem chamadas AJAX para APIs da Web. No entanto, o jQuery opera no nível do DOM (Modelo de Objeto do Documento) do navegador e, por padrão, oferece apenas um modelo de imperativo, em vez de declarativo.

Por exemplo, imagine que, se o valor da caixa de texto exceder 10, um elemento na página precisará ficar visível. No jQuery, isso normalmente será implementado com a escrita de um manipulador de eventos com um código que inspecionará o valor da caixa de texto e definirá a visibilidade do elemento de destino com base nesse valor. Essa é uma abordagem imperativa, baseada em código. Em vez disso, outra estrutura poderá usar a associação de dados para associar a visibilidade do elemento ao valor da caixa de texto de forma declarativa. Isso não exige nenhuma codificação, mas apenas a decoração dos elementos envolvidos com atributos de associação de dados. À medida que os comportamentos do lado do cliente crescem mais complexos, as abordagens de ligação de dados frequentemente resultam em soluções mais simples com menos código e complexidade condicional.

### <a name="jquery-vs-a-spa-framework"></a>jQuery vs. uma estrutura de SPA

| **Fator** | **jQuery** | **Angular**|
|--------------------------|------------|-------------|
| Abstrai o DOM | **Sim** | **Sim** |
| Suporte do AJAX | **Sim** | **Sim** |
| Associação de dados declarativa | **Não** | **Sim** |
| Roteamento no estilo MVC | **Não** | **Sim** |
| Modelagem de texto | **Não** | **Sim** |
| Roteamento de link profundo | **Não** | **Sim** |

A maioria dos recursos que o jQuery não tem intrinsecamente pode ser adicionada com a adição de outras bibliotecas. No entanto, uma estrutura de SPA como o Angular fornece esses recursos de forma mais integrada, pois foi projetado com todos eles em mente, desde o início. Além disso, o jQuery é uma biblioteca imperativa, o que significa que você precisa chamar o jQuery Functions para fazer qualquer coisa com o jQuery. Grande parte do trabalho e da funcionalidade fornecida pelas estruturas de SPA pode ser feita de forma declarativa, sem a necessidade de codificação real.

A associação de dados é um ótimo exemplo disso. No jQuery, normalmente só usa uma linha de código para obter o valor de um elemento DOM ou para definir um valor de elemento. No entanto, você precisa escrever esse código sempre que precisar alterar o valor do elemento e, às vezes, isso ocorrerá em várias funções em uma página. Outro exemplo comum é a visibilidade do elemento. No jQuery, pode haver muitos locais diferentes em que você escreveria código para controlar se determinados elementos estavam visíveis. Em cada um desses casos, ao usar a associação de dados, nenhuma codificação precisa ser feita. Você simplesmente associaria o valor ou a visibilidade dos elementos em questão a um *ViewModel* na página e as alterações feitas nesse ViewModel seriam refletidas automaticamente nos elementos associados.

### <a name="angular-spas"></a>SPAs do Angular

O angular permanece uma das estruturas JavaScript mais populares do mundo. Desde o angular 2, a equipe reconstruiu a estrutura do zero (usando [TypeScript](https://www.typescriptlang.org/)) e remarcava o nome do AngularJS original para simplesmente angular. Agora, há vários anos, o angular reprojetado continua a ser uma estrutura robusta para a criação de aplicativos de página única.

Os aplicativos do Angular baseiam-se em componentes. Os componentes combinam modelos HTML com objetos especiais e controlam uma parte da página. Um componente simples da documentação do Angular é mostrado aqui:

```js
import { Component } from '@angular/core';

@Component({
    selector: 'my-app',
    template: `<h1>Hello {{name}}</h1>`
})

export class AppComponent { name = 'Angular'; }
```

Os componentes são definidos com a função de decorador @Component, que usa metadados sobre o componente. A propriedade selector identifica a ID do elemento na página em que este componente será exibido. A propriedade de modelo é um modelo HTML simples que inclui um espaço reservado que corresponde à propriedade de nome do componente, definida na última linha.

Trabalhando com componentes e modelos, em vez de elementos DOM, os aplicativos do Angular podem operar em um nível superior de abstração e com menos código geral comparado aos aplicativos escritos apenas com JavaScript (também chamado "JS baunilha") ou com o jQuery. O Angular também impõe uma ordem de como organizar os arquivos de script do lado do cliente. Por convenção, os aplicativos do Angular usam uma estrutura de pastas comum, com arquivos de script de módulo e componente localizados em uma pasta do aplicativo. Os scripts do Angular referentes à criação, à implantação e ao teste do aplicativo geralmente estão localizados em uma pasta de nível superior.

Você pode desenvolver aplicativos angulares usando uma CLI. Para começar o desenvolvimento local com o Angular (supondo que você já tenha instalado o GIT e o npm) basta clonar um repositório do GitHub e executar `npm install` e `npm start`. Além disso, o angular envia sua própria CLI, que pode criar projetos, adicionar arquivos e ajudar com tarefas de teste, agrupamento e implantação. Essa amigável de CLI torna o angular especialmente compatível com ASP.NET Core, que também oferece excelente suporte à CLI.

A Microsoft desenvolveu um aplicativo de referência, o [eShopOnContainers](https://aka.ms/MicroservicesArchitecture), que inclui uma implementação SPA do Angular. Esse aplicativo inclui módulos do Angular para gerenciar a cesta de compras da loja online, carregar e exibir itens do catálogo e manipular a criação de ordens. Exiba e baixe o aplicativo de exemplo no [GitHub](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Web/WebSPA).

### <a name="react"></a>React

Ao contrário do Angular, que oferece uma implementação completa do padrão Modelo-Exibição-Controlador, o React está voltado para exibições. Ele não é uma estrutura, apenas uma biblioteca e, portanto, para criar um SPA, precisará utilizar bibliotecas adicionais. Há uma série de bibliotecas que são projetadas para serem usadas com o reagir a produzir aplicativos avançados de página única.

Um dos recursos mais importantes do React é o uso de um DOM virtual. O DOM virtual oferece várias vantagens ao React, incluindo o desempenho (o DOM virtual pode otimizar quais partes do DOM real precisam ser atualizadas) e a capacidade de teste (sem a necessidade de ter um navegador para testar o React e suas interações com seu DOM virtual).

O React também é incomum na forma como funciona com HTML. Em vez de ter uma separação estrita entre o código e a marcação (talvez com a exibição das referências ao JavaScript em atributos HTML), o React adiciona o HTML diretamente ao seu código JavaScript como JSX. O JSX é uma sintaxe semelhante ao HTML que pode ser compilada até um JavaScript puro. Por exemplo:

```js
<ul>
{ authors.map(author =>
    <li key={author.id}>{author.name}</li>
)}
</ul>
```

Se você já conhece o JavaScript, aprender a usar o React deve ser fácil. A curva de aprendizado ou a sintaxe especial envolvida não é tão grande quanto a do Angular ou de outras bibliotecas populares.

Como o React não é uma estrutura completa, geralmente, você desejará ter outras bibliotecas para manipular aspectos como roteamento, chamadas à API Web e gerenciamento de dependências. A vantagem é que você pode escolher a melhor biblioteca para cada um deles, mas a desvantagem é que você precisa tomar todas essas decisões e verificar se todas as bibliotecas escolhidas funcionam bem em conjunto quando terminar. Caos deseje um bom ponto de partida, use um kit de início como o React Slingshot, que pré-empacota um conjunto de bibliotecas compatíveis com o React.

### <a name="vue"></a>Vue

Do seu guia de introdução, "Vue é uma estrutura progressiva para a criação de interfaces do usuário. Ao contrário de outras estruturas monolíticos, o Vue é projetado desde o início para ser populado incrementalmente. A biblioteca principal concentra-se apenas na camada de exibição e é fácil de pegar e integrar com outras bibliotecas ou projetos existentes. Por outro lado, o Vue é perfeitamente capaz de capacitar aplicativos de página única sofisticados quando usados em combinação com ferramentas modernas e bibliotecas de suporte. "

A introdução ao Vue simplesmente requer a inclusão de seu script em um arquivo HTML:

```html
<!-- development version, includes helpful console warnings -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

Com a estrutura adicionada, você é capaz de renderizar declarativamente os dados para o DOM usando a sintaxe de modelagem simples do Vue:

```html
<div id="app">
  {{ message }}
</div>
```

em seguida, adicione o seguinte script:

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

Isso é suficiente para renderizar "Olá Vue!" na página. No entanto, observe que Vue não está simplesmente renderizando a mensagem para o div uma vez. Ele dá suporte a associação de dados e a atualizações dinâmicas, de forma que, se o valor das `message` alterações, o valor no `<div>` seja imediatamente atualizado para refletir.

É claro que isso só aborda a superfície do que o Vue é capaz de fazer. Ele ganhou uma grande popularidade nos últimos anos e tem uma ampla comunidade. Há uma [lista enorme e crescente de componentes de suporte e bibliotecas](https://github.com/vuejs/awesome-vue#redux) que funcionam com o Vue para estendê-lo também. Se você pretende adicionar o comportamento do lado do cliente ao seu aplicativo Web ou considerar a criação de um SPA completo, vale a pena investigar o Vue.

### <a name="choosing-a-spa-framework"></a>Escolhendo uma estrutura de SPA

Ao considerar qual estrutura de JavaScript funcionará melhor para dar suporte ao SPA, tenha em mente as seguintes considerações:

- Sua equipe está familiarizada com a estrutura e suas dependências (incluindo o TypeScript em alguns casos)?

- Quão "teimosa" é a estrutura e você concorda com sua forma padrão de fazer as coisas?

- Ela (ou uma biblioteca complementar) inclui todos os recursos necessários para seu aplicativo?

- Está bem documentado?

- Quão ativa é sua comunidade? Novos projetos estão sendo compilados com ele?

- Quão ativa é sua equipe principal? Os problemas estão sendo resolvidos e novas versões são fornecidas regularmente?

As estruturas de JavaScript continuam evoluindo em uma velocidade vertiginosa. Use as considerações listadas acima para ajudar a atenuar o risco de escolher uma estrutura da qual mais tarde você se arrependerá de ter uma dependência. Caso você seja particularmente avesso a riscos, considere uma estrutura que oferece suporte comercial e/ou que está sendo desenvolvida por uma empresa grande.

> ### <a name="references--client-web-technologies"></a>Referências – Tecnologias da Web do cliente
>
> - **HTML e CSS**  
> <https://www.w3.org/standards/webdesign/htmlcss>
> - **Sass vs. LESS**  
> <https://www.keycdn.com/blog/sass-vs-less/>
> - **Criando estilos para aplicativos ASP.NET Core com o LESS, Sass e Font Awesome**  
> <https://docs.microsoft.com/aspnet/core/client-side/less-sass-fa>
> - **Desenvolvimento no lado do cliente no ASP.NET Core**  
> <https://docs.microsoft.com/aspnet/core/client-side/>
> - **jQuery**  
> <https://jquery.com/>
> - **jQuery vs. AngularJS**  
> <https://www.airpair.com/angularjs/posts/jquery-angularjs-comparison-migration-walkthrough>
> - **Angular**  
> <https://angular.io/>
> - **React**  
> <https://reactjs.org/>
> - **Vue**  
> <https://vuejs.org/>
> - **Angular versus reagir vs Vue: qual estrutura escolher em 2020**
> <https://www.codeinwp.com/blog/angular-vs-vue-vs-react/>
> - **As principais estruturas de JavaScript para desenvolvimento de front-end no 2020**  
> <https://www.freecodecamp.org/news/complete-guide-for-front-end-developers-javascript-frameworks-2019/>

>[!div class="step-by-step"]
>[Anterior](common-web-application-architectures.md) 
> [Avançar](develop-asp-net-core-mvc-apps.md)
