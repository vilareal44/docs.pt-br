---
title: Executando operações de cadeia de caracteres que não levam em conta a cultura em coleções
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CaseInsensitiveComparer class, using
- CollectionsUtil.CreateCaseInsensitiveHashtable method
- culture-insensitive string operations, collections
- collections [.NET Framework], culture-insensitive string operations
- CaseInsensitiveHashCodeProvider class, using
- ArrayList.Sort method
- SortedList class, culture-insensitive string operations
- culture parameter
ms.assetid: 5cdc9396-a64b-4615-a1cd-b605db4c5983
ms.openlocfilehash: 377fa58e052e8f8e35a546c21fe2b4fb00cb103d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288258"
---
# <a name="performing-culture-insensitive-string-operations-in-collections"></a>Executando operações de cadeia de caracteres que não levam em conta a cultura em coleções

Há classes e membros no namespace <xref:System.Collections> que fornecem, por padrão, o comportamento sensível à cultura. Os construtores sem parâmetros para as classes <xref:System.Collections.CaseInsensitiveComparer> e <xref:System.Collections.CaseInsensitiveHashCodeProvider> inicializam uma nova instância usando a propriedade <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType>. Todas as sobrecargas do método <xref:System.Collections.Specialized.CollectionsUtil.CreateCaseInsensitiveHashtable%2A?displayProperty=nameWithType> criam uma nova instância da classe <xref:System.Collections.Hashtable> usando a propriedade `Thread.CurrentCulture` por padrão. As sobrecargas do método <xref:System.Collections.ArrayList.Sort%2A?displayProperty=nameWithType> executam, por padrão, classificações sensíveis à cultura usando `Thread.CurrentCulture`. A classificação e a pesquisa em uma <xref:System.Collections.SortedList> podem ser afetadas por `Thread.CurrentCulture`, quando as cadeias de caracteres são usadas como as chaves. Siga as recomendações de uso fornecidas nesta seção para obter os resultados insensíveis à cultura dessas classes e métodos no namespace `Collections`.

> [!NOTE]
> Passar <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> para um método de comparação realiza uma comparação que não diferencia a cultura. No entanto, não causa uma comparação não linguística, por exemplo, para caminhos de arquivos, chaves do Registro e variáveis de ambiente. Também não oferece suporte a decisões de segurança com base no resultado da comparação. Para obter uma comparação não linguística ou suporte para decisões de segurança com base no resultado, o aplicativo deve usar um método de comparação que aceite um valor <xref:System.StringComparison>. Assim, o aplicativo deve passar <xref:System.StringComparison>.

## <a name="using-the-caseinsensitivecomparer-and-caseinsensitivehashcodeprovider-classes"></a>Usar as classes CaseInsensitiveHashCodeProvider e CaseInsensitiveComparer

Os construtores sem parâmetros `CaseInsensitiveHashCodeProvider` e `CaseInsensitiveComparer` inicializam uma nova instância da classe usando o `Thread.CurrentCulture`, resultando no comportamento sensível à cultura. O exemplo de código a seguir demonstra o construtor para um `Hashtable` que é sensível à cultura, pois usa os construtores sem parâmetros para `CaseInsensitiveHashCodeProvider` e `CaseInsensitiveComparer`.

```vb
internalHashtable = New Hashtable(CaseInsensitiveHashCodeProvider.Default, CaseInsensitiveComparer.Default)
```

```csharp
internalHashtable = new Hashtable(CaseInsensitiveHashCodeProvider.Default, CaseInsensitiveComparer.Default);
```

Se você quiser criar um `Hashtable` insensível à cultura usando as classes `CaseInsensitiveComparer` e `CaseInsensitiveHashCodeProvider`, inicialize novas instâncias dessas classes usando os construtores que aceitam um parâmetro `culture`. Para o parâmetro `culture`, especifique <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>. O exemplo de código a seguir demonstra o construtor para um `Hashtable` insensível à cultura.

```vb
internalHashtable = New Hashtable(New
    CaseInsensitiveHashCodeProvider(CultureInfo.InvariantCulture),
    New CaseInsensitiveComparer(CultureInfo.InvariantCulture))
```

```csharp
internalHashtable = new Hashtable(new CaseInsensitiveHashCodeProvider
    (CultureInfo.InvariantCulture),
    new CaseInsensitiveComparer(CultureInfo.InvariantCulture));
```

## <a name="using-the-collectionsutilcreatecaseinsensitivehashtable-method"></a>Usar o método CollectionsUtil.CreateCaseInsensitiveHashTable

