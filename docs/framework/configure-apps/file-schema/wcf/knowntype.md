---
title: <knownType>
ms.date: 03/30/2017
ms.assetid: ee2b7be3-7148-4a3a-b861-48e7330615e5
ms.openlocfilehash: 61f51b2ecd572ba254317a01e0f514503c7cc9e4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70397880"
---
# \<knownType>
Especifica um tipo a ser usado pelo <xref:System.Runtime.Serialization.DataContractSerializer> durante a desserialização. O elemento especifica um "tipo conhecido" que é retornado por um campo ou propriedade de um "tipo declarado". Para obter mais informações, consulte [tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.serialization>**](system-runtime-serialization.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<dataContractSerializer>**](datacontractserializer.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<declaredTypes>**](declaredtypes.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<add>**](add-of-declaredtypes-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<knownType>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<knownType type="String">
  <parameter index="Integer"
             type="String" />
</knownType>
```  
  
## <a name="type"></a>Type  
 `string`  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|type|Especifica o tipo (incluindo o namespace), o nome do assembly, a versão, a cultura e o token de chave pública.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<parameter>](parameter.md)|Especifica um índice de parâmetro quando o tipo declarado é um tipo genérico.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<add>](add-of-declaredtypes-element.md)|Adiciona um tipo declarado à coleção de tipos declarados.|  
  
## <a name="remarks"></a>Comentários  
 Para obter mais informações sobre tipos conhecidos, consulte [tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md) e <xref:System.Runtime.Serialization.DataContractSerializer> .  
  
 Consulte o [\<dataContractSerializer>](datacontractserializer-element.md) para obter um exemplo de como usar esse elemento.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<add type="MyCompany.Library.Shape,
           MyAssembly, Version=2.0.0.0, Culture=neutral,
           PublicKeyToken=XXXXXX, processorArchitecture=MSIL">
  <knownType type="MyCompany.Library.Circle,
                   MyAssembly, Version=2.0.0.0, Culture=neutral,
                   PublicKeyToken=XXXXXX,
                   processorArchitecture=MSIL"/>
</add>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- [Tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md)
- [\<dataContractSerializer>](datacontractserializer-element.md)
- [\<add>](add-of-declaredtypes-element.md)
