---
title: Funções de retorno de chamada
description: Leia sobre as funções de retorno de chamada, que são o código com um aplicativo gerenciado que ajuda uma função DLL não gerenciada a concluir uma tarefa.
ms.date: 03/30/2017
helpviewer_keywords:
- callback function
- platform invoke, calling unmanaged functions
ms.assetid: c0aa8533-3b3b-42e8-9f60-84919793098c
ms.openlocfilehash: e28756b5ed935aff83363b38d6f33982e87718b2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621711"
---
# <a name="callback-functions"></a>Funções de retorno de chamada
Uma função de retorno de chamada é o código em um aplicativo gerenciado que ajuda uma função de DLL não gerenciada a concluir uma tarefa. As chamadas a uma função de retorno de chamada passam indiretamente de um aplicativo gerenciado, por meio de uma função de DLL e novamente para a implementação gerenciada. Algumas das muitas funções de DLL chamadas com a invocação de plataforma exigem que uma função de retorno de chamada no código gerenciado seja executada corretamente.  
  
 Para chamar a maioria das funções de DLL por meio do código gerenciado, crie uma definição gerenciada da função e, em seguida, chame-a. O processo é simples.  
  
 O uso de uma função de DLL que exige uma função de retorno de chamada apresenta algumas etapas adicionais. Primeiro, você deve determinar se a função exige um retorno de chamada examinando a documentação da função. Em seguida, você precisa criar a função de retorno de chamada no aplicativo gerenciado. Por fim, chame a função de DLL, passando um ponteiro para a função de retorno de chamada como um argumento.

 A ilustração a seguir resume as etapas de implementação e a função de retorno de chamada:  
  
 ![Diagrama mostrando o processo de retorno de chamada da invocação de plataforma.](./media/callback-functions/platform-invoke-callback-process.gif)  
  
 As funções de retorno de chamada são ideais para uso em situações em que uma tarefa é executada repetidamente. Outro uso comum é com funções de enumeração, como **EnumFontFamilies**, **EnumPrinters** e **EnumWindows** na API do Windows. A função **EnumWindows** enumera por meio de todas as janelas existentes no computador, chamando a função de retorno de chamada para executar uma tarefa em cada janela. Para obter instruções e um exemplo, consulte [Como implementar funções de retorno de chamada](how-to-implement-callback-functions.md).  
  
## <a name="see-also"></a>Consulte também

- [Como: Implementar funções de retorno de chamada](how-to-implement-callback-functions.md)
- [Chamando uma função de DLL](calling-a-dll-function.md)
