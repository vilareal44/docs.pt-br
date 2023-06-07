---
title: Como alterar a capitalização no .NET
description: Saiba como alterar a capitalização de cadeias de caracteres no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- strings [.NET Framework], case
- case sensitivity
- ToUpper method
- ToLower method
- uppercase
- lowercase
ms.assetid: 6805f81b-e9ad-4387-9f4c-b9bdb21b87c0
ms.openlocfilehash: e838d6df778802d7eaab3f12205698cc6ca5f72b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290585"
---
# <a name="change-case-in-net"></a>Alterar maiúsculas e minúsculas no .NET

Se você escrever um aplicativo que aceita entrada de um usuário, nunca poderá ter certeza de qual caso (superior ou inferior) será usado para inserir os dados. Muitas vezes, você quer que as cadeias de caracteres tenham a grafia de maiúsculas e minúsculas consistente, especialmente se você estiver exibindo-as na interface do usuário. A tabela seguinte descreve três métodos de alteração de capitalização. Os primeiros dois métodos fornecem uma sobrecarga que aceita uma cultura.  
  
|Nome do método|Uso|  
|-----------------|---------|  
|<xref:System.String.ToUpper%2A?displayProperty=nameWithType>|Converte todos os caracteres em uma cadeia de caracteres para maiúsculas.|  
|<xref:System.String.ToLower%2A?displayProperty=nameWithType>|Converte todos os caracteres em uma cadeia de caracteres para minúsculas.|  
|<xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType>|Converte uma cadeia de caracteres para capitalização de título.|  
  
