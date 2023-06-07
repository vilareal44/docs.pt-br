---
title: Como criar um RSS Feed básico
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 431879b8-a5f8-4947-ad1e-4768c726aca8
ms.openlocfilehash: 872fe325a6705e79d026cd7f6e1f7cfef5145307
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599016"
---
# <a name="how-to-create-a-basic-rss-feed"></a>Como criar um RSS Feed básico
Windows Communication Foundation (WCF) permite que você crie um serviço que expõe um feed de distribuição. Este tópico discute como criar um serviço de distribuição que expõe um feed de agregação de RSS.  
  
### <a name="to-create-a-basic-syndication-service"></a>Para criar um serviço de distribuição básico  
  
1. Defina um contrato de serviço usando uma interface marcada com o <xref:System.ServiceModel.Web.WebGetAttribute> atributo. Cada operação exposta como um feed de distribuição deve retornar um <xref:System.ServiceModel.Syndication.Rss20FeedFormatter> objeto.  
  
     [!code-csharp[htRssBasic#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#0)]
     [!code-vb[htRssBasic#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#0)]  
  
    > [!NOTE]
    > Todas as operações de serviço que aplicam o <xref:System.ServiceModel.Web.WebGetAttribute> atributo são mapeadas para solicitações HTTP Get. Para mapear a operação para um método HTTP diferente, use o <xref:System.ServiceModel.Web.WebInvokeAttribute> em vez disso. Para obter mais informações, consulte [como: criar um serviço http Web do WCF básico](how-to-create-a-basic-wcf-web-http-service.md).  
  
2. Implemente o contrato de serviço.  
  
     [!code-csharp[htRssBasic#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#1)]
     [!code-vb[htRssBasic#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#1)]  
  
3. Crie um <xref:System.ServiceModel.Syndication.SyndicationFeed> objeto e adicione um autor, uma categoria e uma descrição.  
  
     [!code-csharp[htRssBasic#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#2)]
     [!code-vb[htRssBasic#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#2)]  
  
4. Crie vários <xref:System.ServiceModel.Syndication.SyndicationItem> objetos.  
  
     [!code-csharp[htRssBasic#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#3)]
     [!code-vb[htRssBasic#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#3)]  
  
5. Adicione o <xref:System.ServiceModel.Syndication.SyndicationItem> ao feed.  
  
     [!code-csharp[htRssBasic#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#4)]
     [!code-vb[htRssBasic#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#4)]  
  
6. Retorne o feed.  
  
     [!code-csharp[htRssBasic#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#5)]
     [!code-vb[htRssBasic#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#5)]  
  
### <a name="to-host-a-service"></a>Para hospedar um serviço  
  
1. Crie um objeto <xref:System.ServiceModel.Web.WebServiceHost>.  
  
     [!code-csharp[htRssBasic#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#6)]
     [!code-vb[htRssBasic#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#6)]  
  
2. Abra o host do serviço e aguarde até que o usuário pressione ENTER.  
  
     [!code-csharp[htRssBasic#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#8)]
     [!code-vb[htRssBasic#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#8)]  
  
### <a name="to-call-getblog-with-an-http-get"></a>Para chamar getblog () com um HTTP GET  
  
1. Abra o Internet Explorer, digite a seguinte URL e pressione ENTER: `http://localhost:8000/BlogService/GetBlog` . A URL contém o endereço base do serviço ( `http://localhost:8000/BlogService` ), o endereço relativo do ponto de extremidade e a operação de serviço a ser chamada.  
  
### <a name="to-call-getblog-from-code"></a>Para chamar getblog () do código  
  
1. Crie um <xref:System.Xml.XmlReader> com o endereço base e o método que você está chamando.  
  
     [!code-csharp[htRssBasic#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/snippets.cs#9)]
     [!code-vb[htRssBasic#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/snippets.vb#9)]  
  
2. Chame o <xref:System.ServiceModel.Syndication.SyndicationFeed.Load%28System.Xml.XmlReader%29> método estático, passando o <xref:System.Xml.XmlReader> que você acabou de criar.  
  
     [!code-csharp[htRssBasic#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/snippets.cs#10)]
     [!code-vb[htRssBasic#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/snippets.vb#10)]  
  
     Isso invoca a operação de serviço e popula um novo <xref:System.ServiceModel.Syndication.SyndicationFeed> com o formatador retornado da operação de serviço.  
  
3. Acessar o objeto feed.  
  
     [!code-csharp[htRssBasic#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/snippets.cs#11)]
     [!code-vb[htRssBasic#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/snippets.vb#11)]  
  
## <a name="example"></a>Exemplo  
 A seguir está a listagem completa de códigos deste exemplo.  
  
 [!code-csharp[htRssBasic#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/htrssbasic/cs/program.cs#12)]
 [!code-vb[htRssBasic#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htrssbasic/vb/program.vb#12)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Ao compilar o código anterior, referencie System. ServiceModel. dll e System. ServiceModel. Web. dll.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
