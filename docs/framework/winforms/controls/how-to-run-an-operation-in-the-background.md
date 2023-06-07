---
title: Como executar uma operação no plano de fundo
description: Saiba como usar a classe BackgroundWorker para executar uma operação de Windows Forms demorada em segundo plano.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- background tasks
- forms [Windows Forms], multithreading
- forms [Windows Forms], background operations
- background threads
- BackgroundWorker class [Windows Forms], examples
- threading [Windows Forms], background operations
- background operations
ms.assetid: 5b56e2aa-dc05-444f-930c-2d7b23f9ad5b
ms.openlocfilehash: 6b2a97f5acf1e906dfe141aee62e99a4e50dca9f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621568"
---
# <a name="how-to-run-an-operation-in-the-background"></a>Como executar uma operação no plano de fundo
Se você tiver uma operação que levará muito tempo para ser concluída e não quiser causar atrasos na interface do usuário, poderá usar a <xref:System.ComponentModel.BackgroundWorker> classe para executar a operação em outro thread.  
  
 O exemplo de código a seguir mostra como executar uma operação demorada em segundo plano. O formulário tem botões **Iniciar** e **Cancelar** . Clique no botão **Iniciar** para executar uma operação assíncrona. Clique no botão **Cancelar** para interromper uma operação assíncrona em execução. O resultado de cada operação é exibido em um <xref:System.Windows.Forms.MessageBox> .  
  
 Há um suporte abrangente para esta tarefa no Visual Studio.  
  
 Consulte também [a explicação: executando uma operação em segundo plano](walkthrough-running-an-operation-in-the-background.md).  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[System.ComponentModel.BackgroundWorker.Example#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.BackgroundWorker.Example/CS/Form1.cs#1)]
 [!code-vb[System.ComponentModel.BackgroundWorker.Example#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.BackgroundWorker.Example/VB/Form1.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Referências aos assemblies System, System.Drawing e System.Windows.Forms.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ComponentModel.BackgroundWorker>
- <xref:System.ComponentModel.DoWorkEventArgs>
- [Como implementar um formulário que usa uma operação em segundo plano](how-to-implement-a-form-that-uses-a-background-operation.md)
- [Componente BackgroundWorker](backgroundworker-component.md)
