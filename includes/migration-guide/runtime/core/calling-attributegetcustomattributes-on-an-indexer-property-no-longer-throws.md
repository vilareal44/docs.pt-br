---
ms.openlocfilehash: 34093fd8c47049a0bd59020228043c06035af421
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621031"
---
### <a name="calling-attributegetcustomattributes-on-an-indexer-property-no-longer-throws-ambiguousmatchexception-if-the-ambiguity-can-be-resolved-by-indexs-type"></a>Chamar Attribute.GetCustomAttributes na propriedade de um indexador não vai mais gerar AmbiguousMatchException se a ambiguidade puder ser resolvida pelo tipo do índice

#### <a name="details"></a>Detalhes

Antes do .NET Framework 4.6, chamar <code>GetCustomAttribute(s)</code> na propriedade de um indexador que diferia de outra propriedade apenas pelo tipo do índice resultaria em um <xref:System.Reflection.AmbiguousMatchException?displayProperty=fullName>. A partir do .NET Framework 4.6, os atributos da propriedade serão corretamente retornados.

#### <a name="suggestion"></a>Sugestão

Lembre-se de que GetCustomAttribute(s) funcionará com mais frequência agora. Se um aplicativo anteriormente contava com o <xref:System.Reflection.AmbiguousMatchException?displayProperty=fullName>, a reflexão agora deverá ser usada para procurar explicitamente vários indexadores.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Attribute.GetCustomAttribute(System.Reflection.MemberInfo,System.Type)?displayProperty=nameWithType></li><li><xref:System.Attribute.GetCustomAttribute(System.Reflection.MemberInfo,System.Type,System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Attribute.GetCustomAttributes(System.Reflection.MemberInfo)?displayProperty=nameWithType></li><li><xref:System.Attribute.GetCustomAttributes(System.Reflection.MemberInfo,System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Attribute.GetCustomAttributes(System.Reflection.MemberInfo,System.Type)?displayProperty=nameWithType></li><li><xref:System.Attribute.GetCustomAttributes(System.Reflection.MemberInfo,System.Type,System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttribute(System.Reflection.MemberInfo,System.Type)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttribute(System.Reflection.MemberInfo,System.Type,System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttribute%60%601(System.Reflection.MemberInfo)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttribute%60%601(System.Reflection.MemberInfo,System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttributes(System.Reflection.MemberInfo)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttributes(System.Reflection.MemberInfo,System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttributes(System.Reflection.MemberInfo,System.Type)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttributes(System.Reflection.MemberInfo,System.Type,System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttributes%60%601(System.Reflection.MemberInfo)?displayProperty=nameWithType></li><li><xref:System.Reflection.CustomAttributeExtensions.GetCustomAttributes%60%601(System.Reflection.MemberInfo,System.Boolean)?displayProperty=nameWithType></li></ul>|
