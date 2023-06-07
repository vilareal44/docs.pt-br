---
title: Práticas recomendadas de confiabilidade
description: Consulte as práticas recomendadas para confiabilidade em aplicativos de servidor baseados em host .NET, como SQL Server. Impeça-os de vazar recursos ou se tornar desativado.
ms.date: 03/30/2017
helpviewer_keywords:
- marking locks
- rebooting databases
- denial of service attacks
- back-out code
- SQL Server [.NET Framework], reliability
- synchronization, reliability
- single-threaded COM components
- slow leaks
- suspending threads
- asynchronous exception handling
- leaked resources [.NET Framework]
- unmanaged memory
- memory, reliability
- threading [.NET Framework], reliability
- process-wide domain shared states
- shared states
- SafeHandle class, reliability
- reliability contracts [.NET Framework]
- cleanup operations
- constrained execution regions
- CERs
- finalizers, reliability
- reliability [.NET Framework]
- blocks, reliability
- finally clauses
- cross-application domain shared states
- catch blocks
- identifying locks
- writing reliable code
- impersonation
- GC.KeepAlive method
- managed threading
- locks, reliability
- STA-dependent features
- fibers
ms.assetid: cf624c1f-c160-46a1-bb2b-213587688da7
ms.openlocfilehash: 134b71153f95dffd4525f307d291ce4389e0ce60
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474235"
---
# <a name="reliability-best-practices"></a>Práticas recomendadas de confiabilidade

As seguintes regras de confiabilidade estão orientadas ao SQL Server. No entanto, eles também se aplicam a qualquer aplicativo para servidores baseado em host. É extremamente importante que os servidores como o SQL Server não tenham perda de recursos e não fiquem inoperantes.  No entanto, isso não pode ser feito para escrever código de recuo para cada método que altera o estado de um objeto.  A meta é não gravar código gerenciado 100 por cento confiável que se recuperará de erros em todos os locais com o código de recuo.  Isso seria uma tarefa difícil, com pouca probabilidade de êxito.  O CLR (Common Language Runtime) não pode fornecer com facilidade garantias suficientemente fortes para que o código gerenciado possa tornar a tarefa de escrever código perfeito viável.  Observe que, diferentemente do ASP.NET, o SQL Server usa somente um processo que não pode ser reciclado sem a interrupção de um banco de dados por um período de tempo inaceitavelmente longo.

Com essas garantias mais fracas e em execução em um único processo, a confiabilidade é baseada no encerramento de threads ou na reciclagem de domínios do aplicativo quando necessário, além tomar precauções para garantir que os recursos do sistema operacional, tais como memória ou identificadores, não vazem.  Mesmo com essa restrição de confiabilidade mais simples, ainda há um requisito de confiabilidade significativo:

- Nunca perder recursos do sistema operacional.

- Identificar todos os bloqueios gerenciados em todos os formulários para o CLR.

- Nunca interromper o estado compartilhado entre domínios do aplicativo, permitindo que a reciclagem <xref:System.AppDomain> funcionasse sem problemas.

Embora seja teoricamente possível escrever código gerenciado para tratar exceções <xref:System.Threading.ThreadAbortException>, <xref:System.StackOverflowException> e <xref:System.OutOfMemoryException>, esperar que os desenvolvedores escrevam um código tão robusto em todo um aplicativo é pouco razoável.  Por esse motivo, as exceções fora de banda resultam no encerramento do thread em execução; e se o thread encerrado estava editando o estado compartilhado, o que pode ser determinado dependendo de o thread manter ou não um bloqueio, o <xref:System.AppDomain> é descarregado.  Quando um método que estiver editando o estado compartilhado for encerrado, o estado será corrompido porque não é possível gravar código de recup confiável para atualizações para estado compartilhado.

No .NET Framework versão 2.0, o único host que exige confiabilidade é o SQL Server.  Se o assembly será executado no SQL Server, você deverá fazer o trabalho de confiabilidade para todas as partes do assembly, mesmo se houver recursos específicos que ficarão desabilitados quando em execução no banco de dados.  Isso é necessário porque o mecanismo de análise de código examina o código no nível de assembly e não pode diferenciar o código desabilitado. Outra consideração sobre a programação do SQL Server é que o SQL Server executa tudo em um processo e a reciclagem de <xref:System.AppDomain> é usada para limpar todos os recursos como memória e identificadores do sistema operacional.

Você não pode depender de finalizadores ou destruidores ou blocos `try/finally` para código de recuo. Eles podem ser interrompidos ou não chamados.

Exceções assíncronas podem ser geradas em locais inesperados, possivelmente em toda instrução do computador: <xref:System.Threading.ThreadAbortException>, <xref:System.StackOverflowException> e <xref:System.OutOfMemoryException>.

Threads gerenciados não são necessariamente threads Win32 em SQL; eles podem ser fibras.

É muito difícil alterar um estado compartilhado mutável entre domínios do aplicativo ou em todo o processo com segurança e isso deve ser evitado sempre que possível.

