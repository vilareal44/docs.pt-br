---
title: Reflexão e tipos genéricos
description: Introdução aos tipos de reflexão e genérico no .NET. Ao contrário de um tipo comum, um tipo genérico é associado a um conjunto de parâmetros de tipo ou argumentos de tipo.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generics [.NET Framework], reflection emit
- reflection emit, generic types
- reflection, generic types
- type arguments
- generics [.NET Framework], reflection
- viewing type information
- type information, viewing
- types, generic
- type parameters
ms.assetid: f7180fc5-dd41-42d4-8a8e-1b34288e06de
ms.openlocfilehash: dd0dda92dc4473e05c59072973076cbb06bcaa06
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865288"
---
# <a name="reflection-and-generic-types"></a>Reflexão e tipos genéricos
 Do ponto de vista da reflexão, a diferença entre um tipo genérico e um tipo comum é que um tipo genérico tem associado a ele um conjunto de parâmetros de tipo (se for uma definição de tipo genérico) ou argumentos de tipo (se for um tipo construído). Um método genérico é diferente de um método comum da mesma maneira.  
  
 Há duas chaves para entender como a reflexão trata tipos e métodos genéricos:  
  
- Os parâmetros de tipo de definições de tipo genérico e definições de método genérico são representadas por instâncias da classe <xref:System.Type>.  
  
    > [!NOTE]
    > Muitas propriedades e métodos de <xref:System.Type> têm um comportamento diferente quando um objeto <xref:System.Type> representa um parâmetro de tipo genérico. Essas diferenças são documentadas nos tópicos de propriedade e método. Por exemplo, veja <xref:System.Type.IsAutoClass%2A> e <xref:System.Type.DeclaringType%2A>. Além disso, alguns membros são válidos somente quando um objeto <xref:System.Type> representa um parâmetro de tipo genérico. Por exemplo, veja <xref:System.Type.GetGenericTypeDefinition%2A>.  
  
- Se uma instância do <xref:System.Type> representa um tipo genérico, ela inclui uma matriz de tipos que representam os parâmetros de tipo (para definições de tipo genérico) ou os argumentos de tipo (para tipos construídos). O mesmo é verdadeiro para uma instância da classe <xref:System.Reflection.MethodInfo> que representa um método genérico.  
  
 A reflexão fornece métodos de <xref:System.Type> e <xref:System.Reflection.MethodInfo> que permitem a você acessar a matriz de parâmetros de tipo e determinar se uma instância de <xref:System.Type> representa um parâmetro de tipo ou um tipo real.  
  
 Para obter o código de exemplo que demonstra os métodos discutidos aqui, consulte [Como examinar tipos genéricos e criar instâncias deles com a reflexão](how-to-examine-and-instantiate-generic-types-with-reflection.md).  
  
 A discussão a seguir pressupõe familiaridade com a terminologia de genéricos, como a diferença entre os parâmetros e argumentos de tipo e tipos construídos abertos ou fechados. Para obter mais informações, consulte [Genéricos](../../standard/generics/index.md).  

## <a name="is-this-a-generic-type-or-method"></a>Este é um tipo ou um método genérico?  
 Quando usar a reflexão para examinar um tipo desconhecido, representado por uma instância de <xref:System.Type>, use a propriedade <xref:System.Type.IsGenericType%2A> para determinar se o tipo desconhecido é genérico. Ele retornará `true` se o tipo for genérico. Da mesma forma, quando você examinar um método desconhecido, representado por uma instância da classe <xref:System.Reflection.MethodInfo>, use a propriedade <xref:System.Reflection.MethodBase.IsGenericMethod%2A> para determinar se o método é genérico.  
  
### <a name="is-this-a-generic-type-or-method-definition"></a>Esta é uma definição de tipo ou método genérico?  
 Use a propriedade <xref:System.Type.IsGenericTypeDefinition%2A> para determinar se um objeto <xref:System.Type> representa uma definição de tipo genérico e use o método <xref:System.Reflection.MethodBase.IsGenericMethodDefinition%2A> para determinar se um <xref:System.Reflection.MethodInfo> representa uma definição de método genérico.  
  
 As definições de tipo e método genéricos são os modelos dos quais os tipos instanciáveis são criados. Os tipos genéricos na biblioteca de classes do .NET Framework, como <xref:System.Collections.Generic.Dictionary%602>, são definições de tipo genérico.  
  
### <a name="is-the-type-or-method-open-or-closed"></a>O tipo ou o método é aberto ou fechado?  
 Um tipo ou método genérico será fechado se os tipos instanciáveis tiverem sido substituídos para todos os seus parâmetros de tipo, incluindo todos os parâmetros de tipo de todos os tipos delimitadores. Você poderá criar uma instância de um tipo genérico somente se ele for fechado. A propriedade <xref:System.Type.ContainsGenericParameters%2A?displayProperty=nameWithType> retornará `true` se um tipo for aberto. Para métodos, o método <xref:System.Reflection.MethodBase.ContainsGenericParameters%2A?displayProperty=nameWithType> executa a mesma função.

