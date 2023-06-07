---
title: Conceitos e terminologia (transformação funcional) (C#)
description: As funcionalidades de programação funcional facilitam a transformação de XML. Saiba mais sobre os conceitos e a terminologia de transformações funcionais puras em C#.
ms.date: 07/20/2015
ms.assetid: 03defb3a-7e17-4ab1-8efa-4dd66621e860
ms.openlocfilehash: ca05a914f6156730e773d1effebfc72626b16507
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063270"
---
# <a name="concepts-and-terminology-functional-transformation-c"></a>Conceitos e terminologia (transformação funcional) (C#)

Este tópico apresenta os conceitos e a terminologia de transformações e puras. A abordagem funcional de transformação a transformação passa de dados codificação que geralmente é mais rápido, mais programar completo expressive, e mais fácil depurar e manter que a programação mais tradicional, mais imperativa.

Observe que os tópicos nesta seção não são destinados explicar totalmente funcional programação. Em vez disso, esses tópicos identificam alguns dos recursos funcionais de programação que facilitam transformar XML de uma forma para outra.

## <a name="what-is-pure-functional-transformation"></a>Transformação que é funcional pura?

Na *transformação funcional pura*, um conjunto de funções, chamadas *funções puras*, define como transformar um conjunto de dados estruturados de sua forma original em outra forma. A palavra "pura" indica que as funções são *combináveis*, que exigem que elas sejam:

- *Autocontidas*, de modo que elas possam ser ordenadas e reorganizadas livremente sem complicação ou interdependências com o restante do programa. As transformações puras não têm nenhum conhecimento de efetuam-no ou em cima do seu ambiente. Isto é, as funções usadas na transformação não têm *efeito colateral*.

- *Sem monitoração de estado*, de modo que a execução da mesma função ou conjunto específico de funções, na mesma entrada, sempre resultará na mesma saída. As transformações puras não têm memória do seu uso prévio.

> [!IMPORTANT]
> No restante deste tutorial, o termo “function” pura é usado em um sentido geral indicar uma abordagem de programação, e não em um recurso de linguagem específica.
>
> Observe que as funções puras devem ser implementadas como métodos no C#.
>
> Além disso, você não deve confundir funções puras com os métodos puros virtuais em C++. O último indica que a classe recipiente é abstrato e que nenhum corpo do método é fornecido.

### <a name="functional-programming"></a>Programação funcional

A *programação funcional* é uma abordagem de programação que dá suporte diretamente à transformação funcional pura.

Historicamente, as linguagens de programação funcionais de uso geral, como o ML, Scheme, Haskell e F#, têm sido, principalmente, de interesse da comunidade acadêmica. Embora sempre foi possível escrever transformações funcionais puras no C#, a dificuldade em fazê-lo não tornou essa opção atrativa à maioria dos programadores. Nas versões recentes do C#, no entanto, os novos constructos de linguagem como expressões lambda e inferência de tipos tornaram a programação funcional muito mais fácil e mais produtiva.

Para obter mais informações sobre a programação funcional, consulte programação [funcional vs. programação imperativa (C#)](./functional-programming-vs-imperative-programming.md).

#### <a name="domain-specific-fp-languages"></a>Linguagens específicas do domínio FP

Embora as linguagens de programação e gerais não sejam amplamente adotadas, as linguagens de programação e específicas do domínio específicas tinham melhor êxito. Por exemplo, folhas de estilos em cascata (CSS) são usadas para determinar a aparência de muitas páginas da Web, e as folhas de estilos extensíveis de transformações de language (XSLT) são usadas amplamente na manipulação de dados XML. Para obter mais informações sobre XSLT, consulte [Transformações XSLT](../../../../standard/data/xml/xslt-transformations.md).

## <a name="terminology"></a>Terminologia

A tabela a seguir define os termos relacionados às transformações funcionais.

função de ordem superior (primeira classe) \
Uma função que pode ser tratado como um objeto através de programação. Por exemplo, uma função de pedido superior pode ser passada para ou retornado de outras funções. No C#, delegados e expressões lambda são recursos de linguagem que dão suporte a funções de ordem superior. Para gravar uma função de pedido superior, você declara um ou mais argumentos para tomar representantes, e você frequentemente usa expressões lambda para chamá-lo. Muitos dos operadores de consulta padrão são funções de pedido superior.

Para obter mais informações, consulte [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md).

expressão lambda \
Essencialmente, uma função anônimo embutido que pode ser usada em que um tipo delegate é esperada. Esta é uma definição simplificada de expressões lambda, mas é suficiente para fins deste tutorial.

Para obter mais informações, consulte [Expressões lambda](../../../language-reference/operators/lambda-expressions.md).

coleção \
Um conjunto estruturada de dados, geralmente um tipo de uniforme. Para ser compatível com LINQ, uma coleção deve implementar a interface de <xref:System.Collections.IEnumerable> ou a interface de <xref:System.Linq.IQueryable> (ou uma de suas contrapartes genéricos, de <xref:System.Collections.Generic.IEnumerator%601> ou de <xref:System.Linq.IQueryable%601>).

tupla (tipos anônimos) \
Um conceito matemático, um tuple é uma sequência finito rotuladas de objetos, cada um de um tipo específico. Um tuple também é conhecido como uma lista ordenada. Tipos anônimos são uma implementação de linguagem desse conceito, que permitem que um tipo sem nome da classe ser declarados e um objeto do tipo a ser instanciada ao mesmo tempo.

Para obter mais informações, consulte [Tipos Anônimos](../../classes-and-structs/anonymous-types.md).

inferência de tipos (digitação implícita) \
A capacidade de um compilador de determinar o tipo de uma variável na ausência de uma declaração de tipo explícita.

Para obter mais informações, consulte [Variáveis locais de tipo implícito](../../classes-and-structs/implicitly-typed-local-variables.md).

execução adiada e avaliação lenta \
O atraso de avaliação de uma expressão até que o valor resolvido é realmente necessário. A execução adiada é suportado em coleções.

Para obter mais informações, consulte [Introdução a consultas LINQ (C#)](./introduction-to-linq-queries.md) e [Execução adiada e avaliação lenta em LINQ to XML (C#)](./deferred-execution-and-lazy-evaluation-in-linq-to-xml.md).

Esses recursos de idioma serão usados em exemplos de código em todo esta seção.

## <a name="see-also"></a>Consulte também

- [Introdução às transformações funcionais puras (C#)](./introduction-to-pure-functional-transformations.md)
- [Programação funcional versus programação imperativa (C#)](./functional-programming-vs-imperative-programming.md)
