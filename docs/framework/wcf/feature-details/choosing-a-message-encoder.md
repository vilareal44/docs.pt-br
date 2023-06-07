---
title: Escolhendo um codificador de mensagem
ms.date: 03/30/2017
ms.assetid: 2204d82d-d962-4922-a79e-c9a231604f19
ms.openlocfilehash: dbc5981013fe5e023f1d6d9eaf64b2e1fa18e2df
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84587333"
---
# <a name="choose-a-message-encoder"></a>Escolher um codificador de mensagem

Este artigo discute os critérios para escolher entre os codificadores de mensagem que estão incluídos no Windows Communication Foundation (WCF): binário, texto e MTOM (mecanismo de otimização de transmissão de mensagens).  
  
 No WCF, você especifica como transferir dados em uma rede entre pontos de extremidade por meio de uma *Associação*, que é composta de uma sequência de elementos de *Associação*. Um codificador de mensagem é representado por um elemento de ligação de codificação de mensagem na pilha de associação. Uma associação inclui elementos de associação de protocolo opcionais, como um elemento de associação de segurança ou um elemento de associação de mensagens confiáveis, um elemento de associação de codificação de mensagem necessário e um elemento de associação de transporte necessário.  
  
 O elemento de associação de codificação de mensagens está abaixo dos elementos opcionais de associação de protocolo e acima do elemento de associação de transporte necessário. No lado de saída, um codificador de mensagem serializa o saída <xref:System.ServiceModel.Channels.Message> e o passa para o transporte. No lado de entrada, um codificador de mensagem recebe a forma serializada de um <xref:System.ServiceModel.Channels.Message> do transporte e a passa para a camada de protocolo mais alta, se presente, ou para o aplicativo, se não.  
  
 Ao se conectar a um cliente ou servidor pré-existente, talvez você não tenha a opção de usar uma codificação de mensagem específica, já que precisa codificar suas mensagens de uma forma que o outro lado esteja esperando. No entanto, se você estiver escrevendo um serviço WCF, poderá expor seu serviço por meio de vários pontos de extremidade, cada um usando uma codificação de mensagem diferente. Isso permite que os clientes escolham a melhor codificação para conversar com seu serviço sobre o ponto de extremidade que é melhor para eles, além de dar aos seus clientes a flexibilidade de escolher a codificação mais adequada para eles. O uso de vários pontos de extremidade também permite combinar as vantagens de diferentes codificações de mensagens com outros elementos de ligação.  
  
## <a name="system-provided-encoders"></a>Codificadores fornecidos pelo sistema  
 O WCF inclui três codificadores de mensagens, que são representados pelas três classes a seguir:  
  
- <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>, o codificador de mensagem de texto oferece suporte tanto à codificação XML simples quanto à codificação SOAP. O modo de codificação XML sem formatação do codificador de mensagem de texto é chamado de "XML antigo" (POX) para distingui-lo da codificação SOAP baseada em texto. Para habilitar POX, defina a <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement.MessageVersion%2A> propriedade como <xref:System.ServiceModel.Channels.MessageVersion.None%2A> . Use o codificador de mensagem de texto para interoperar com pontos de extremidade não WCF.  
  
- <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>, o codificador de mensagem binária usa um formato binário compacto e é otimizado para comunicação WCF para WCF e, portanto, não é interoperável. Esse também é o codificador de melhor desempenho de todos os concodificadores que o WCF fornece.  
  
- <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>, o elemento Binding, especifica a codificação de caracteres e o controle de versão de mensagem para mensagens usando a codificação MTOM. O MTOM é uma tecnologia eficiente para transmitir dados binários em mensagens do WCF. O codificador MTOM tenta criar um equilíbrio entre eficiência e interoperabilidade. A codificação MTOM transmite a maioria dos XML na forma textual, mas otimiza grandes blocos de dados binários transmitindo-os como estão, sem conversão em texto. Em termos de eficiência, entre os codificadores que o WCF fornece, o MTOM está entre o texto (o mais lento) e o binário (o mais rápido).  
  
## <a name="how-to-choose-a-message-encoder"></a>Como escolher um codificador de mensagem  
 A tabela a seguir descreve os fatores comuns usados para escolher um codificador de mensagem. Priorize os fatores que são importantes para seu aplicativo e, em seguida, escolha os codificadores de mensagem que funcionam melhor com esses fatores. Certifique-se de considerar quaisquer fatores adicionais não listados nesta tabela e quaisquer codificadores de mensagem personalizados que possam ser necessários em seu aplicativo.  
  
