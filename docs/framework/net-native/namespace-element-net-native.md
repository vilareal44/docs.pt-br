---
title: <Namespace> (.NET Nativo)
ms.date: 03/30/2017
ms.assetid: 57c614e5-18a9-4e87-bfd5-d0fe3396a192
ms.openlocfilehash: 06d88a7b0f95c7c1dbe98818b847c92e08a57a19
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79180961"
---
# <a name="namespace-element-net-native"></a>\<Namespace> (.NET Nativo)
Aplica a política de reflexão de runtime a todos os tipos em um namespace especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<Namespace Name="namespace_name"
           Activate="policy_type"
           Browse="policy_type"  
           Dynamic="policy_setting"  
           Serialize="policy_setting"  
           DataContractSerializer="policy_setting"  
           DataContractJsonSerializer="policy_setting"  
           XmlSerializer="policy_setting"  
           MarshalObject="policy_setting"  
           MarshalDelegate="policy_setting"  
           MarshalStructure="policy_setting" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Tipo de atributo|Descrição|  
|---------------|--------------------|-----------------|  
|`Name`|Geral|Atributo obrigatório. Especifica o nome do namespace.|  
|`Activate`|Reflexão|Atributo opcional. Controla o acesso de runtime a construtores para habilitar a ativação de instâncias.|  
|`Browse`|Reflexão|Atributo opcional. Controla a consulta para obter informações sobre elementos do programa, mas não permite qualquer acesso de runtime.|  
|`Dynamic`|Reflexão|Atributo opcional. Controla o acesso a todos os tipos de membro ao runtime, incluindo construtores, métodos, campos, propriedades e eventos, habilitando a programação dinâmica.|  
|`Serialize`|Serialização|Atributo opcional. Controla o acesso ao runtime para construtores, campos e propriedades para habilitar a serialização e desserialização das instâncias por bibliotecas como o serializador Newtonsoft JSON.|  
|`DataContractSerializer`|Serialização|Atributo opcional. Controla a política de serialização que usa a classe <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>.|  
|`DataContractJsonSerializer`|Serialização|Atributo opcional. Controla a política de serialização JSON que usa a classe <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType>.|  
|`XmlSerializer`|Serialização|Atributo opcional. Controla a política de serialização XML que usa a classe <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>.|  
|`MarshalObject`|Interoperabilidade|Atributo opcional. Política de controles de marshaling de tipos de referência para o Windows Runtime e COM.|  
|`MarshalDelegate`|Interoperabilidade|Atributo opcional. Controla a diretiva de marshaling de tipos delegados como ponteiros de função para código nativo.|  
|`MarshalStructure`|Interoperabilidade|Atributo opcional. Controla a política de estruturas de marshaling para código nativo.|  
  
## <a name="name-attribute"></a>Atributo de nome  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*namespace_name*|O nome do namespace. Se o \<Namespace> elemento for um filho de um [\<Application>](application-element-net-native.md) [\<Library>](library-element-net-native.md) elemento, ou [\<Assembly>](assembly-element-net-native.md) , *Namespace_Name* deverá ser um nome de namespace totalmente qualificado. Se o \<Namespace> elemento for filho de outro \<Namespace> elemento, *Namespace_Name* deverá ser um nome de namespace relativo.|  
  
## <a name="all-other-attributes"></a>Todos os outros atributos  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*policy_setting*|A configuração a ser aplicada a este tipo de política para todos os tipos no namespace. Os valores possíveis são `All`, `Auto`, `Excluded`, `Public`, `PublicAndInternal`, `Required Public`, `Required PublicAndInternal` e `Required All`. Para obter mais informações, consulte [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md).|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`<Namespace>`|Aplica a política de reflexão de runtime a todos os tipos em um namespace pai.|  
|[\<Type>](type-element-net-native.md)|Aplica a política de reflexão a um tipo.|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|Aplica a política de reflexão a um tipo genérico construído.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<Application>](application-element-net-native.md)|Serve como um contêiner para os tipos amplos de aplicativos cujos metadados estão disponíveis para reflexão no tempo de execução. O [\<Application>](application-element-net-native.md) elemento pode ter zero, um ou mais [\<Assembly>](assembly-element-net-native.md) elementos.|  
|[\<Assembly>](assembly-element-net-native.md)|Aplica a política de reflexão de runtime a todos os tipos em um assembly especificado.|  
|[\<Library>](library-element-net-native.md)|Define o assembly que contém tipos e membros de tipo cujos metadados estão disponíveis para reflexão em tempo de execução. O [\<Library>](library-element-net-native.md) elemento pode ter zero ou um [\<Assembly>](assembly-element-net-native.md) elemento.|  
|`<Namespace>`|Aplica a política de reflexão de tempo de execução a todos os tipos em um namespace pai.|  
  
## <a name="remarks"></a>Comentários  
 Os atributos `Activate`, `Browse`, `Dynamic` e `Serialize` são todos opcionais. Se nenhum estiver presente, o elemento `<Namespace>` serve somente como um contêiner para elementos filho. Se eles estiverem presentes, o elemento `<Namespace>` aplica a política de reflexão de runtime a todos os tipos no namespace especificado.  
  
 Quando é um filho do [\<Assembly>](assembly-element-net-native.md) elemento, o `<Namespace>` elemento substitui a política de reflexão de tempo de execução definida pelo [\<Assembly>](assembly-element-net-native.md) elemento.  
  
## <a name="see-also"></a>Confira também

- [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md)
- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Elementos da diretiva de runtime](runtime-directive-elements.md)
