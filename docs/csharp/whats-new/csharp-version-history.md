---
title: O histórico da linguagem C# – Guia do C#
description: Qual era a aparência da linguagem nas primeiras versões e como ela evoluiu desde então?
author: erikdietrich
ms.date: 04/08/2020
ms.openlocfilehash: b5c320e4c55803547fa44793a46e4a3da65bd0cb
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063465"
---
# <a name="the-history-of-c"></a>O histórico da linguagem C\#

Este artigo fornece um histórico de cada versão principal da linguagem C#. A equipe C# continua a inovar e a adicionar novos recursos. Os status detalhados do recurso de linguagem, incluindo os recursos considerados para as versões futuras, podem ser encontrados [no repositório dotnet/roslyn](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md) no GitHub.

> [!IMPORTANT]
> A linguagem C# depende de tipos e métodos nos quais a especificação C# é definida como uma *biblioteca padrão* para alguns dos recursos. A plataforma .NET fornece esses tipos e métodos em alguns pacotes. Um exemplo é o processamento de exceção. Cada instrução ou expressão `throw` é verificada para garantir que o objeto que está sendo gerado é derivado de <xref:System.Exception>. Da mesma forma, cada `catch` é verificado para garantir que o tipo que está sendo capturado é derivado de <xref:System.Exception>. Cada versão pode adicionar novos requisitos. Para usar os recursos de linguagem mais recentes em ambientes mais antigos, talvez seja necessário instalar bibliotecas específicas. Essas dependências estão documentadas na página de cada versão específica. Saiba mais sobre as [relações entre linguagem e biblioteca](relationships-between-language-and-library.md) para obter informações sobre essa dependência.

As ferramentas de compilação do C# consideram a versão mais recente da linguagem principal como a versão padrão da linguagem. Pode haver versões de ponto entre as versões principais, detalhadas em outros artigos nesta seção. Para usar as últimas funcionalidades em uma versão de ponto, você precisa [configurar a versão da linguagem do compilador](../language-reference/configure-language-version.md) e selecionar a versão. Houve lançamentos de três pontos desde o C# 7,0:

