---
title: 'Como: implementar a interface INotifyPropertyChanged'
description: Saiba como implementar a interface INotifyPropertyChanged em objetos comerciais que são usados em Windows Forms Associação de dados.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- INotifyPropertyChanged interface [Windows Forms], implementing
ms.assetid: eac428af-b43b-46ac-80d9-1a5f88658725
ms.openlocfilehash: 83d2ef32787d2dbcd877bc77dcede10111098f8a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619262"
---
# <a name="how-to-implement-the-inotifypropertychanged-interface"></a>Como: implementar a interface INotifyPropertyChanged
O exemplo de código a seguir demonstra como implementar a <xref:System.ComponentModel.INotifyPropertyChanged> interface. Implemente essa interface em objetos comerciais que são usados em Windows Forms Associação de dados. Quando implementada, a interface se comunica com um controle associado que a propriedade altera em um objeto comercial.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[System.ComponentModel.IPropertyChangeExample#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.IPropertyChangeExample/CS/Form1.cs#1)]
 [!code-vb[System.ComponentModel.IPropertyChangeExample#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.IPropertyChangeExample/VB/Form1.vb#1)]  
  
## <a name="see-also"></a>Confira também

- [Como: aplicar o padrão PropertyNameChanged](how-to-apply-the-propertynamechanged-pattern.md)
- [Associação de dados do Windows Forms](windows-forms-data-binding.md)
- [Como acionar notificações de alteração usando um BindingSource e a interface INotifyPropertyChanged](./controls/raise-change-notifications--bindingsource.md)
- [Notificação de alteração na associação de dados do Windows Forms](change-notification-in-windows-forms-data-binding.md)
