---
title: posicionar controles
description: Saiba como usar o Designer de Formulários do Windows no Visual Studio ou a propriedade Location para posicionar seus controles.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
f1_keywords:
- Location
- Location.Y
- Location.X
helpviewer_keywords:
- controls [Windows Forms]
- controls [Windows Forms], moving
- snaplines
- controls [Windows Forms], positioning
ms.assetid: 4693977e-34a4-4f19-8221-68c3120c2b2b
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 0aa3faade71e0f7e0a9d5e676327a80747524b8c
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904293"
---
# <a name="how-to-position-controls-on-windows-forms"></a>Como: posicionar controles em Windows Forms

Para posicionar controles, use o Designer de Formulários do Windows no Visual Studio ou especifique a <xref:System.Windows.Forms.Control.Location%2A> propriedade.

## <a name="position-a-control-on-the-design-surface-of-the-windows-forms-designer"></a>Posicionar um controle na superfície de design do Designer de Formulários do Windows

No Visual Studio, arraste o controle para o local apropriado com o mouse.

> [!NOTE]
> Selecione o controle e mova-o com as teclas de direção para posicioná-lo com mais precisão. Além disso, as *guias de alinhamento* ajudam a posicionar os controles com precisão em seu formulário. Para obter mais informações, consulte [Instruções passo a passo: organizando controles nos Windows Forms usando guias de alinhamento](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md).

## <a name="position-a-control-using-the-properties-window"></a>Posicionar um controle usando o janela Propriedades

1. No Visual Studio, selecione o controle que você deseja posicionar.

2. Na janela **Propriedades** , insira valores para a <xref:System.Windows.Forms.Control.Location%2A> propriedade, separados por uma vírgula, para posicionar o controle dentro de seu contêiner.

   O primeiro número (X) é a distância entre a borda esquerda do contêiner; o segundo número (Y) é a distância entre a borda superior da área do recipiente, medida em pixels.

   > [!NOTE]
   > Você pode expandir a <xref:System.Windows.Forms.Control.Location%2A> propriedade para digitar os valores **X** e **Y** individualmente.

## <a name="position-a-control-programmatically"></a>Posicionar um controle programaticamente

1. Defina a <xref:System.Windows.Forms.Control.Location%2A> Propriedade do controle como um <xref:System.Drawing.Point> .

    ```vb
    Button1.Location = New Point(100, 100)
    ```

    ```csharp
    button1.Location = new Point(100, 100);
    ```

    ```cpp
    button1->Location = Point(100, 100);
    ```

2. Altere a coordenada X do local do controle usando o <xref:System.Windows.Forms.Control.Left%2A> subpropriedade.

    ```vb
    Button1.Left = 300
    ```

    ```csharp
    button1.Left = 300;
    ```

    ```cpp
    button1->Left = 300;
    ```

## <a name="increment-a-controls-location-programmatically"></a>Incrementar o local de um controle programaticamente

Defina <xref:System.Windows.Forms.Control.Left%2A> subpropriedade para incrementar a coordenada X do controle.

```vb
Button1.Left += 200
```

```csharp
button1.Left += 200;
```

```cpp
button1->Left += 200;
```

> [!NOTE]
> Use a <xref:System.Windows.Forms.Control.Location%2A> propriedade para definir as posições X e Y de um controle simultaneamente. Para definir uma posição individualmente, use o <xref:System.Windows.Forms.Control.Left%2A> (**X**) ou <xref:System.Windows.Forms.Control.Top%2A> (**Y**) subpropriedade do controle. Não tente definir implicitamente as coordenadas X e Y da <xref:System.Drawing.Point> estrutura que representa o local do botão, pois essa estrutura contém uma cópia das coordenadas do botão.

## <a name="see-also"></a>Veja também

- [Controles de Windows Forms](index.md)
- [Instruções passo a passo: organizando controles no Windows Forms usando guias de alinhamento](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)
- [Explicação passo a passo: organizando controles no Windows Forms utilizando um TableLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)
- [Passo a passo: organizando controles nos Windows Forms utilizando um FlowLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel.md)
- [Identificando controles dos Windows Forms individuais e fornecendo atalhos para eles](labeling-individual-windows-forms-controls-and-providing-shortcuts-to-them.md)
- [Controles a serem usados em Windows Forms](controls-to-use-on-windows-forms.md)
- [Controles dos Windows Forms por função](windows-forms-controls-by-function.md)
- [Como configurar o local da tela dos Windows Forms](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/52aha046(v=vs.100))
