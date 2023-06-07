---
title: Variância em interfaces genéricas (C#)
description: Exiba informações de suporte de variação para interfaces genéricas, incluindo informações atualizadas para interfaces existentes no .NET Framework 4 e 4,5.
ms.date: 06/06/2019
ms.assetid: 4828a8f9-48c0-4128-9749-7fcd6bf19a06
ms.openlocfilehash: 91218742c01eeb6e65ea26d9dc41ed7c98827377
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105628"
---
# <a name="variance-in-generic-interfaces-c"></a>Variância em interfaces genéricas (C#)

O .NET Framework 4 introduziu o suporte à variação para diversas interfaces genéricas existentes. O suporte à variação possibilita a conversão implícita de classes que implementam essas interfaces.

A partir do .NET Framework 4, as seguintes interfaces são variantes:

- <xref:System.Collections.Generic.IEnumerable%601> (T é covariante)

- <xref:System.Collections.Generic.IEnumerator%601> (T é covariante)

- <xref:System.Linq.IQueryable%601> (T é covariante)

- <xref:System.Linq.IGrouping%602> (`TKey` e `TElement` são covariantes)

- <xref:System.Collections.Generic.IComparer%601> (T é contravariante)

- <xref:System.Collections.Generic.IEqualityComparer%601> (T é contravariante)

- <xref:System.IComparable%601> (T é contravariante)

A partir do .NET Framework 4.5, as seguintes interfaces são variantes:

- <xref:System.Collections.Generic.IReadOnlyList%601> (T é covariante)

- <xref:System.Collections.Generic.IReadOnlyCollection%601> (T é covariante)

A covariância permite que um método tenha um tipo de retorno mais derivados que aquele definidos pelo parâmetro de tipo genérico da interface. Para ilustrar o recurso de covariância, considere estas interfaces genéricas: `IEnumerable<Object>` e `IEnumerable<String>`. A interface `IEnumerable<String>` não herda a interface `IEnumerable<Object>`. No entanto, o tipo `String` herda o tipo `Object` e, em alguns casos, talvez você queira atribuir objetos dessas interfaces uns aos outros. Isso é mostrado no exemplo de código a seguir.

```csharp
IEnumerable<String> strings = new List<String>();
IEnumerable<Object> objects = strings;
```

Em versões anteriores do .NET Framework, esse código causa um erro de compilação em C# e, se `Option Strict` estiver ativado, em Visual Basic. Mas agora você pode usar `strings` em vez de `objects`, conforme mostrado no exemplo anterior, porque a interface <xref:System.Collections.Generic.IEnumerable%601> é covariante.

A contravariância permite que um método tenha tipos de argumentos menos derivados que aquele especificado pelo parâmetro genérico da interface. Para ilustrar a contravariância, suponha que você tenha criado uma classe `BaseComparer` para comparar instâncias da classe `BaseClass`. A classe `BaseComparer` implementa a interface `IEqualityComparer<BaseClass>`. Como a interface <xref:System.Collections.Generic.IEqualityComparer%601> agora é contravariante, você pode usar `BaseComparer` para comparar instâncias de classes que herdam a classe `BaseClass`. Isso é mostrado no exemplo de código a seguir.

```csharp
// Simple hierarchy of classes.
class BaseClass { }
class DerivedClass : BaseClass { }

// Comparer class.
class BaseComparer : IEqualityComparer<BaseClass>
{
    public int GetHashCode(BaseClass baseInstance)
    {
        return baseInstance.GetHashCode();
    }
    public bool Equals(BaseClass x, BaseClass y)
    {
        return x == y;
    }
}
class Program
{
    static void Test()
    {
        IEqualityComparer<BaseClass> baseComparer = new BaseComparer();

        // Implicit conversion of IEqualityComparer<BaseClass> to
        // IEqualityComparer<DerivedClass>.
        IEqualityComparer<DerivedClass> childComparer = baseComparer;
    }
}
```

Para ver mais exemplos, consulte [Usando variação em interfaces para coleções genéricas (C#)](./using-variance-in-interfaces-for-generic-collections.md).

A variação em interfaces genéricas tem suporte somente para tipos de referência. Tipos de valor não dão suporte à variação. Por exemplo, `IEnumerable<int>` não pode ser convertido implicitamente em `IEnumerable<object>`, porque inteiros são representados por um tipo de valor.

```csharp
IEnumerable<int> integers = new List<int>();
// The following statement generates a compiler error,
// because int is a value type.
// IEnumerable<Object> objects = integers;
```

Também é importante lembrar que as classes que implementam interfaces variantes ainda são invariantes. Por exemplo, embora <xref:System.Collections.Generic.List%601> implemente a interface covariante <xref:System.Collections.Generic.IEnumerable%601>, você não pode converter implicitamente `List<String>` para `List<Object>`. Isso é ilustrado no exemplo de código a seguir.

```csharp
// The following line generates a compiler error
// because classes are invariant.
// List<Object> list = new List<String>();

// You can use the interface object instead.
IEnumerable<Object> listObjects = new List<String>();
```

## <a name="see-also"></a>Confira também

- [Usando variação em interfaces para Coleções Genéricas (C#)](./using-variance-in-interfaces-for-generic-collections.md)
- [Criando interfaces genéricas variáveis (C#)](./creating-variant-generic-interfaces.md)
- [Interfaces genéricas](../../../../standard/generics/interfaces.md)
- [Variação em delegados (C#)](./variance-in-delegates.md)
