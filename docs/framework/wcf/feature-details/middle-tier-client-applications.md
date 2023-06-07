---
title: Aplicativos cliente de camada intermediária
ms.date: 03/30/2017
ms.assetid: f9714a64-d0ae-4a98-bca0-5d370fdbd631
ms.openlocfilehash: c50223a55765f211dae710f96bffa7716ce36b32
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598808"
---
# <a name="middle-tier-client-applications"></a>Aplicativos cliente de camada intermediária
Este tópico discute vários problemas específicos para aplicativos cliente de camada intermediária que usam o Windows Communication Foundation (WCF).  
  
## <a name="increasing-middle-tier-client-performance"></a>Aumento do desempenho do cliente de camada intermediária  
 Em comparação com as tecnologias de comunicação anteriores, como serviços Web usando ASP.NET, a criação de uma instância de cliente WCF pode ser mais complexa devido ao rico conjunto de recursos do WCF. Por exemplo, quando um <xref:System.ServiceModel.ChannelFactory%601> objeto é aberto, ele pode estabelecer uma sessão segura com o serviço, um procedimento que aumenta o tempo de inicialização da instância do cliente. Normalmente, esses recursos de recurso adicionais não afetam os aplicativos cliente, uma vez que o cliente WCF faz várias chamadas e, em seguida, fecha.  
  
 Os aplicativos cliente de camada intermediária, no entanto, podem criar muitos objetos de cliente WCF rapidamente e, como resultado, experimentar maiores requisitos de inicialização. Há duas abordagens principais para aumentar o desempenho de aplicativos de camada intermediária ao chamar serviços:  
  
- Armazene o objeto do cliente WCF em cache e reutilize-o para chamadas subsequentes sempre que possível.  
  
- Crie um <xref:System.ServiceModel.ChannelFactory%601> objeto e, em seguida, use esse objeto para criar novos objetos de canal do cliente WCF para cada chamada.  
  
 Os problemas a serem considerados ao usar essas abordagens incluem:  
  
- Se o serviço estiver mantendo um estado específico do cliente usando uma sessão, você não poderá reutilizar o cliente WCF de camada intermediária com solicitações de camada de vários clientes porque o estado do serviço está vinculado a um dos clientes da camada intermediária.  
  
- Se o serviço precisar executar a autenticação por cliente, você deverá criar um novo cliente para cada solicitação de entrada na camada intermediária, em vez de reutilizar o cliente WCF de camada intermediária (ou o objeto de canal do cliente WCF) porque as credenciais de cliente da camada intermediária não podem ser modificadas depois que o cliente WCF (ou <xref:System.ServiceModel.ChannelFactory%601> ) tiver sido criado.  
  
- Embora os canais e clientes criados pelos canais sejam thread-safe, eles podem não dar suporte à gravação de mais de uma mensagem na conexão simultaneamente. Se você estiver enviando mensagens grandes, especialmente se for streaming, a operação de envio poderá bloquear a espera por outro envio para ser concluída. Isso causa dois tipos de problemas: uma falta de simultaneidade e a possibilidade de deadlock se o fluxo de controle retornar ao serviço reutilizando o canal (ou seja, o cliente compartilhado chama um serviço cujo caminho de código resulta em um retorno de chamada para o cliente compartilhado). Isso é verdadeiro independentemente do tipo de cliente WCF que você reutiliza.  
  
- Você deve lidar com canais com falha independentemente de compartilhar o canal. No entanto, quando os canais são reutilizados, um canal com falha pode reduzir mais de uma solicitação pendente ou enviar.  
  
 Para obter um exemplo que demonstra as práticas recomendadas para reutilizar um cliente para várias solicitações, consulte [vinculação de dados em um cliente ASP.net](../samples/data-binding-in-an-aspnet-client.md).  
  
 Além disso, você pode aumentar o desempenho de inicialização para os clientes que usam tipos de dados que são serializáveis usando o <xref:System.Xml.Serialization.XmlSerializer> código de serialização Generate e compile para esses tipos de dados no tempo de execução, o que pode resultar em um desempenho de inicialização lento. A [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) pode melhorar o desempenho de inicialização para esses aplicativos, gerando o código de serialização necessário dos assemblies compilados para o aplicativo. Para obter mais informações, consulte [como: melhorar o tempo de inicialização de aplicativos cliente WCF usando o XmlSerializer](startup-time-of-wcf-client-applications-using-the-xmlserializer.md).  
  
## <a name="see-also"></a>Consulte também

- [Usando um cliente do WCF para acessar serviços](accessing-services-using-a-client.md)