## <a name="generating-closed-generic-types"></a>Gerando tipos genéricos fechados  
 Quando você tiver uma definição de método ou tipo genérico, use o método <xref:System.Type.MakeGenericType%2A> para criar um tipo genérico fechado ou o método <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A> para criar um <xref:System.Reflection.MethodInfo> para um método genérico fechado.  
  
### <a name="getting-the-generic-type-or-method-definition"></a>Obtendo a definição do tipo ou método genérico  
 Se você tiver um método ou tipo genérico aberto que não seja uma definição de método ou tipo genérico, não poderá criar instâncias dele e não poderá fornecer os parâmetros de tipo que estão faltando. Você deve ter uma definição de tipo ou método genérico. Use o método <xref:System.Type.GetGenericTypeDefinition%2A> para obter a definição de tipo genérico ou o método <xref:System.Reflection.MethodInfo.GetGenericMethodDefinition%2A> para obter a definição de método genérico.  
  
 Por exemplo, se você tiver um objeto <xref:System.Type> representando `Dictionary<int, string>` (`Dictionary(Of Integer, String)` no Visual Basic) e desejar criar o tipo `Dictionary<string, MyClass>`, poderá usar o método <xref:System.Type.GetGenericTypeDefinition%2A> para obter um <xref:System.Type> representando `Dictionary<TKey, TValue>` e, em seguida, usar o método <xref:System.Type.MakeGenericType%2A> para produzir um <xref:System.Type> representando `Dictionary<int, MyClass>`.  
  
 Para obter um exemplo de um tipo genérico aberto que não é um tipo genérico, consulte "Parâmetro de tipo ou argumento de tipo" posteriormente neste tópico.

## <a name="examining-type-arguments-and-type-parameters"></a>Examinando os parâmetros de tipo e os argumentos de tipo  
 Use o método <xref:System.Type.GetGenericArguments%2A?displayProperty=nameWithType> para obter uma matriz de objetos <xref:System.Type> que representam os parâmetros de tipo ou os argumentos de tipo de um tipo genérico e use o método <xref:System.Reflection.MethodInfo.GetGenericArguments%2A?displayProperty=nameWithType> para fazer o mesmo para um método genérico.  
  
 Uma vez que você souber que um objeto <xref:System.Type> representa um parâmetro de tipo, há muitas perguntas adicionais que a reflexão pode responder. Você pode determinar a origem do parâmetro de tipo, sua posição e suas restrições.  
  
### <a name="type-parameter-or-type-argument"></a>Parâmetro de tipo ou argumento de tipo  
 Para determinar se um elemento específico da matriz é um parâmetro de tipo ou um argumento de tipo, use a propriedade <xref:System.Type.IsGenericParameter%2A>. A propriedade <xref:System.Type.IsGenericParameter%2A> será `true` se o elemento for um parâmetro de tipo.  
  
 Um tipo genérico pode ser aberto sem ser uma definição de tipo genérico, nesse caso, ele tem uma mistura de argumentos de tipo e parâmetros de tipo. Por exemplo, no código a seguir, a classe `D` deriva de um tipo criado substituindo o primeiro parâmetro de tipo de `D` pelo segundo parâmetro de tipo de `B`.  
  
```csharp  
class B<T, U> {}  
class D<V, W> : B<int, V> {}  
```  
  
```vb  
Class B(Of T, U)  
End Class  
Class D(Of V, W)  
    Inherits B(Of Integer, V)  
End Class  
```  
  
```cpp  
generic<typename T, typename U> ref class B {};  
generic<typename V, typename W> ref class D : B<int, V> {};  
```  
  
 Se você obtiver um objeto <xref:System.Type> representando `D<V, W>` e usar a propriedade <xref:System.Type.BaseType%2A> para obter seu tipo base, o `type B<int, V>` resultante será aberto, mas não será uma definição de tipo genérico.  
  
### <a name="source-of-a-generic-parameter"></a>Origem de um parâmetro genérico  
 Um parâmetro de tipo genérico pode vir do tipo sendo examinado, de um tipo delimitador ou de um método genérico. Você pode determinar a origem do parâmetro de tipo genérico da seguinte maneira:  
  
- Primeiro, use a propriedade <xref:System.Type.DeclaringMethod%2A> para determinar se o parâmetro de tipo é proveniente de um método genérico. Se o valor da propriedade não for uma referência nula (`Nothing` no Visual Basic), a origem será um método genérico.  
  