Condições de falta de memória não são raras no SQL Server.

Se bibliotecas hospedadas no SQL Server não atualizarem corretamente o estado compartilhado, há uma grande probabilidade de que o código não se recuperará até o banco de dados ter sido reiniciado.  Além disso, em alguns casos extremos, é possível que isso possa causar falha no processo do SQL Server, fazendo com que o banco de dados seja reinicializado.  Reinicializar o banco de dados pode deixar um site da Web inoperante ou afetar as operações da empresa, afetando a disponibilidade.  Uma perda lenta de recursos de sistema operacional como memória ou identificadores pode fazer com que o servidor eventualmente falhe ao alocar identificadores sem possibilidade de recuperação; também é possível que o desempenho do servidor se degrade lentamente e reduza a disponibilidade de aplicativos do cliente.  É claro que queremos evitar esses cenários.

## <a name="best-practice-rules"></a>Regras de práticas recomendadas

A introdução se concentrou no que a revisão de código para o código gerenciado que é executado no servidor precisaria capturar para aumentar a estabilidade e a confiabilidade do framework. Todas essas verificações são uma boa prática em geral e uma necessidade absoluta no servidor.

No caso de uma restrição de recurso ou um deadlock, o SQL Server anulará um thread ou subdividirá um <xref:System.AppDomain>.  Quando isso acontece, o único elemento que certamente está em execução é o código de recuo em uma CER (região de execução restrita).

### <a name="use-safehandle-to-avoid-resource-leaks"></a>Usar SafeHandle para evitar vazamentos de recursos

No caso de um descarregamento de <xref:System.AppDomain>, você não pode depender de blocos `finally` ou finalizadores serem executados, portanto, é importante abstrair todo acesso a recursos do sistema operacional por meio da classe <xref:System.Runtime.InteropServices.SafeHandle> em vez de <xref:System.IntPtr>, <xref:System.Runtime.InteropServices.HandleRef> ou classes semelhantes. Isso permite que o CLR acompanhe e feche os identificadores que você usa até mesmo no caso de desativação de <xref:System.AppDomain>.  <xref:System.Runtime.InteropServices.SafeHandle> estará usando um finalizador crítico que o CLR sempre executará.

O identificador de sistema operacional é armazenado no SafeHandle desde o momento em que ele é criado até o momento em que ele é liberado.  Não há período durante o qual um <xref:System.Threading.ThreadAbortException> pode ocorrer para que haja perda de um identificador.  Além disso, uma invocação de plataforma faz a contagem de referências do identificador, o que permite um acompanhamento preciso do tempo de vida dele e evita um problema de segurança com uma condição de corrida entre `Dispose` e um método que está usando atualmente o identificador.

A maioria das classes que atualmente têm um finalizador apenas para limpar um identificador de sistema operacional não precisarão mais do finalizador. Em vez disso, o finalizador estará na classe derivada <xref:System.Runtime.InteropServices.SafeHandle>.

Observe que <xref:System.Runtime.InteropServices.SafeHandle> não é substituição para <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType>.  Ainda há possibilidade de contenção de recursos e vantagens de desempenho para descartar explicitamente os recursos de sistema operacional.  Observe que apenas blocos `finally` que descartam explicitamente os recursos podem não ser executados até a conclusão.

<xref:System.Runtime.InteropServices.SafeHandle> permite que você implemente seu próprio método <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> que executa o trabalho para liberar o identificador, tal como passar o estado para uma rotina de liberação de identificador de sistema operacional ou liberar um conjunto de identificadores em um loop.  O CLR assegura que esse método seja executado.  É responsabilidade do autor da implementação de <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> assegurar que o identificador seja liberado em todas as circunstâncias. Falha em fazê-lo causará perda do identificador, o que geralmente resultará em perda de recursos nativos associados com o identificador. Portanto, é essencial estruturar classes derivadas <xref:System.Runtime.InteropServices.SafeHandle> de modo que a implementação de <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> não exija a alocação de todos os recursos que podem não estar disponíveis em tempo de invocação. Observe que é permitido chamar os métodos que podem falhar dentro da implementação do <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A>, contanto que seu código possa lidar com tais falhas e concluir o contrato para liberar o identificador nativo. Para fins de depuração, <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> tem um valor retornado <xref:System.Boolean> que pode ser definido como `false` se é encontrado um erro catastrófico que impede a liberação do recurso. Isso ativará o MDA [releaseHandleFailed](../debug-trace-profile/releasehandlefailed-mda.md), se habilitado, para ajudar a identificar o problema. Ele não afeta o runtime de nenhuma outra forma; <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> não será chamado novamente para o mesmo recurso e, consequentemente, o identificador será perdido.

<xref:System.Runtime.InteropServices.SafeHandle> não é apropriado em determinados contextos.  Já que o método <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> pode ser executado em um thread do finalizador <xref:System.GC>, quaisquer identificadores que seja necessário liberar em um determinado thread não devem ser encapsulados em um <xref:System.Runtime.InteropServices.SafeHandle>.

