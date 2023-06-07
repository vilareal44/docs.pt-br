---
title: Como ancorar e encaixar controles filho em um controle FlowLayoutPanel
description: Saiba como ancorar e encaixar controles filho de forma programática em um controle FlowLayoutPanel Windows Forms.
ms.date: 03/30/2017
helpviewer_keywords:
- layout [Windows Forms], child controls
- FlowLayoutPanel control [Windows Forms], child controls
- controls [Windows Forms], child
- child controls [Windows Forms], anchoring and docking
ms.assetid: a2bcdfca-9b63-45e6-9c0e-3411015cba98
ms.openlocfilehash: b4fb3bf6d148a526a75926bd67c1f967286332bf
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174530"
---
# <a name="how-to-anchor-and-dock-child-controls-in-a-flowlayoutpanel-control"></a>Como ancorar e encaixar controles filho em um controle FlowLayoutPanel

O <xref:System.Windows.Forms.FlowLayoutPanel> controle oferece suporte <xref:System.Windows.Forms.Control.Anchor%2A> às <xref:System.Windows.Forms.Control.Dock%2A> Propriedades e em seus controles filho.

### <a name="to-anchor-and-dock-child-controls-in-a-flowlayoutpanel-control"></a>Ancorar e encaixar controles filho em um controle FlowLayoutPanel

1. Crie um <xref:System.Windows.Forms.FlowLayoutPanel> controle no formulário.

2. Defina o <xref:System.Windows.Forms.Control.Width%2A> do <xref:System.Windows.Forms.FlowLayoutPanel> controle como **300**e defina seu <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> como <xref:System.Windows.Forms.FlowDirection.TopDown> .

3. Crie dois <xref:System.Windows.Forms.Button> controles e coloque-os no <xref:System.Windows.Forms.FlowLayoutPanel> controle.

4. Defina o <xref:System.Windows.Forms.Control.Width%2A> do primeiro botão como **200**.

5. Defina a <xref:System.Windows.Forms.Control.Dock%2A> Propriedade do segundo botão como <xref:System.Windows.Forms.DockStyle.Fill> .

    > [!NOTE]
    > O segundo botão assume a mesma largura que o primeiro. Ele não se estende pela largura do <xref:System.Windows.Forms.FlowLayoutPanel> controle.

6. Defina a <xref:System.Windows.Forms.Control.Dock%2A> Propriedade do segundo botão como `None` . Isso faz com que o botão assuma a largura original.

7. Defina a <xref:System.Windows.Forms.Control.Anchor%2A> Propriedade do segundo botão como `Left, Right` .

    > [!IMPORTANT]
    > O segundo botão assume a mesma largura que o primeiro. Ele não se estende pela largura do <xref:System.Windows.Forms.FlowLayoutPanel> controle. Essa é a regra geral para ancorar e encaixar no <xref:System.Windows.Forms.FlowLayoutPanel> controle: para direções de fluxo vertical, o <xref:System.Windows.Forms.FlowLayoutPanel> controle calcula a largura de uma coluna implícita do controle filho mais largo na coluna. Todos os outros controles nesta coluna com <xref:System.Windows.Forms.Control.Anchor%2A> <xref:System.Windows.Forms.Control.Dock%2A> Propriedades or são alinhados ou ampliados para se ajustarem a essa coluna implícita. O comportamento funciona de maneira similar a direções de fluxo horizontal. O <xref:System.Windows.Forms.FlowLayoutPanel> controle calcula a altura de uma linha implícita do controle filho mais alto na linha e todos os controles filho ancorados ou ancorados nessa linha são alinhados ou dimensionados para se ajustarem à linha implícita.

## <a name="example"></a>Exemplo

A ilustração a seguir mostra quatro botões que são ancorados e encaixados em relação ao botão azul em um <xref:System.Windows.Forms.FlowLayoutPanel> . O <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> é <xref:System.Windows.Forms.FlowDirection.LeftToRight>.

![Ancoragem de FlowLayoutPanel](./media/net-flpanchorexp.gif "NET_FLPanchorExp")

A ilustração a seguir mostra quatro botões que são ancorados e encaixados em relação ao botão azul em um <xref:System.Windows.Forms.FlowLayoutPanel> . O <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> é <xref:System.Windows.Forms.FlowDirection.TopDown>.

![Ancoragem de FlowLayoutPanel](./media/vs-flpanchor2.gif "VS_FLPanchor2")

O exemplo de código a seguir demonstra vários <xref:System.Windows.Forms.Control.Anchor%2A> valores de propriedade para um <xref:System.Windows.Forms.Button> controle em um <xref:System.Windows.Forms.FlowLayoutPanel> controle.

[!code-csharp[System.Windows.Forms.FlowLayoutPanel.AnchorExampleForm#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlowLayoutPanel.AnchorExampleForm/CS/FlpAnchorExampleForm.cs#1)]
[!code-vb[System.Windows.Forms.FlowLayoutPanel.AnchorExampleForm#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlowLayoutPanel.AnchorExampleForm/VB/FlpAnchorExampleForm.vb#1)]

## <a name="compiling-the-code"></a>Compilando o código

Este exemplo requer:

- Referências aos assemblies System, System.Data, System.Drawing e System.Windows.Forms.

## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.FlowLayoutPanel>
- [Visão geral do controle FlowLayoutPanel](flowlayoutpanel-control-overview.md)
