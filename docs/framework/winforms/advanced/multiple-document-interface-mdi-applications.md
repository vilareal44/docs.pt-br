---
title: Aplicativos de Interface de Documentos Múltiplos (MDI)
description: Saiba como Windows Forms aplicativos de MDI (interface de vários documentos) permitem que você exiba vários documentos ao mesmo tempo, com cada documento exibido em sua própria janela.
ms.date: 03/30/2017
helpviewer_keywords:
- forms [Windows Forms], MDI
- windows [Windows Forms], mDI
- Windows Forms, MDI applications
- MDI
ms.assetid: 599faf75-13cf-49cc-ad3c-255545e5cb97
ms.openlocfilehash: 0912a989ac1710d72c9db1cceb0e695f0ca85dee
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174647"
---
# <a name="multiple-document-interface-mdi-applications"></a>Aplicativos de Interface de Documentos Múltiplos (MDI)
Aplicativos de interface de documentos múltiplos (MDI) permitem que você exiba vários documentos ao mesmo tempo, com cada documento exibido em sua própria janela. Aplicativos MDI geralmente têm um item de menu de Janela com submenus para alternar entre janelas ou documentos.  
  
> [!NOTE]
> Existem algumas diferenças de comportamento entre formulários MDI e janelas da interface de documento único (SDI) nos Windows Forms. A propriedade `Opacity` não afeta a aparência dos formulários filho MDI. Além disso, o <xref:System.Windows.Forms.Form.CenterToParent%2A> método não afeta o comportamento de formulários filho MDI.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Como criar formulários pai MDI](how-to-create-mdi-parent-forms.md)  
 Fornece instruções para criar o contêiner para diversos documentos em um aplicativo MDI.  
  
 [Como criar formulários filho MDI](how-to-create-mdi-child-forms.md)  
 Fornece instruções de como criar uma ou mais janelas que operam em um formulário pai MDI.  
  
 [Como determinar o filho MDI ativo](how-to-determine-the-active-mdi-child.md)  
 Fornece instruções de como verificar a janela filho que tem o foco (e enviar seu conteúdo para a área de transferência).  
  
 [Como enviar dados para o filhos MDI ativos](how-to-send-data-to-the-active-mdi-child.md)  
 Fornece instruções de como transportar informações para a janela filho ativa.  
  
 [Como Organizar Formulários Filho MDI](how-to-arrange-mdi-child-forms.md)  
 Fornece instruções de como organizar lado a lado, em cascata ou organizar as janelas filho de um aplicativo MDI.