RCWs (Runtime Callable Wrappers) podem ser limpos pelo CLR sem código adicional.  Para o código que usa a invocação de plataforma e trata um objeto COM como um `IUnknown*` ou um <xref:System.IntPtr>, o código deve ser reescrito para usar um RCW.  <xref:System.Runtime.InteropServices.SafeHandle> pode não ser adequado para esse cenário devido à possibilidade de um método de liberação não gerenciado retornar a chamada em código gerenciado.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Use <xref:System.Runtime.InteropServices.SafeHandle> para encapsular recursos do sistema operacional. Não use <xref:System.Runtime.InteropServices.HandleRef> ou campos do tipo <xref:System.IntPtr>.

### <a name="ensure-finalizers-do-not-have-to-run-to-prevent-leaking-operating-system-resources"></a>Garantir que os finalizadores não precisem ser executados para evitar vazamento de recursos do sistema operacional

Examine seus finalizadores cuidadosamente para assegurar que, mesmo se eles não forem executados, um recurso crítico do sistema operacional não será perdido.  Ao contrário de um descarregamento de <xref:System.AppDomain> normal quando o aplicativo está em execução em um estado estável ou quando um servidor como o SQL Server é desligado, objetos não são finalizados durante um descarregamento de <xref:System.AppDomain> abrupto.  Verifique se recursos não são perdidos no caso de um descarregamento abrupto, já que a correção de um aplicativo não pode ser garantida, mas a integridade do servidor deve ser mantida evitando-se a perda de recursos.  Use <xref:System.Runtime.InteropServices.SafeHandle> para liberar eventuais recursos do sistema operacional.

### <a name="ensure-that-finally-clauses-do-not-have-to-run-to-prevent-leaking-operating-system-resources"></a>Certifique-se de que cláusulas finally não precisem ser executadas para evitar vazamento de recursos do sistema operacional

Não há garantia de que cláusulas `finally` sejam executadas fora de CERs, exigindo que os desenvolvedores de biblioteca não confiem no código dentro de um bloco `finally` para liberar recursos não gerenciados.  Usar <xref:System.Runtime.InteropServices.SafeHandle> é a solução recomendada.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Use <xref:System.Runtime.InteropServices.SafeHandle> para limpar os recursos do sistema operacional, em vez de `Finalize`. Não use <xref:System.IntPtr>; use <xref:System.Runtime.InteropServices.SafeHandle> para encapsular recursos. Se a cláusula finally precisa ser executada, coloque-a em uma CER.

### <a name="all-locks-should-go-through-existing-managed-locking-code"></a>Todos os bloqueios devem passar pelo código de bloqueio gerenciado existente

O CLR deve saber quando o código está em um bloqueio para que ele saiba subdividir o <xref:System.AppDomain> em vez de apenas anular o thread.  Anular o thread pode ser perigoso, já que os dados operados pelo thread podem ser deixados em um estado inconsistente. Portanto, todo o <xref:System.AppDomain> deve ser reciclado.  As consequências de falhar em identificar um bloqueio podem ser deadlocks ou resultados incorretos. Use os métodos <xref:System.Threading.Thread.BeginCriticalRegion%2A> e <xref:System.Threading.Thread.EndCriticalRegion%2A> para identificar regiões de bloqueio.  Eles são métodos estáticos na classe <xref:System.Threading.Thread> que se aplicam somente ao thread atual, ajudando a impedir que um thread edite a contagem de bloqueio de outro thread.

<xref:System.Threading.Monitor.Enter%2A> e <xref:System.Threading.Monitor.Exit%2A> têm essa notificação de CLR como interna, portanto, o uso deles é recomendado, bem como o uso da [instrução lock](../../csharp/language-reference/keywords/lock-statement.md), a qual usa esses métodos.

Outros mecanismos de bloqueio como bloqueios de rotação e <xref:System.Threading.AutoResetEvent> devem chamar esses métodos para notificar o CLR que uma seção crítica está sendo inserida.  Esses métodos não usam nenhum bloqueio; eles informam o CLR que o código está em execução em uma seção crítica e anular o thread poderia deixar o estado compartilhado inconsistente.  Se você definiu seu próprio tipo de bloqueio, por exemplo, uma classe <xref:System.Threading.ReaderWriterLock> personalizada, use esses métodos de contagem de bloqueio.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Marcar e identificar todos os bloqueios usando <xref:System.Threading.Thread.BeginCriticalRegion%2A> e <xref:System.Threading.Thread.EndCriticalRegion%2A>. Não use <xref:System.Threading.Interlocked.CompareExchange%2A>, <xref:System.Threading.Interlocked.Increment%2A> e <xref:System.Threading.Interlocked.Decrement%2A> em um loop.  Não faça uma plataforma de invocação das variantes Win32 desses métodos.  Não use <xref:System.Threading.Thread.Sleep%2A> em loop.  Não use campos voláteis.

### <a name="cleanup-code-must-be-in-a-finally-or-a-catch-block-not-following-a-catch"></a>O código de limpeza deve estar em um bloco finally ou catch, não seguindo um catch

