---
title: Como implementar um formulário que use uma operação em segundo plano
description: Saiba como implementar um formulário do Windows que usa uma operação em segundo plano para que uma operação possa continuar a ser executada enquanto outra operação continua.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- threading [Windows Forms], forms
- BackgroundWorker component
- background tasks
- forms [Windows Forms], multithreading
- components [Windows Forms], asynchronous
- forms [Windows Forms], background operations
- background threads
- threading [Windows Forms], background operations
- background operations
ms.assetid: 9f483f93-1613-4be1-a021-b4934e9c78f3
ms.openlocfilehash: 23bf4bc02fbf998d92dfce6d84e4e337cbefe7d9
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174517"
---
# <a name="how-to-implement-a-form-that-uses-a-background-operation"></a>Como implementar um formulário que use uma operação em segundo plano
O programa de exemplo a seguir cria um formulário que calcula os números de Fibonacci. O cálculo é executado em um thread separado do thread da interface do usuário, portanto, a interface do usuário continua a ser executada sem atrasos conforme o cálculo continua.  
  
 Há um suporte abrangente para esta tarefa no Visual Studio.  
  
 Consulte também [passo a passos: Implementando um formulário que usa uma operação em segundo plano](walkthrough-implementing-a-form-that-uses-a-background-operation.md).  
  
## <a name="example"></a>Exemplo  
 [!code-cpp[System.ComponentModel.BackgroundWorker#1](~/samples/snippets/cpp/VS_Snippets_Winforms/System.ComponentModel.BackgroundWorker/CPP/fibonacciform.cpp#1)]
 [!code-csharp[System.ComponentModel.BackgroundWorker#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.BackgroundWorker/CS/fibonacciform.cs#1)]
 [!code-vb[System.ComponentModel.BackgroundWorker#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.BackgroundWorker/VB/fibonacciform.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Referências aos assemblies System, System.Drawing e System.Windows.Forms.  
  
## <a name="robust-programming"></a>Programação robusta  
  
> [!CAUTION]
> O uso de multithreading de qualquer tipo pode expor o computador a bugs muito sérios e complexos. Consulte as [Melhores práticas de threading gerenciado](../../../standard/threading/managed-threading-best-practices.md) antes de implementar qualquer solução que use multithreading.  
  
## <a name="see-also"></a>Veja também

- <xref:System.ComponentModel.BackgroundWorker>
- <xref:System.ComponentModel.DoWorkEventArgs>
- [Visão geral do padrão assíncrono baseado em evento](../../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)
- [Práticas recomendadas de Threading gerenciado](../../../standard/threading/managed-threading-best-practices.md)
