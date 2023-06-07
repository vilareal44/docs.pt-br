---
title: Como criar uma associação de sessão confiável personalizada com HTTPS
ms.date: 03/30/2017
ms.assetid: fa772232-da1f-4c66-8c94-e36c0584b549
ms.openlocfilehash: 70f8f4f33626ab0d1705e03750bfd9baa324e60a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598990"
---
# <a name="how-to-create-a-custom-reliable-session-binding-with-https"></a>Como criar uma associação de sessão confiável personalizada com HTTPS

Este tópico demonstra o uso de segurança de transporte de protocolo SSL (SSL) com sessões confiáveis. Para usar uma sessão confiável por HTTPS, você deve criar uma associação personalizada que usa uma sessão confiável e o transporte HTTPS. Você pode habilitar a sessão confiável de maneira imperativa usando código ou declarativamente no arquivo de configuração. Este procedimento usa os arquivos de configuração de cliente e serviço para habilitar a sessão confiável e o [**\<httpsTransport>**](../../configure-apps/file-schema/wcf/httpstransport.md) elemento.

A parte principal desse procedimento é que o **\<endpoint>** elemento de configuração contém um `bindingConfiguration` atributo que faz referência a uma configuração de associação personalizada denominada `reliableSessionOverHttps` . O [**\<binding>**](../../configure-apps/file-schema/wcf/bindings.md) elemento de configuração faz referência a esse nome para especificar que uma sessão confiável e o transporte HTTPS sejam usados por meio da inclusão **\<reliableSession>** de **\<httpsTransport>** elementos e.

Para a cópia de origem deste exemplo, consulte [sessão confiável de associação personalizada sobre HTTPS](../samples/custom-binding-reliable-session-over-https.md).

### <a name="configure-the-service-with-a-custombinding-to-use-a-reliable-session-with-https"></a>Configurar o serviço com uma Personalizadabinding para usar uma sessão confiável com HTTPS

1. Defina um contrato de serviço para o tipo de serviço.

   [!code-csharp[c_HowTo_CreateReliableSessionHTTPS#1121](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/service.cs#1121)]

1. Implemente o contrato de serviço em uma classe de serviço. Observe que as informações de endereço ou de associação não são especificadas dentro da implementação do serviço. Não é necessário escrever código para recuperar o endereço ou as informações de associação do arquivo de configuração.

   [!code-csharp[c_HowTo_CreateReliableSessionHTTPS#1122](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/service.cs#1122)]

1. Crie um arquivo *Web. config* para configurar um ponto de extremidade para o `CalculatorService` com uma associação personalizada denominada `reliableSessionOverHttps` que usa uma sessão confiável e o transporte HTTPS.

   [!code-xml[c_HowTo_CreateReliableSessionHTTPS#2111](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/common/web.config#2111)]

1. Crie um arquivo *Service. svc* que contenha a linha:

   `<%@ServiceHost language=c# Service="CalculatorService" %>`

1. Coloque o arquivo *Service. svc* no diretório virtual do serviços de informações da Internet (IIS).

### <a name="configure-the-client-with-a-custombinding-to-use-a-reliable-session-with-https"></a>Configurar o cliente com uma Personalizadabinding para usar uma sessão confiável com HTTPS

1. Use a [ferramenta de utilitário de metadados ServiceModel (*svcutil. exe*)](../servicemodel-metadata-utility-tool-svcutil-exe.md) da linha de comando para gerar código de metadados de serviço.

   ```console
   Svcutil.exe <Metadata Exchange (MEX) address or HTTP GET address>
   ```

1. O cliente gerado contém a `ICalculator` interface que define o contrato de serviço que a implementação do cliente deve satisfazer.

   [!code-csharp[C_HowTo_CreateReliableSessionHTTPS#1221](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/client.cs#1221)]

1. O aplicativo cliente gerado também contém a implementação do `ClientCalculator` . Observe que as informações de endereço e de associação não são especificadas dentro da implementação do serviço. Não é necessário escrever código para recuperar o endereço e as informações de associação do arquivo de configuração.

   [!code-csharp[C_HowTo_CreateReliableSessionHTTPS#1222](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/client.cs#1222)]

1. Configure uma associação personalizada chamada `reliableSessionOverHttps` para usar o transporte HTTPS e as sessões confiáveis.

   [!code-xml[C_HowTo_CreateReliableSessionHTTPS#2211](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/common/app.config#2211)]

1. Crie uma instância do `ClientCalculator` em um aplicativo e, em seguida, chame as operações de serviço.

   [!code-csharp[C_HowTo_CreateReliableSessionHTTPS#1223](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/client.cs#1223)]

1. Compile e execute o cliente.  

## <a name="net-framework-security"></a>Segurança do .NET Framework

Como o certificado usado neste exemplo é um certificado de teste criado com *MakeCert. exe*, um alerta de segurança é exibido quando você tenta acessar um endereço https, como `https://localhost/servicemodelsamples/service.svc` , no seu navegador.

## <a name="see-also"></a>Consulte também

- [Sessões confiáveis](reliable-sessions.md)