Código de limpeza nunca deve seguir um bloco `catch`; ele deve estar em um `finally` ou no bloco `catch` em si. Isso deve ser uma prática recomendada normal. Um bloco `finally` é geralmente preferível porque ele executa o mesmo código de erro quando uma exceção é gerada e quando o fim do bloco `try` é normalmente encontrado.  No caso de uma exceção inesperada ser gerada, por exemplo uma <xref:System.Threading.ThreadAbortException>, o código de limpeza não será executado.  Quaisquer recursos não gerenciados que você limparia em um `finally` devem idealmente estar encapsulados em um <xref:System.Runtime.InteropServices.SafeHandle> para evitar perdas.  Observe que a palavra-chave `using` do C# pode ser usada com eficiência para descartar objetos, incluindo identificadores.

Embora a reciclagem de <xref:System.AppDomain> possa limpar recursos no thread do finalizador, ainda é importante colocar o código de limpeza no local correto. Observe que, se um thread recebe uma exceção assíncrona sem manter um bloqueio, o CLR tenta encerrar o thread sem a necessidade de reciclar o <xref:System.AppDomain>.  Assegurar que recursos sejam limpos cedo em vez de tarde ajuda por meio da disponibilização de mais recursos e do melhor gerenciamento do tempo de vida. Se você não fechar explicitamente um identificador para um arquivo em algum caminho de código de erro, aguarde que o finalizador <xref:System.Runtime.InteropServices.SafeHandle> o limpe; na próxima vez que seu código executar, ele pode poderá falhar ao tentar acessar exatamente o mesmo arquivo, se o finalizador ainda não tiver executado.  Por esse motivo, garantir que o código de limpeza existe e está funcionando corretamente ajudará a recuperar-se de falhas de modo mais claro e rápido, embora isso não seja estritamente necessário.

#### <a name="code-analysis-rule"></a>Regra de análise de código

O código de limpeza após `catch` deve estar em um bloco `finally`. Faça chamadas para descartar em um bloco finally. Blocos `catch` devem terminar com um lançamento ou relançamento. Embora haja exceções, tais como o código detectar se é possível estabelecer uma conexão de rede em que você pode obter uma de um grande número de exceções, qualquer código que requer a captura de um número de exceções em circunstâncias normais deve fornecer uma indicação de que o código deve ser testado para ver se terá êxito.

### <a name="process-wide-mutable-shared-state-between-application-domains-should-be-eliminated-or-use-a-constrained-execution-region"></a>O estado compartilhado mutável de todo o processo entre domínios de aplicativo deve ser eliminado ou usar uma região de execução restrita

Conforme descrito na introdução, pode ser muito difícil escrever código gerenciado que monitore o estado compartilhado em todo o processo entre domínios de aplicativo de maneira confiável.  Um estado compartilhado por todo o processo é qualquer tipo de estrutura de dados compartilhada entre domínios do aplicativo, podendo ser em código Win32, dentro do CLR ou então em código gerenciado usando comunicação remota.  Qualquer estado compartilhado mutável é muito difícil de escrever corretamente em código gerenciado e qualquer estado compartilhado estático pode ser feito somente com muito cuidado.  Se você tiver um estado compartilhado por todo o computador, encontre alguma forma de eliminá-lo ou proteger o estado compartilhado usando uma CER (região de execução restrita).  Observe que qualquer biblioteca com estado compartilhado que não é identificada e corrigida pode causar falha em um host como o SQL Server, que requer um descarregamento de <xref:System.AppDomain> limpo.

Se o código usa um objeto COM, evite compartilhar esse objeto COM entre domínios do aplicativo.

### <a name="locks-do-not-work-process-wide-or-between-application-domains"></a>Os bloqueios não funcionam em todo o processo ou entre domínios de aplicativo.

No passado, <xref:System.Threading.Monitor.Enter%2A> e a [instrução lock](../../csharp/language-reference/keywords/lock-statement.md) foram usados para criar bloqueios de processo global.  Por exemplo, isso ocorre ao bloquear classes ágeis de <xref:System.AppDomain>, tais como instâncias de <xref:System.Type> de assemblies não compartilhados, objetos de <xref:System.Threading.Thread>, cadeias de caracteres internas e algumas cadeias de caracteres compartilhadas entre domínios do aplicativo usando comunicação remota.  Esses bloqueios não são mais para todo o processo.  Para identificar a presença de um bloqueio de entre domínios do aplicativo em todo o processo, determine se o código dentro do bloqueio usa qualquer recurso externo persistente como um arquivo em disco ou, possivelmente, um banco de dados.

