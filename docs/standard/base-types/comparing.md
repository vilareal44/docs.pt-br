---
title: Comparando cadeias de caracteres em .NET
description: Leia sobre métodos para comparar cadeias de caracteres no .NET. Saiba mais sobre os métodos Compare, CompareOrdinal, CompareTo, StartsWith, EndsWith, Equals, IndexOf, & LastIndexOf.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- value comparisons of strings
- LastIndexOf method
- CompareTo method
- IndexOf method
- Compare method
- strings [.NET Framework], comparing
- CompareOrdinal method
- EndsWith method
- Equals method
- StartsWith method
ms.assetid: 977dc094-fe19-4955-98ec-d2294d04a4ba
ms.openlocfilehash: 5ed73d18341c3b9c6e61e12fdf322b9a67affd4a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602187"
---
# <a name="comparing-strings-in-net"></a>Comparando cadeias de caracteres em .NET
O .NET fornece vários métodos para comparar os valores de cadeias de caracteres. A tabela a seguir lista e descreve os métodos de comparação de valores.  
  
|Nome do método|Uso|  
|-----------------|---------|  
|<xref:System.String.Compare%2A?displayProperty=nameWithType>|Compara os valores das duas cadeias de caracteres. Retorna um valor inteiro.|  
|<xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType>|Compara duas cadeias de caracteres sem considerar a cultura local. Retorna um valor inteiro.|  
|<xref:System.String.CompareTo%2A?displayProperty=nameWithType>|Compara o objeto atual de cadeia de caracteres a outra cadeia de caracteres. Retorna um valor inteiro.|  
|<xref:System.String.StartsWith%2A?displayProperty=nameWithType>|Determina se uma cadeia de caracteres começa com a cadeia de caracteres passada. Retorna um valor booliano.|  
|<xref:System.String.EndsWith%2A?displayProperty=nameWithType>|Determina se uma cadeia de caracteres termina com a cadeia de caracteres passada. Retorna um valor booliano.|  
|<xref:System.String.Equals%2A?displayProperty=nameWithType>|Determina se duas cadeias de caracteres são iguais. Retorna um valor booliano.|  
|<xref:System.String.IndexOf%2A?displayProperty=nameWithType>|Retorna a posição do índice de um caractere ou uma cadeia de caracteres, começando do início da cadeia de caracteres que você está examinando. Retorna um valor inteiro.|  
|<xref:System.String.LastIndexOf%2A?displayProperty=nameWithType>|Retorna a posição do índice de um caractere ou uma cadeia de caracteres, começando do fim da cadeia de caracteres que você está examinando. Retorna um valor inteiro.|  
  
## <a name="compare"></a>Comparar  
 O método estático <xref:System.String.Compare%2A?displayProperty=nameWithType> fornece uma maneira completa de comparar duas cadeias de caracteres. Esse método é cultural. Você pode usar essa função para comparar duas cadeias de caracteres ou subcadeias de duas cadeias de caracteres. Além disso, sobrecargas são fornecidas para considerar ou ignorar a variação cultural ou de caso. A tabela a seguir mostra os três valores inteiros que esse método pode retornar.  
  
|Valor retornado|Condição|  
|------------------|---------------|  
|Um inteiro negativo|A primeira cadeia de caracteres precede a segunda cadeia de caracteres na ordem de classificação.<br /><br /> -ou-<br /><br /> A primeira cadeia de caracteres é `null`.|  
|0|A primeira cadeia de caracteres e a segunda cadeia de caracteres são iguais.<br /><br /> -ou-<br /><br /> Ambas as cadeias de caracteres são `null`.|  
|Um número inteiro positivo<br /><br /> -ou-<br /><br /> 1|A primeira cadeia de caracteres segue a segunda cadeia de caracteres na ordem de classificação.<br /><br /> -ou-<br /><br /> A segunda cadeia de caracteres é `null`.|  
  
