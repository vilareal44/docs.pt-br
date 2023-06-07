---
title: Ordem dos eventos
description: Aprenda detalhes sobre a ordem de eventos em Windows Forms durante vários estágios importantes no tempo de vida de aplicativos e controles.
ms.date: 03/30/2017
helpviewer_keywords:
- events [Windows Forms], order of
- focus event order
- application shutdown event order
- sequence [Windows Forms], of events
- validation events [Windows Forms], order of
- application startup event order
ms.assetid: e81db09b-4453-437f-b78a-62d7cd5c9829
ms.openlocfilehash: b16d544d11500b2c684e87a915fc4b8eec071faa
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904332"
---
# <a name="order-of-events-in-windows-forms"></a>Ordem dos eventos em formulários do Windows Forms
A ordem na qual os eventos são gerados em aplicativos do Windows Forms é de interesse específico para os desenvolvedores preocupados com tratamento de cada um desses eventos sucessivamente. Quando uma situação exigir tratamento meticuloso de eventos, como quando você estiver redesenhando partes do formulário, será necessário um reconhecimento da ordem exata na qual os eventos são gerados em tempo de execução. Este tópico fornece alguns detalhes sobre a ordem dos eventos durante vários estágios importantes no tempo de vida de aplicativos e controles. Para obter detalhes específicos sobre a ordem de eventos de entrada do mouse, consulte [Eventos de mouse no Windows Forms](mouse-events-in-windows-forms.md). Para obter uma visão geral de eventos no Windows Forms, consulte [Visão geral de eventos](events-overview-windows-forms.md). Para obter detalhes sobre a composição de manipuladores de eventos, consulte [Visão geral de manipuladores de eventos](event-handlers-overview-windows-forms.md).  
  
## <a name="application-startup-and-shutdown-events"></a>Eventos de inicialização e desligamento de aplicativos  
 As <xref:System.Windows.Forms.Form> <xref:System.Windows.Forms.Control> classes e expõem um conjunto de eventos relacionados à inicialização e ao desligamento do aplicativo. Quando um Aplicativo do Windows Forms é iniciado, os eventos de inicialização do formulário principal são gerados na seguinte ordem:  
  
- <xref:System.Windows.Forms.Control.HandleCreated?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Control.BindingContextChanged?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Form.Load?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Control.VisibleChanged?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Form.Activated?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Form.Shown?displayProperty=nameWithType>  
  
 Quando um aplicativo é fechado, os eventos de desligamento do formulário principal são gerados na seguinte ordem:  
  
- <xref:System.Windows.Forms.Form.Closing?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Form.FormClosing?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Form.Closed?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Form.FormClosed?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.Form.Deactivate?displayProperty=nameWithType>  
  
 O <xref:System.Windows.Forms.Application.ApplicationExit> evento da <xref:System.Windows.Forms.Application> classe é gerado após os eventos de desligamento do formulário principal.  
  
> [!NOTE]
> Visual Basic 2005 inclui eventos de aplicativo adicionais, como <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup?displayProperty=nameWithType> e <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown?displayProperty=nameWithType> .  
  
## <a name="focus-and-validation-events"></a>Eventos de foco e validação  
 Quando você altera o foco usando o teclado (guia, SHIFT + TAB e assim por diante), chamando os <xref:System.Windows.Forms.Control.Select%2A> <xref:System.Windows.Forms.Control.SelectNextControl%2A> métodos ou, ou definindo a <xref:System.Windows.Forms.ContainerControl.ActiveControl%2A> propriedade para o formulário atual, os eventos de foco da <xref:System.Windows.Forms.Control> classe ocorrem na seguinte ordem:  
  
- <xref:System.Windows.Forms.Control.Enter>  
  
- <xref:System.Windows.Forms.Control.GotFocus>  
  
- <xref:System.Windows.Forms.Control.Leave>  
  
- <xref:System.Windows.Forms.Control.Validating>  
  
- <xref:System.Windows.Forms.Control.Validated>  
  
- <xref:System.Windows.Forms.Control.LostFocus>  
  
 Quando você altera o foco usando o mouse ou chamando o <xref:System.Windows.Forms.Control.Focus%2A> método, os eventos de foco da <xref:System.Windows.Forms.Control> classe ocorrem na seguinte ordem:  
  
- <xref:System.Windows.Forms.Control.Enter>  
  
- <xref:System.Windows.Forms.Control.GotFocus>  
  
- <xref:System.Windows.Forms.Control.LostFocus>  
  
- <xref:System.Windows.Forms.Control.Leave>  
  
- <xref:System.Windows.Forms.Control.Validating>  
  
- <xref:System.Windows.Forms.Control.Validated>  
  
## <a name="see-also"></a>Veja também

- [Criando manipuladores de eventos no Windows Forms](creating-event-handlers-in-windows-forms.md)