- [C# 7,3](csharp-7-3.md):
  - O C# 7.3 está disponível a partir do [Visual Studio 2017 versão 15.7](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) e do [SDK do .NET Core 2.1](../../core/whats-new/dotnet-core-2-1.md).
- [C# 7,2](csharp-7-2.md):
  - O C# 7,2 está disponível a partir do [Visual Studio 2017 versão 15,5](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) e do [SDK do .NET Core 2,0](../../core/whats-new/dotnet-core-2-0.md).
- [C# 7,1](csharp-7-1.md):
  - O C# 7.1 está disponível a partir do [Visual Studio 2017 versão 15.3](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) e do [SDK do .NET Core 2.0](../../core/whats-new/dotnet-core-2-0.md).

## <a name="c-version-10"></a>C# versão 1.0

Quando voltar e olhar, a versão 1,0 do C#, lançada com o Visual Studio .NET 2002, ficou muito parecida com o Java. Como [parte de suas metas de design declaradas para ECMA](https://feeldotneteasy.blogspot.com/2011/01/c-design-goals.html), ela buscava ser uma "linguagem simples, moderna, de uso geral e orientada a objeto".  No momento, parece que o Java alcançou essas metas de design iniciais.

Mas agora, se examinar novamente a C# 1.0, você poderá se sentir um pouco confuso. Carecia das funcionalidades assíncronas internas e algumas das funcionalidades relacionadas a genéricos que você nem valoriza. Na verdade, ela não tinha nada relacionado a genéricos.  E a [LINQ](../linq/index.md)? Ainda não estava disponível. Essas adições levariam alguns anos para sair.

A versão 1.0 do C# parecia ter poucos recursos, em comparação com os dias de hoje. Você teria que escrever código um tanto detalhado. Mas, ainda assim, você poderia iniciar em algum lugar. A versão 1.0 do C# era uma alternativa viável ao Java na plataforma Windows.

Os principais recursos do C# 1.0 incluíam:

- [Classes](../programming-guide/classes-and-structs/classes.md)
- [Estruturas](../language-reference/builtin-types/struct.md)
- [Interfaces](../programming-guide/interfaces/index.md)
- [Eventos](../events-overview.md)
- [Propriedades](../properties.md)
- [Representantes](../delegates-overview.md)
- [Operadores e expressões](../language-reference/operators/index.md)
- [Instruções](../programming-guide/statements-expressions-operators/statements.md)
- [Atributos](../programming-guide/concepts/attributes/index.md)

## <a name="c-version-12"></a>C# versão 1.2

C# versão 1,2 fornecida com o Visual Studio .NET 2003. Ele continha algumas pequenas melhorias na linguagem. Muito notável é que, a partir desta versão, o código gerado em um loop `foreach` chamou <xref:System.IDisposable.Dispose%2A>, em um <xref:System.Collections.IEnumerator>, quando o <xref:System.Collections.IEnumerator> implementou <xref:System.IDisposable>.

## <a name="c-version-20"></a>C# versão 2.0

Neste momento, as coisas começam a ficar interessantes. Vamos dar uma olhada em alguns recursos principais do C# 2.0, lançado em 2005, junto com o Visual Studio 2005:

- [Genéricos](../programming-guide/generics/index.md)
- [Tipos parciais](../programming-guide/classes-and-structs/partial-classes-and-methods.md#partial-classes)
- [Métodos anônimos](../language-reference/operators/delegate-operator.md)
- [Tipos de valor anuláveis](../language-reference/builtin-types/nullable-value-types.md)
- [Iterators](../programming-guide/concepts/iterators.md)
- [Covariância e contravariância](../programming-guide/concepts/covariance-contravariance/index.md)

Outros recursos do C# 2.0 adicionaram funcionalidades a recursos existentes:

- Acessibilidade separada getter/setter
- Conversões de grupo de método (delegados)
- Classes estáticas
- Inferência de delegado

Embora C# possa ter começado como uma linguagem OO (orientada a objeto) genérica, a versão 2.0 do C# tratou de mudar isso rapidamente. Depois de se acostumarem com a ideia da linguagem OO, os desenvolvedores começaram a sofrer com vários pontos problemáticos graves. E os procuraram de maneira significativa.

Com genéricos, tipos e métodos podem operar em um tipo arbitrário enquanto ainda mantêm a segurança de tipos. Por exemplo, ter um <xref:System.Collections.Generic.List%601> permite que você tenha `List<string>` ou `List<int>` e execute operações fortemente tipadas nessas cadeias de caracteres ou inteiros enquanto itera neles. Usar genéricos é melhor que criar `ListInt` que deriva de `ArrayList` ou converter de `Object` para cada operação.

A versão 2.0 do C# trouxe iteradores. Em resumo, o iteradores permitem que você examine todos os itens em um `List` (ou outros tipos Enumeráveis) com um loop `foreach`. Ter iteradores como uma parte importante da linguagem aprimorou drasticamente a legibilidade da linguagem e a capacidade das pessoas de raciocinar a respeito do código.

E ainda assim, o C# continuava na tentativa de alcançar o mesmo nível do Java. O Java já tinha liberado versões que incluíam genéricos e iteradores. Mas isso seria alterado logo, pois as linguagens continuaram a evoluir separadamente.

## <a name="c-version-30"></a>C# versão 3.0

O C# versão 3.0 chegou no final de 2007, juntamente com o Visual Studio 2008, porém o pacote completo de recursos de linguagem veio, na verdade, com o C# versão 3.5. Esta versão foi o marco de uma alteração significativa no crescimento do C#. Ela estabeleceu o C# como uma linguagem de programação realmente formidável. Vamos dar uma olhada em alguns recursos importantes nesta versão:

- [Propriedades implementadas automaticamente](../programming-guide/classes-and-structs/auto-implemented-properties.md)
- [Tipos anônimos](../programming-guide/classes-and-structs/anonymous-types.md)
- [Expressões de consulta](../linq/query-expression-basics.md)
- [Expressões lambda](../language-reference/operators/lambda-expressions.md)
- [Árvores de expressão](../expression-trees.md)
- [Métodos de extensão](../programming-guide/classes-and-structs/extension-methods.md)
- [Variáveis locais digitadas implicitamente](../language-reference/keywords/var.md)
- [Métodos parciais](../language-reference/keywords/partial-method.md)
- [Inicializadores de objeto e coleção](../programming-guide/classes-and-structs/object-and-collection-initializers.md)

Numa retrospectiva, muitos desses recursos parecerem inevitáveis e inseparáveis. Todos eles se encaixam estrategicamente. Costuma-se pensar que o recurso irresistível dessa versão do C# foi a expressão de consulta, também conhecida como LINQ (consulta integrada à linguagem).

Uma exibição mais detalhada analisa árvores de expressão, expressões lambda e tipos anônimos como a base na qual o LINQ é construído. Mas, de uma forma ou de outra, o C# 3.0 apresentou um conceito revolucionário. O C# 3.0 começou a definir as bases para transformar o C# em uma linguagem híbrida orientada a objeto e funcional.

Especificamente, agora você pode escrever consultas declarativas no estilo SQL para executar operações em coleções, entre outras coisas. Em vez de escrever um loop `for` para calcular a média de uma lista de inteiros, agora você pode fazer isso simplesmente como `list.Average()`. A combinação de expressões de consulta e métodos de extensão fez parecer que essa lista de inteiros se tornasse muito mais inteligente.

Levou algum tempo para que as pessoas entendessem e integrassem o conceito, mas isso aconteceu gradualmente. E agora, anos mais tarde, o código é muito mais conciso, simples e funcional.

## <a name="c-version-40"></a>C# versão 4.0

O C# versão 4,0, lançado com o Visual Studio 2010, teria tido um tempo difícil de viver até o status inovador da versão 3,0. Com a versão 3.0, o C# tirou verdadeiramente a linguagem da sombra do Java e a colocou em proeminência. A linguagem foi rapidamente se tornando elegante.

A próxima versão introduziu alguns novos recursos interessantes:

- [Associação dinâmica](../language-reference/builtin-types/reference-types.md)
- [Argumentos opcionais/nomeados](../programming-guide/classes-and-structs/named-and-optional-arguments.md)
- [Genérico covariante e contravariante](../../standard/generics/covariance-and-contravariance.md)
- [Tipos de interoperabilidade inseridos](../../framework/interop/type-equivalence-and-embedded-interop-types.md)

Os tipos de interoperabilidade inseridos atenuaram um problema de implantação. A contravariância e a covariância genérica oferecem maior capacidade para usar genéricos, mas eles são um tanto acadêmicos e provavelmente mais apreciados por autores de estruturas e bibliotecas. Os parâmetros nomeados e opcionais permitem eliminar várias sobrecargas de método e oferecem conveniência. Mas nenhum desses recursos é exatamente uma alteração de paradigma.

O recurso principal foi a introdução da palavra-chave `dynamic`. A palavra-chave `dynamic` introduziu na versão 4.0 do C# a capacidade de substituir o compilador na tipagem em tempo de compilação. Com o uso da palavra-chave dinâmica, você pode criar constructos semelhantes a linguagens dinamicamente tipadas, como JavaScript. Você pode criar um `dynamic x = "a string"` e, em seguida, adicionar seis a ela, deixando que o runtime decida o que acontece em seguida.

Associação dinâmica tem potencial de erros, mas também grande eficiência na linguagem.

## <a name="c-version-50"></a>C# versão 5.0

A versão C# 5,0, lançada com o Visual Studio 2012, foi uma versão focada da linguagem. Quase todo o esforço para essa versão foi dedicado a outro conceito inovador de linguagem: os modelos `async` e `await` para programação assíncrona.  Aqui está a lista dos recursos principais:

- [Membros assíncronos](../async.md)
- [Atributos de informações do chamador](../language-reference/attributes/caller-information.md)

### <a name="see-also"></a>Consulte Também

- [Code Project: Caller Info Attributes in C# 5.0](https://www.codeproject.com/Tips/606379/Caller-Info-Attributes-in-Csharp) (Code Project: Atributos de informações do chamador em C# 5.0)

O atributo de informações do chamador permite facilmente recuperar informações sobre o contexto no qual você está executando sem recorrer a uma infinidade de código de reflexão clichê. Ele tem muitos usos em diagnóstico e tarefas de registro em log.

Mas `async` e `await` são as verdadeiras estrelas dessa versão. Quando esses recursos foram lançados em 2012, o C# virou o jogo novamente, implantando a assincronia na linguagem como uma participante da maior importância. Se você já teve que lidar com operações de longa execução e a implementação de redes de retorno de chamada, você provavelmente adorou esse recurso de linguagem.

## <a name="c-version-60"></a>C# versão 6.0

Nas versões 3.0 e 5.0, o C# recebeu alguns novos recursos importantes em uma linguagem orientada a objeto. Com a versão 6,0, lançada com o Visual Studio 2015, ela desapareceria com um recurso de Killer dominante e, em vez disso, lançaria muitos recursos menores que tornaram a programação C# mais produtiva. Eis algumas delas:

- [Importações estáticas](./csharp-6.md#using-static)
- [Filtros de exceção](./csharp-6.md#exception-filters)
- [Inicializadores de propriedade automática](./csharp-6.md#auto-property-initializers)
- [Membros aptos para expressão](./csharp-6.md#expression-bodied-function-members)
- [Propagador nulo](./csharp-6.md#null-conditional-operators)
- [Interpolação de cadeia de caracteres](./csharp-6.md#string-interpolation)
- [operador nameof](./csharp-6.md#the-nameof-expression)
- [Inicializadores de índice](csharp-6.md#extension-add-methods-in-collection-initializers)

Outros novos recursos incluem:

- Await em blocos catch/finally
- Valores padrão para propriedades somente getter

Cada um desses recursos é interessante em seus próprios méritos. Mas, se você os observar em conjunto, verá um padrão interessante. Nesta versão, o C# eliminou o clichê de linguagem para tornar o código mais conciso e legível. Portanto, para os fãs de código simples e conciso, essa versão da linguagem foi um grande benefício.

Fizeram ainda outra coisa com esta versão, embora não seja um recurso de linguagem tradicional em si. Lançaram [Roslyn, o compilador como um serviço](https://github.com/dotnet/roslyn). Agora o compilador de C# é escrito em C#, e você pode usar o compilador como parte de seus esforços de programação.

## <a name="c-version-70"></a>C# versão 7.0

A versão 7,0 do C# foi lançada com o Visual Studio 2017. Esta versão tem algumas coisas interessantes e evolutivas na mesma direção que o C# 6.0, mas sem o compilador como um serviço. Aqui estão alguns dos novos recursos:

- [Variáveis out](./csharp-7.md#out-variables)
- [Tuplas e desconstrução](./csharp-7.md#tuples)
- [Correspondência de padrões](./csharp-7.md#pattern-matching)
- [Funções locais](./csharp-7.md#local-functions)
- [Membros aptos para expressão expandidos](./csharp-7.md#more-expression-bodied-members)
- [Locais e retornos de ref](./csharp-7.md#ref-locals-and-returns)

Outros recursos incluíam:

- [Descartes](./csharp-7.md#discards)
- [Literais binários e os separadores de dígito](./csharp-7.md#numeric-literal-syntax-improvements)
- [Expressões throw](./csharp-7.md#throw-expressions)

Todas essas funcionalidades oferecem novos recursos interessantes para desenvolvedores e a oportunidade de escrever um código mais limpo do que nunca. Um ponto alto é a condensação da declaração de variáveis a serem usadas com a palavra-chave `out` e a permissão de vários valores retornados por meio de tupla.

Mas o C# está sendo colocado para um uso cada vez mais amplo. Agora o .NET Core tem qualquer sistema operacional como destino e tem a visão firme na nuvem e na portabilidade.  Essas novas funcionalidades certamente ocupam a mente e o tempo dos designers da linguagem, além de levarem a novos recursos.

## <a name="c-version-71"></a>C# versão 7,1

C# começou a liberar *lançamentos de ponto* com c# 7,1. Esta versão adicionou o elemento de configuração [seleção de versão de idioma](../language-reference/configure-language-version.md) , três novos recursos de linguagem e novo comportamento de compilador.

Os novos recursos de linguagem nesta versão são:

- [`async``Main`método](./csharp-7-1.md#async-main)
  - O ponto de entrada para um aplicativo pode ter o modificador `async`.
- [`default`expressões literais](./csharp-7-1.md#default-literal-expressions)
  - Use expressões literais padrão em expressões de valor padrão quando o tipo de destino pode ser inferido.
- [Nomes de elementos de tupla inferidos](./csharp-7-1.md#inferred-tuple-element-names)
  - Em muitos casos, os nomes dos elementos de tupla podem ser inferidos com base na inicialização da tupla.
- [Restrições em parâmetros de tipo genérico](./csharp-7-1.md#pattern-matching-on-generic-type-parameters)
  - Você pode usar expressões de correspondência de padrão em variáveis cujo tipo é um parâmetro de tipo genérico.

Por fim, o compilador traz duas opções `-refout` e `-refonly`, que controlam a [geração de assembly de referência](./csharp-7-1.md#reference-assembly-generation).

## <a name="c-version-72"></a>C# versão 7,2

O C# 7,2 adicionou vários recursos de linguagem pequena:

- [Técnicas para escrever código eficiente seguro](./csharp-7-2.md#safe-efficient-code-enhancements)
  - Uma combinação de aprimoramentos de sintaxe que permitem trabalhar com tipos de valor usando a semântica de referência.
- [Argumentos nomeados que não estejam à direita](./csharp-7-2.md#non-trailing-named-arguments)
  - Os argumentos nomeados podem ser seguidos por argumentos posicionais.
- [Sublinhados à esquerda em literais numéricos](./csharp-7-2.md#leading-underscores-in-numeric-literals)
  - Agora os literais numéricos podem ter sublinhados à esquerda, antes dos dígitos impressos.
- [`private protected`modificador de acesso](./csharp-7-2.md#private-protected-access-modifier)
  - O modificador de acesso `private protected` permite o acesso a classes derivadas no mesmo assembly.
- [Expressões condicionais `ref`](./csharp-7-2.md#conditional-ref-expressions)
  - O resultado de uma expressão condicional (`?:`) agora já pode ser uma referência.

## <a name="c-version-73"></a>C# versão 7,3

Há dois temas principais para a versão C# 7.3. Um tema fornece recursos que permitem que o código seguro tenha o mesmo desempenho que o código não seguro. O segundo tema fornece melhorias incrementais aos recursos existentes. Além disso, novas opções do compilador foram adicionadas nesta versão.

Os novos recursos a seguir são compatíveis com o tema de melhor desempenho para código seguro:

- [Você pode acessar campos fixos sem fixação.](csharp-7-3.md#indexing-fixed-fields-does-not-require-pinning)
- [Você pode reatribuir `ref` variáveis locais.](csharp-7-3.md#ref-local-variables-may-be-reassigned)
- [Você pode usar inicializadores em `stackalloc` matrizes.](csharp-7-3.md#stackalloc-arrays-support-initializers)
- [Você pode usar `fixed` instruções com qualquer tipo que dê suporte a um padrão.](csharp-7-3.md#more-types-support-the-fixed-statement)
- [Você pode usar restrições genéricas adicionais.](csharp-7-3.md#enhanced-generic-constraints)

Os seguintes recursos e aprimoramentos foram feitos nos recursos existentes:

- [Você pode testar `==` e `!=` com tipos de tupla.](csharp-7-3.md#tuples-support--and-)
- [Você pode usar variáveis de expressão em mais locais.](csharp-7-3.md#extend-expression-variables-in-initializers)
- [Você pode anexar atributos ao campo de suporte de propriedades autoimplementadas.](csharp-7-3.md#attach-attributes-to-the-backing-fields-for-auto-implemented-properties)
- [Resolução de método quando os argumentos diferem por `in` foram aprimorados.](csharp-7-3.md#in-method-overload-resolution-tiebreaker)
- [A resolução de sobrecarga agora tem menos casos ambíguos.](csharp-7-3.md#improved-overload-candidates)

As novas opções do compilador são:

- [`-publicsign`para habilitar a assinatura de OSS (software livre) de assemblies.](csharp-7-3.md#public-or-open-source-signing)
- [`-pathmap`para fornecer um mapeamento para diretórios de origem.](csharp-7-3.md#pathmap)

## <a name="c-version-80"></a>C# versão 8,0

O c# 8,0 é a primeira versão principal do C# que se destina especificamente ao .NET Core. Alguns recursos dependem de novos recursos do CLR, outros em tipos de biblioteca adicionados somente no .NET Core. O c# 8,0 adiciona os seguintes recursos e aprimoramentos à linguagem C#:

- [Membros somente leitura](./csharp-8.md#readonly-members)
- [Métodos de interface padrão](./csharp-8.md#default-interface-methods)
- [Aprimoramentos de correspondência de padrões](./csharp-8.md#more-patterns-in-more-places):
  - [Expressões Switch](./csharp-8.md#switch-expressions)
  - [Padrões da propriedade](./csharp-8.md#property-patterns)
  - [Padrões de tupla](./csharp-8.md#tuple-patterns)
  - [Padrões posicionais](./csharp-8.md#positional-patterns)
- [Declarações using](./csharp-8.md#using-declarations)
- [Funções locais estáticas](./csharp-8.md#static-local-functions)
- [Estruturas ref descartáveis](./csharp-8.md#disposable-ref-structs)
- [Tipos de referência anuláveis](../language-reference/builtin-types/nullable-reference-types.md)
- [Fluxos assíncronos](./csharp-8.md#asynchronous-streams)
- [Índices e intervalos](./csharp-8.md#indices-and-ranges)
- [Atribuição de União nula](./csharp-8.md#null-coalescing-assignment)
- [Tipos construídos não gerenciados](./csharp-8.md#unmanaged-constructed-types)
- [Stackalloc em expressões aninhadas](./csharp-8.md#stackalloc-in-nested-expressions)
- [Aprimoramento de cadeias de caracteres idênticas interpoladas](./csharp-8.md#enhancement-of-interpolated-verbatim-strings)

Os membros da interface padrão exigem aprimoramentos no CLR. Esses recursos foram adicionados ao CLR para .NET Core 3,0. Intervalos e índices e fluxos assíncronos exigem novos tipos nas bibliotecas do .NET Core 3,0. Os tipos de referência anuláveis, enquanto implementados no compilador, são muito mais úteis quando as bibliotecas são anotadas para fornecer informações semânticas sobre o estado nulo dos argumentos e valores de retorno. Essas anotações estão sendo adicionadas nas bibliotecas do .NET Core.

_Artigo_ [_originalmente publicado no blog NDepend_](https://blog.ndepend.com/c-versions-look-language-history/)_, cortesia de Erik Dietrich e Patrick Smacchia._
