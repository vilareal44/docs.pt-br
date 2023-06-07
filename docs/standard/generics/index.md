---
title: Generics in .NET (Genéricos no .NET)
description: Saiba mais sobre os genéricos no .NET. Defina & usar genéricos, aprenda suas vantagens & desvantagens, entenda o suporte à biblioteca de classes & idioma e muito mais.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generic methods, type inference
- generics [.NET Framework], collections
- generic interfaces [.NET Framework]
- constructed generic types
- nested generic types
- generic type definitions
- generic classes [.NET Framework]
- generics [.NET Framework], interfaces
- generics [.NET Framework], about
- generics [.NET Framework]
- generic collections [.NET Framework]
- generic delegates [.NET Framework]
- generic type arguments
- generics [.NET Framework], delegates
- generics [.NET Framework], features
- constraints [.NET Framework]
- generic types
- generic type parameters
ms.assetid: 2994d786-c5c7-4666-ab23-4c83129fe39c
ms.openlocfilehash: 91bb96c04c8a5d410f0a88c7e8eedf622fe66c94
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599809"
---
# <a name="generics-in-net"></a>Generics in .NET (Genéricos no .NET)

Os genéricos permitem que você personalize um método, uma classe, uma estrutura ou uma interface para o tipo exato de dados no qual ele atua. Por exemplo, em vez de usar a classe <xref:System.Collections.Hashtable>, que permite que as chaves e os valores sejam de qualquer tipo, você pode usar a classe genérica <xref:System.Collections.Generic.Dictionary%602> e especificar o tipo permitido para a chave e o tipo permitido para o valor. Entre os benefícios de genéricos estão maior reutilização de códigos e segurança de tipos.  