Observe que usar um bloqueio dentro de um <xref:System.AppDomain> pode causar problemas se o código protegido usa um recurso externo, porque esse código pode ser executado simultaneamente em vários domínios de aplicativo.  Isso pode ser um problema ao gravar para um arquivo de log ou associar a um soquete para todo o processo.  Essas alterações significam que, com exceção de usar uma instância nomeada <xref:System.Threading.Mutex> ou <xref:System.Threading.Semaphore>, não há modo fácil de se obter um bloqueio global no processo usando código gerenciado.  Crie código que não executa simultaneamente em dois domínios do aplicativo ou use as classes <xref:System.Threading.Mutex> ou <xref:System.Threading.Semaphore>.  Se o código existente não pode ser alterado, não use um mutex nomeado do Win32 para alcançar essa sincronização, porque a execução em modo fibra significa que você não pode assegurar que um mutex será adquirido e liberado pelo mesmo thread do sistema operacional.  Você deve usar a classe <xref:System.Threading.Mutex> gerenciada ou um <xref:System.Threading.ManualResetEvent> ou <xref:System.Threading.AutoResetEvent> nomeado ou ainda um <xref:System.Threading.Semaphore> para sincronizar o bloqueio de código de maneira que o CLR reconheça, em vez de sincronizar o bloqueio usando código não gerenciado.

#### <a name="avoid-locktypeofmytype"></a>Evite lock(typeof(MyType))

Objetos <xref:System.Type> públicos e privados em assemblies compartilhados com apenas uma cópia do código compartilhado entre todos os domínios do aplicativo também apresentam problemas.  Para assemblies compartilhados, há apenas uma instância de um <xref:System.Type> por processo, o que significa que vários domínios do aplicativo compartilham exatamente a mesma instância de <xref:System.Type>.  A execução de um bloqueio em uma instância de <xref:System.Type> usa um bloqueio que afeta todo o processo, não apenas o <xref:System.AppDomain>.  Se um <xref:System.AppDomain> usar um bloqueio em um objeto <xref:System.Type> e depois esse thread for anulado repentinamente, ele não liberará o bloqueio.  Esse bloqueio poderá depois pode causar deadlock em outros domínios do aplicativo.

Uma boa maneira de aplicar bloqueios em métodos estáticos envolve a adição de um objeto de sincronização interno estático ao código.  Isso pode ser inicializado no construtor da classe se houver um, mas caso contrário, ele pode ser inicializado assim:

```csharp
private static Object s_InternalSyncObject;
private static Object InternalSyncObject
{
    get
    {
        if (s_InternalSyncObject == null)
        {
            Object o = new Object();
            Interlocked.CompareExchange(
                ref s_InternalSyncObject, o, null);
        }
        return s_InternalSyncObject;
    }
}
```

Então, ao usar um bloqueio, use a propriedade `InternalSyncObject` para obter um objeto no qual o bloqueio será usado.  Você não precisa usar a propriedade se você inicializou o objeto de sincronização interna no seu construtor de classe.  O código de inicialização de bloqueio de verificação dupla deve ser semelhante a este exemplo:

```csharp
public static MyClass SingletonProperty
{
    get
    {
        if (s_SingletonProperty == null)
        {
            lock(InternalSyncObject)
            {
                // Do not use lock(typeof(MyClass))
                if (s_SingletonProperty == null)
                {
                    MyClass tmp = new MyClass(…);
                    // Do all initialization before publishing
                    s_SingletonProperty = tmp;
                }
            }
        }
        return s_SingletonProperty;
    }
}
```

#### <a name="a-note-about-lockthis"></a>Uma observação sobre o bloqueio (isso)

É geralmente aceitável usar um bloqueio em um objeto individual que é acessível publicamente.  No entanto, se o objeto é um objeto singleton que pode causar deadlock em um subsistema inteiro, considere usar o padrão de design acima também.  Por exemplo, um bloqueio em um objeto <xref:System.Security.SecurityManager> pode causar um deadlock dentro de <xref:System.AppDomain>, tornando todo o <xref:System.AppDomain> inutilizável. É recomendável não usar um bloqueio em um objeto publicamente acessível desse tipo.  No entanto, um bloqueio em uma matriz ou coleção individual geralmente não deve representar um problema.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Não use bloqueios em tipos que podem ser usados em domínios do aplicativo ou não têm um forte senso de identidade. Não chame <xref:System.Threading.Monitor.Enter%2A> em um <xref:System.Type>, <xref:System.Reflection.MethodInfo>, <xref:System.Reflection.PropertyInfo>, <xref:System.String>, <xref:System.ValueType>, <xref:System.Threading.Thread> ou qualquer objeto que derive de <xref:System.MarshalByRefObject>.

### <a name="remove-gckeepalive-calls"></a>Remova o GC. Chamadas KeepAlive

Uma quantidade significativa de código existente não usa <xref:System.GC.KeepAlive%2A> quando deveria ou então usa quando ele não é adequado.  Depois de converter em <xref:System.Runtime.InteropServices.SafeHandle>, as classes não precisam chamar <xref:System.GC.KeepAlive%2A>, supondo que elas não têm um finalizador, mas dependem de <xref:System.Runtime.InteropServices.SafeHandle> para finalizar os identificadores do sistema operacional.  Embora o custo de desempenho de retenção de uma chamada para <xref:System.GC.KeepAlive%2A> possa ser insignificante, a percepção de que uma chamada para <xref:System.GC.KeepAlive%2A> é necessária ou suficiente para resolver um problema de tempo de vida que talvez não exista mais torna mais difícil manter o código.  No entanto, ao usar os RCWs (Runtime Callable Wrappers) do CLR de interoperabilidade COM, <xref:System.GC.KeepAlive%2A> ainda é exigido pelo código.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Remova <xref:System.GC.KeepAlive%2A>.

