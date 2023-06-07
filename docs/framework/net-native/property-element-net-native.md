---
title: <Property> (.NET Nativo)
ms.date: 03/30/2017
ms.assetid: ad4ba56d-3bcb-4c10-ba90-1cc66e2175a1
ms.openlocfilehash: b9bc89804a872dddf1a56c2a3dadc9c3df4f5fd1
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128204"
---
# <a name="property-element-net-native"></a>\<Property> (.NET Nativo)
Aplica a política de reflexão de runtime a uma propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<Property Name="property_name"  
          Browse="policy_type"  
          Dynamic="policy_type"  
          Serialize="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Tipo de atributo|Descrição|  
|---------------|--------------------|-----------------|  
|`Name`|Geral|Atributo obrigatório. Especifica o nome da propriedade.|  
|`Browse`|Reflexão|Atributo opcional. Controla consultas para obter informações sobre a propriedade ou para enumerá-la, mas não permite qualquer acesso dinâmico no tempo de execução.|  
|`Dynamic`|Reflexão|Atributo opcional. Controla o acesso do runtime à propriedade para habilitar programação dinâmica. Essa política garante que uma propriedade pode ser definida ou recuperada dinamicamente no tempo de execução.|  
|`Serialize`|Serialização|Atributo opcional. Controla o acesso do runtime a uma propriedade para habilitar as instâncias de tipo a serem serializadas por bibliotecas como o serializador Newtonsoft JSON ou a ser usado para a associação de dados.|  
  
## <a name="name-attribute"></a>Atributo de nome  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*method_name*|O nome da propriedade. O tipo da propriedade é definido pelo [\<Type>](type-element-net-native.md) elemento pai ou [\<TypeInstantiation>](typeinstantiation-element-net-native.md) .|  
  
## <a name="all-other-attributes"></a>Todos os outros atributos  
  
|Valor|Descrição|  
|-----------|-----------------|  
|*policy_setting*|A configuração a ser aplicada a este tipo de política para a propriedade. Os valores possíveis são `Auto`, `Excluded`, `Included` e `Required`. Para obter mais informações, consulte [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md).|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|Aplica a política de reflexão a um tipo e todos os seus membros.|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|Aplica a política de reflexão a um tipo genérico construído e todos os seus membros.|  
  
## <a name="remarks"></a>Comentários  
 Se a política da propriedade não for definida explicitamente, ela herdará a política de runtime do seu elemento pai.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa reflexão para instanciar um objeto `Book` e exibir seus valores de propriedade. O arquivo default.rd.xml original para o projeto aparece da seguinte maneira:  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
   <Application>  
      <Namespace Name="LibraryApplications"  Browse="Required Public" >  
         <Type Name="Book"   Activate="All" />  
      </Namespace>  
   </Application>  
</Directives>  
```  
  
 O arquivo se aplica ao valor `All` para a política `Activate` da classe `Book`, que permite acesso aos construtores de classe por meio de reflexão. A política `Browse` para a classe `Book` é herdada do seu namespace pai. Isso é definido para `Required Public`, que disponibiliza metadados no runtime.  
  
 Este é o código-fonte para o exemplo. A `outputBlock` variável representa um <xref:Windows.UI.Xaml.Controls.TextBlock> controle.  
  
 [!code-csharp[ProjectN_Reflection#6](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/property1.cs#6)]  
  
 No entanto, compilar e executar este exemplo gera uma exceção [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md). Embora tenhamos disponibilizados metadados para o tipo `Book` disponível, não realizamos implementações de getters de propriedades disponíveis dinamicamente. Podemos corrigir esse erro de uma das seguintes maneiras:  
  
- definindo a `Dynamic` política para o `Book` tipo em seu [\<Type>](type-element-net-native.md) elemento.  
  
- Adicionando um elemento aninhado [\<Property>](property-element-net-native.md) para cada propriedade cujo getter gostaríamos de invocar, como faz o seguinte arquivo default. Rd. xml.  
  
    ```xml  
    <Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
       <Application>  
          <Namespace Name="LibraryApplications"  Browse="Required Public" >  
             <Type Name="Book"   Activate="All" >  
                <Property Name="Title" Dynamic="Required" />  
                <Property Name="Author" Dynamic="Required" />  
                  <Property Name="ISBN" Dynamic="Required" />  
             </Type>  
          </Namespace>  
       </Application>  
    </Directives>  
    ```  
  
## <a name="see-also"></a>Confira também

- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)
- [Elementos da diretiva de runtime](runtime-directive-elements.md)
- [Configurações da política da diretiva de runtime](runtime-directive-policy-settings.md)