## <a name="defining-and-using-generics"></a>Definindo e usando genéricos
 Genéricos são classes, estruturas, interfaces e métodos que possuem espaços reservados (parâmetros de tipo) para um ou mais dos tipos que eles armazenam ou usam. Uma classe de coleção genérica pode usar um parâmetro de tipo como um espaço reservado para o tipo de objetos que ela armazena; os parâmetros de tipo aparecem como os tipos de seus campos e os tipos de parâmetro de seus métodos. Um método genérico pode usar seu parâmetro de tipo como o tipo de seu valor de retorno ou como o tipo de um de seus parâmetros formais. O código a seguir ilustra uma definição de classe genérica simples.  
  
 [!code-cpp[Conceptual.Generics.Overview#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source.cpp#2)]
 [!code-csharp[Conceptual.Generics.Overview#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source.cs#2)]
 [!code-vb[Conceptual.Generics.Overview#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source.vb#2)]  
  
 Quando você cria uma instância de uma classe genérica, pode especificar os tipos reais para substituir os parâmetros de tipo. Isso estabelece uma nova classe genérica, conhecida como uma classe genérica construída, com seus tipos escolhidos substituídos em todos os locais em que aparecem os parâmetros de tipo. O resultado é uma classe fortemente tipada que é personalizada para sua escolha de tipos, como mostra o código a seguir.  
  
 [!code-cpp[Conceptual.Generics.Overview#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source.cpp#3)]
 [!code-csharp[Conceptual.Generics.Overview#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source.cs#3)]
 [!code-vb[Conceptual.Generics.Overview#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source.vb#3)]  

### <a name="generics-terminology"></a>Terminologia de genéricos  
 Os seguintes termos são usados para discutir genéricos no .NET:  
  
- A *definição de tipo genérico* é uma classe, estrutura ou declaração de interface que funciona como um modelo, com espaços reservados para os tipos que ela pode conter ou usar. Por exemplo, a classe <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> pode conter dois tipos: chaves e valores. Como uma definição de tipo genérico é apenas um modelo, não é possível criar instâncias de uma classe, estrutura ou interface que seja uma definição de tipo genérico.  
  
- *Parâmetros de tipo genérico* ou *parâmetros de tipo* são os espaços reservados em uma definição de método ou de tipo genérico. O tipo genérico <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> tem dois parâmetros de tipo, `TKey` e `TValue`, que representam os tipos de suas chaves e valores.  
  
- Um *tipo genérico construído* ou um *tipo construído* é o resultado de especificação de tipos para os parâmetros de tipo genérico de uma definição de tipo genérico.  
  
- Um *argumento de tipo genérico* é qualquer tipo que seja substituído por um parâmetro de tipo genérico.  
  
- O termo geral *tipo genérico* inclui definições de tipo genérico e de tipos construídos.  
  
- *Covariância* e *contravariância* de parâmetros de tipo genérico permitem que você use tipos genéricos construídos cujos argumentos de tipo sejam mais derivados (covariância) ou menos derivados (contravariância) de um tipo construído de destino. A covariância e a contravariância são referidas coletivamente como *variância*. Para obter mais informações, consulte [Covariância e contravariância](covariance-and-contravariance.md).  
  
- *Restrições* são limites colocados em parâmetros de tipo genérico. Por exemplo, você pode limitar um parâmetro de tipo a tipos que implementam a interface genérica <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType>, para garantir que instâncias do tipo possam ser classificadas. Você também pode restringir parâmetros de tipo a tipos que tenham uma determinada classe base, que tenham um construtor sem parâmetros ou que sejam tipos de referência ou tipos de valor. Os usuários do tipo genérico não podem substituir argumentos de tipo que não satisfaçam as restrições.  
  
- A *definição de método genérico* é um método com duas listas de parâmetros: uma lista de parâmetros de tipo genérico e uma lista de parâmetros formais. Os parâmetros de tipo podem aparecer como o tipo de retorno ou como os tipos de parâmetros formais, como mostra o código a seguir.  
  
 [!code-cpp[Conceptual.Generics.Overview#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source.cpp#4)]
 [!code-csharp[Conceptual.Generics.Overview#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source.cs#4)]
 [!code-vb[Conceptual.Generics.Overview#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source.vb#4)]  
  
 Métodos genéricos podem aparecer em tipos genéricos ou não genéricos. É importante observar que um método não é genérico apenas porque ele pertence a um tipo genérico ou até mesmo porque possui parâmetros formais cujos tipos sejam os parâmetros genéricos do tipo de delimitador. Um método é genérico somente se ele tiver sua própria lista de parâmetros de tipo. No código a seguir, somente o método `G` é genérico.  
  
 [!code-cpp[Conceptual.Generics.Overview#5](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source.cpp#5)]
 [!code-csharp[Conceptual.Generics.Overview#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source.cs#5)]
 [!code-vb[Conceptual.Generics.Overview#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source.vb#5)]  
  
## <a name="advantages-and-disadvantages-of-generics"></a>Vantagens e desvantagens de genéricos
 Há muitas vantagens no uso de coleções e delegados genéricos:  
  
- Segurança de tipo. Genéricos deslocam a carga de segurança de tipos do seu local até o compilador. Não é necessário escrever código para testar o tipo de dados correto, pois isso é aplicado no tempo de compilação. A necessidade de conversão de tipos e a possibilidade de erros de tempo de execução são reduzidas.  
  
- Menos código e código que seja reutilizado com mais facilidade. Não é necessário herdar de um tipo base e substituir membros. Por exemplo, o <xref:System.Collections.Generic.LinkedList%601> está pronto para uso imediato. Por exemplo, você pode criar uma lista vinculada de cadeias de caracteres com a seguinte declaração de variável:  
  
     [!code-cpp[HowToGeneric#24](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/source2.cpp#24)]
     [!code-csharp[HowToGeneric#24](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/source2.cs#24)]
     [!code-vb[HowToGeneric#24](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/source2.vb#24)]  
  
- Melhor desempenho. Tipos de coleções genéricas geralmente executam melhor para armazenar e manipular tipos de valor, pois não é necessário colocar os tipos de valor em caixa.  
  
- Delegados genéricos permitem retornos fortemente tipados sem a necessidade de criar múltiplas classes de delegados. Por exemplo, o delegado genérico <xref:System.Predicate%601> permite criar um método que implementa seus próprios critérios de pesquisa para um tipo específico e usar seu método com método do tipo <xref:System.Array>, tais como <xref:System.Array.Find%2A>, <xref:System.Array.FindLast%2A> e <xref:System.Array.FindAll%2A>.  
  
- Genéricos simplificam o código gerado dinamicamente. Quando você usa genéricos com código gerado dinamicamente, não é necessário gerar o tipo. Isso aumenta o número de cenários nos quais você pode usar métodos dinâmicos leves, em vez de gerar assemblies inteiros. Para obter mais informações, confira [Como definir e executar métodos dinâmicos](../../framework/reflection-and-codedom/how-to-define-and-execute-dynamic-methods.md) e <xref:System.Reflection.Emit.DynamicMethod>.  
  
 Veja a seguir algumas limitações de genéricos:  
  
- Os tipos genéricos podem ser derivados da maioria das classes base, tais como <xref:System.MarshalByRefObject> (e restrições podem ser usadas para exigir que parâmetros do tipo genérico derivem de classes base como <xref:System.MarshalByRefObject>). No entanto, o .NET Framework não dá suporte para tipos genéricos vinculados a um contexto. Um tipo genérico pode ser derivado de <xref:System.ContextBoundObject>, mas tentar criar uma instância desse tipo causa uma <xref:System.TypeLoadException>.  
  
- Enumerações não podem ter parâmetros do tipo genérico. Uma enumeração pode ser genérica somente incidentalmente (por exemplo, porque ela está aninhada em um tipo genérico que é definido usando Visual Basic, C# ou C++). Para saber mais, confira "Enumerações" em [Common Type System](../base-types/common-type-system.md).  
  
- Métodos dinâmicos leves não podem ser genéricos.  
  
- No Visual Basic, C# e C++, um tipo aninhado que está embutido em um tipo genérico não pode ser instanciado, a menos que os tipos tenham sido atribuídos aos parâmetros de tipo de todos os tipos de delimitadores. Outra maneira de dizer isso é que em reflexão, um tipo aninhado que é definido usando essas linguagens inclui os parâmetros de tipo de todos os seus tipos de delimitadores. Isso permite que os parâmetros de tipos de delimitadores sejam usados nas definições de membro de um tipo aninhado. Para obter mais informações, consulte "Tipos aninhados" em <xref:System.Type.MakeGenericType%2A>.  
  
    > [!NOTE]
    > Um tipo aninhado que é definido pela emissão do código em um assembly dinâmico ou usando o [Ilasm.exe (IL Assembler)](../../framework/tools/ilasm-exe-il-assembler.md) não é necessário para incluir parâmetros de tipo de seus tipos de delimitadores; no entanto, se ele não os incluir, os parâmetros de tipo não estarão no escopo na classe aninhada.  
  
     Para obter mais informações, consulte "Tipos aninhados" em <xref:System.Type.MakeGenericType%2A>.  

## <a name="class-library-and-language-support"></a>Biblioteca de classes e suporte ao idioma  
 O .NET fornece várias classes de coleção genérica nos seguintes namespaces:  
  
- O namespace <xref:System.Collections.Generic> contém a maioria dos tipos de coleção genérica fornecida pelo .NET, tal como as classes genéricas <xref:System.Collections.Generic.List%601> e <xref:System.Collections.Generic.Dictionary%602>.  
  
- O namespace <xref:System.Collections.ObjectModel> contém tipos adicionais de coleção genérica, tais como a classe genérica <xref:System.Collections.ObjectModel.ReadOnlyCollection%601>, que são úteis para expor modelos de objeto para usuários de suas classes.  
  
 Interfaces genéricas para a implementação de comparações de classificação e de igualdade são fornecidas no namespace <xref:System>, juntamente com tipos de delegados genéricos para manipuladores de eventos, conversões e predicados de pesquisa.  
  
 Foi adicionado suporte para genéricos para o namespace <xref:System.Reflection> para examinar tipos genéricos e métodos genéricos, para <xref:System.Reflection.Emit> para emitir assemblies dinâmicos que contenham tipos e métodos genéricos e para <xref:System.CodeDom> para gerar gráficos de origem que incluem genéricos.  
  
 O Common Language Runtime fornece novos opcodes e prefixos para oferecer suporte a tipos genéricos na Microsoft Intermediate Language (MSIL), incluindo <xref:System.Reflection.Emit.OpCodes.Stelem>, <xref:System.Reflection.Emit.OpCodes.Ldelem>, <xref:System.Reflection.Emit.OpCodes.Unbox_Any>, <xref:System.Reflection.Emit.OpCodes.Constrained> e <xref:System.Reflection.Emit.OpCodes.Readonly>.  
  
 Visual C++, C# e Visual Basic todos oferecem suporte completo para definir e usar genéricos. Para saber mais sobre suporte à linguagem, confira [Tipos genéricos no Visual Basic](../../visual-basic/programming-guide/language-features/data-types/generic-types.md), [Introdução aos genéricos](../../csharp/programming-guide/generics/index.md) e [Visão geral dos genéricos no Visual C++](/cpp/windows/overview-of-generics-in-visual-cpp).

## <a name="nested-types-and-generics"></a>Tipos e genéricos aninhados  
 Um tipo que é aninhado em um tipo genérico pode depender dos parâmetros de tipo do tipo genérico de delimitador. O Common Language Runtime considera tipos aninhados como genéricos, mesmo que eles não tenham seus próprios parâmetros de tipo genérico. Quando você cria uma instância de um tipo aninhado, deverá especificar argumentos de tipo para todos os tipos de delimitadores.  

## <a name="related-topics"></a>Tópicos relacionados  
  
|Title|Descrição|  
|-----------|-----------------|  
|[Coleções genéricas no .NET](collections.md)|Descreve as classes de coleção genérica e outros tipos genéricos no .NET.|  
|[Delegados genéricos para manipular matrizes e listas](delegates-for-manipulating-arrays-and-lists.md)|Descreve delegados genéricos para conversões, predicados de pesquisa e ações a serem tomadas nos elementos de uma matriz ou coleção.|  
|[Interfaces genéricas](interfaces.md)|Descreve interfaces genéricas que fornecem funcionalidade comum entre famílias de tipos genéricos.|  
|[Covariância e contravariância](covariance-and-contravariance.md)|Descreve covariância e contravariância em parâmetros de tipo genérico.|  
|[Tipos de coleção comumente usados](../collections/commonly-used-collection-types.md)|Fornece informações de resumo sobre as características e os cenários de uso dos tipos de coleção no .NET, incluindo tipos genéricos.|  
|[Quando usar coleções genéricas](../collections/when-to-use-generic-collections.md)|Descreve regras gerais para determinar quando usar tipos de coleção genérica.|  
|[Como: Definir um tipo genérico com a emissão de reflexão](../../framework/reflection-and-codedom/how-to-define-a-generic-type-with-reflection-emit.md)|Explica como gerar assemblies dinâmicos que incluem tipos e métodos genéricos.|  
|[Tipos genéricos no Visual Basic](../../visual-basic/programming-guide/language-features/data-types/generic-types.md)|Descreve o recurso genérico para usuários do Visual Basic, incluindo tópicos de instruções para uso e definição de tipos genéricos.|  
|[Introdução aos genéricos](../../csharp/programming-guide/generics/index.md)|Fornece uma visão geral da definição e do uso de tipos genéricos para usuários do C#.|  
|[Visão geral dos genéricos no Visual C++](/cpp/windows/overview-of-generics-in-visual-cpp)|Descreve o recurso de genéricos para usuários do C++, incluindo as diferenças entre genéricos e modelos.|  

## <a name="reference"></a>Referência  
 <xref:System.Collections.Generic>  
  
 <xref:System.Collections.ObjectModel>  
  
 <xref:System.Reflection.Emit.OpCodes?displayProperty=nameWithType>  
