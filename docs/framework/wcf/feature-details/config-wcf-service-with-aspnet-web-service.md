---
title: Como configurar um serviço do WCF para interoperar com os clientes de serviço Web do ASP.NET
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 48e1cd90-de80-4d6c-846e-631878955762
ms.openlocfilehash: 52f7857a2dc7108eb308fde942bf153d85d8e8ed
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593601"
---
# <a name="how-to-configure-wcf-service-to-interoperate-with-aspnet-web-service-clients"></a>Como configurar um serviço do WCF para interoperar com os clientes de serviço Web do ASP.NET

Para configurar um ponto de extremidade de serviço do Windows Communication Foundation (WCF) para ser interoperável com clientes de serviço Web ASP.NET, use o <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType> tipo como o tipo de associação para o ponto de extremidade de serviço.  
  
 Opcionalmente, você pode habilitar o suporte para HTTPS e autenticação de cliente de nível de transporte na associação. Os clientes do serviço Web ASP.NET não dão suporte à codificação de mensagem MTOM, portanto, a <xref:System.ServiceModel.BasicHttpBinding.MessageEncoding%2A?displayProperty=nameWithType> propriedade deve ser deixada como seu valor padrão, que é <xref:System.ServiceModel.WSMessageEncoding.Text?displayProperty=nameWithType> . Os clientes do serviço Web ASP.NET não dão suporte ao WS-Security, portanto, <xref:System.ServiceModel.BasicHttpBinding.Security%2A?displayProperty=nameWithType> deve ser definido como <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> .  
  
 Para disponibilizar os metadados para um serviço WCF para ferramentas de geração de proxy do serviço Web do ASP.NET (ou seja, [ferramenta de linguagem de descrição de serviços Web (WSDL. exe)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/7h3ystb6(v%3dvs.100)), [ferramenta de descoberta de serviços Web (disco. exe)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cy2a3ybs(v=vs.100))e o recurso **Adicionar referência Web** no Visual Studio), você deve expor um ponto de extremidade de metadados http/Get.  
  
## <a name="add-an-endpoint-in-code"></a>Adicionar um ponto de extremidade no código  
  
1. Criar uma nova <xref:System.ServiceModel.BasicHttpBinding> instância  
  
2. Opcionalmente, habilite a segurança de transporte para essa associação de ponto de extremidade de serviço definindo o modo de segurança para a associação a <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> . Para obter detalhes, consulte [segurança de transporte](transport-security.md).  
  
3. Adicione um novo ponto de extremidade de aplicativo ao host de serviço usando a instância de associação que você acabou de criar. Para obter detalhes sobre como adicionar um ponto de extremidade de serviço no código, consulte o [como criar um ponto de extremidade de serviço em código](how-to-create-a-service-endpoint-in-code.md).  
  
4. Habilite um ponto de extremidade de metadados HTTP/GET para seu serviço. Para obter detalhes, consulte [como publicar metadados para um serviço usando código](how-to-publish-metadata-for-a-service-using-code.md).  
  
## <a name="add-an-endpoint-in-a-configuration-file"></a>Adicionar um ponto de extremidade em um arquivo de configuração  
  
1. Crie uma nova <xref:System.ServiceModel.BasicHttpBinding> configuração de associação. Para obter detalhes, consulte [como especificar uma associação de serviço na configuração](../how-to-specify-a-service-binding-in-configuration.md).  
  
2. Opcionalmente, habilite a segurança de transporte para essa configuração de associação de ponto de extremidade de serviço definindo o modo de segurança para a associação a <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> . Para obter detalhes, consulte [segurança de transporte](transport-security.md).  
  
3. Configure um novo ponto de extremidade de aplicativo para seu serviço usando a configuração de associação que você acabou de criar. Para obter detalhes sobre como adicionar um ponto de extremidade de serviço em um arquivo de configuração, consulte [como criar um ponto de extremidade de serviço na configuração](how-to-create-a-service-endpoint-in-configuration.md).  
  
4. Habilite um ponto de extremidade de metadados HTTP/GET para seu serviço. Para obter detalhes, consulte [como publicar metadados para um serviço usando um arquivo de configuração](how-to-publish-metadata-for-a-service-using-a-configuration-file.md).  
  
## <a name="example"></a>Exemplo  
 O código de exemplo a seguir demonstra como adicionar um ponto de extremidade WCF compatível com clientes de serviço Web ASP.NET em código e, como alternativa, em arquivos de configuração.  
  
 [!code-csharp[C_HowTo-WCFServiceAndASMXClient#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/cs/program.cs#0)]
 [!code-vb[C_HowTo-WCFServiceAndASMXClient#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/vb/program.vb#0)]
 [!code-xml[C_HowTo-WCFServiceAndASMXClient#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/common/app.config#1)]
  
## <a name="see-also"></a>Confira também

- [Como criar um ponto de extremidade de serviço em código](how-to-create-a-service-endpoint-in-code.md)
- [Como publicar metadados utilizando código para um serviço](how-to-publish-metadata-for-a-service-using-code.md)
- [Como especificar uma associação de serviço na configuração](../how-to-specify-a-service-binding-in-configuration.md)
- [Como criar um ponto de extremidade de serviço em configuração](how-to-create-a-service-endpoint-in-configuration.md)
- [Como publicar metadados para um serviço usando um arquivo de configuração](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)
- [Segurança de transporte](transport-security.md)
- [Utilizando metadados](using-metadata.md)
