---
title: Armazenamento em cache em aplicativos do .NET Framework
description: Use o Caching em aplicativos .NET. Leia sobre como armazenar em cache dados, Caching em aplicativos ASP.NET ou serviços REST do WCF e estender o cache no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- ASP.NET caching
- caching [.NET Framework]
- caching [ASP.NET]
ms.assetid: c4b47ee0-4b82-4124-9bce-818088385e34
ms.openlocfilehash: 9b08a07e9b446c2998150a327dccdc8d0481722a
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309762"
---
# <a name="caching-in-net-framework-applications"></a>Armazenamento em cache em aplicativos do .NET Framework
O cache permite que você armazene dados na memória para acesso rápido. Quando os dados são acessados novamente, os aplicativos podem obter os dados do cache, em vez de recuperá-los da fonte original. Isso pode melhorar o desempenho e a escalabilidade. Além disso, o cache torna os dados disponíveis quando a fonte de dados está temporariamente indisponível.  
  
 O .NET Framework fornece a funcionalidade de cache, que pode ser usada para melhorar o desempenho e a escalabilidade dos aplicativos cliente e de servidor do Windows, incluindo o ASP.NET.  
  
> [!NOTE]
> No .NET Framework 3,5 e versões anteriores, o ASP.NET forneceu uma implementação de cache na memória no <xref:System.Web.Caching> namespace. Nas versões anteriores do .NET Framework, o Caching estava disponível apenas no <xref:System.Web> namespace e, portanto, exigia uma dependência em classes ASP.net. No .NET Framework 4, o namespace <xref:System.Runtime.Caching> contém APIs projetadas para aplicativos Web e não Web.  
  
## <a name="caching-data"></a>Armazenando dados em cache  
 É possível armazenar as informações em cache usando classes no namespace <xref:System.Runtime.Caching>. As classes de cache desse namespace fornecem os seguintes recursos:  
  
- Tipos abstratos que fornecem a base para a criação de implementações personalizadas de cache.  
  
- Uma implementação concreta de cache de objetos na memória.  
  
 A classe de cache de base abstrata (<xref:System.Runtime.Caching.ObjectCache>) define as seguintes tarefas de cache:  
  
- Criação e gerenciamento de entradas de cache.  
  
- Especificação de informações de expiração e remoção.  
  
- Gatilho de eventos que são acionados em resposta a alterações nas entradas de cache.  
  
 A classe <xref:System.Runtime.Caching.MemoryCache> é uma implementação de cache de objetos na memória da classe <xref:System.Runtime.Caching.ObjectCache>. É possível usar a classe <xref:System.Runtime.Caching.MemoryCache> para a maioria das tarefas de cache.  
  
> [!NOTE]
> A classe <xref:System.Runtime.Caching.MemoryCache> é modelada no objeto de cache do ASP.NET definido no namespace <xref:System.Web.Caching>. Portanto, a lógica de cache interna é semelhante à lógica fornecida em versões anteriores do ASP.NET.  
  
 Para obter um exemplo de como usar o cache em um aplicativo WPF, consulte [Passo a passo: Armazenando dados de aplicativo em cache em um aplicativo WPF](../wpf/advanced/walkthrough-caching-application-data-in-a-wpf-application.md).  
  
## <a name="caching-in-aspnet-applications"></a>Armazenando em cache nos aplicativos ASP.NET  
 As classes de cache do namespace <xref:System.Runtime.Caching> fornecem funcionalidade de armazenamento de dados em cache no ASP.NET.  
  
> [!NOTE]
> Se seu aplicativo for direcionado para o .NET Framework 3,5 ou anterior, você deverá usar as classes de cache definidas no <xref:System.Web.Caching> namespace. Para obter mais informações, consulte [Visão geral do cache do ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/ms178597(v=vs.100)).  
  
