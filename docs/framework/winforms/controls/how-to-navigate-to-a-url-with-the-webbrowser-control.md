---
title: Como navegar até uma URL com o controle WebBrowser
description: Saiba como usar o Windows Forms WebBrowser. navegar pelo controle para navegar até uma URL específica. Saiba também como determinar quando o novo documento será carregado.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
f1_keywords:
- WebBrowser.Navigate
helpviewer_keywords:
- WebBrowser control [Windows Forms], examples
- URLs [Windows Forms], navigating to
- WebBrowser control [Windows Forms], navigating to URLs
- examples [Windows Forms], WebBrowser control
ms.assetid: b3ec38cb-f509-4d0b-bd79-9f3611259c62
ms.openlocfilehash: e6ad360cc73a84ca040869832bb59d354cb78bd5
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325570"
---
# <a name="how-to-navigate-to-a-url-with-the-webbrowser-control"></a>Como navegar até uma URL com o controle WebBrowser
O exemplo de código a seguir demonstra como navegar o <xref:System.Windows.Forms.WebBrowser> controle para uma URL específica.

 Para determinar quando o novo documento está totalmente carregado, manipule o <xref:System.Windows.Forms.WebBrowser.DocumentCompleted> evento. Para ver uma demonstração desse evento, consulte [como: imprimir com um controle WebBrowser](how-to-print-with-a-webbrowser-control.md).

## <a name="example"></a>Exemplo

```vb
Me.webBrowser1.Navigate("https://www.microsoft.com")
```

```csharp
this.webBrowser1.Navigate("https://www.microsoft.com");
```

## <a name="compiling-the-code"></a>Compilando o código
 Este exemplo requer:

- Um controle <xref:System.Windows.Forms.WebBrowser> chamado `webBrowser1`.

- Referências aos assemblies `System` e `System.Windows.Forms`.

## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.WebBrowser>
- <xref:System.Windows.Forms.WebBrowser.DocumentCompleted?displayProperty=nameWithType>
- <xref:System.Windows.Forms.WebBrowser.Navigating?displayProperty=nameWithType>
- <xref:System.Windows.Forms.WebBrowser.Navigated?displayProperty=nameWithType>
- [Controle WebBrowser](webbrowser-control-windows-forms.md)
- [Como imprimir com um controle WebBrowser](how-to-print-with-a-webbrowser-control.md)
