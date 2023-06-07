---
title: <declaredTypes>
ms.date: 03/30/2017
helpviewer_keywords:
- dataContractSerializer element
- declaredTypes element
- DataContractSerializer
- KnownTypes
- <declaredTypes> element
ms.assetid: f35184e4-9d9e-4d37-8fb4-d5b58220eb3e
ms.openlocfilehash: c45a4e67d0a2d98c0e9c1a91e07f25b81370244c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398059"
---
# \<declaredTypes>
Contém os tipos conhecidos que o <xref:System.Runtime.Serialization.DataContractSerializer> usa ao desserializar.  
  
 Para obter mais informações sobre contratos de dados e tipos conhecidos, consulte [tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.serialization>**](system-runtime-serialization.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<dataContractSerializer>**](datacontractserializer.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<declaredTypes>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<configuration>
  <system.runtime.serialization>
    <dataContractSerializer>
      <declaredTypes>
        <add type="String ">
          <knownType type="String">
            <parameter index="Integer"/>
          </knownType>
        </add>
      </declaredTypes>
    <dataContractSerializer>
  </system.runtime.serialization>
</configuration>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<add>](add-of-declaredtypes-element.md)|Adiciona tipos que exigem tipos conhecidos.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<dataContractSerializer>](datacontractserializer-of-system-runtime-serialization.md)|Contém dados de configuração para o <xref:System.Runtime.Serialization.DataContractSerializer> .|  
  
## <a name="remarks"></a>Comentários  
 Para obter mais informações sobre tipos conhecidos, consulte [tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md) e <xref:System.Runtime.Serialization.DataContractSerializer> .  
  
## <a name="example"></a>Exemplo  
 O código XML a seguir mostra tipos declarados e tipos conhecidos adicionados a um `DataContractSerializer` elemento. O exemplo mostra três tipos que estão sendo adicionados. O primeiro é um tipo personalizado chamado "Orders" que usa um tipo conhecido chamado "item". O segundo tipo declarado é um <xref:System.Collections.Generic.List%601> que usa `Item` como um tipo conhecido. Por fim, o terceiro tipo declarado é um <xref:System.Collections.Generic.Dictionary%602> . O <xref:System.Collections.Generic.Dictionary%602> tipo de classe é um tipo genérico, com dois parâmetros de tipo. O primeiro representa a chave e a segunda representa o valor. O exemplo a seguir adiciona um <xref:System.Collections.Generic.List%601> do segundo tipo (o valor) à lista de tipos conhecidos. Você deve usar o `index` atributo para especificar o parâmetro de tipo a ser usado no tipo conhecido. Nesse caso, o tipo de valor é indicado pelo atributo de índice definido como "1" (a coleção é baseada em zero).  
  
```xml  
<configuration>
  <system.runtime.serialization>
    <dataContractSerializer>
      <declaredTypes>
        <add type="Examples.Types.Orders, SerializationTypes, Version = 2.0.0.0, Culture = neutral, PublicKeyToken=null">
          <knownType type="Examples.Types.Item, SerializationTypes, Version=2.0.0.0, Culture=neutral, PublicKey=null" />
        </add>
        <add type="System.Collections.Generic.List`1, SerializationTypes, Version = 2.0.0.0, Culture = neutral, PublicKeyToken=null">
          <knownType type="Examples.Types.Item, SerializationTypes, Version=2.0.0.0, Culture=neutral, PublicKey=null" />
        </add>
        <add type="System.Collections.Generic.Dictionary`2, SerializationTypes, Version = 2.0.0.0, Culture = neutral, PublicKeyToken=null">
          <knownType type="System.Collections.Generic.List`1, SerializationTypes, Version = 2.0.0.0, Culture = neutral, PublicKeyToken=null">
            <parameter index="1"/>
          </knownType>
        </add>
      </declaredTypes>
    <dataContractSerializer>
  </system.runtime.serialization>
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- [\<dataContractSerializer>](datacontractserializer-element.md)
- [Tipos conhecidos de contrato de dados](../../../wcf/feature-details/data-contract-known-types.md)
- [\<add>](add-of-declaredtypes-element.md)
