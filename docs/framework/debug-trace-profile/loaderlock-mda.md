---
title: MDA loaderLock
description: Examine o MDA (Assistente de depuração gerenciada) do loaderLock no .NET, que detecta tentativas de execução de código gerenciado em um thread que mantém o bloqueio do carregador de sistema operacional do Windows.
ms.date: 03/30/2017
helpviewer_keywords:
- deadlocks [.NET Framework]
- LoaderLock MDA
- MDAs (managed debugging assistants), loader locks
- managed debugging assistants (MDAs), loader locks
- operating system loader locks
- loader locks
- locks, threads
ms.assetid: 8c10fa02-1b9c-4be5-ab03-451d943ac1ee
ms.openlocfilehash: 055b07a805c5f0b613519d6019950a9b249a4b38
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051610"
---
# <a name="loaderlock-mda"></a>MDA loaderLock
O MDA (Assistente de Depuração Gerenciado) de `loaderLock` detecta tentativas de executar código gerenciado em um thread que mantém o bloqueio do carregador do sistema operacional Microsoft Windows.  Qualquer execução desse tipo é inválida porque pode levar a deadlocks e ao uso de DLLs antes de elas terem sido inicializadas pelo carregador do sistema operacional.  
  
## <a name="symptoms"></a>Sintomas  
 A falha mais comum ao executar o código dentro do bloqueio do carregador do sistema operacional é que os threads enfrentarão deadlock ao tentar chamar outras funções do Win32 que também exijam o bloqueio do carregador.  Exemplos dessas funções são `LoadLibrary`, `GetProcAddress`, `FreeLibrary` e `GetModuleHandle`.  O aplicativo não pode chamar diretamente essas funções; o CLR (Common Language Runtime) pode chamar essas funções como resultado de uma chamada de nível superior como <xref:System.Reflection.Assembly.Load%2A> ou a primeira chamada para uma plataforma de invocação de método.  
  
 Os deadlocks também podem ocorrer se um thread está aguardando o início ou término de outro thread.  Quando a execução de um thread é iniciada ou concluída, ele deve adquirir o bloqueio do carregador do sistema operacional para enviar eventos para as DLLs afetadas.  
  
 Por fim, há casos em que chamadas para DLLs podem ocorrer antes que essas DLLs tenham sido corretamente inicializadas pelo carregador do sistema operacional.  Ao contrário das falhas de deadlock, que podem ser diagnosticadas examinando as pilhas de todos os threads envolvidos no deadlock, é muito difícil diagnosticar o uso de DLLs não inicializadas sem usar esse MDA.  
  
## <a name="cause"></a>Causa  
 Assemblies C++ mistos gerenciados/não gerenciados criados para versões do .NET Framework 1.0 ou 1.1 geralmente tentam executar código gerenciado dentro do bloqueio do carregador, a menos que cuidado especial tenha sido tomado, por exemplo, vinculando com **/NOENTRY**.
  
 Assemblies C++ mistos gerenciados/não gerenciados criados para o .NET Framework versão 2.0 são menos suscetíveis a esses problemas, tendo o mesmo risco reduzido que aplicativos usando DLLs não gerenciadas que violam as regras do sistema operacional.  Por exemplo, se o ponto de entrada `DllMain` de uma DLL não gerenciada chama `CoCreateInstance` para obter um objeto gerenciado que foi exposto a COM, o resultado é uma tentativa de executar código gerenciado dentro do bloqueio do carregador. Para obter mais informações sobre problemas de bloqueio do carregador do .NET Framework versão 2.0 e posteriores, consulte [Inicialização de assemblies mistos](/cpp/dotnet/initialization-of-mixed-assemblies).  
  
## <a name="resolution"></a>Resolução  
 No Visual C++ .NET 2002 e Visual C++ .NET 2003, DLLs compiladas com a opção do compilador `/clr` podem enfrentar um deadlock de forma não determinística quando carregadas; esse problema foi chamado de problema de carregamento de DLLs mistas ou de bloqueio do carregador. No Visual C++ 2005 e posterior, quase todo o não determinismo foi removido do processo de carregamento de DLLs mistas. No entanto, existem alguns cenários restantes para os quais o bloqueio de carregador pode ocorrer (de forma determinística). Para obter detalhes das causas e resoluções para os problemas de bloqueio do carregador restantes, consulte [Inicialização de assemblies mistos](/cpp/dotnet/initialization-of-mixed-assemblies). Se esse tópico não identifica o seu problema de bloqueio do carregador, você precisa examinar a pilha do thread para determinar por que o bloqueio do carregador está ocorrendo e como corrigir o problema. Examine o rastreamento de pilha do thread que ativou esse MDA.  O thread está tentando ilegalmente chamar código gerenciado, mantendo o bloqueio do carregador do sistema operacional.  Você provavelmente verá um `DllMain` da DLL ou um ponto de entrada equivalente na pilha.  As regras do sistema operacional para o que você pode fazer legalmente de dentro de um ponto de entrada desse tipo são muito limitadas.  Essas regras impedem qualquer execução gerenciada.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Normalmente, vários threads dentro do processo enfrentarão deadlock.  Um desses threads provavelmente será um thread responsável pela execução de uma coleta de lixo, portanto, esse deadlock poderá ter um impacto significativo em todo o processo.  Além disso, ele impedirá todas as operações adicionais que exijam o bloqueio do carregador do sistema operacional, tais como carregar e descarregar assemblies ou DLLs e iniciar ou interromper threads.  
  
 Em alguns casos incomuns, também é possível que violações de acesso ou problemas semelhantes sejam disparados em DLLs que são chamadas antes de terem inicializadas.  
  
## <a name="output"></a>Saída  
 Esse MDA relata que uma execução gerenciada inválida está sendo tentada.  Você deve examinar a pilha do thread para determinar por que o bloqueio do carregador está acontecendo e como corrigir o problema.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <loaderLock/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