### <a name="use-the-hostprotection-attribute"></a>Usar o atributo HostProtection

O <xref:System.Security.Permissions.HostProtectionAttribute> (HPA) fornece o uso de ações de segurança declarativa para determinar os requisitos de proteção de host, permitindo que o host impeça até mesmo código totalmente confiável de chamar determinados métodos que não são apropriados para o host especificado, tais como <xref:System.Environment.Exit%2A> ou <xref:System.Windows.Forms.MessageBox.Show%2A> para o SQL Server.

O HPA afeta somente em aplicativos não gerenciados que hospedam o Common Language Runtime e implementam a proteção de host, como o SQL Server. Quando aplicada, os resultados de ação de segurança na criação de uma demanda de link baseada nos recursos de host que a classe ou método expõe. Se o código for executado em um aplicativo cliente ou em um servidor que não seja protegido pelo host, o atributo "evapora"; ele não é detectado e, portanto, não é aplicado.

> [!IMPORTANT]
> O objetivo desse atributo é impor diretrizes de modelo de programação específicas do host, mas não o comportamento de segurança.  Embora uma demanda de link seja usada para verificar a conformidade com requisitos de modelo de programação, o <xref:System.Security.Permissions.HostProtectionAttribute> não é uma permissão de segurança.

Se o host não tem requisitos de modelo de programação, as demandas de link não ocorrem.

Esse atributo identifica o seguinte:

- Métodos ou classes que não se ajustam ao modelo de programação do host, mas são benignas.

- Métodos ou classes que não se ajustam ao modelo de programação de host e podem levar à desestabilização do código do usuário gerenciado por servidor.

- Métodos ou classes que não se ajustam ao modelo de programação de host e podem levar à desestabilização do processo do servidor em si.

> [!NOTE]
> Se você está criando uma biblioteca de classes que pode vir a ser chamada por aplicativos que podem vir a ser executados em um ambiente de host protegido, você deve aplicar esse atributo aos membros que expõem categorias de recursos <xref:System.Security.Permissions.HostProtectionResource>. Os membros da biblioteca de classes do .NET Framework com esse atributo fazem apenas com que o chamador imediato seja verificado.  O membro da biblioteca deve também causar uma verificação de seu chamador imediato da mesma maneira.

Encontre mais informações sobre HPA em <xref:System.Security.Permissions.HostProtectionAttribute>.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Para o SQL Server, todos os métodos usados para apresentar a sincronização ou threading devem ser identificados com o HPA. Isso inclui métodos que compartilham o estado, que são sincronizados ou que gerenciam processos externos. Os valores de <xref:System.Security.Permissions.HostProtectionResource> que afetam o SQL Server são <xref:System.Security.Permissions.HostProtectionResource.SharedState>, <xref:System.Security.Permissions.HostProtectionResource.Synchronization> e <xref:System.Security.Permissions.HostProtectionResource.ExternalProcessMgmt>. No entanto, qualquer método que expõe qualquer <xref:System.Security.Permissions.HostProtectionResource> deve ser identificado por um HPA, não apenas aqueles usando recursos que afetam o SQL.

### <a name="do-not-block-indefinitely-in-unmanaged-code"></a>Não bloquear indefinidamente em código não gerenciado

Bloquear em código não gerenciado em vez de em código gerenciado pode causar um ataque de negação de serviço, pois o CLR não é capaz de anular o thread.  Um thread bloqueado impede que o CLR descarregue o <xref:System.AppDomain>, pelo menos até que algumas operações extremamente não seguras sejam feitas.  O bloqueio usando um primitivo de sincronização do Windows é um exemplo claro de algo que não podemos permitir.  O bloqueio em uma chamada para `ReadFile` em um soquete deve ser evitado, se possível — o ideal é que a API do Windows forneça um mecanismo para uma operação como essa para atingir o tempo limite.

Qualquer método que chame recursos nativos deve idealmente usar uma chamada de Win32 com um tempo limite razoável e finito.  Se o usuário tem permissão para especificar o tempo limite, o usuário não deve ter permissão para especificar um tempo limite infinito sem alguma permissão de segurança específica.  Como diretriz, se um método será bloqueado por mais de aprox. 10 segundos, você precisará estar usando uma versão que dê suporte a tempos limite ou precisará de mais suporte a CLR.