> [!IMPORTANT]
> O método <xref:System.String.Compare%2A?displayProperty=nameWithType> destina-se principalmente para uso em ordenação ou classificação de cadeias de caracteres. Você não deve usar o método <xref:System.String.Compare%2A?displayProperty=nameWithType> para testar a igualdade (ou seja, para procurar explicitamente um valor retornado de 0 sem considerar se uma cadeia de caracteres é menor que ou maior que a outra). Em vez disso, para determinar se duas cadeias de caracteres são iguais, use o método <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType>.  
  
 O exemplo a seguir usa o método <xref:System.String.Compare%2A?displayProperty=nameWithType> para determinar os valores relativos das duas cadeias de caracteres.  
  
 [!code-cpp[Conceptual.String.BasicOps#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#6)]
 [!code-csharp[Conceptual.String.BasicOps#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#6)]
 [!code-vb[Conceptual.String.BasicOps#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#6)]  
  
 Este exemplo exibe `-1` no console.  
  
 O exemplo anterior diferencia a cultura por padrão. Para realizar uma comparação de cadeia de caracteres que não diferencia a cultura, use uma sobrecarga do método <xref:System.String.Compare%2A?displayProperty=nameWithType> que permite que você especifique a cultura a ser usada, fornecendo um parâmetro *culture*. Confira um exemplo que demonstra como usar o método <xref:System.String.Compare%2A?displayProperty=nameWithType> para realizar uma comparação de cadeia de caracteres que não diferenciam a cultura em [Executando comparações de cadeias de caracteres que não diferenciam a cultura](../globalization-localization/performing-culture-insensitive-string-comparisons.md).  
  
## <a name="compareordinal"></a>CompareOrdinal  
 O método <xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> compara dois objetos de cadeia de caracteres sem considerar a cultura local. Os valores de retorno desse método são idênticos aos valores retornados pelo método **Compare** na tabela anterior.  
  
> [!IMPORTANT]
> O método <xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> destina-se principalmente para uso em ordenação ou classificação de cadeias de caracteres. Você não deve usar o método <xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> para testar a igualdade (ou seja, para procurar explicitamente um valor retornado de 0 sem considerar se uma cadeia de caracteres é menor que ou maior que a outra). Em vez disso, para determinar se duas cadeias de caracteres são iguais, use o método <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType>.  
  
 O exemplo a seguir usa o método **CompareOrdinal** para comparar os valores de duas cadeias de caracteres.  
  
 [!code-cpp[Conceptual.String.BasicOps#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#7)]
 [!code-csharp[Conceptual.String.BasicOps#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#7)]
 [!code-vb[Conceptual.String.BasicOps#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#7)]  
  
 Este exemplo exibe `-32` no console.  
  
## <a name="compareto"></a>CompareTo  
 O método <xref:System.String.CompareTo%2A?displayProperty=nameWithType> compara a cadeia de caracteres que o objeto atual de cadeia de caracteres encapsula com outro objeto ou cadeia de caracteres. Os valores de retorno desse método são idênticos aos valores retornados pelo método <xref:System.String.Compare%2A?displayProperty=nameWithType> na tabela anterior.  
  
> [!IMPORTANT]
> O método <xref:System.String.CompareTo%2A?displayProperty=nameWithType> destina-se principalmente para uso em ordenação ou classificação de cadeias de caracteres. Você não deve usar o método <xref:System.String.CompareTo%2A?displayProperty=nameWithType> para testar a igualdade (ou seja, para procurar explicitamente um valor retornado de 0 sem considerar se uma cadeia de caracteres é menor que ou maior que a outra). Em vez disso, para determinar se duas cadeias de caracteres são iguais, use o método <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType>.  
  
 O exemplo a seguir usa o método <xref:System.String.CompareTo%2A?displayProperty=nameWithType> para comparar o objeto `string1` ao objeto `string2`.  
  
 [!code-cpp[Conceptual.String.BasicOps#8](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#8)]
 [!code-csharp[Conceptual.String.BasicOps#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#8)]
 [!code-vb[Conceptual.String.BasicOps#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#8)]  
  
 Este exemplo exibe `-1` no console.  
  
 Todas as sobrecargas do método <xref:System.String.CompareTo%2A?displayProperty=nameWithType> executam comparações que diferenciam a cultura e com diferenciação de maiúsculas e minúsculas por padrão. Nenhuma sobrecarga desse método é fornecida que permite que você execute uma comparação sem diferenciação de cultura. Para ter maior clareza do código, é recomendável usar o método **String.Compare**, especificando <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> para operações que diferenciam a cultura ou <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> para operações que não diferenciam a cultura. Confira exemplos que demonstram como usar o método **String.Compare** para realizar comparações de cadeia de caracteres que diferenciam a cultura e não em [Executando comparações de cadeias de caracteres que não diferenciam a cultura](../globalization-localization/performing-culture-insensitive-string-comparisons.md).  
  
## <a name="equals"></a>É igual a  
 O método **String.Equals** pode facilmente determinar se duas cadeias de caracteres são as mesmas. Esse método que diferencia maiúsculas de minúsculas retorna um valor Booliano **true** ou **false**. Ele pode ser usado de uma classe existente, conforme ilustrado no exemplo a seguir. O exemplo a seguir usa o método **Equals** para determinar se um objeto de cadeia de caracteres contém a frase "Hello World".  
  
 [!code-cpp[Conceptual.String.BasicOps#9](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#9)]
 [!code-csharp[Conceptual.String.BasicOps#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#9)]
 [!code-vb[Conceptual.String.BasicOps#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#9)]  
  
 Este exemplo exibe `True` no console.  
  
 Esse método também pode ser usado como um método estático. O exemplo a seguir compara dois objetos de cadeia de caracteres usando um método estático.  
  
 [!code-cpp[Conceptual.String.BasicOps#10](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#10)]
 [!code-csharp[Conceptual.String.BasicOps#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#10)]
 [!code-vb[Conceptual.String.BasicOps#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#10)]  
  
 Este exemplo exibe `True` no console.  
  
## <a name="startswith-and-endswith"></a>StartsWith e EndsWith  
 Você pode usar o método **String.StartsWith** para determinar se um objeto de cadeia de caracteres começa com os mesmos caracteres que englobam outra cadeia de caracteres. Esse método que diferencia maiúsculas de minúsculas retorna **true** se o objeto atual de cadeia de caracteres começa com a cadeia de caracteres passada e **false** se não existir. O exemplo a seguir usa esse método para determinar se um objeto de cadeia de caracteres começa com "Hello".  
  
 [!code-cpp[Conceptual.String.BasicOps#11](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#11)]
 [!code-csharp[Conceptual.String.BasicOps#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#11)]
 [!code-vb[Conceptual.String.BasicOps#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#11)]  
  
 Este exemplo exibe `True` no console.  
  
 O método **String.EndsWith** compara uma cadeia de caracteres passada com os caracteres que existem no final do objeto de cadeia de caracteres atual. Ele também retorna um valor Booliano. O exemplo a seguir verifica o fim de uma cadeia de caracteres usando o método **EndsWith**.  
  
 [!code-cpp[Conceptual.String.BasicOps#12](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#12)]
 [!code-csharp[Conceptual.String.BasicOps#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#12)]
 [!code-vb[Conceptual.String.BasicOps#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#12)]  
  
 Este exemplo exibe `False` no console.  
  
## <a name="indexof-and-lastindexof"></a>IndexOf e LastIndexOf  
 Você pode usar o método **String.IndexOf** para determinar a posição da primeira ocorrência de um determinado caractere dentro de uma cadeia de caracteres. Esse método que diferencia maiúsculas de minúsculas inicia a contagem do início de uma cadeia de caracteres e retorna a posição de um caractere passado usando um índice baseado em zero. Se o caractere não for encontrado, um valor de -1 será retornado.  
  
 O exemplo a seguir usa o método **IndexOf** para pesquisar a primeira ocorrência do caractere '`l`' em uma cadeia de caracteres.  
  
 [!code-cpp[Conceptual.String.BasicOps#13](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#13)]
 [!code-csharp[Conceptual.String.BasicOps#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#13)]
 [!code-vb[Conceptual.String.BasicOps#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#13)]  
  
 Este exemplo exibe `2` no console.  
  
 O método **String.LastIndexOf** é semelhante ao método **String.IndexOf**, exceto que ele retorna a posição da última ocorrência de um determinado caractere dentro de uma cadeia de caracteres. Ele diferencia maiúsculas de minúsculas e usa um índice baseado em zero.  
  
 O exemplo a seguir usa o método **LastIndexOf** para pesquisar a última ocorrência do caractere '`l`' em uma cadeia de caracteres.  
  
 [!code-cpp[Conceptual.String.BasicOps#14](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#14)]
 [!code-csharp[Conceptual.String.BasicOps#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#14)]
 [!code-vb[Conceptual.String.BasicOps#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#14)]  
  
 Este exemplo exibe `9` no console.  
  
 Os dois métodos são úteis quando usados em conjunto com o método **String.Remove**. Você pode usar tanto o método **IndexOf** quanto o **LastIndexOf** para recuperar a posição de um caractere e, em seguida, fornecer essa posição para o método **Remove** para remover um caractere ou uma palavra que começa com esse caractere.  
  
## <a name="see-also"></a>Consulte também

- [Operações básicas de cadeia de caracteres](basic-string-operations.md)
- [Executando operações de cadeia de caracteres que não levam em conta a cultura](../globalization-localization/performing-culture-insensitive-string-operations.md)
- [Classificação de tabelas de peso (para .NET em Windows)](https://www.microsoft.com/download/details.aspx?id=10921)
- [Tabela do elemento de ordenação Unicode padrão (para .NET Core em Linux e macOS)](https://www.unicode.org/Public/UCA/latest/allkeys.txt)
