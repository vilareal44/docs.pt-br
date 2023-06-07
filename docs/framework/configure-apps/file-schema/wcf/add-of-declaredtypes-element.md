---
title: <add>do <declaredTypes> elemento
ms.date: 03/30/2017
helpviewer_keywords:
- data contracts
- dataContractSerializer element
- DataContractSerializer
- DataContractAttribute
ms.assetid: c3d37ae4-8f1c-463f-b195-658c5a7e90a1
ms.openlocfilehash: a001e8743b2c24f68b1b23cbccf3e5ac162c4e71
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400651"
---
# <a name="add-of-declaredtypes-element"></a>\<add>do \<declaredTypes> elemento
Adiciona um tipo usado pelo <xref:System.Runtime.Serialization.DataContractSerializer> durante a desserialização. Cada tipo declarado inclui os tipos conhecidos que serão retornados como um campo ou Propriedade do tipo declarado.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.serialization>**](system-runtime-serialization.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<dataContractSerializer>**](datacontractserializer.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<declaredTypes>**](declaredtypes.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<add type="String">
  <knownType type="String">
    <parameter index="Integer"
               type="String" />
  </knownType>
</add>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|type|Atributo de cadeia de caracteres obrigatório.<br /><br /> Especifica o nome do tipo (incluindo o namespace), o nome do assembly, o número de versão, a cultura e o token de chave pública.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<knownType>](knowntype.md)|Especifica o tipo conhecido para o tipo declarado que está sendo adicionado. Se o tipo declarado for um tipo genérico, você também deverá adicionar um elemento de parâmetro ao `<knownType>` elemento para especificar qual parâmetro genérico é usado para retornar o tipo conhecido.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<declaredTypes>](declaredtypes.md)|Contém os tipos que exigem tipos conhecidos durante a desserialização pelo <xref:System.Runtime.Serialization.DataContractSerializer> .|  
  
## <a name="remarks"></a>Comentários  
 Para obter mais informações sobre tipos conhecidos, consulte [tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md) e <xref:System.Runtime.Serialization.DataContractSerializer> .  
  
 Consulte o [\<dataContractSerializer>](datacontractserializer-element.md) para obter um exemplo de como usar esse elemento.  
  
> [!NOTE]
> Se você adicionar o <xref:System.Object> tipo como um `<declaredType>` , um <xref:System.Configuration.ConfigurationErrorsException> será lançado. Isso ocorre porque o <xref:System.Object> tipo não pode ser usado como um tipo declarado na configuração.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<add type="MyCompany.Library.Shape,
           MyAssembly, Version=2.0.0.0, Culture=neutral,
           PublicKeyToken=XXXXXX, processorArchitecture=MSIL">
  <knownType type="MyCompany.Library.Circle,
                   MyAssembly, Version=2.0.0.0, Culture=neutral,
                   PublicKeyToken=XXXXXX,
                   processorArchitecture=MSIL" />
</add>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- [Tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md)
- [\<dataContractSerializer>](datacontractserializer-element.md)
- [\<add>desse\<declaredTypes>](add-of-declaredtypes-element.md)