> [!NOTE]
> Ao desenvolver novos aplicativos, recomendamos o uso da classe <xref:System.Runtime.Caching.MemoryCache>. A API fornecida no namespace <xref:System.Runtime.Caching> é como a API fornecida no namespace <xref:System.Web.Caching.Cache>. Portanto, a API será familiar se você usou o cache em versões anteriores do ASP.NET. Para obter um exemplo de como usar o cache em aplicativos ASP.NET, consulte [Passo a passo: Armazenando dados de aplicativo em cache no ASP.NET](https://docs.microsoft.com/previous-versions/ff477235(v=vs.100)).  
  
### <a name="output-caching"></a>Cache de saída  
 Para armazenar dados de aplicativo em cache manualmente, use a classe <xref:System.Runtime.Caching.MemoryCache> no ASP.NET. O ASP.NET também dá suporte ao cache de saída, que armazena a saída gerada de páginas, controles e respostas HTTP na memória. É possível configurar o cache de saída de forma declarativa em uma página da Web ASP.NET ou usando as configurações do arquivo Web.config. Para obter mais informações, consulte [Elemento outputCache para armazenar em cache (esquema de configurações do ASP.NET)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms228124(v=vs.100)).  
  
 O ASP.NET permite estender o cache de saída criando provedores de cache de saída personalizados. Usando provedores personalizados, você pode armazenar o conteúdo armazenado em cache usando outros dispositivos de armazenamento, como discos, armazenamento em nuvem e mecanismos de cache distribuído. Para criar um provedor de cache de saída personalizado, crie uma classe que é derivada da classe <xref:System.Web.Caching.OutputCacheProvider> e configure o aplicativo para usar o provedor de cache de saída personalizado.  
  
## <a name="caching-in-wcf-rest-services"></a>Armazenando em cache nos serviços REST do WCF  
 Para os serviços REST do WCF, o .NET Framework permite aproveitar o cache de saída declarativo disponível no ASP.NET. Isso permite armazenar em cache as respostas das operações de serviço REST do WCF. Quando um usuário envia uma solicitação HTTP GET para um serviço que está configurado para armazenar em cache, o ASP.NET envia a resposta armazenada em cache novamente e o método de serviço não é chamado. Depois que o cache expirar, na próxima vez que um usuário enviar uma solicitação HTTP GET, o método de serviço será chamado e a resposta será armazenada em cache novamente.  
  
 O .NET Framework também permite implementar o cache de HTTP GET condicional. Em cenários REST, uma solicitação de HTTP GET condicional é geralmente usada pelos serviços para implementar o cache inteligente de HTTP, conforme descrito na [Especificação de HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616.html). Para obter mais informações, consulte [Suporte ao cache em serviços Web HTTP do WCF](../wcf/feature-details/caching-support-for-wcf-web-http-services.md).  
  
## <a name="extending-caching-in-the-net-framework"></a>Estendendo o cache no .NET Framework  
 O cache do .NET Framework foi projetado para ser extensível. A classe <xref:System.Runtime.Caching.ObjectCache> permite criar uma implementação personalizada de cache. Essa classe fornece os membros que estão disponíveis para todos os aplicativos gerenciados, incluindo o Windows Forms, o WPF (Windows Presentation Foundation) e o WCF (Windows Communications Foundation). Você pode fazer isso para criar uma classe de cache que usa outro mecanismo de armazenamento ou se desejar ter um controle granular sobre as operações de cache.  
  
 Para estender o cache, faça o seguinte:  
  
- Crie uma classe personalizada que é derivada da classe <xref:System.Runtime.Caching.ObjectCache> e, em seguida, forneça uma implementação personalizada de cache na classe derivada.  
  
- Crie uma classe que é derivada da classe <xref:System.Runtime.Caching.MemoryCache> e personalize ou estenda a classe derivada. Para obter um exemplo de como fazer isso, consulte [Armazenando dados de aplicativo em cache usando vários objetos de cache em um aplicativo ASP.NET](https://docs.microsoft.com/archive/blogs/aspnetue/caching-application-data-by-using-multiple-cache-objects-in-an-asp-net-application).  
  
- Crie uma classe que é derivada da classe <xref:System.Web.Caching.OutputCacheProvider> e configure o aplicativo para usar o provedor de cache de saída personalizado.  
  
 Para obter mais informações, consulte a entrada [Extensible Output Caching with ASP.NET 4 (VS 2010 and .NET 4.0 Series)](https://weblogs.asp.net/scottgu/extensible-output-caching-with-asp-net-4-vs-2010-and-net-4-0-series) (Cache de saída extensível com o ASP.NET 4 [VS 2010 e Série do .NET 4.0]) no blog de Scott Guthrie.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.Caching.ObjectCache>
- <xref:System.Runtime.Caching.MemoryCache>
- [Passo a passo: armazenar dados de aplicativo em cache em um aplicativo WPF](../wpf/advanced/walkthrough-caching-application-data-in-a-wpf-application.md)
- [Passo a passo: Armazenando dados de aplicativo em cache no ASP.NET](https://docs.microsoft.com/previous-versions/ff477235(v=vs.100))
