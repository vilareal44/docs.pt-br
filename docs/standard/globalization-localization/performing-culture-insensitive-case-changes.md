---
title: Executando alterações de maiúsculas e minúsculas que não levam em conta a cultura
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- String.ToLower method
- culture, case sensitivity
- Char.ToLower method
- case, changing in culture-insensitive strings
- Char.ToUpper method
- culture-insensitive string operations, case changes
- String.ToUpper method
- culture parameter
ms.assetid: 822d551c-c69a-4191-82f4-183d82c9179c
ms.openlocfilehash: 6baef7b0a5bbdacd33d84df01b1aa943897a9e3d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84276810"
---
# <a name="performing-culture-insensitive-case-changes"></a>Executando alterações de maiúsculas e minúsculas que não levam em conta a cultura
Os métodos <xref:System.String.ToUpper%2A?displayProperty=nameWithType>, <xref:System.String.ToLower%2A?displayProperty=nameWithType>, <xref:System.Char.ToUpper%2A?displayProperty=nameWithType> e <xref:System.Char.ToLower%2A?displayProperty=nameWithType> fornecem sobrecargas que não aceitam quaisquer parâmetros. Por padrão, essas sobrecargas sem parâmetros executam alterações de maiúsculas e minúsculas com base no valor do <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType>. Isso produz resultados sensíveis a maiúsculas e minúsculas que podem variar de acordo com a cultura. Para deixar claro se você deseja que as mudanças de maiúsculas e minúsculas sejam sensíveis à cultura ou não sejam insensíveis à cultura, você deve usar as sobrecargas desses métodos que exigem que você especifique explicitamente um parâmetro `culture`. Para mudanças de maiúsculas e minúsculas sensíveis à cultura, especifique `CultureInfo.CurrentCulture` para o parâmetro `culture`. Para mudanças de maiúsculas e minúsculas insensíveis à cultura, especifique `CultureInfo.InvariantCulture` para o parâmetro `culture`.  
  
 Muitas vezes, as cadeias de caracteres são convertidas em um formato padrão para permitir uma pesquisa mais fácil. Quando as cadeias de caracteres são usadas dessa maneira, você deve especificar `CultureInfo.InvariantCulture` para o parâmetro `culture`, pois o valor de <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> pode mudar potencialmente entre o tempo que o formato é alterado e o tempo que a pesquisa ocorre.  
  
 Se uma decisão de segurança é baseada em uma operação de modificação de maiúsculas e minúsculas, a operação deve ser insensível à cultura para garantir que o resultado não seja afetado pelo valor de `CultureInfo.CurrentCulture`. Confira a seção "Comparações de cadeia de caracteres que usam a cultura atual" [Práticas recomendadas para usar cadeias de caracteres](../base-types/best-practices-strings.md) para obter um exemplo que demonstra como as operações de cadeias de caracteres que diferenciam a cultura podem produzir resultados inconsistentes.  
  
## <a name="using-the-stringtoupper-and-stringtolower-methods"></a>Usando os Métodos String.ToUpper e String.ToLower  
 Para clareza de código, recomenda-se que você sempre use sobrecargas dos métodos `String.ToUpper` e `String.ToLower` que permitem especificar um parâmetro `culture` explicitamente. Por exemplo, o código a seguir realiza uma pesquisa de identificador. A operação `key.ToLower` é sensível à cultura por padrão, mas esse comportamento não fica claro ao ler o código.  
  
### <a name="example"></a>Exemplo  
  
```vb  
Shared Function LookupKey(key As String) As Object  
   Return internalHashtable(key.ToLower())  
End Function  
```  
  
```csharp  
static object LookupKey(string key)
{  
    return internalHashtable[key.ToLower()];  
}  
```  
  
 Se você quiser que a operação `key.ToLower` seja insensível à cultura, você deve alterar o exemplo anterior da seguinte forma para usar explicitamente o `CultureInfo.InvariantCulture` ao mudar a caixa.  
  
```vb  
Shared Function LookupKey(key As String) As Object  
    Return internalHashtable(key.ToLower(CultureInfo.InvariantCulture))  
End Function  
```  
  
```csharp  
static object LookupKey(string key)
{  
    return internalHashtable[key.ToLower(CultureInfo.InvariantCulture)];  
}  
```  
  
## <a name="using-the-chartoupper-and-chartolower-methods"></a>Usando os Métodos Char.ToUpper e Char.ToLower  
 Embora os métodos `Char.ToUpper` e `Char.ToLower` tenham as mesmas características que os métodos `String.ToUpper` e `String.ToLower`, as únicas culturas afetadas são turca (Turquia) e azerbaijana (Latino, Azerbaijão). Essas são apenas duas culturas com diferenças de maiúsculas e minúsculas de um único caractere. Para obter mais detalhes sobre este mapeamento de maiúsculas e minúsculas único, confira a seção "Maiúsculas e minúsculas" no tópico da classe <xref:System.String>. Para clareza de código e para garantir resultados consistentes, recomenda-se que você sempre use as sobrecargas desses métodos que permitem que você especifique explicitamente um parâmetro `culture`.  
  
## <a name="see-also"></a>Veja também

- <xref:System.String.ToUpper%2A?displayProperty=nameWithType>
- <xref:System.String.ToLower%2A?displayProperty=nameWithType>
- <xref:System.Char.ToUpper%2A?displayProperty=nameWithType>
- <xref:System.Char.ToLower%2A?displayProperty=nameWithType>
- [Executando operações de cadeia de caracteres que não levam em conta a cultura](performing-culture-insensitive-string-operations.md)
