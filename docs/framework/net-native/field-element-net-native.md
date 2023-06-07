---
title: <Field> (.NET Nativo)
ms.date: 03/30/2017
ms.assetid: 6a14125f-1a8d-41a1-8a32-659ca0ad12de
ms.openlocfilehash: 2a63b88c399a999cd00750dee1614352cea10e80
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128416"
---
# <a name="field-element-net-native"></a>\<Field> (.NET Nativo)
Aplica a política de reflexão do runtime a um campo.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<Field Name="field_name"  
       Browse="policy_type"  
       Dynamic="policy_type"  
       Serialize="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Tipo de atributo|Descrição|  
|---------------|--------------------|-----------------|  
|`Name`|Geral|Atributo obrigatório. Especifica o nome do campo.|  
|`Browse`|Reflexão|Atributo opcional. Controla consultas para obter informações sobre o campo ou para enumerá-lo, mas não permite qualquer acesso dinâmico no tempo de execução.|  
|`Dynamic`|Reflexão|Atributo opcional. Controla o acesso do runtime ao campo para habilitar programação dinâmica. Essa política garante que um campo pode ser definido ou recuperado dinamicamente no tempo de execução.|  
|`Serialize`|Serialização|Atributo opcional. Controla o acesso do runtime a um campo para habilitar as instâncias de tipo a serem serializadas por bibliotecas como o serializador Newtonsoft JSON ou a ser usado para a associação de dados.|  
  
## <a name="name-attribute"></a>Atributo de nome  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*method_name*|O nome do campo. O tipo do campo é definido pelo [\<Type>](type-element-net-native.md) elemento pai ou [\<TypeInstantiation>](typeinstantiation-element-net-native.md) .|  
  
## <a name="all-other-attributes"></a>Todos os outros atributos  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*policy_setting*|A configuração a ser aplicada a este tipo de política para o campo. Os valores possíveis são `Auto`, `Excluded`, `Included` e `Required`. Para obter mais informações, consulte [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md).|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|Aplica a política de reflexão a um tipo e todos os seus membros.|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|Aplica a política de reflexão a um tipo genérico construído e todos os seus membros.|  
  
## <a name="remarks"></a>Comentários  
 Se a política do campo não for definida explicitamente, ele herdará a política de runtime do seu elemento pai.  
  
## <a name="see-also"></a>Confira também

- [Elementos da diretiva de runtime](runtime-directive-elements.md)
- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md)
