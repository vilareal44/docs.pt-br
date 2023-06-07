---
title: Definir a tela de fundo de um painel
description: Saiba como definir a cor do plano de fundo e a imagem de plano de fundo de um painel de Windows Forms usando o designer.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- background colors [Windows Forms], Windows Forms Panel controls
- background images [Windows Forms], Windows Forms Panel controls
- Panel control [Windows Forms], background
- colors [Windows Forms], Windows Forms Panel controls
ms.assetid: 096cbd8d-45cc-47b8-b1ef-a27f60ea8be0
ms.openlocfilehash: 109ff6184de9c79d1576207bbeb29ad939670b6f
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86173374"
---
# <a name="how-to-set-the-background-of-a-windows-forms-panel"></a>Como definir a tela de fundo de um painel Windows Forms
Um controle de Windows Forms <xref:System.Windows.Forms.Panel> pode exibir uma cor de plano de fundo e uma imagem de tela de fundo. A <xref:System.Windows.Forms.Control.BackColor%2A> propriedade define a cor do plano de fundo para os controles contidos, como rótulos e botões de opção. Se a <xref:System.Windows.Forms.Control.BackgroundImage%2A> propriedade não for definida, a <xref:System.Windows.Forms.Control.BackColor%2A> seleção preencherá o painel inteiro. Se a <xref:System.Windows.Forms.Control.BackgroundImage%2A> propriedade for definida, a imagem será exibida atrás dos controles contidos.  
  
### <a name="to-set-the-background-programmatically"></a>Para definir o plano de fundo programaticamente  
  
1. Defina a propriedade do painel <xref:System.Windows.Forms.Control.BackColor%2A> como um valor do tipo <xref:System.Drawing.Color?displayProperty=nameWithType> .  
  
    ```vb  
    Panel1.BackColor = Color.AliceBlue  
    ```  
  
    ```csharp  
    panel1.BackColor = Color.AliceBlue;  
    ```  
  
    ```cpp  
    panel1->BackColor = Color::AliceBlue;  
    ```  
  
2. Defina a propriedade do painel <xref:System.Windows.Forms.Control.BackgroundImage%2A> usando o <xref:System.Drawing.Image.FromFile%2A> método da <xref:System.Drawing.Image?displayProperty=nameWithType> classe.  
  
    ```vb  
    ' You should replace the bolded image
    ' in the sample below with an image of your own choosing.  
    Panel1.BackgroundImage = Image.FromFile _  
        (System.Environment.GetFolderPath _  
        (System.Environment.SpecialFolder.Personal) _  
        & "\Image.gif")  
    ```  
  
    ```csharp  
    // You should replace the bolded image
    // in the sample below with an image of your own choosing.  
    // Note the escape character used (@) when specifying the path.  
    panel1.BackgroundImage = Image.FromFile  
       (System.Environment.GetFolderPath  
       (System.Environment.SpecialFolder.Personal)  
       + @"\Image.gif");  
    ```  
  
    ```cpp  
    // You should replace the bolded image
    // in the sample below with an image of your own choosing.  
    panel1->BackgroundImage = Image::FromFile(String::Concat(  
       System::Environment::GetFolderPath  
       (System::Environment::SpecialFolder::Personal),  
       "\\Image.gif"));  
    ```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.Control.BackColor%2A>
- <xref:System.Windows.Forms.Control.BackgroundImage%2A>
- [Controle de painel](panel-control-windows-forms.md)
- [Visão geral do controle Panel](panel-control-overview-windows-forms.md)
