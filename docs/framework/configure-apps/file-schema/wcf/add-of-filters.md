---
title: <add> de <filters>
ms.date: 03/30/2017
ms.assetid: e3bf437c-dd99-49f3-9792-9a8721e6eaad
ms.openlocfilehash: 280c516b17a133930bc4b6621a8c9bc7f4781085
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70850558"
---
# <a name="add-of-filters"></a>\<add> de \<filters>
Um filtro XPath que especifica o tipo de mensagem a ser registrada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<diagnostics>**](diagnostics.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<messageLogging>**](messagelogging.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<filters>**](filters.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<filters>
  <add filter="String" />
</filters>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|filtro|Uma cadeia de caracteres que especifica uma consulta em um documento XML definido por uma expressão XPath 1,0. Para obter mais informações, consulte <xref:System.ServiceModel.Dispatcher.XPathMessageFilter>.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<filters>](filters.md)|Contém uma coleção de filtros XPath usados para controlar que tipo de mensagem é registrada.|  
  
## <a name="remarks"></a>Comentários  
 Os filtros são aplicados somente na camada de transporte, especificada por `logMessagesAtTransportLevel` is `true` . O nível de serviço e o log de mensagens malformados não são afetados por filtros.  
  
 Para adicionar um filtro à coleção, use a `add` palavra-chave. Quando um ou mais filtros são definidos, somente as mensagens que correspondem a pelo menos um dos filtros são registradas. Se nenhum filtro for definido, todas as mensagens passarão.  
  
 Os filtros dão suporte à sintaxe XPath completa e são aplicados na ordem em que aparecem no arquivo de configuração. Um filtro sintaticamente incorreto resulta em uma exceção de configuração.  
  
 Veja a seguir um exemplo de como configurar um filtro que registra somente as mensagens que têm uma seção de cabeçalho SOAP.  
  
## <a name="example"></a>Exemplo  
 Veja a seguir um exemplo de como configurar um filtro que registra somente as mensagens que têm uma seção de cabeçalho SOAP.  
  
```xml  
<messageLogging logEntireMessage="true"
                logMalformedMessages="true"
                logMessagesAtServiceLevel="true"
                logMessagesAtTransportLevel="true"
                maxMessagesToLog="420">
  <filters>
    <add xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
      /soap:Envelope/soap:Headers
    </add>
  </filters>
</messageLogging>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.DiagnosticSection>
- <xref:System.ServiceModel.Diagnostics>
- <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement.Filters%2A>
- <xref:System.ServiceModel.Configuration.XPathMessageFilterElement>
- <xref:System.ServiceModel.Dispatcher.XPathMessageFilter>
- [Configurando registros de mensagens em log](../../../wcf/diagnostics/configuring-message-logging.md)
- [\<messageLogging>](messagelogging.md)
