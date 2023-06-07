---
title: Hospedar um controle de Windows Forms no WPF
description: Saiba como hospedar controles de Windows Forms no Windows Presentation Foundation, que já fornece muitos controles com um rico conjunto de recursos.
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hosting Windows Forms control in WPF [WPF]
ms.assetid: 9cb88415-39b0-4c46-80c4-ff325b674286
ms.openlocfilehash: ec67cf9fabaa5b7aadbb2a3c21ebf8dd828ee20f
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164978"
---
# <a name="walkthrough-hosting-a-windows-forms-control-in-wpf"></a>Passo a passo: hospedar um controle do Windows Forms no WPF

O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] fornece muitos controles com um amplo conjunto de recursos. No entanto, às vezes você pode querer usar controles de Windows Forms em suas [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] páginas. Por exemplo, você pode ter um investimento substancial em controles de Windows Forms existentes ou pode ter um controle de Windows Forms que fornece funcionalidade exclusiva.

Este tutorial mostra como hospedar um Windows Forms <xref:System.Windows.Forms.MaskedTextBox?displayProperty=nameWithType> controle em uma [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] página usando código.

Para obter uma listagem de código completa das tarefas mostradas neste passo a passos, consulte [hospedando um controle de Windows Forms no exemplo do WPF](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/HostingWfInWPF).

## <a name="prerequisites"></a>Prerequisites

É necessário o Visual Studio para concluir este passo a passo.

## <a name="hosting-the-windows-forms-control"></a>Hospedando o controle dos Windows Forms

### <a name="to-host-the-maskedtextbox-control"></a>Para hospedar o controle MaskedTextBox

1. Crie um projeto de aplicativo WPF chamado `HostingWfInWpf`.

2. Adicione referências aos assemblies a seguir.

    - WindowsFormsIntegration

    - System.Windows.Forms

3. Abra o MainWindow.xaml no WPF Designer.

4. Nomeie o <xref:System.Windows.Controls.Grid> elemento `grid1` .

     [!code-xaml[HostingWfInWPF#1](~/samples/snippets/csharp/VS_Snippets_Wpf/HostingWfInWPF/CSharp/HostingWfInWPF/Window1.xaml#1)]

5. No modo de exibição modo de exibição de Design ou XAML, selecione o <xref:System.Windows.Window> elemento.

6. Na janela Propriedades, clique na guia **eventos** .

7. Clique duas vezes no <xref:System.Windows.FrameworkElement.Loaded> evento.

8. Insira o código a seguir para manipular o <xref:System.Windows.FrameworkElement.Loaded> evento.

     [!code-csharp[HostingWfInWPF#10](~/samples/snippets/csharp/VS_Snippets_Wpf/HostingWfInWPF/CSharp/HostingWfInWPF/Window1.xaml.cs#10)]
     [!code-vb[HostingWfInWPF#10](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HostingWfInWPF/VisualBasic/HostingWfInWpf/Window1.xaml.vb#10)]

9. Na parte superior do arquivo, adicione a seguinte `Imports` instrução ou `using` .

     [!code-csharp[HostingWfInWPF#11](~/samples/snippets/csharp/VS_Snippets_Wpf/HostingWfInWPF/CSharp/HostingWfInWPF/Window1.xaml.cs#11)]
     [!code-vb[HostingWfInWPF#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HostingWfInWPF/VisualBasic/HostingWfInWpf/Window1.xaml.vb#11)]

10. Selecione **F5** para compilar e executar o aplicativo.

## <a name="see-also"></a>Confira também

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Criar XAML no Visual Studio](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [Passo a passo: hospedar um controle do Windows Forms no WPF usando XAML](walkthrough-hosting-a-windows-forms-control-in-wpf-by-using-xaml.md)
- [Passo a passo: hospedar um controle composto do Windows Forms no WPF](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [Passo a passo: hospedar um controle composto do WPF nos Windows Forms](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
- [Controles dos Windows Forms e controles WPF equivalentes](windows-forms-controls-and-equivalent-wpf-controls.md)
- [Hospedando um controle de Windows Forms no exemplo do WPF](https://github.com/Microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/HostingWfInWPF)