O método `CollectionsUtil.CreateCaseInsensitiveHashTable` é um atalho útil para criar uma nova instância da classe `Hashtable` que não diferencia maiúsculas de minúsculas das cadeias de caractere. No entanto, todas as sobrecargas do método `CollectionsUtil.CreateCaseInsensitiveHashTable` são sensíveis à cultura, pois usam a propriedade `Thread.CurrentCulture`. Não é possível criar um `Hashtable` insensível à cultura usando esse método. Para criar um `Hashtable` insensível à cultura, use o construtor `Hashtable` que aceita um parâmetro `culture`. Para o parâmetro `culture`, especifique `CultureInfo.InvariantCulture`. O exemplo de código a seguir demonstra o construtor para um `Hashtable` insensível à cultura.

```vb
internalHashtable = New Hashtable(New
    CaseInsensitiveHashCodeProvider(CultureInfo.InvariantCulture),
    New CaseInsensitiveComparer(CultureInfo.InvariantCulture))
```

```csharp
internalHashtable = new Hashtable(new CaseInsensitiveHashCodeProvider
    (CultureInfo.InvariantCulture),
    new CaseInsensitiveComparer(CultureInfo.InvariantCulture));
```

<a name="cpconperformingculture-insensitivestringoperationsincollectionsanchor1"></a>

## <a name="using-the-sortedlist-class"></a>Usar a classe SortedList

Um `SortedList` representa uma coleção de pares chave/valor que são classificados pelas chaves e são acessíveis por chave e por índice. Quando você usa um `SortedList` no qual as cadeias de caracteres são as chaves, a classificação e a pesquisa podem ser afetadas pela propriedade `Thread.CurrentCulture`. Para obter o comportamento insensível à cultura de um `SortedList`, crie um `SortedList` usando um dos construtores que aceita um parâmetro `comparer`. O parâmetro `comparer` especifica a implementação de <xref:System.Collections.IComparer> a ser usada ao comparar chaves. Para o parâmetro, especifique uma classe de comparação personalizada que usa `CultureInfo.InvariantCulture` para comparar as chaves. O exemplo a seguir ilustra uma classe de comparação personalizada insensível à cultura que você pode especificar como o parâmetro `comparer` para um construtor `SortedList`.

```vb
Imports System.Collections
Imports System.Globalization

Friend Class InvariantComparer
    Implements IComparer
    Private m_compareInfo As CompareInfo
    Friend Shared [Default] As New InvariantComparer()

    Friend Sub New()
        m_compareInfo = CultureInfo.InvariantCulture.CompareInfo
    End Sub

    Public Function Compare(a As Object, b As Object) As Integer _
            Implements IComparer.Compare
        Dim sa As String = CType(a, String)
        Dim sb As String = CType(b, String)
        If Not (sa Is Nothing) And Not (sb Is Nothing) Then
            Return m_compareInfo.Compare(sa, sb)
        Else
            Return Comparer.Default.Compare(a, b)
        End If
    End Function
End Class
```

```csharp
using System;
using System.Collections;
using System.Globalization;

internal class InvariantComparer : IComparer
{
    private CompareInfo m_compareInfo;
    internal static readonly InvariantComparer Default = new
        InvariantComparer();

    internal InvariantComparer()
    {
        m_compareInfo = CultureInfo.InvariantCulture.CompareInfo;
    }

    public int Compare(Object a, Object b)
    {
        String sa = a as String;
        String sb = b as String;
        if (sa != null && sb != null)
            return m_compareInfo.Compare(sa, sb);
        else
            return Comparer.Default.Compare(a,b);
    }
}
```

Em geral, se você usar `SortedList` em cadeias de caracteres sem especificar um comparador personalizado invariável, uma alteração em `Thread.CurrentCulture` após o preenchimento da lista poderá invalidar a lista.

## <a name="using-the-arraylistsort-method"></a>Usar o método ArrayList.Sort

Sobrecargas do método `ArrayList.Sort` executam, por padrão, classificações sensíveis à cultura usando a propriedade `Thread.CurrentCulture`. Os resultados podem variar de acordo com a cultura, devido a ordens de classificação diferentes. Para eliminar o comportamento sensível à cultura, use as sobrecargas desse método para aceitar uma implementação `IComparer`. Para o parâmetro `comparer`, especifique uma classe de comparação invariável personalizada que usa `CultureInfo.InvariantCulture`. Um exemplo de uma classe de comparação invariável personalizada é fornecido no tópico [Usar a classe SortedList](#cpconperformingculture-insensitivestringoperationsincollectionsanchor1).

## <a name="see-also"></a>Veja também

- <xref:System.Collections.CaseInsensitiveComparer>
- <xref:System.Collections.CaseInsensitiveHashCodeProvider>
- <xref:System.Collections.ArrayList.Sort%2A?displayProperty=nameWithType>
- <xref:System.Collections.SortedList>
- <xref:System.Collections.Hashtable>
- <xref:System.Collections.IComparer>
- [Executando operações de cadeia de caracteres que não levam em conta a cultura](performing-culture-insensitive-string-operations.md)
- <xref:System.Collections.Specialized.CollectionsUtil.CreateCaseInsensitiveHashtable%2A?displayProperty=nameWithType>
