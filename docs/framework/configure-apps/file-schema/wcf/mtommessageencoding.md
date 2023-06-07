---
title: <mtomMessageEncoding>
ms.date: 03/30/2017
ms.assetid: 7865d171-cd1e-430a-8421-39cc13541d1b
ms.openlocfilehash: bd38bf812e6d8d9e57d99bf1a5b77ebb776193a5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738842"
---
# \<mtomMessageEncoding>
Especifica a codificação e o controle de versão de mensagem usados para mensagens baseadas no mecanismo de otimização de transmissão de mensagens SOAP (MTOM).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<mtomMessageEncoding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<mtomMessageEncoding maxBufferSize="Integer"
                     maxReadPoolSize="Integer"
                     maxWritePoolSize="Integer"
                     messageVersion="Soap11Addressing1/Soap12Addressing10"
                     writeEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|maxBufferSize|Um inteiro que especifica o tamanho máximo do buffer que pode ser usado.|  
|maxReadPoolSize|Um inteiro que especifica quantas mensagens podem ser lidas simultaneamente sem alocar novos leitores. Tamanhos de pool maiores tornam o sistema mais tolerante a picos de atividade no custo de um conjunto de trabalho maior. O padrão é 64.|  
|maxWritePoolSize|Um inteiro que especifica quantas mensagens podem ser enviadas simultaneamente sem alocar novos gravadores. Tamanhos de pool maiores tornam o sistema mais tolerante a picos de atividade no custo de um conjunto de trabalho maior. O padrão é 16.|  
|messageVersion|Especifica a versão SOAP das mensagens enviadas usando a associação. Os valores válidos são<br /><br /> - Soap11Addressing1<br />- Soap12Addressing10<br /><br /> O padrão é Soap12Addressing10. Esse atributo é do tipo <xref:System.ServiceModel.Channels.MessageVersion> .|  
|writeEncoding|Especifica a codificação do conjunto de caracteres a ser usada para emitir mensagens na associação. Os valores válidos são<br /><br /> -UnicodeFffeTextEncoding: codificação BigEndian Unicode<br />-Utf16TextEncoding: codificação Unicode<br />-Utf8TextEncoding: codificação de 8 bits<br /><br /> O padrão é Utf8TextEncoding. Esse atributo é do tipo <xref:System.Text.Encoding> .|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens SOAP que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 A codificação é o processo de transformar uma mensagem em uma sequência de bytes. A decodificação é o processo reverso. O Windows Communication Foundation (WCF) inclui três tipos de codificação para mensagens SOAP: texto, binário e MTOM (mecanismo de otimização de transmissão de mensagens).  
  
 O `MtomMessageEncoding` elemento Especifica a codificação de caracteres e o controle de versão de mensagem e outras configurações usadas para mensagens usando uma codificação MTOM (mecanismo de otimização de transmissão de mensagens). O MTOM é uma tecnologia eficiente para transmitir dados binários em mensagens do WCF. O codificador MTOM tenta criar um equilíbrio entre eficiência e interoperabilidade. A codificação MTOM transmite a maioria dos XML na forma textual, mas otimiza grandes blocos de dados binários transmitindo-os como estão, sem conversão para seu formato codificado em base64.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<mtomMessageEncoding maxReadPoolSize="211"
                     maxWritePoolSize="2132"
                     messageVersion="Soap11Addressing10"
                     textEncoding="utf-8" />
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.MtomMessageEncodingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>
- <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>
- [Decodificador de mensagens](message-encoding.md)
- [Escolhendo um codificador de mensagem](../../../wcf/feature-details/choosing-a-message-encoder.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