Aqui estão alguns exemplos de APIs problemáticas.  Pipes (anônimos e nomeados) podem ser criados com um tempo limite; no entanto, o código deve assegurar que ele nunca chame `CreateNamedPipe` nem `WaitNamedPipe` com NMPWAIT_WAIT_FOREVER.  Além disso, pode haver bloqueio inesperado mesmo se um tempo limite é especificado.  Chamar `WriteFile` em um pipe anônimo resultará em bloqueio até que todos os bytes sejam gravados, o que significa que, se o buffer tem dados não lidos, a chamada de `WriteFile` é bloqueada até o leitor ter liberado espaço no buffer do pipe.  Os soquetes devem sempre usar alguma API que respeita um mecanismo de tempo limite.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Bloquear sem um tempo limite em código não gerenciado é um ataque de negação de serviço. Não execute chamadas de invocação de plataforma para `WaitForSingleObject`, `WaitForSingleObjectEx`, `WaitForMultipleObjects`, `MsgWaitForMultipleObjects` e `MsgWaitForMultipleObjectsEx`.  Não use NMPWAIT_WAIT_FOREVER.

### <a name="identify-any-sta-dependent-features"></a>Identificar todos os recursos dependentes do STA

Identifique qualquer código que use STAs (apartments de thread único) COM.  STAs são desabilitados no processo do SQL Server.  Recursos que dependem de `CoInitialize`, assim como contadores de desempenho ou a área de transferência, devem ser desabilitados no SQL Server.

### <a name="ensure-finalizers-are-free-of-synchronization-problems"></a>Garantir que os finalizadores estejam livres de problemas de sincronização

Vários threads finalizadores podem existir em futuras versões do .NET Framework, o que significa que os finalizadores para instâncias diferentes do mesmo tipo são executados simultaneamente.  Eles não precisam ser completamente thread-safe; o coletor de lixo garante que apenas um thread executará o finalizador para uma determinada instância do objeto.  No entanto, os finalizadores devem ser codificados para evitar deadlocks e condições de corrida quando executados simultaneamente em várias instâncias de objeto diferentes.  Ao usar qualquer estado externo, por exemplo, ao gravar em um arquivo de log em um finalizador, problemas de threading devem ser solucionados.  Não dependa da finalização para fornecer acesso thread-safe. Não use o armazenamento local de thread, gerenciado ou nativo, para armazenar o estado no thread do finalizador.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Os finalizadores devem estar livres de problemas de sincronização. Não use um estado mutável estático em um finalizador.

### <a name="avoid-unmanaged-memory-if-possible"></a>Evite memória não gerenciada, se possível

Memória não gerenciada pode ser perdida, assim como um identificador de sistema operacional. Se possível, tente usar memória na pilha usando [stackalloc](../../csharp/language-reference/operators/stackalloc.md) ou um objeto gerenciado fixo, como a [instrução fixed](../../csharp/language-reference/keywords/fixed-statement.md) ou um <xref:System.Runtime.InteropServices.GCHandle> usando um byte[]. O <xref:System.GC> limpa esses elementos eventualmente. No entanto, se você precisar alocar memória não gerenciada, considere o uso de uma classe que deriva de <xref:System.Runtime.InteropServices.SafeHandle> para encapsular a alocação de memória.

Observe que há pelo menos um caso em que <xref:System.Runtime.InteropServices.SafeHandle> não é adequado. Para chamadas de método COM que alocam ou liberam memória, é comum que uma DLL aloque memória por meio de `CoTaskMemAlloc` e, em seguida, outra DLL libere essa memória com `CoTaskMemFree`.  Usar <xref:System.Runtime.InteropServices.SafeHandle> nesses locais seria inadequado, já que ele tentará associar o tempo de vida da memória não gerenciada ao tempo de vida do <xref:System.Runtime.InteropServices.SafeHandle> em vez de permitir que outra DLL controle o tempo de vida da memória.

### <a name="review-all-uses-of-catchexception"></a>Examinar todos os usos de catch (exceção)

Blocos catch que capturam todas as exceções, em vez de uma exceção específica, agora capturarão exceções assíncronas também. Examine cada bloco catch(Exception), procurando por nenhuma liberação de recursos importantes ou código de recuo que possa ser ignorado, bem como comportamento potencialmente incorreto dentro do próprio bloco catch para tratar uma <xref:System.Threading.ThreadAbortException>, <xref:System.StackOverflowException> ou <xref:System.OutOfMemoryException>.  Observe que é possível que esse código esteja registrando em log ou fazendo algumas suposições de que ele pode apenas ver determinadas exceções ou que sempre que uma exceção ocorre, ele falhou exatamente por um motivo específico.  Essas pressuposições talvez precisem ser atualizadas para incluir <xref:System.Threading.ThreadAbortException>.