|Fator|Descrição|Codificadores que dão suporte a esse fator|  
|------------|-----------------|---------------------------------------|  
|Conjuntos de caracteres com suporte|<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>e <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> dão suporte apenas às codificações UTF8 e UTF16 Unicode (*big-endian* e *little-endian*). Se outras codificações forem necessárias, como UTF7 ou ASCII, um codificador personalizado deverá ser usado. Para obter um codificador personalizado de exemplo, consulte [codificador de mensagem personalizada](https://docs.microsoft.com/dotnet/framework/wcf/samples/custom-message-encoder-custom-text-encoder).|Texto|  
|Inspeção|A inspeção é a capacidade de examinar as mensagens durante a transmissão. Codificações de texto, com ou sem o uso de SOAP, permitem que as mensagens sejam inspecionadas e analisadas por muitos aplicativos sem o uso de ferramentas especializadas. O uso da segurança de transferência, no nível de mensagem ou de transporte, afeta sua capacidade de inspecionar mensagens. A confidencialidade protege uma mensagem de ser examinada e a integridade protege a modificação de uma mensagem.|Texto|  
|Confiabilidade|Confiabilidade é a resiliência de um codificador para erros de transmissão. A confiabilidade também pode ser fornecida na mensagem, no transporte ou na camada de aplicativo. Todos os codificadores do WCF padrão pressupõem que outra camada esteja fornecendo confiabilidade. O codificador tem pouca capacidade de se recuperar de um erro de transmissão.|Nenhum|  
|Simplicidade|A simplicidade representa a facilidade com a qual você pode criar codificadores e decodificadores para uma especificação de codificação. As codificações de texto são particularmente vantajosas para simplificar, e a codificação de texto POX tem a vantagem adicional de não exigir suporte para o processamento de SOAP.|Texto (POX)|  
|Tamanho|A codificação determina a quantidade de sobrecarga imposta no conteúdo. O tamanho das mensagens codificadas está diretamente relacionado à taxa de transferência máxima de operações de serviço. Em geral, as codificações binárias são mais compactas do que codificações de texto. Quando o tamanho da mensagem estiver em um nível Premium, considere também compactar o conteúdo da mensagem durante a codificação. No entanto, a compactação adiciona custos de processamento para o remetente e o destinatário da mensagem.|Binário|  
|Streaming|O streaming permite que os aplicativos comecem a processar uma mensagem antes que toda a mensagem tenha chegado. Usar o streaming com eficiência requer que os dados importantes de uma mensagem estejam disponíveis no início da mensagem para que o aplicativo de recebimento não precise esperar que ele chegue. Além disso, os aplicativos que usam transferência em fluxo devem organizar os dados na mensagem de forma incremental para que o conteúdo não tenha dependências de encaminhamento. Em muitos casos, você deve comprometer entre o conteúdo de streaming e ter o menor tamanho de transferência possível para esse conteúdo.|Nenhum|  
|Suporte a ferramentas de terceiros|As áreas de suporte para uma codificação incluem desenvolvimento e diagnóstico. Os desenvolvedores de terceiros fizeram um grande investimento em bibliotecas e kits de programas para lidar com mensagens codificadas no formato POX.|Texto (POX)|  
|Interoperabilidade|Esse fator refere-se à capacidade de um codificador do WCF interoperar com serviços não WCF.|Texto<br /><br /> MTOM (parcial)|  
  
Observação: ao usar o codificador binário, o uso da configuração IgnoreWhitespace ao criar um XMLReader não terá nenhum efeito.  Por exemplo, se você fizer o seguinte dentro de uma operação de serviço:  

```csharp
public void OperationContract(XElement input)
{
    Console.WriteLine("{0}", input.Value);
    int counter = 0;
    var xreader = input.CreateReader();
    var reader = XmlReader.Create(xreader, new XmlReaderSettings() { IgnoreWhitespace = true });
    while (reader.Read())
    {
        counter++;
    }

    Console.WriteLine("Read {0} lines with reader", counter);
}
```  
  
A configuração IgnoreWhitespace é ignorada.  
  
## <a name="compression-and-the-binary-encoder"></a>Compactação e o codificador binário

A partir do WCF 4.5, o codificador binário do WCF adiciona suporte para compactação. Isso permite que você use o algoritmo gzip/deflat para enviar mensagens compactadas de um cliente WCF e também responder com mensagens compactadas de um serviço WCF hospedado internamente. Esse recurso habilita a compactação nos transportes HTTP e TCP. Um serviço WCF hospedado do IIS sempre pode ser habilitado para enviar respostas compactadas Configurando o servidor host IIS. O tipo de compactação é configurado com a propriedade <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.CompressionFormat%2A?displayProperty=nameWithType>. Essa propriedade é definida como um dos <xref:System.ServiceModel.Channels.CompressionFormat?displayProperty=nameWithType> valores de enumeração:

- <xref:System.ServiceModel.Channels.CompressionFormat.Deflate>
- <xref:System.ServiceModel.Channels.CompressionFormat.GZip>
- <xref:System.ServiceModel.Channels.CompressionFormat.None>
  
Como essa propriedade só é exposta no binaryMessageEncodingBindingElement, você precisará criar uma associação personalizada como a seguinte para usar esse recurso:

 ```xml
 <customBinding>
   <binding name="BinaryCompressionBinding">
     <binaryMessageEncoding compressionFormat ="GZip" />
     <httpTransport />
  </binding>
</customBinding>
 ```

O cliente e o serviço precisam concordar em enviar e receber mensagens compactadas e, portanto, a propriedade compressionFormat deve ser configurada no elemento binaryMessageEncoding no cliente e no serviço. Uma ProtocolException será gerada se o serviço ou o cliente não estiver configurado para compactação, mas o outro lado for. A habilitação da compactação deve ser cuidadosamente considerada. A compactação será útil principalmente se a largura de banda da rede for um afunilamento. No caso em que a CPU é o afunilamento, a compactação diminuirá a taxa de transferência. Os testes apropriados devem ser feitos em um ambiente simulado para descobrir se isso beneficia o aplicativo  
  
## <a name="see-also"></a>Confira também

- [Associações](bindings.md)
