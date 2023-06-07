---
title: Chamando métodos assíncronos usando IAsyncResult
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- ending asynchronous operations
- waiting for asynchronous operations
- asynchronous method calling
- calling asynchronous methods
- asynchronous programming, calling asynchronous methods
- IAsyncResult interface, calling asynchronous methods
- stopping asynchronous operations
ms.assetid: 07fba116-045b-473c-a0b7-acdbeb49861f
ms.openlocfilehash: 88ca1b5bfbb8bfbdfef01dea8af07c5d56784c5c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289909"
---
# <a name="calling-asynchronous-methods-using-iasyncresult"></a>Chamando métodos assíncronos usando IAsyncResult
Tipos em bibliotecas de classes de terceiros e no .NET Framework podem fornecer métodos que permitem que um aplicativo continue a ser executado durante o desempenho de operações assíncronas em threads diferentes do thread principal do aplicativo. As seções a seguir descrevem e fornecem exemplos de código que demonstram diferentes maneiras de chamar métodos assíncronos que usam o padrão de design <xref:System.IAsyncResult>.  
  
- [Bloqueio da execução do aplicativo encerrando uma operação assíncrona](blocking-application-execution-by-ending-an-async-operation.md).  
  
- [Bloqueando a execução do aplicativo usando um AsyncWaitHandle](blocking-application-execution-using-an-asyncwaithandle.md).  
  
- [Sondando o status de uma operação assíncrona](polling-for-the-status-of-an-asynchronous-operation.md).  
  
- [Usando um delegado AsyncCallback para finalizar uma operação assíncrona](using-an-asynccallback-delegate-to-end-an-asynchronous-operation.md).  
  
## <a name="see-also"></a>Veja também

- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
- [Visão geral do padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md)
