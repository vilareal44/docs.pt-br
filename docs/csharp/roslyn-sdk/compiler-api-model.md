---
title: Conceitos e modelo de objeto do SDK do .NET Compiler Platform
description: Esta visão geral fornece o contexto necessário para trabalhar efetivamente com o SDK do .NET Compiler. Você aprenderá sobre as camadas de API, os principais tipos envolvidos e o modelo de objeto geral.
ms.date: 07/13/2020
ms.custom: mvc
ms.openlocfilehash: a65d282dd3c58279bbfd635c0386d50ce3f30055
ms.sourcegitcommit: e7748001b1cee80ced691d8a76ca814c0b02dd9b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86374462"
---
# <a name="understand-the-net-compiler-platform-sdk-model"></a>Entender o modelo do SDK do .NET Compiler Platform

Os compiladores processam o código escrito seguindo regras estruturadas que geralmente diferem da forma como os humanos leem e entendem um código. Uma compreensão básica do modelo usado pelos compiladores é essencial para compreender as APIs usadas ao criar ferramentas baseadas no Roslyn.

## <a name="compiler-pipeline-functional-areas"></a>Áreas funcionais do pipeline do compilador

O SDK do .NET Compiler Platform expõe a análise de código dos compiladores C# e Visual Basic para você como um consumidor, fornecendo uma camada de API que espelha um pipeline de compilador tradicional.

![etapas de processamento do pipeline do compilador do código-fonte para o código de objeto](media/compiler-api-model/compiler-pipeline.png)

Cada fase desse pipeline é um componente separado. Primeiro, a fase de análise cria tokens do texto de origem e o analisa na sintaxe que segue a gramática da linguagem. Depois, a fase de declaração analisa os metadados de origem e importados para formar símbolos nomeados. Em seguida, a fase de associação corresponde os identificadores no código aos símbolos. Por fim, a fase de emissão emite um assembly com todas as informações criadas pelo compilador.

![API do pipeline do compilador fornece acesso a cada etapa que faz parte do pipeline do compilador](media/compiler-api-model/compiler-pipeline-api.png)

Correspondente a cada uma dessas fases, o SDK do .NET Compiler Platform expõe um modelo de objeto que permite o acesso às informações da fase. A fase de análise expõe uma árvore de sintaxe, a fase de declaração expõe uma tabela de símbolos hierárquica, a fase de ligação expõe o resultado da análise semântica do compilador e a fase de emissão é uma API que produz códigos de bytes de IL.

![os serviço de linguagem disponíveis na API do compilador em cada etapa do pipeline do compilador](media/compiler-api-model/compiler-pipeline-lang-svc.png)

Cada compilador combina esses componentes como um único inteiro de ponta a ponta.

Essas APIs são as mesmas usadas pelo Visual Studio. Por exemplo, os recursos de estrutura de tópicos e formatação do código usam as árvores de sintaxe, o **pesquisador de objetos**e os recursos de navegação usam a tabela de símbolos, refatoração e **ir para definição** usam o modelo semântico, e **Editar e continuar** usa todos eles, incluindo a API de emissão.

## <a name="api-layers"></a>Camadas de API

O SDK do compilador .NET consiste em várias camadas de APIs: APIs de compilador, APIs de diagnóstico, APIs de script e APIs de espaços de trabalho.

### <a name="compiler-apis"></a>APIs do compilador

A camada do compilador contém os modelos de objeto que correspondem às informações expostas em cada fase do pipeline do compilador, tanto sintáticas quanto semânticas. A camada do compilador também contém um instantâneo imutável de uma única invocação de um compilador, incluindo referências de assembly, opções do compilador e arquivos de código-fonte. Há duas APIs distintas que representam a linguagem C# e a linguagem Visual Basic. As duas APIs são semelhantes em forma, mas adaptadas para alta fidelidade a cada idioma individual. Essa camada não tem dependências em componentes do Visual Studio.

### <a name="diagnostic-apis"></a>APIs de diagnóstico

Como parte de sua análise, o compilador pode produzir um conjunto de diagnósticos que abrangem tudo, desde a sintaxe, semântica e erros de atribuição definitiva a vários diagnósticos de avisos e informativos. A camada de API do Compilador expõe o diagnóstico por meio de uma API extensível que permite que os analisadores definidos pelo usuário sejam conectados ao processo de compilação. Ela possibilita que o diagnóstico definido pelo usuário, como aqueles gerados por ferramentas como o StyleCop ou FxCop, seja produzido junto com o diagnóstico definido pelo compilador. A produção de diagnóstico dessa maneira tem o benefício de integrar naturalmente com ferramentas como o MSBuild e o Visual Studio, que dependem de diagnósticos para experiências como interromper uma compilação com base na política e mostrar rabiscos ao vivo no editor e sugerir correções de código.

### <a name="scripting-apis"></a>APIs de script

APIs de hospedagem e script fazem parte da camada do compilador. Você pode usá-las para execução de snippets de código e acúmulo de um contexto de execução em runtime.
O REPL (Loop de Leitura-Avaliação-Impressão) interativo do C# usa essas APIs. O REPL permite usar o C# como a linguagem de scripts, executando o código de forma interativa à medida que ele é escrito.

### <a name="workspaces-apis"></a>APIs dos workspaces

A camada Workspaces contém a API de Workspace, que é o ponto de partida para fazer a análise de código e refatoração em soluções inteiras. Ele ajuda você a organizar todas as informações sobre os projetos em uma solução em um único modelo de objeto, oferecendo acesso direto aos modelos de objeto da camada do compilador sem a necessidade de analisar arquivos, configurar opções ou gerenciar dependências de projeto para projeto.

Além disso, a camada Workspaces expõe um conjunto de APIs usado ao implementar ferramentas de análise de código e refatoração que funcionam em um ambiente de host como o IDE do Visual Studio. Exemplos incluem as APIs Localizar Todas as Referências, Formatação e Geração de Código.

Essa camada não tem dependências em componentes do Visual Studio.
