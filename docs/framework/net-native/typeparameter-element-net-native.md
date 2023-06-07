---
title: <TypeParameter> (.NET Nativo)
ms.date: 03/30/2017
ms.assetid: d37bb1b7-1ddc-4c6d-8ecf-583f804a2479
ms.openlocfilehash: c69b535f3a01c287d30189138130066fc10a77e2
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128919"
---
# <a name="typeparameter-element-net-native"></a>\<TypeParameter> (.NET Nativo)
Aplica a política ao tipo representado por um argumento Type passado para um método.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<Parameter Name="parameter_name"  
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
|`Name`|Geral|Atributo obrigatório. O nome do parâmetro do tipo <xref:System.Type>. Por exemplo, para a assinatura do método `Type.GetInterfaceMap(Type interfaceType)`, o valor do atributo `Name` é "interfaceType".|  
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
|*parameter_name*|O nome do parâmetro do tipo <xref:System.Type>. Por exemplo, para a assinatura do método `Type.GetInterfaceMap(Type interfaceType)`, o valor do atributo `Name` é "interfaceType".|  
  
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
  
## <a name="remarks"></a>Comentários  
 O `<TypeParameter>` elemento é semelhante ao [\<Parameter>](parameter-element-net-native.md) elemento, exceto que ele só pode ser aplicado a parâmetros do tipo <xref:System.Type> . Ele aplica a política a qualquer tipo que é representado no tempo de execução pelo argumento do tipo especificado pelo atributo `Name`.  
  
 Por exemplo, o serializador NewtonSoft JSON inclui um método `JsonConvert.DeserializeObject(String value, Type type)` estático. As seguintes diretivas de reflexão:  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
   <Type Name="Newtonsoft.Json.JsonConvert" >  
      <Method Name="DeserializeObject">  
         <GenericParameter Name="type" Serialize="Required All" />  
      </Method>  
   </Type>  
</Directives>  
```  
  
 especificam que os metadados para o tipo de runtime representado pelo argumento `type` deve ser disponibilizado para serialização. Se essas diretivas de runtime para um projeto que inclui o seguinte código-fonte:  
  
```csharp  
Type t = typeof(StockQuote);  
Object obj = JsonConvert.DeserializeObject(data, t);  
```  
  
 as diretivas de reflexão disponibilizam os metadados para o tipo `StockQuote` disponível para o serializador NewtonSoft JSON no tempo de execução.  
  
## <a name="see-also"></a>Confira também

- [\<Method>Elementos](method-element-net-native.md)
- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md)
- [Elementos da diretiva de runtime](runtime-directive-elements.md)
