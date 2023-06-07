---
title: <comContract>
ms.date: 03/30/2017
ms.assetid: 3f8e1c0c-cfdf-4c79-ac65-c64e9323a51c
ms.openlocfilehash: b499294af71ba230dcf985d4af1d013b1ca260cf
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70850033"
---
# \<comContract>
Especifica um contrato de serviço de integração COM+.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<comContracts>**](comcontracts.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<comContract>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<comContracts>
  <comContract contract="String"
               namespace="String"
               name="String"
               requireSession="Boolean">
    <exposedMethods>
      <exposedMethod name="String" />
    </exposedMethods>
    <userDefinedTypes>
      <userDefinedType name="String"
                       typeLibID="String"
                       typeLibVersion="String"
                       typeDefID="String">
      </userDefinedType>
    </userDefinedTypes>
    <persistableTypes>
      <persistableType id="String"
                       name="String">
      </persistableType>
    </persistableTypes>
  </comContract>
</comContracts>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|contrato|Uma cadeia de caracteres que contém o tipo de contrato.|  
|name|Uma cadeia de caracteres que contém o nome do contrato.|  
|namespace|Uma cadeia de caracteres que contém o namespace do contrato.|  
|requiresSession|Um valor booliano que especifica se o contrato só pode ser usado em associações de sessão. Quando o serviço é inicializado, o Integration Runtime garante que essa configuração seja consistente com o tipo de associação a ser usada. Uma exceção será gerada se uma ou mais das associações para o contrato estiverem em conflito. Se essa propriedade for `false` , e um canal unidirecional estiver em uso e houver quaisquer parâmetros [out], uma exceção também será gerada.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|persistableTypes|Todos os tipos persistentes.|  
|userDefinedTypes|Uma coleção de tipos definidos pelo usuário (UDT) que deve ser incluída no contrato de serviço.|  
|exposedMethods|Uma coleção de métodos COM+ que são expostos quando a interface em um componente COM+ é exposta como um serviço Web.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|comContracts|Contém uma coleção de `comContract` elementos.|  
  
## <a name="remarks"></a>Comentários  
 Os contratos do serviço de integração COM+ estão atualmente restritos ao `http://tempuri.org` namespace e o nome do contrato é derivado da interface com de suporte. No entanto, você pode especificar alternativas usando a `comContracts` seção, bem como o `comContract` elemento no arquivo de configuração. Por exemplo, você pode usar a configuração a seguir para especificar o namespace, o nome do contrato e os tipos definidos pelo usuário a serem incluídos, bem como outras configurações para um contrato de serviço.  
  
```xml  
<comContracts>
  <comContract contract="{5163B1E7-F0CF-4B6A-9A02-4AB654F34284}"
               namespace="http://tempuri.org/5163B1E7-F0CF-4B6A-9A02-4AB654F34284"
               name="_Broker"
               requireSession="true">
    <exposedMethods>
      <exposedMethod name="BuyStock" />
      <exposedMethod name="SellStock" />
      <exposedMethod name="ExecuteTransaction" />
    </exposedMethods>
  </comContract>
</comContracts>
```  
  
 Quando o serviço é inicializado, os namespaces e os nomes de contrato especificados são aplicados às descrições de serviço geradas.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ComContractElementCollection>
- <xref:System.ServiceModel.Configuration.ComContractElement>
- [\<comContracts>](comcontracts.md)
- [Integração com aplicativos COM+](../../../wcf/feature-details/integrating-with-com-plus-applications.md)
- [Como configurar configurações de serviço de COM+](../../../wcf/feature-details/how-to-configure-com-service-settings.md)