> [!WARNING]
> Observe que os métodos <xref:System.String.ToUpper%2A?displayProperty=nameWithType> e <xref:System.String.ToLower%2A?displayProperty=nameWithType> não devem ser usados para converter cadeias de caracteres a fim de compará-las ou testá-las quanto à igualdade. Confira mais informações na seção [Comparação de cadeias de caracteres em maiúsculas e minúsculas](#Comparing).  
  
<a name="Comparing"></a>
## <a name="compare-strings-of-mixed-case"></a>Comparar cadeias de caracteres de maiúsculas e minúsculas  

 Para comparar cadeias de caracteres em maiúsculas e minúsculas e determinar a ordem delas, chame uma das sobrecargas do método <xref:System.String.CompareTo%2A?displayProperty=nameWithType> com um parâmetro `comparisonType` e forneça um valor de <xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType>, <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> ou <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> para o argumento `comparisonType`. Para fazer uma comparação usando uma cultura específica que não seja a atual, chame uma sobrecarga do método <xref:System.String.CompareTo%2A?displayProperty=nameWithType> com um parâmetro `culture` e um `options` e forneça um valor de <xref:System.Globalization.CompareOptions.IgnoreCase?displayProperty=nameWithType> como o argumento `options`.  
  
 Para comparar cadeias de caracteres em maiúsculas e minúsculas e determinar se são iguais, chame uma das sobrecargas do método <xref:System.String.Equals%2A?displayProperty=nameWithType> com um parâmetro `comparisonType` e forneça um valor de <xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType>, <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> ou <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> para o argumento `comparisonType`.  
  
 Para obter mais informações, consulte [Práticas recomendadas para o uso de cadeias de caracteres](best-practices-strings.md).  
  
## <a name="toupper"></a>ToUpper  
 O método <xref:System.String.ToUpper%2A?displayProperty=nameWithType> altera todos os caracteres em uma cadeia para maiúsculas. O exemplo a seguir converte a cadeia de caracteres "Olá, mundo!" de maiúsculas e minúsculas para maiúsculas.  
  
 [!code-csharp[Strings.ChangingCase#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.ChangingCase/cs/Example.cs#1)]
 [!code-vb[Strings.ChangingCase#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.ChangingCase/vb/Example.vb#1)]  
  
 O exemplo anterior diferencia culturas por padrão e aplica as convenções de capitalização da cultura atual. Para realizar uma alteração de capitalização que não diferencia a cultura ou para aplicar as convenções de capitalização de uma cultura específica, use a sobrecarga do método <xref:System.String.ToUpper%28System.Globalization.CultureInfo%29?displayProperty=nameWithType> e forneça um valor de <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> ou um objeto <xref:System.Globalization.CultureInfo?displayProperty=nameWithType> que representa a cultura especificada para o parâmetro *culture*. Confira um exemplo que demonstra como usar o método <xref:System.String.ToUpper%2A> para realizar uma [Alteração de capitalização que não diferencia a cultura](../globalization-localization/performing-culture-insensitive-case-changes.md).  
  
## <a name="tolower"></a>ToLower  
 O método <xref:System.String.ToLower%2A?displayProperty=nameWithType> é semelhante ao anterior, porém ele converte todos os caracteres de uma cadeia para minúsculas. O exemplo a seguir converte a cadeia de caracteres "Olá, mundo!" para minúsculas.  
  
 [!code-csharp[Strings.ChangingCase#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Strings.ChangingCase/cs/Example.cs#2)]
 [!code-vb[Strings.ChangingCase#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Strings.ChangingCase/vb/Example.vb#2)]  
  
 O exemplo anterior diferencia culturas por padrão e aplica as convenções de capitalização da cultura atual. Para realizar uma alteração de capitalização que não diferencia a cultura ou para aplicar as convenções de capitalização de uma cultura específica, use a sobrecarga do método <xref:System.String.ToLower%28System.Globalization.CultureInfo%29?displayProperty=nameWithType> e forneça um valor de <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> ou um objeto <xref:System.Globalization.CultureInfo?displayProperty=nameWithType> que representa a cultura especificada para o parâmetro *culture*. Confira um exemplo que demonstra como usar o método <xref:System.String.ToLower%28System.Globalization.CultureInfo%29> para realizar uma [Alteração de capitalização que não diferencia a cultura](../globalization-localization/performing-culture-insensitive-case-changes.md).  
  
## <a name="totitlecase"></a>ToTitleCase  
 O <xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType> converte o primeiro caractere de cada palavra em maiúscula e os restantes em minúsculas. No entanto, as palavras que são totalmente maiúsculas são consideradas acrônimos e não são convertidas.  
  
 O método <xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType> diferencia a cultura, ou seja, ele usa as convenções de capitalização de uma determinada cultura. Para chamar o método, é preciso antes recuperar o objeto <xref:System.Globalization.TextInfo> que representa as convenções de capitalização da cultura específica da propriedade <xref:System.Globalization.CultureInfo.TextInfo%2A?displayProperty=nameWithType> de uma determinada cultura.  
  
 O exemplo a seguir passa todas as cadeias de caracteres de uma matriz para o método <xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType>.  As cadeias de caracteres incluem cadeias de caracteres de títulos próprios assim como acrônimos. As cadeias de caracteres são convertidas para a capitalização de título usando as convenções de capitalização da cultura do inglês (Estados Unidos).  
  
 [!code-csharp[System.Globalization.TextInfo.ToTitleCase#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.globalization.textinfo.totitlecase/cs/totitlecase2.cs#1)]
 [!code-vb[System.Globalization.TextInfo.ToTitleCase#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.globalization.textinfo.totitlecase/vb/totitlecase2.vb#1)]  
  
 Embora diferencie a cultura, o método <xref:System.Globalization.TextInfo.ToTitleCase%2A?displayProperty=nameWithType> não fornece regras linguisticamente corretas de capitalização. No exemplo anterior, o método converte "um conto de duas cidades" para "Um Conto De Duas Cidades". No entanto, a capitalização linguisticamente correta para a cultura en-US seria "Um Conto de Duas Cidades."  
  
## <a name="see-also"></a>Veja também

- [Operações básicas de cadeia de caracteres](basic-string-operations.md)
- [Executando operações de cadeia de caracteres que não levam em conta a cultura](../globalization-localization/performing-culture-insensitive-string-operations.md)
