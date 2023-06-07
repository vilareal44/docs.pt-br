---
title: Como expor um feed como Atom e RSS
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: fe374932-67f5-487d-9325-f868812b92e4
ms.openlocfilehash: e4ce1fa7b494c2317a1bddc57ee6b150c84b9a96
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593140"
---
# <a name="how-to-expose-a-feed-as-both-atom-and-rss"></a>Como expor um feed como Atom e RSS
Windows Communication Foundation (WCF) permite que você crie um serviço que expõe um feed de distribuição. Este tópico discute como criar um serviço de distribuição que expõe um feed de distribuição usando o Atom 1,0 e o RSS 2,0. Esse serviço expõe um ponto de extremidade que pode retornar um formato de distribuição. Para simplificar, o serviço usado neste exemplo é hospedado internamente. Em um ambiente de produção, um serviço desse tipo seria hospedado no IIS ou WAS. Para obter mais informações sobre as diferentes opções de hospedagem do WCF, consulte [Hosting](hosting.md).  
  
### <a name="to-create-a-basic-syndication-service"></a>Para criar um serviço de distribuição básico  
  
1. Defina um contrato de serviço usando uma interface marcada com o <xref:System.ServiceModel.Web.WebGetAttribute> atributo. Cada operação exposta como um feed de agregação retorna um <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> objeto. Observe os parâmetros para o <xref:System.ServiceModel.Web.WebGetAttribute> . `UriTemplate`Especifica a URL usada para invocar esta operação de serviço. A cadeia de caracteres para esse parâmetro contém literais e uma variável entre chaves ({*Format*}). Essa variável corresponde ao parâmetro da operação de serviço `format` . Para obter mais informações, consulte <xref:System.UriTemplate>. `BodyStyle`afeta como as mensagens enviadas e recebidas por essa operação de serviço são gravadas. <xref:System.ServiceModel.Web.WebMessageBodyStyle.Bare>Especifica que os dados enviados para e dessa operação de serviço não são encapsulados por elementos XML definidos pela infraestrutura. Para obter mais informações, consulte <xref:System.ServiceModel.Web.WebMessageBodyStyle>.  
  
     [!code-csharp[htAtomRss#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#0)]
     [!code-vb[htAtomRss#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/program.vb#0)]  
  
    > [!NOTE]
    > Use o <xref:System.ServiceModel.ServiceKnownTypeAttribute> para especificar os tipos que são retornados pelas operações de serviço nesta interface.  
  
2. Implemente o contrato de serviço.  
  
     [!code-csharp[htAtomRss#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#1)]
     [!code-vb[htAtomRss#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/program.vb#1)]  
  
3. Crie um <xref:System.ServiceModel.Syndication.SyndicationFeed> objeto e adicione um autor, uma categoria e uma descrição.  
  
     [!code-csharp[htAtomRss#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#2)]
     [!code-vb[htAtomRss#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/program.vb#2)]  
  
4. Crie vários <xref:System.ServiceModel.Syndication.SyndicationItem> objetos.  
  
     [!code-csharp[htAtomRss#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#3)]
     [!code-vb[htAtomRss#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/program.vb#3)]  
  
5. Adicione os <xref:System.ServiceModel.Syndication.SyndicationItem> objetos ao feed.  
  
     [!code-csharp[htAtomRss#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#4)]
     [!code-vb[htAtomRss#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/program.vb#4)]  
  
6. Use o parâmetro format para retornar o formato solicitado.  
  
     [!code-csharp[htAtomRss#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#5)]
     [!code-vb[htAtomRss#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/program.vb#5)]  
  
### <a name="to-host-the-service"></a>Para hospedar o serviço  
  
1. Crie um objeto <xref:System.ServiceModel.Web.WebServiceHost>. A <xref:System.ServiceModel.Web.WebServiceHost> classe adiciona automaticamente um ponto de extremidade no endereço base do serviço, a menos que um seja especificado no código ou na configuração. Neste exemplo, nenhum ponto de extremidade é especificado, portanto, o ponto de extremidades padrão é exposto.  
  
     [!code-csharp[htAtomRss#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#6)]
     [!code-vb[htAtomRss#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/program.vb#6)]  
  
2. Abra o host de serviço, carregue o feed do serviço, exiba o feed e aguarde até que o usuário pressione ENTER.  
  
     [!code-csharp[htAtomRss#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#8)]
     [!code-vb[htAtomRss#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/program.vb#8)]  
  
### <a name="to-call-getblog-with-an-http-get"></a>Para chamar getblog com um HTTP GET  
  
1. Abra o Internet Explorer, digite a seguinte URL e pressione ENTER: `http://localhost:8000/BlogService/GetBlog` .
  
     A URL contém o endereço base do serviço ( `http://localhost:8000/BlogService` ), o endereço relativo do ponto de extremidade e a operação de serviço a ser chamada.  
  
### <a name="to-call-getblog-from-code"></a>Para chamar getblog () do código  
  
1. Crie um <xref:System.Xml.XmlReader> com o endereço base e o método que você está chamando.  
  
     [!code-csharp[htAtomRss#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/snippets.cs#9)]
     [!code-vb[htAtomRss#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/snippets.vb#9)]  
  
2. Chame o <xref:System.ServiceModel.Syndication.SyndicationFeed.Load%28System.Xml.XmlReader%29> método estático, passando o <xref:System.Xml.XmlReader> que você acabou de criar.  
  
     [!code-csharp[htAtomRss#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/snippets.cs#10)]
     [!code-vb[htAtomRss#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/snippets.vb#10)]  
  
     Isso invoca a operação de serviço e popula um novo <xref:System.ServiceModel.Syndication.SyndicationFeed> com o formatador retornado da operação de serviço.  
  
3. Acessar o objeto feed.  
  
     [!code-csharp[htAtomRss#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/snippets.cs#11)]
     [!code-vb[htAtomRss#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatomrss/vb/snippets.vb#11)]  
  
## <a name="example"></a>Exemplo  
 A seguir está a listagem completa de códigos deste exemplo.  
  
 [!code-csharp[htAtomRss#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatomrss/cs/program.cs#12)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Ao compilar o código anterior, referencie System. ServiceModel. dll e System. ServiceModel. Web. dll.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
