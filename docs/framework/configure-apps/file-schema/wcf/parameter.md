---
title: <parameter>
ms.date: 03/30/2017
ms.assetid: 0fb41e2d-64f7-44ab-993e-05892eac6d82
ms.openlocfilehash: 07fa410109a7bd2fa315132c4737301698bb3a93
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400108"
---
# \<parameter>
Especifica o parâmetro genérico quando um tipo declarado é um tipo genérico.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.serialization>**](system-runtime-serialization.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<dataContractSerializer>**](datacontractserializer.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<declaredTypes>**](declaredtypes.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<add>**](add-of-declaredtypes-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<knownType>**](knowntype.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<parameter>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<parameter index="Integer"
           type="String" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|índice|Quando o tipo declarado é um tipo genérico, especifica o parâmetro genérico que retornará o tipo conhecido.|  
|type|Uma cadeia de caracteres que descreve o tipo conhecido usado para serialização e desserialização.|  
  
## <a name="index-attribute"></a>Atributo de índice  
  
|Valor|Descrição|  
|-----------|-----------------|  
|"0"|O primeiro parâmetro no tipo genérico. Por exemplo, um <xref:System.Collections.Generic.List%601> tem apenas um parâmetro. Se for usado como o tipo declarado, o índice será definido como "0".|  
|"1"|O segundo parâmetro em um tipo genérico. Por exemplo, um <xref:System.Collections.Generic.Dictionary%602> tem dois parâmetros. Se o tipo conhecido for retornado pelo segundo parâmetro, defina o atributo de índice como "1".|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<knownType>](knowntype.md)|Especifica um tipo conhecido que pode ser retornado por um campo ou Propriedade do tipo declarado.|  
  
## <a name="remarks"></a>Comentários  
 Para obter mais informações sobre tipos conhecidos, consulte [tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md) e <xref:System.Runtime.Serialization.DataContractSerializer> .  
  
 Consulte o [\<dataContractSerializer>](datacontractserializer-element.md) para obter um exemplo de como usar esse elemento.  
  
 Este elemento de configuração não pode ter ambos os atributos ao mesmo tempo. Se ambos os atributos forem definidos, <xref:System.Configuration.ConfigurationErrorsException> ocorrerá um.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- [Tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md)
- [\<dataContractSerializer>](datacontractserializer-element.md)
- [\<add>](add-of-declaredtypes-element.md)
