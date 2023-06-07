---
title: Elemento <EnableAmPmParseAdjustment>
ms.date: 03/30/2017
ms.assetid: fda998a5-f538-4f8b-a18c-ee7f35e16938
ms.openlocfilehash: 8920e51fcaaca5cb78b80a99ea321163c9b5240f
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73117368"
---
# <a name="enableampmparseadjustment-element"></a>Elemento \<EnableAmPmParseAdjustment>
Determina se os métodos de análise de data e hora usam um conjunto ajustado de regras para analisar as cadeias de caracteres de data que contêm um designador de dia, mês, hora e AM/PM.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<EnableAmPmParseAdjustment>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<EnableAmPmParseAdjustment enabled="0"|"1" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se os métodos de análise de data e hora usam um conjunto ajustado de regras para analisar cadeias de caracteres de data que contêm apenas um designador de dia, mês, hora e AM/PM.|  
  
### <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|0|Os métodos de análise de data e hora não usam regras ajustadas para a análise de cadeias de caracteres de data que contêm apenas um designador de dia, mês, hora e AM/PM.|  
|1|Os métodos de análise de data e hora usam regras ajustadas para a análise de cadeias de caracteres de data que contêm apenas um designador de dia, mês, hora e AM/PM.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre opções de inicialização do runtime.|  
  
## <a name="remarks"></a>Comentários  
 O `<EnableAmPmParseAdjustment>` elemento controla como os métodos a seguir analisam uma cadeia de caracteres de data que contém um dia e mês numéricos seguidos por uma hora e um designador AM/PM (como "4/10 6 am"):  
  
- <xref:System.DateTime.Parse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTime.TryParse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTimeOffset.TryParse%2A?displayProperty=nameWithType>  
  
- <xref:System.Convert.ToDateTime%2A?displayProperty=nameWithType>  
  
 Nenhum outro padrão é afetado.  
  
 O `<EnableAmPmParseAdjustment>` elemento não tem nenhum efeito nos <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> métodos,, <xref:System.DateTime.TryParseExact%2A?displayProperty=nameWithType> <xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType> e <xref:System.DateTimeOffset.TryParseExact%2A?displayProperty=nameWithType> .  
  
> [!IMPORTANT]
> No .NET Core e .NET Native, as regras de análise de AM/PM ajustadas são habilitadas por padrão.  
  
 Se a regra de ajuste de análise não estiver habilitada, o primeiro dígito da cadeia de caracteres será interpretado como a hora do relógio de 12 horas e o restante da cadeia de caracteres, exceto o designador AM/PM, será ignorado. A data e a hora retornadas pelo método de análise consiste na data atual e na hora do dia extraídas da cadeia de caracteres de data.  
  
 Se a regra de ajuste de análise estiver habilitada, o método de análise interpretará o dia e o mês como pertencentes ao ano atual e interpretará a hora como a hora do relógio de 12 horas.  
  
 A tabela a seguir ilustra a diferença no <xref:System.DateTime> valor quando o <xref:System.DateTime.Parse%28System.String%29?displayProperty=nameWithType> método é usado para analisar a cadeia de caracteres "" 4/10 6 am "com a `<EnableAmPmParseAdjustment>` Propriedade do elemento `enabled` definida como" 0 "ou" 1 ". Ele assume que a data de hoje é 5 de janeiro de 2017 e exibe a data como se ela fosse formatada usando a cadeia de caracteres de formato "G" da cultura especificada.  
  
|Nome da cultura|habilitado = "0"|habilitado = "1"|  
|------------------|------------------|------------------|  
|pt-BR|1/5/2017 4:00:00 AM|4/10/2017 6:00:00 AM|  
|en-GB|5/1/2017 6:00:00|10/4/2017 6:00:00|  
  
## <a name="see-also"></a>Confira também

- [\<runtime>Elementos](runtime-element.md)
- [\<configuration>Elementos](../configuration-element.md)
