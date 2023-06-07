---
title: <webMessageEncoding>
ms.date: 03/30/2017
ms.assetid: 892ca485-e21a-4a44-8e40-633161ef6796
ms.openlocfilehash: 4aa87acaf9080959ba8b53e3ec3216314dc745b6
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73732585"
---
# \<webMessageEncoding>
Habilita XML de texto simples, codificações mensagem JSON (JavaScript Object Notation) e conteúdo binário "bruto" para ser lido e gravado quando usado em uma associação WCF (Windows Communication Foundation).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<webMessageEncoding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<webMessageEncoding maxReadPoolSize="Integer"
                    maxWritePoolSize="Integer"
                    writeEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`maxReadPoolSize`|A quantidade de mensagens que podem ser lidas simultaneamente sem alocar novos leitores. Tamanhos de pool maiores tornam o sistema mais tolerante a picos de atividade no custo de um conjunto de trabalho maior. O padrão é 64 leitores para cada um dos codificadores internos (text, JSON e "RAW").<br /><br /> O aumento desse número aumenta o consumo de memória, mas prepara o codificador para lidar com picos repentinos de mensagens de entrada, pois ele é capaz de usar leitores do pool que já foram criados, em vez de criar novos.|  
|`maxWritePoolSize`|A quantidade de mensagens que podem ser enviadas simultaneamente sem alocar novos gravadores. Tamanhos de pool maiores tornam o sistema mais tolerante a picos de atividade no custo de um conjunto de trabalho maior. O padrão é 16 gravadores para cada um dos codificadores internos (text, JSON e "RAW").<br /><br /> O aumento desse número aumenta o consumo de memória, mas prepara o codificador para lidar com picos repentinos de mensagens de saída, pois ele é capaz de usar gravadores do pool que já foram criados, em vez de criar novos.|  
|`writeEncoding`|Especifica a codificação do conjunto de caracteres a ser usada para emitir mensagens na associação. Os valores válidos são:<br /><br /> -UnicodeFffeTextEncoding: codificação Unicode big endian.<br />-Utf16TextEncoding: codificação Unicode.<br />-Utf8TextEncoding: codificação de 8 bits.<br /><br /> O padrão é Utf8TextEncoding. Esse atributo é do tipo <xref:System.Text.Encoding> .|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens SOAP que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 A codificação é o processo de transformar uma mensagem em uma sequência de bytes. A decodificação é o processo reverso. Esses processos exigem a especificação de uma codificação de caracteres.  
  
 O `webMessageEncoding` elemento funciona delegando a uma série de codificadores internos para manipular as codificações XML e JSON de texto sem formatação e dados binários "brutos". Essa delegação é feita por um codificador de mensagem composta.  
  
 Esse elemento Binding e seu codificador composto são usados para controlar a codificação em cenários que não usam mensagens SOAP usadas pelo `webHttpBinding` elemento. Esses cenários incluem "XML antigo" (POX), transferência de estado de reapresentação (REST), agregação real (RSS) e agregação Atom, além de JavaScript e XML assíncronos (AJAX). O codificador de mensagens compostas não dá suporte a SOAP ou WS-Addressing.  
  
 O elemento Binding pode ser configurado com uma codificação Write Character usando o `writeEncoding` atributo. O <xref:System.Text.Encoding> valor fornecido especifica o comportamento na gravação para os casos JSON e XML textual. Na leitura, qualquer codificação de mensagem e codificação de texto válidas são compreendidas.  
  
 `maxReadPoolSize`e `maxWritePoolSize` também pode ser usado para definir o número máximo de leitores e gravadores a serem alocados, respectivamente. Por padrão, os leitores 64 e 16 gravadores são alocados.  
  
 As restrições de complexidade padrão também são definidas usando o [\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100)) elemento para proteger contra uma classe de ataques de dos (negação de serviço) que tentam usar a complexidade da mensagem para vincular os recursos de processamento do ponto de extremidade.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<webMessageEncoding maxReadPoolSize="256"
                    maxWritePoolSize="128"
                    messageVersion="None"
                    textEncoding="utf-8" />
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.WebMessageEncodingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>
- <xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement>
- [Decodificador de mensagens](message-encoding.md)
- [Escolhendo um codificador de mensagem](../../../wcf/feature-details/choosing-a-message-encoder.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
