---
title: <GenericParameter> (.NET Nativo)
ms.date: 03/30/2017
ms.assetid: cbd49732-3615-49a5-a900-f96947cdc3e6
ms.openlocfilehash: d0b18211206a8f9d4365ab3affe6d1c376003348
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128429"
---
# <a name="genericparameter-element-net-native"></a>\<GenericParameter> (.NET Nativo)
Aplica a política ao tipo de parâmetro de um tipo ou método genérico.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<GenericParameter Name="generic_parameter_name"  
                  Activate="policy_type"  
                  Browse="policy_type"  
                  Dynamic="policy_type"  
                  Serialize="policy_type"  
                  DataContractSerializer="policy_type"  
                  DataContractJsonSerializer="policy_type"  
                  XmlSerializer="policy_type"  
                  MarshalObject="policy_type"  
                  MarshalDelegate="policy_type"  
                  MarshalStructure="policy_type" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Tipo de atributo|Descrição|  
|---------------|--------------------|-----------------|  
|`Name`|Geral|Atributo obrigatório. O nome do parâmetro genérico. Por exemplo, para o delegado genérico <xref:System.Func%603>, o valor do atributo `Name` é "TResult" para aplicar a política de runtime ao valor de retorno do delegado.|  
|`Activate`|Reflexão|Atributo opcional. Controla o acesso de runtime a construtores para habilitar a ativação de instâncias.|  
|`Browse`|Reflexão|Atributo opcional. Controla a consulta para obter informações sobre elementos do programa, mas não permite qualquer acesso de runtime.|  
|`Dynamic`|Reflexão|Atributo opcional. Controla o acesso a todos os tipos de membro ao runtime, incluindo construtores, métodos, campos, propriedades e eventos, habilitando a programação dinâmica.|  
|`Serialize`|Serialização|Atributo opcional. Controla o acesso ao runtime para construtores, campos e propriedades para habilitar a serialização e desserialização das instâncias por bibliotecas como o serializador Newtonsoft JSON.|  
|`DataContractSerializer`|Serialização|Atributo opcional. Controla a política de serialização que usa a classe <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>.|  
|`DataContractJsonSerializer`|Serialização|Atributo opcional. Controla a política de serialização JSON que usa a classe <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType>.|  
|`XmlSerializer`|Serialização|Atributo opcional. Controla a política de serialização XML que usa a classe <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>.|  
|`MarshalObject`|Interoperabilidade|Atributo opcional. Política de controles de marshaling de tipos de referência para o Windows Runtime e COM.|  
|`MarshalDelegate`|Interoperabilidade|Atributo opcional. Controla a diretiva de marshaling de tipos delegados como ponteiros de função para código nativo.|  
|`MarshalStructure`|Interoperabilidade|Atributo opcional. Controla a política de marshaling de tipos de valor para código nativo.|  
  
## <a name="name-attribute"></a>Atributo de nome  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*generic_parameter_name*|Atributo obrigatório. O nome do parâmetro de tipo genérico. Por exemplo, para o delegado genérico <xref:System.Func%603>, um valor igual a “TResult” para *generic_parameter_name* aplica a política de runtime ao valor retornado do representante.|  
  
## <a name="all-other-attributes"></a>Todos os outros atributos  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*policy_setting*|A configuração a ser aplicada a este tipo de política. Os valores possíveis são `All`, `Public`, `PublicAndInternal`, `Required Public`, `Required PublicAndInternal` e `Required All`. Para obter mais informações, consulte [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md).|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<Method>](method-element-net-native.md)|Aplica a política de reflexão de runtime a um construtor ou método.|  
|[\<Type>](type-element-net-native.md)|Aplica a política de tempo reflexão de runtime a um tipo específico, como uma classe ou estrutura.|  
  
## <a name="remarks"></a>Comentários  
 O `<GenericParameter>` elemento é um filho do [\<Method>](method-element-net-native.md) [\<Type>](type-element-net-native.md) elemento ou e é usado para aplicar a política a um parâmetro de tipo genérico específico, que é especificado por seu nome na assinatura de método ou tipo genérico.  
  
 O elemento `<GenericParameter>` é mais útil quando usado com serializadores. O exemplo a seguir usa o `<GenericParameter>` elemento para aplicar a política ao tipo `T` em chamadas para as sobrecargas do método [JsonConvert. deserializaobject \<T> (String)](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm) do serializador JSON NewtonSoft.  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
   <Type Name="Newtonsoft.Json.JsonConvert" >  
      <Method Name="DeserializeObject{T}">  
         <GenericParameter Name="T" Serialize="Required All" />  
      </Method>  
   </Type>  
</Directives>  
```  
  
## <a name="see-also"></a>Confira também

- [\<Method>Elementos](method-element-net-native.md)
- [\<Type>Elementos](type-element-net-native.md)
- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md)
- [Elementos da diretiva de runtime](runtime-directive-elements.md)
