---
title: Consumindo funções de DLL não gerenciadas
description: Consuma funções de DLL não gerenciadas usando o serviço de invocação de plataforma, que permite que o código gerenciado chame funções não gerenciadas implementadas em bibliotecas de DLL.
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged functions, calling
- COM interop, DLL functions
- unmanaged functions
- COM interop, platform invoke
- platform invoke, about platform invoke
- DLL functions, consuming unmanaged
- interoperation with unmanaged code, DLL functions
- interoperation with unmanaged code, platform invoke
- platform invoke
- DLL functions
ms.assetid: eca7606e-ebfb-4f47-b8d9-289903fdc045
ms.openlocfilehash: 880cbd4701ae4aee35038f6402b3beb70e60290c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622179"
---
# <a name="consuming-unmanaged-dll-functions"></a>Consumindo funções de DLL não gerenciadas
A invocação de plataforma é um serviço que permite que um código gerenciado chame funções não gerenciadas implementadas em DLLs (bibliotecas de vínculo dinâmico), como aquelas na API do Windows. Ela localiza e invoca uma função exportada e realiza marshaling dos argumentos (inteiros, cadeias de caracteres, matrizes, estruturas e assim por diante) além do limite de interoperação, conforme necessário.  
  
 Esta seção apresenta as tarefas associadas às funções DLL não gerenciadas de consumo e fornece mais informações sobre invocação de plataforma. Além das tarefas a seguir, há considerações gerais e um link que fornece exemplos e informações adicionais.  
  
#### <a name="to-consume-exported-dll-functions"></a>Para consumir funções de DLL exportadas  
  
1. [Identifique as funções nas DLLs](identifying-functions-in-dlls.md).  
  
     No mínimo, é necessário especificar o nome da função e o nome da DLL que ela contém.  
  
2. [Crie uma classe para conter funções de DLL](creating-a-class-to-hold-dll-functions.md).  
  
     Use uma classe existente, crie uma classe individual para cada função não gerenciada ou crie uma classe que contém um conjunto de funções não gerenciadas relacionadas.  
  
3. [Crie protótipos em um código gerenciado](creating-prototypes-in-managed-code.md).  
  
     [Visual Basic] Use a instrução **Declare** com as palavras-chave **Function** e **Lib**. Em alguns casos raros, é possível usar o **DllImportAttribute** com as palavras-chave **Shared Function**. Esses casos são explicados mais adiante nesta seção.  
  
     C# Use o **DllImportAttribute** para identificar a dll e a função. Marque o método com os modificadores **static** e **extern**.  
  
     [C++] Use o **DllImportAttribute** para identificar a DLL e a função. Marque o método wrapper ou a função com **extern "C"**.  
  
4. [Chame uma função de DLL](calling-a-dll-function.md).  
  
     Chame o método na classe gerenciada como faria com qualquer outro método gerenciado. [Passar estruturas](passing-structures.md) e [implementar funções de retorno de chamada](callback-functions.md) são casos especiais.  
  
 Para obter exemplos que demonstram como construir declarações baseadas no .NET a serem usadas com a invocação de plataforma, consulte [Realizando marshaling de dados com a invocação de plataforma](marshaling-data-with-platform-invoke.md).  
  
## <a name="a-closer-look-at-platform-invoke"></a>Visão aprofundada da invocação de plataforma  
 A invocação de plataforma se baseia nos metadados para localizar funções exportadas e realizar marshaling em seus argumentos em tempo de execução. A ilustração a seguir mostra este processo.  
  
 ![Diagrama que mostra uma chamada de invocação de plataforma.](./media/consuming-unmanaged-dll-functions/platform-invoke-call.gif)  
  
 Quando uma invocação de plataforma chama uma função não gerenciada, ela executa a seguinte sequência de ações:  
  
1. Localiza a DLL que contém a função.  
  
2. Carrega a DLL na memória.  
  
3. Localiza o endereço da função na memória e efetua push de seus argumentos para a pilha, realizando marshaling dos dados, conforme necessário.  
  
    > [!NOTE]
    > A localização e o carregamento da DLL, bem como a localização do endereço da função na memória, ocorrem apenas na primeira chamada à função.  
  
4. Transfere o controle para a função não gerenciada.  
  
 A invocação de plataforma gera exceções geradas pela função não gerenciada para o chamador gerenciado.

## <a name="see-also"></a>Consulte também

- [Interoperação com código não gerenciado](index.md)
- [Exemplos de invocação de plataforma](platform-invoke-examples.md)
- [Realizando marshaling de interoperabilidade](interop-marshaling.md)
