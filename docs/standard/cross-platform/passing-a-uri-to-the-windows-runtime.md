---
title: Passando um URI para o Windows Runtime
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Runtime, .NET Framework support for
- Windows Runtime, passing a URI to
ms.assetid: 3eb5ce6f-f304-4f87-8e81-0f25092f5ad4
ms.openlocfilehash: 7152fb02abf430c0d0e27b82550e4006e3958e93
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288882"
---
# <a name="passing-a-uri-to-the-windows-runtime"></a>Passando um URI para o Windows Runtime
Os métodos de Windows Runtime só aceitam URIs absolutos. Se você passar um URI relativo para um método Windows Runtime, uma <xref:System.ArgumentException> exceção será lançada. Aqui está o porquê: quando você usa o Windows Runtime no código .NET Framework, a <xref:Windows.Foundation.Uri?displayProperty=nameWithType> classe aparece como <xref:System.Uri?displayProperty=nameWithType> no IntelliSense. A <xref:System.Uri?displayProperty=nameWithType> classe permite URIs relativos, mas a <xref:Windows.Foundation.Uri?displayProperty=nameWithType> classe não. Isso também é verdadeiro para métodos que você expõe em componentes Windows Runtime. Se seu componente expõe um método que usa um URI, a assinatura presente no código inclui <xref:System.Uri?displayProperty=nameWithType>. No entanto, para os usuários do seu componente, a assinatura inclui <xref:Windows.Foundation.Uri?displayProperty=nameWithType> . Os URIs enviados aos seus componentes devem ser absolutos.  
  
Este tópico mostra como detectar URIs absolutos e como criar um URI desse tipo ao fazer referência a um recurso do pacote do aplicativo.  
  
## <a name="detecting-and-using-an-absolute-uri"></a>Detectando e usando um URI absoluto  
Use a <xref:System.Uri.IsAbsoluteUri%2A?displayProperty=nameWithType> propriedade para garantir que um URI seja absoluto antes de passá-lo para o Windows Runtime. Usar essa propriedade é mais eficiente do que identificar e tratar a exceção <xref:System.ArgumentException>.  
  
## <a name="using-an-absolute-uri-for-a-resource-in-the-app-package"></a>Usando um URI absoluto para um recurso no pacote do aplicativo  
Se quiser especificar um URI para um recurso presente no pacote do aplicativo, você pode usar o esquema `ms-appx` ou `ms-appx-web` para criar um URI absoluto.  
  
O exemplo a seguir mostra como definir a <xref:Windows.UI.Xaml.Controls.WebView.Source%2A> propriedade para um <xref:Windows.UI.Xaml.Controls.WebView> controle e a <xref:Windows.UI.Xaml.Controls.Image.Source%2A> propriedade de um <xref:Windows.UI.Xaml.Controls.Image> controle para recursos contidos em uma pasta chamada pages, usando XAML e código.  
  
[!code-xaml[System.URIToWindowsURI#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.uritowindowsuri/cs/mainpage.xaml#1)]  
[!code-csharp[System.URIToWindowsURI#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.uritowindowsuri/cs/mainpage.xaml.cs#2)]
[!code-vb[System.URIToWindowsURI#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.uritowindowsuri/vb/mainpage.xaml.vb#2)]  
  
Para obter mais informações sobre esses esquemas, consulte [esquemas de URI](/windows/uwp/app-resources/uri-schemes) no centro de desenvolvimento do Windows.  
  
## <a name="see-also"></a>Veja também

- [Suporte do .NET Framework para Aplicativos da Windows Store e Windows Runtime](support-for-windows-store-apps-and-windows-runtime.md)
