---
title: 'Antecipando a adoção do Windows Communication Foundation: facilitando a futura integração'
ms.date: 03/30/2017
ms.assetid: 3028bba8-6355-4ee0-9ecd-c56e614cb474
ms.openlocfilehash: 6392194b3124c999031123225dcc28c8de6171e9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576519"
---
# <a name="anticipating-adopting-the-windows-communication-foundation-easing-future-integration"></a>Antecipando a adoção do Windows Communication Foundation: facilitando a futura integração
Se você usar o ASP.NET hoje e antecipar o uso do WCF no futuro, este tópico fornecerá orientação para garantir que os novos serviços Web do ASP.NET funcionarão bem junto com os aplicativos WCF.  
  
## <a name="general-recommendations"></a>Recomendações gerais  
 Adote o ASP.NET 2,0 para novos serviços. Isso dará acesso aos aprimoramentos e aprimoramentos da nova versão. No entanto, ele também permitirá a possibilidade de usar componentes do ASP.NET 2,0 junto com os componentes do WCF no mesmo aplicativo.  
  
## <a name="protocols"></a>Protocolos  
 Use o novo recurso do ASP.NET 2.0 para validar a conformidade com o WS-I Basic Profile 1,1:  
  
```csharp  
[WebService(Namespace = "http://tempuri.org/")]  
[WebServiceBinding(  
     ConformsTo = WsiProfiles.BasicProfile1_1,  
     EmitConformanceClaims=true)]  
public interface IEcho  
```  
  
 Os serviços Web do ASP.NET que estão em conformidade com o WS-I Basic Profile 1,1 serão interoperáveis com clientes WCF usando a associação predefinida do WCF, <xref:System.ServiceModel.BasicHttpBinding> .  
  
## <a name="service-development"></a>Desenvolvimento do serviço  
 Evite usar o <xref:System.Web.Services.Protocols.SoapDocumentServiceAttribute> atributo para ter mensagens roteadas para métodos com base no nome totalmente qualificado do elemento body da mensagem SOAP em vez do cabeçalho HTTP SoapAction. O WCF usa o cabeçalho HTTP SOAPaction para mensagens de roteamento.  
  
## <a name="data-representation"></a>Representação de dados  
 O XML no qual <xref:System.Xml.Serialization.XmlSerializer> serializa um tipo por padrão é semanticamente idêntico ao XML no qual o <xref:System.Runtime.Serialization.DataContractSerializer> serializa um tipo, desde que o namespace para o XML seja definido explicitamente. Ao definir um tipo de dados para uso com serviços Web do ASP.NET em antecipação de adoção do WCF no futuro, faça o seguinte:  
  
1. Defina o tipo usando .NET Framework classes em vez de esquema XML.  
  
2. Adicione somente o <xref:System.SerializableAttribute> e o <xref:System.Xml.Serialization.XmlRootAttribute> à classe, usando o último para definir explicitamente o namespace para o tipo. Não adicione atributos adicionais do <xref:System.Xml.Serialization> namespace para controlar como a classe de .NET Framework deve ser convertida em XML.  
  
 Ao adotar essa abordagem, você deve ser capaz de fazer as classes .NET em contratos de dados com a adição de <xref:System.Runtime.Serialization.DataContractAttribute> e <xref:System.Runtime.Serialization.DataMemberAttribute> sem alterar significativamente o XML no qual as classes são serializadas para transmissão. Os tipos usados em mensagens por ASP.NET Web Services poderão ser processados como contratos de dados por aplicativos WCF, produzindo, entre outros benefícios, melhor desempenho em aplicativos WCF.  
  
## <a name="security"></a>Segurança  
 Evite usar as opções de autenticação fornecidas pelo Serviços de Informações da Internet (IIS). Os clientes WCF não dão suporte a eles. Se um serviço precisar ser protegido, use as opções fornecidas pelo WCF, pois essas opções são mais ricas e são baseadas em protocolos padrão.  
  
## <a name="see-also"></a>Consulte também

- [Antecipando a adoção do Windows Communication Foundation: facilitando a migração futura](anticipating-adopting-wcf-migration.md)
