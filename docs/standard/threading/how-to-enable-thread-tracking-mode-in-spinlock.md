---
title: Como habilitar o modo de controle de thread em SpinLock
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SpinLock, how to enable thread-tracking
ms.assetid: 62ee2e68-0bdd-4869-afc9-f0a57a11ae01
ms.openlocfilehash: 1a46655530948bb8732362dbd6d86ead495b0998
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84279524"
---
# <a name="how-to-enable-thread-tracking-mode-in-spinlock"></a>Como habilitar o modo de controle de thread em SpinLock
<xref:System.Threading.SpinLock?displayProperty=nameWithType> é um bloqueio de exclusão mútua de baixo nível que você pode usar para cenários com tempos de espera muito curtos. O <xref:System.Threading.SpinLock> não oferece reinserção. Depois que um thread entra no bloqueio, ele deve sair do bloqueio corretamente antes de poder entrar novamente. Normalmente, qualquer tentativa de reintroduzir o bloqueio causaria um deadlock, e os deadlocks podem ser muito difíceis de depurar. Como um auxílio ao desenvolvimento, o <xref:System.Threading.SpinLock?displayProperty=nameWithType> suporta um modo de controle de thread que faz com que uma exceção seja acionada quando um thread tenta reintroduzir um bloqueio que ele já possui. Isso permite que você localize mais facilmente o ponto em que o bloqueio não foi encerrado corretamente. Você pode ativar o modo de controle de thread usando o construtor <xref:System.Threading.SpinLock> que leva um parâmetro de entrada booleano e passando um argumento de `true`. Depois de concluir as fases de desenvolvimento e teste, desative o modo de controle de thread para um melhor desempenho.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra o modo de controle de thread. As linhas que saem corretamente do bloqueio são comentadas para simular um erro de codificação que causa um dos seguintes resultados:  
  
- Uma exceção será lançada se o <xref:System.Threading.SpinLock> foi criado usando um argumento de `true` (`True` no Visual Basic).  
  
- Um deadlock se o <xref:System.Threading.SpinLock> foi criado usando um argumento de `false` (`False` no Visual Basic).  
  
 [!code-csharp[CDS_SpinLock#01](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinlock/cs/spinlockdemo.cs#01)]
 [!code-vb[CDS_SpinLock#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinlock/vb/spinlock_threadtracking.vb#01)]  
  
## <a name="see-also"></a>Veja também

- [SpinLock](spinlock.md)
