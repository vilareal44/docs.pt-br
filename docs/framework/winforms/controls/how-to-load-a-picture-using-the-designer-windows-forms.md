---
title: Como carregar uma imagem usando o designer
description: Saiba como usar o controle PictureBox Windows Forms para carregar e exibir uma imagem em um formulário em tempo de design.
ms.date: 03/30/2017
helpviewer_keywords:
- picture formats
- images [Windows Forms], displaying on Windows Forms
- pictures [Windows Forms], displaying
- forms [Windows Forms], displaying images
- PictureBox control [Windows Forms], adding pictures
ms.assetid: 4dc7b973-afb1-4276-8322-20825af96655
ms.openlocfilehash: a05ffe19412fc7a4e3e02f01336d89cce39fac8a
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325595"
---
# <a name="how-to-load-a-picture-using-the-designer-windows-forms"></a>Como carregar uma imagem usando o designer (Windows Forms)

Com o controle de Windows Forms <xref:System.Windows.Forms.PictureBox> , você pode carregar e exibir uma imagem em um formulário em tempo de design, definindo a <xref:System.Windows.Forms.PictureBox.Image%2A> propriedade como uma imagem válida. A tabela a seguir mostra os tipos de arquivo disponíveis.

|Tipo|Extensão de nome de arquivo|
|---|---|
|Bitmap|.bmp|
|Ícone|.ico|
|GIF|.gif|
|Metarquivo|.wmf|
|JPEG|.jpg|

## <a name="to-display-a-picture-at-design-time"></a>Para exibir uma imagem em tempo de design

1. Desenhe um <xref:System.Windows.Forms.PictureBox> controle em um formulário.

2. Na janela **Propriedades** , selecione a <xref:System.Windows.Forms.PictureBox.Image%2A> propriedade e, em seguida, selecione o botão de reticências para exibir a caixa de diálogo **abrir** .

3. Se você estiver procurando um tipo de arquivo específico (por exemplo, arquivos. gif), selecione-o na caixa **arquivos do tipo** .

4. Selecione o arquivo que você deseja exibir.

## <a name="to-clear-the-picture-at-design-time"></a>Para limpar a imagem em tempo de design

1. Na janela **Propriedades** , selecione a <xref:System.Windows.Forms.PictureBox.Image%2A> propriedade. Clique com o botão direito do mouse na pequena imagem em miniatura que aparece à esquerda do nome do objeto de imagem e escolha **Redefinir**.

## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.PictureBox>
- [Visão geral do controle PictureBox](picturebox-control-overview-windows-forms.md)
- [Como modificar o tamanho ou a colocação de uma imagem em tempo de execução](how-to-modify-the-size-or-placement-of-a-picture-at-run-time-windows-forms.md)
- [Como definir imagens em tempo de execução](how-to-set-pictures-at-run-time-windows-forms.md)
- [Controle PictureBox](picturebox-control-windows-forms.md)