- Se a origem não for um método genérico, use a propriedade <xref:System.Type.DeclaringType%2A> para determinar o tipo genérico ao qual o parâmetro de tipo genérico pertence.  
  
 Se o parâmetro de tipo pertencer a um método genérico, a propriedade <xref:System.Type.DeclaringType%2A> retornará o tipo que declarou o método genérico, o que é irrelevante.  
  
### <a name="position-of-a-generic-parameter"></a>Posição de um parâmetro genérico  
 Em raras situações, é necessário determinar a posição de um parâmetro de tipo na lista de parâmetros de tipo de sua classe declarante. Por exemplo, suponha que você tenha um objeto <xref:System.Type> representando o tipo `B<int, V>` do exemplo anterior. O método <xref:System.Type.GetGenericArguments%2A> fornece uma lista de argumentos de tipo e ao examinar `V` você pode usar as propriedades <xref:System.Type.DeclaringMethod%2A> e <xref:System.Type.DeclaringType%2A> para descobrir de onde ele vem. Você pode usar a propriedade <xref:System.Type.GenericParameterPosition%2A> para determinar sua posição na lista de parâmetros de tipo em que foi definido. Neste exemplo, `V` está na posição 0 (zero) na lista de parâmetros de tipo em que foi definido.  
  
### <a name="base-type-and-interface-constraints"></a>Tipo base e restrições de interface  
 Use o método <xref:System.Type.GetGenericParameterConstraints%2A> para obter a restrição do tipo base e as restrições de interface de um parâmetro de tipo. A ordem dos elementos da matriz não é significativa. Um elemento representará uma restrição de interface se ele for um tipo de interface.  
  
### <a name="generic-parameter-attributes"></a>Atributos de parâmetro genérico  
 A propriedade <xref:System.Type.GenericParameterAttributes%2A> obtém um valor <xref:System.Reflection.GenericParameterAttributes> que indica a variância (covariância ou contravariância) e as restrições especiais de um parâmetro de tipo.  
  
#### <a name="covariance-and-contravariance"></a>Covariância e contravariância  
 Para determinar se um parâmetro de tipo é covariante ou contravariante, aplique a máscara <xref:System.Reflection.GenericParameterAttributes.VarianceMask?displayProperty=nameWithType> ao valor <xref:System.Reflection.GenericParameterAttributes> retornado pela propriedade <xref:System.Type.GenericParameterAttributes%2A>. Se o resultado for <xref:System.Reflection.GenericParameterAttributes.None?displayProperty=nameWithType>, o parâmetro de tipo será invariável. Consulte [Covariância e contravariância](../../standard/generics/covariance-and-contravariance.md).  
  
#### <a name="special-constraints"></a>Restrições especiais  
 Para determinar as restrições especiais de um parâmetro de tipo, aplique a máscara <xref:System.Reflection.GenericParameterAttributes.SpecialConstraintMask?displayProperty=nameWithType> ao valor <xref:System.Reflection.GenericParameterAttributes> retornado pela propriedade <xref:System.Type.GenericParameterAttributes%2A>. Se o resultado for <xref:System.Reflection.GenericParameterAttributes.None?displayProperty=nameWithType>, não haverá nenhuma restrição especial. Um parâmetro de tipo pode ser restringido para ser um tipo de referência, para ser um tipo de valor não nulo e para ter um construtor sem parâmetros.

## <a name="invariants"></a>Invariáveis  
 Para obter uma tabela de condições invariáveis para termos comuns na reflexão de tipos genéricos, consulte <xref:System.Type.IsGenericType%2A?displayProperty=nameWithType>. Para encontrar termos adicionais relacionados aos métodos genéricos, consulte <xref:System.Reflection.MethodBase.IsGenericMethod%2A?displayProperty=nameWithType>.  

## <a name="related-topics"></a>Tópicos Relacionados  
  
|Título|Descrição|  
|-----------|-----------------|  
|[Como: Examinar tipos genéricos e criar instâncias deles com a reflexão](how-to-examine-and-instantiate-generic-types-with-reflection.md)|Mostra como usar as propriedades e os métodos de <xref:System.Type> e <xref:System.Reflection.MethodInfo> para examinar tipos genéricos.|  
|[Genéricos](../../standard/generics/index.md)|Descreve o recurso de genéricos e como ele tem suporte no .NET Framework.|  
|[Como: Definir um tipo genérico com a emissão de reflexão](how-to-define-a-generic-type-with-reflection-emit.md)|Mostra como usar a emissão de reflexão para gerar tipos genéricos nos assemblies dinâmicos.|  
|[Exibindo informações de tipo](viewing-type-information.md)|Descreve a classe <xref:System.Type> e fornece exemplos de código que ilustram como usar <xref:System.Type> com várias classes de reflexão para obter informações sobre construtores, métodos, campos, propriedades e eventos.|
