---
title: Mutexes
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- wait handles
- threading [.NET Framework], Mutex class
- Mutex class, about Mutex class
- threading [.NET Framework], cross-process synchronization
ms.assetid: 9dd06e25-12c0-4a9e-855a-452dc83803e2
ms.openlocfilehash: f9267bdd19a14995851f2689651c001815812912
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291169"
---
# <a name="mutexes"></a>Mutexes
Você pode usar um objeto <xref:System.Threading.Mutex> para fornecer acesso exclusivo a um recurso. A classe <xref:System.Threading.Mutex> usa mais recursos do sistema do que a classe <xref:System.Threading.Monitor>, mas pode realizar marshaling entre limites de domínio de aplicativo, pode ser usada com vários esperas e pode ser usada para sincronizar threads em processos diferentes. Para obter uma comparação dos mecanismos de sincronização gerenciados, confira [Visão geral dos primitivos de sincronização](overview-of-synchronization-primitives.md).  
  
 Para obter exemplos de código, consulte a documentação de referência para os construtores <xref:System.Threading.Mutex.%23ctor%2A>.  
  
## <a name="using-mutexes"></a>Como usar mutexes  
 Um thread chama o método <xref:System.Threading.WaitHandle.WaitOne%2A> de um mutex para solicitar a propriedade. A chamada fica bloqueada até que o mutex esteja disponível, ou até que o intervalo de tempo limite opcional expire. O estado de um mutex será sinalizado se nenhum thread for seu proprietário.  
  
 Um thread libera um mutex chamando seu método <xref:System.Threading.Mutex.ReleaseMutex%2A>. Os mutexes têm afinidade de thread; ou seja, o mutex pode ser liberado somente pelo thread ao qual pertence. Se um thread liberar um mutex do qual não é proprietário, uma <xref:System.ApplicationException> será gerada no thread.  
  
 Como a classe <xref:System.Threading.Mutex> é derivada de <xref:System.Threading.WaitHandle>, você também pode chamar os métodos estáticos <xref:System.Threading.WaitHandle.WaitAll%2A> ou <xref:System.Threading.WaitHandle.WaitAny%2A> de <xref:System.Threading.WaitHandle> para solicitar a propriedade de um <xref:System.Threading.Mutex>, em combinação com outros identificadores de espera.  
  
 Se um thread possui um <xref:System.Threading.Mutex>, esse thread pode especificar o mesmo <xref:System.Threading.Mutex> em chamadas repetidas de solicitação e espera sem bloquear a execução; no entanto, ele deve liberar o <xref:System.Threading.Mutex> a mesma quantidade de vezes a fim de liberar a propriedade.  
  
## <a name="abandoned-mutexes"></a>Mutexes abandonados  
 Se um thread é encerrado sem liberar um <xref:System.Threading.Mutex>, diz-se que o mutex está abandonado. Isso geralmente indica um erro grave de programação, pois o recurso que o mutex está protegendo pode ser deixado em um estado inconsistente. No .NET Framework versão 2.0, uma <xref:System.Threading.AbandonedMutexException> é gerada no próximo thread que adquire o mutex.  
  
> [!NOTE]
> No .NET Framework versões 1.0 e 1.1, um <xref:System.Threading.Mutex> abandonado é definido como o estado sinalizado, e o próxima thread em espera obtém a propriedade. Se nenhum thread estiver esperando, o <xref:System.Threading.Mutex> permanecerá em um estado sinalizado. Nenhuma exceção é lançada.  
  
 No caso de um mutex de todo o sistema, um mutex abandonado pode indicar que um aplicativo foi finalizado abruptamente (por exemplo, usando o Gerenciador de Tarefas do Windows).  
  
## <a name="local-and-system-mutexes"></a>Mutexes locais e do sistema  
 Há dois tipos de mutexes: mutexes locais e mutexes de sistema nomeados. Se você criar um objeto <xref:System.Threading.Mutex> um construtor que aceita um nome, ele será associado a um objeto do sistema operacional com esse nome. Os mutexes de sistema nomeados são visíveis em todo o sistema operacional e podem ser usados para sincronizar as atividades dos processos. Você pode criar vários objetos <xref:System.Threading.Mutex> que representam o mesmo mutex de sistema nomeado, e você pode usar o método <xref:System.Threading.Mutex.OpenExisting%2A> para abrir um mutex de sistema nomeado existente.  
  
 Um mutex local existe somente dentro de seu processo. Ele pode ser usado por qualquer thread em seu processo que tenha referência ao objeto <xref:System.Threading.Mutex> local. Cada objeto <xref:System.Threading.Mutex> é um mutex local separado.  
  
### <a name="access-control-security-for-system-mutexes"></a>Segurança do controle de acesso para mutexes de sistema  
 O .NET Framework versão 2.0 permite a consulta e definição da segurança de controle de acesso do Windows para objetos de sistema nomeados. Recomendamos a proteção dos mutexes de sistema desde o momento da criação, pois os objetos do sistema são globais e, portanto, podem ser bloqueados por outro código além do seu.  
  
 Para saber mais sobre segurança de controle de acesso para mutexes, confira as classes <xref:System.Security.AccessControl.MutexSecurity> e <xref:System.Security.AccessControl.MutexAccessRule>, a enumeração <xref:System.Security.AccessControl.MutexRights>, os métodos <xref:System.Threading.Mutex.GetAccessControl%2A>, <xref:System.Threading.Mutex.SetAccessControl%2A> e <xref:System.Threading.Mutex.OpenExisting%2A> da classe <xref:System.Threading.Mutex> e o construtor <xref:System.Threading.Mutex.%23ctor%28System.Boolean%2CSystem.String%2CSystem.Boolean%40%2CSystem.Security.AccessControl.MutexSecurity%29>.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Threading.Mutex?displayProperty=nameWithType>
- <xref:System.Threading.Mutex.%23ctor%2A>
- <xref:System.Security.AccessControl.MutexSecurity?displayProperty=nameWithType>
- <xref:System.Security.AccessControl.MutexAccessRule?displayProperty=nameWithType>
- <xref:System.Threading.Monitor?displayProperty=nameWithType>
- [Objetos e recursos de threading](threading-objects-and-features.md)
- [Threads e threading](threads-and-threading.md)
- [Threading](index.md)