Considere a possibilidade de alterar todos os locais que capturam todas as exceções para capturar um tipo específico de exceção que você espera ser gerada, tal como uma <xref:System.FormatException> de métodos de formatação de cadeia de caracteres.  Isso impede que o bloco catch encontre exceções inesperadas e ajuda a garantir que o código não oculte bugs capturando exceções inesperadas.  Como regra geral, nunca manipule uma exceção no código de biblioteca (código que requer que você capture uma exceção pode indicar uma falha de design no código que você está chamando).  Em alguns casos, talvez você queira capturar uma exceção e gerar um tipo de exceção diferente para fornecer mais dados.  Usar exceções aninhadas nesse caso, armazenando a causa real da falha na propriedade <xref:System.Exception.InnerException%2A> da nova exceção.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Examine todos os blocos catch no código gerenciado que captura todos os objetos ou captura todas as exceções.  Em C#, isso significa sinalizar `catch` {} e `catch(Exception)` {} .  Considere tornar o tipo de exceção muito específico ou examine o código para garantir que ele não agirá de forma incorreta se detectar um tipo de exceção inesperado.

### <a name="do-not-assume-a-managed-thread-is-a-win32-thread--it-is-a-fiber"></a>Não presuma que um thread gerenciado seja um Thread Win32 – é uma fibra

O uso do armazenamento local de thread gerenciado funciona, mas você não pode usar o armazenamento local de thread não gerenciado ou supor que o código será executado novamente no thread do sistema operacional atual. Não altere as configurações, como o local de thread. Não chame `InitializeCriticalSection` ou `CreateMutex` por meio de invocação de plataforma porque eles requerem que o thread de sistema operacional que entra em um bloqueio também saia do bloqueio. Como isso não será o caso ao usar fibras, mutexes e seções críticas do Win32 não podem ser usados no SQL diretamente.  Observe que a classe <xref:System.Threading.Mutex> gerenciada não lida com essas preocupações de afinidade de thread.

Você pode usar com segurança a maior parte do estado em um objeto <xref:System.Threading.Thread> gerenciado, incluindo o armazenamento local de thread gerenciado e a cultura de interface do usuário atual do thread. Você também pode usar o <xref:System.ThreadStaticAttribute>, que torna o valor de uma variável estática existente acessível somente pelo thread gerenciado atual (essa é outra maneira de fazer o armazenamento local de fibra no CLR). Para a programação de motivos de modelo, você não pode alterar a cultura atual de um thread durante a execução no SQL.

#### <a name="code-analysis-rule"></a>Regra de análise de código

O SQL Server é executado no modo de fibra; não use o armazenamento local de thread. Evite chamadas de invocação de plataforma para `TlsAlloc`, `TlsFree`, `TlsGetValue` e `TlsSetValue.`

### <a name="let-sql-server-handle-impersonation"></a>Permitir que SQL Server manipule a representação

Já que a representação opera em nível de thread e o SQL pode executar em modo de fibra, o código gerenciado não deve representar usuários e não deve chamar `RevertToSelf`.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Permita a representação de identificador do SQL Server. Não use `RevertToSelf`, `ImpersonateAnonymousToken`, `DdeImpersonateClient`, `ImpersonateDdeClientWindow`, `ImpersonateLoggedOnUser`, `ImpersonateNamedPipeClient`, `ImpersonateSelf`, `RpcImpersonateClient`, `RpcRevertToSelf`, `RpcRevertToSelfEx` nem `SetThreadToken`.

### <a name="do-not-call-threadsuspend"></a>Não chamar Thread:: Suspend

A capacidade de suspender um thread pode parecer uma operação simples, mas pode causar deadlocks.  Se um thread mantendo que um bloqueio é suspenso por um segundo thread e, em seguida, o segundo thread tenta usar o mesmo bloqueio, ocorre um deadlock.  <xref:System.Threading.Thread.Suspend%2A> pode atualmente interferir com a segurança, o carregamento de classe, a comunicação remota e a reflexão.

#### <a name="code-analysis-rule"></a>Regra de análise de código

Não chame <xref:System.Threading.Thread.Suspend%2A>. Considere o uso de um primitivo de sincronização real em vez disso, assim como um <xref:System.Threading.Semaphore> ou <xref:System.Threading.ManualResetEvent>.

### <a name="protect-critical-operations-with-constrained-execution-regions-and-reliability-contracts"></a>Proteger operações críticas com regiões de execução restritas e contratos de confiabilidade

Ao executar uma operação complexa que atualiza um status compartilhado ou que precisa de forma determinística ser totalmente bem-sucedido ou falhar totalmente, verifique se ele é protegido por uma CER (região de execução restrita). Isso garante que o código seja executado em todos os casos, até mesmo uma operação de anulação de thread abrupta ou um descarregamento de <xref:System.AppDomain> abrupto.

Uma CER é um bloco `try/finally` específico imediatamente precedido por uma chamada para <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A>.

Fazer isso instrui o compilador Just-In-Time para preparar a todo o código no bloco finally antes de executar o bloco `try`. Isso assegura que o código no bloco finally será criado e que será executado em todos os casos. Não é incomum que uma CER tenha um bloco `try` vazio. Usar uma CER protege contra anulações de thread assíncronas e exceções de falta de memória. Consulte <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A> para um formulário de uma CER que manipula excedentes de pilha de código excessivamente profundo.

## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.ConstrainedExecution>
- [Programação em SQL Server e atributos de proteção de host](sql-server-programming-and-host-protection-attributes.md)
