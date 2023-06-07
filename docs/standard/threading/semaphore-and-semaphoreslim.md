---
title: Semaphore e SemaphoreSlim
description: Saiba mais sobre o Semaphore & SemaphoreSlim. O semáforo de classe é um wrapper fino em volta do objeto de semáforo do Win32. A classe SemaphoreSlim é um semáforo rápido.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- counting semaphores
- semaphores
- threading [.NET Framework], cross-process synchronization
- Semaphore class, about Semaphore class
- SemaphoreSlim class, about SemaphoreSlim class
- threading [.NET Framework], Semaphore class
ms.assetid: 7722a333-b974-47a2-a7c0-f09097fb644e
ms.openlocfilehash: 21f0d7e3fb446a7b750c45cfe8ef3f087a77888a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600445"
---
# <a name="semaphore-and-semaphoreslim"></a>Semaphore e SemaphoreSlim
A classe <xref:System.Threading.Semaphore?displayProperty=nameWithType> representa um sinal com nome (em todo o sistema) ou local. É um wrapper fino em torno do objeto de sinal Win32. Sinais do Win32 são contagem de sinais que podem ser usadas para controlar o acesso a um pool de recursos.  
  
 A classe <xref:System.Threading.SemaphoreSlim> representa um sinal leve e rápido que pode ser usado para aguardar por um único processo quando forem esperados tempos de espera muito curtos. <xref:System.Threading.SemaphoreSlim> conta com o máximo possível em primitivos de sincronização fornecidos pelo Common Language Runtime (CLR). No entanto, fornece também identificadores de espera baseados no kernel inicializados lentamente, conforme necessário para dar suporte à espera de vários sinais. <xref:System.Threading.SemaphoreSlim> oferece suporte também ao uso de tokens de cancelamento, mas não oferece suporte a sinais com nome ou ao uso de um identificador de espera para sincronização.  
  
## <a name="managing-a-limited-resource"></a>Gerenciando um recurso limitado  
 Os threads inserem o sinal chamando o método <xref:System.Threading.WaitHandle.WaitOne%2A>, que é herdado da classe <xref:System.Threading.WaitHandle>, no caso de um objeto <xref:System.Threading.Semaphore?displayProperty=nameWithType> ou o método <xref:System.Threading.SemaphoreSlim.Wait%2A?displayProperty=nameWithType> ou <xref:System.Threading.SemaphoreSlim.WaitAsync%2A?displayProperty=nameWithType>, no caso de um objeto <xref:System.Threading.SemaphoreSlim>. Quando a chamada é retornada, a contagem no sinal é reduzida. Quando um thread solicita entrada e a contagem é zero, o thread é bloqueado. À medida que os threads liberam o sinal chamando o método <xref:System.Threading.Semaphore.Release%2A?displayProperty=nameWithType> ou <xref:System.Threading.SemaphoreSlim.Release%2A?displayProperty=nameWithType>, os threads bloqueados têm permissão para entrar. Não há uma ordem garantida, como primeiro a entrar, primeiro a sair (PEPS) ou último a entrar, primeiro a sair (UEPS) para threads bloqueados para inserir o sinal.  
  
 Um thread pode inserir o sinal várias vezes chamando o método <xref:System.Threading.Semaphore?displayProperty=nameWithType> do objeto <xref:System.Threading.WaitHandle.WaitOne%2A> ou o método <xref:System.Threading.SemaphoreSlim> do objeto <xref:System.Threading.SemaphoreSlim.Wait%2A> repetidas vezes. Para liberar o sinal, o thread pode chamar a sobrecarga do método <xref:System.Threading.Semaphore.Release?displayProperty=nameWithType> ou <xref:System.Threading.SemaphoreSlim.Release?displayProperty=nameWithType> o mesmo número de vezes ou chamar a sobrecarga do método <xref:System.Threading.Semaphore.Release%28System.Int32%29?displayProperty=nameWithType> ou <xref:System.Threading.SemaphoreSlim.Release%28System.Int32%29?displayProperty=nameWithType> e especificar o número de entradas a ser liberado.  
  
### <a name="semaphores-and-thread-identity"></a>Sinais e identidade do thread  
 Os dois tipos de sinais não impõem a identidade do thread em chamadas para os métodos <xref:System.Threading.WaitHandle.WaitOne%2A>, <xref:System.Threading.SemaphoreSlim.Wait%2A>, <xref:System.Threading.Semaphore.Release%2A> e <xref:System.Threading.SemaphoreSlim.Release%2A?displayProperty=nameWithType>. Por exemplo, um cenário de uso comum para sinais envolve um thread de produtor e um thread de consumidor, com um thread sempre incrementando a contagem de sinais e o outro sempre diminuindo-a.  
  
 É responsabilidade do programador garantir que um thread não libere o sinal muitas vezes. Por exemplo, suponha que um sinal tenha uma contagem máxima de dois, e que o thread A e o thread B insiram o sinal. Se um erro de programação no thread B fizer com que ele chame `Release` duas vezes, ambas as chamadas serão bem-sucedidas. A contagem no sinal está completa e quando o thread A eventualmente chama `Release`, uma <xref:System.Threading.SemaphoreFullException> é lançada.  
  
## <a name="named-semaphores"></a>Sinais com nome  
 O sistema operacional Windows permite que os sinais tenham nomes. Um sinal com nome é para todo o sistema. Ou seja, depois que o sinal com nome for criado, ele ficará visível para todos os threads em todos os processos. Assim, o sinal com nome pode ser usado para sincronizar as atividades de processos, bem como os threads.  
  
 Você pode criar um objeto <xref:System.Threading.Semaphore> que representa um sinal de sistema com nome usando um dos construtores que especifica um nome.  
  
> [!NOTE]
> Como os sinais com nome são para todo o sistema, é possível ter vários objetos <xref:System.Threading.Semaphore> que representam o mesmo sinal com nome. Cada vez que você chamar um construtor ou o método <xref:System.Threading.Semaphore.OpenExisting%2A?displayProperty=nameWithType>, um novo objeto <xref:System.Threading.Semaphore> é criado. Especificar o mesmo nome repetidas vezes cria vários objetos que representam o mesmo sinal com nome.  
  
 Tenha cuidado ao usar sinais com nome. Como eles são para todo o sistema, outro processo que usa o mesmo nome pode inserir seu sinal inesperadamente. Código mal-intencionado em execução no mesmo computador pode usar isso como base de um ataque de negação de serviço.  
  
 Use a segurança de controle de acesso para proteger um objeto <xref:System.Threading.Semaphore> que representa um sinal com nome, preferencialmente usando um construtor que especifica um objeto <xref:System.Security.AccessControl.SemaphoreSecurity?displayProperty=nameWithType>. Também é possível aplicar a segurança de controle de acesso usando o método <xref:System.Threading.Semaphore.SetAccessControl%2A?displayProperty=nameWithType>, mas isso deixa uma janela de vulnerabilidade entre o momento em que o sinal é criado e o momento em que ele é protegido. Proteger sinais com a segurança de controle de acesso ajuda a impedir ataques mal-intencionados, mas não resolve o problema de colisão de nome não intencional.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Threading.Semaphore>
- <xref:System.Threading.SemaphoreSlim>
- [Objetos e recursos de Threading](threading-objects-and-features.md)
