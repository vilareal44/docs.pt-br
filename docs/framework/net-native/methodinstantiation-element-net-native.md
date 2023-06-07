---
title: <MethodInstantiation> (.NET Nativo)
ms.date: 03/30/2017
ms.assetid: a3355d78-2a88-4109-8521-830d7cae260a
ms.openlocfilehash: f19bd3c20088431bcbbafac298398b82a664bee9
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128321"
---
# <a name="methodinstantiation-element-net-native"></a>\<MethodInstantiation> (.NET Nativo)
Aplica a política de reflexão de runtime a um método genérico construído.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<MethodInstantiation Name="method_name"  
                     Signature="method_signature"  
                     Arguments="method_arguments"  
                     Browse="policy_type"  
                     Dynamic="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Tipo de atributo|Descrição|  
|---------------|--------------------|-----------------|  
|`Name`|Geral|Atributo obrigatório. Especifica o nome do método.|  
|`Signature`|Geral|Atributo opcional. Especifica os parâmetros nomeados do método. Vários parâmetros nomeados são separados por vírgulas. O atributo `Signature` é usado para diferenciar métodos sobrecarregados.|  
|`Arguments`|Geral|Atributo obrigatório. Especifica os argumentos de tipo genérico. Se houver vários parâmetros, eles são separados por vírgulas.|  
|`Browse`|Reflexão|Atributo opcional. Controla consultas para obter informações sobre o método ou para enumerá-lo, mas não permite qualquer invocação dinâmica no tempo de execução.|  
|`Dynamic`|Reflexão|Atributo opcional. Controla o acesso do runtime a um construtor ou método para habilitar a programação dinâmica. Essa política garante que um membro pode ser invocado dinamicamente no tempo de execução.|  
  
## <a name="name-attribute"></a>Atributo de nome  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*method_name*|O nome do método. O tipo do método é definido pelo [\<Type>](type-element-net-native.md) elemento pai ou [\<TypeInstantiation>](typeinstantiation-element-net-native.md) .|  
  
## <a name="signature-attribute"></a>Atributo de assinatura  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*method_signature*|Especifica os parâmetros nomeados do método. Se vários parâmetros estiverem presentes, eles são separados por vírgulas.|  
  
## <a name="arguments-attribute"></a>Atributo de argumentos  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*method_arguments*|Especifica os argumentos de tipo genérico. Se houver vários parâmetros, eles são separados por vírgulas. Cada argumento deve conter o nome do tipo totalmente qualificado.|  
  
## <a name="all-other-attributes"></a>Todos os outros atributos  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*policy_setting*|A configuração a ser aplicada a este tipo de política para o método. Os valores possíveis são `Auto`, `Excluded`, `Included` e `Required`. Para obter mais informações, consulte [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md).|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|Aplica a política de reflexão a um tipo e todos os seus membros.|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|Aplica a política de reflexão a um tipo genérico construído e todos os seus membros.|  
  
## <a name="remarks"></a>Comentários  
 O elemento `<MethodInstantiation>` substitui a política de reflexão de runtime do seu método genérico aberto correspondente.  
  
## <a name="see-also"></a>Confira também

- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Elementos da diretiva de runtime](runtime-directive-elements.md)
- [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md)
- [\<Method>Elementos](method-element-net-native.md)
