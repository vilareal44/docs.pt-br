---
title: Gerenciando conexões
description: Saiba como os aplicativos que usam HTTP para recursos de dados podem usar as classes .NET Framework do ServicePointManager e do ponto de extremidade para gerenciar conexões.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Internet, connections
- HTTP, classes for connecting
- requesting data from Internet, connections
- sending data, connections
- receiving data, connections
- ServicePoint class, about ServicePoint class
- response to Internet request, connections
- connections [.NET Framework], classes
- network resources, connections
- downloading Internet resources, connections
- ServicePointManager class, about ServicePointManager class
ms.assetid: 9b3d3de7-189f-4f7d-81ae-9c29c441aaaa
ms.openlocfilehash: 124dff1b104e323b929d13f73cf17d740e747c32
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502282"
---
# <a name="managing-connections"></a>Gerenciando conexões
Aplicativos que usam HTTP para se conectar aos recursos de dados podem usar as classes <xref:System.Net.ServicePoint> e <xref:System.Net.ServicePointManager> do .NET Framework para gerenciar conexões com a Internet e para ajudá-las a obter a melhor escala e desempenho.  
  
 A classe **ServicePoint** fornece um aplicativo com um ponto de extremidade ao qual o aplicativo pode se conectar para acessar recursos da Internet. Cada **ServicePoint** contém informações que ajudam a otimizar a conexões com um servidor de Internet compartilhando informações de otimização entre as conexões para melhorar o desempenho.  
  
 Cada **ServicePoint** é identificado por um URI (identificador de recurso uniforme) e é categorizado de acordo com o identificador do esquema e os fragmentos de host do URI. Por exemplo, a mesma instância de **ServicePoint** oferece solicitações para os URIs `http://www.contoso.com/index.htm` e `http://www.contoso.com/news.htm?date=today`, já que eles têm o mesmo identificador de esquema (http) e fragmentos de host (`www.contoso.com`). Se o aplicativo já tem uma conexão persistente para o servidor `www.contoso.com`, ele usa essa conexão para recuperar as solicitações, evitando a necessidade de criar duas conexões.  
  
 **ServicePointManager** é uma classe estática que gerencia a criação e destruição de instâncias de **ServicePoint**. O **ServicePointManager** cria um **ServicePoint** quando o aplicativo solicita um recurso de Internet que não está na coleção de instâncias **ServicePoint** existentes. As instâncias **ServicePoint** são destruídas quando ultrapassaram o tempo ocioso máximo ou quando o número de instâncias **ServicePoint** existentes excede o número máximo de instâncias **ServicePoint** para o aplicativo. Você pode controlar o tempo ocioso máximo padrão e o número máximo de instâncias **ServicePoint** definindo as propriedades <xref:System.Net.ServicePointManager.MaxServicePointIdleTime%2A> e <xref:System.Net.ServicePointManager.MaxServicePoints%2A> no **ServicePointManager**.  
  
 O número de conexões entre um cliente e um servidor pode ter um impacto significativo na taxa de transferência do aplicativo. Por padrão, um aplicativo usando a classe <xref:System.Net.HttpWebRequest> usa um máximo de duas conexões persistentes para um determinado servidor, mas você pode definir o número máximo de conexões usando o aplicativo como critério.  
  
> [!NOTE]
> A especificação HTTP/1.1 limita o número de conexões de um aplicativo para duas conexões por servidor.  
  
 O número ideal de conexões depende das reais condições em que o aplicativo é executado. Aumentar o número de conexões disponíveis para o aplicativo pode não afetar o desempenho do aplicativo. Para determinar o impacto de mais conexões, execute testes de desempenho enquanto varia o número de conexões. Você pode alterar o número de conexões que um aplicativo usa alterando a propriedade <xref:System.Net.ServicePointManager.DefaultConnectionLimit%2A> estática na classe **ServicePointManager** na inicialização do aplicativo, conforme mostrado no exemplo de código a seguir.  
  
```csharp  
// Set the maximum number of connections per server to 4.  
ServicePointManager.DefaultConnectionLimit = 4;  
```  
  
```vb  
' Set the maximum number of connections per server to 4.  
ServicePointManager.DefaultConnectionLimit = 4  
```  
  
 Alterar a propriedade **ServicePointManager.DefaultConnectionLimit** não afeta instâncias de **ServicePoint** inicializadas anteriormente. O código a seguir demonstra como alterar o limite de conexão em um **ServicePoint** existente para o servidor `http://www.contoso.com` para o valor armazenado em `newLimit`.  
  
```csharp  
Uri uri = new Uri("http://www.contoso.com/");  
ServicePoint sp = ServicePointManager.FindServicePoint(uri);  
sp.ConnectionLimit = newLimit;  
```  
  
```vb  
Dim uri As New Uri("http://www.contoso.com/")  
Dim sp As ServicePoint = ServicePointManager.FindServicePoint(uri)  
sp.ConnectionLimit = newLimit  
```  
  
## <a name="see-also"></a>Confira também

- [Agrupamento de conexão](connection-grouping.md)
- [Usando protocolos de aplicativo](using-application-protocols.md)
