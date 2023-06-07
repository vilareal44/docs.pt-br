---
title: Práticas recomendadas de threading gerenciado
description: Conheça as práticas recomendadas de Threading gerenciadas no .NET. Trabalhe com situações difíceis, como a coordenação de vários threads ou a manipulação de threads de bloqueio.
ms.date: 10/15/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- threading [.NET Framework], design guidelines
- threading [.NET Framework], best practices
- managed threading
ms.assetid: e51988e7-7f4b-4646-a06d-1416cee8d557
ms.openlocfilehash: fa0af1461ba568583127316934b9d55577dd4c5a
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662817"
---
# <a name="managed-threading-best-practices"></a>Práticas recomendadas de threading gerenciado
O multithreading requer programação cuidadosa. Para a maioria das tarefas, você pode reduzir a complexidade ao enfileirar solicitações para a execução por parte de threads de pool. Este tópico aborda situações mais difíceis, como coordenar o trabalho de vários threads ou manipular threads que bloqueiam.  
  
> [!NOTE]
> A partir do .NET Framework 4, a biblioteca de paralelismo de tarefas e o PLINQ fornecem APIs que reduzem parte da complexidade e os riscos da programação de multithreading. Para saber mais, confira [Programação paralela em .NET](../parallel-programming/index.md).  
  
## <a name="deadlocks-and-race-conditions"></a>Deadlocks e condições de corrida  
 O multithreading resolve problemas com taxa de transferência e capacidade de resposta, mas, ao fazer isso, ele introduz novos problemas: deadlocks e condições de corrida.  
  
### <a name="deadlocks"></a>Deadlocks  
 Um deadlock ocorre quando um dos dois threads tenta bloquear um recurso que o outro já bloqueou. Nenhum dos threads pode fazer progresso adicional.  
  
 Muitos métodos das classes de threading gerenciadas fornecem tempos limite para ajudá-lo a detectar deadlocks. Por exemplo, o código a seguir tenta adquirir um bloqueio em um objeto denominado `lockObject`. Se o bloqueio não for obtido em 300 milissegundos, <xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType> retornará `false`.  
  
```vb  
If Monitor.TryEnter(lockObject, 300) Then  
    Try  
        ' Place code protected by the Monitor here.  
    Finally  
        Monitor.Exit(lockObject)  
    End Try  
Else  
    ' Code to execute if the attempt times out.  
End If  
```  
  
```csharp  
if (Monitor.TryEnter(lockObject, 300)) {  
    try {  
        // Place code protected by the Monitor here.  
    }  
    finally {  
        Monitor.Exit(lockObject);  
    }  
}  
else {  
    // Code to execute if the attempt times out.  
}  
```  
  
### <a name="race-conditions"></a>Condições de corrida  
 Uma condição de corrida é um bug que ocorre quando o resultado de um programa depende de qual dos dois ou mais threads alcança um determinado bloco de código primeiro. Executar o programa muitas vezes produz resultados diferentes e o resultado de qualquer execução não pode ser previsto.  
  
 Um exemplo simples de uma condição de corrida é incrementar um campo. Suponha que uma classe tem um campo **static** particular (**Shared** no Visual Basic) que é incrementado toda vez que uma instância da classe é criada, usando um código como `objCt++;` (C#) ou `objCt += 1` (Visual Basic). Esta operação requer o carregamento do valor de `objCt` em um registro, incrementando o valor e o armazenando em `objCt`.  
  
 Em um aplicativo com multithreading, um thread que carregou e incrementou o valor pode ser impedido por outro thread que execute todas as três etapas; quando o primeiro thread retomar a execução e armazenar seu valor, ele substituirá `objCt` sem levar em conta o fato de que o valor foi alterado durante o processo.  
  
 Essa condição de corrida específica pode ser evitada facilmente usando métodos da classe <xref:System.Threading.Interlocked>, como <xref:System.Threading.Interlocked.Increment%2A?displayProperty=nameWithType>. Para ler sobre outras técnicas para sincronizar dados entre vários threads, confira [Sincronizando dados para multithreading](synchronizing-data-for-multithreading.md).  
  
 Condições de corrida também podem ocorrer quando você sincroniza as atividades de vários threads. Sempre que você escreve uma linha de código, é preciso considerar o que poderia acontecer se um thread fosse impedido antes de executar a linha (ou antes de qualquer uma das instruções de máquina individuais que compõem a linha) e outro thread o substituísse.  
  
## <a name="static-members-and-static-constructors"></a>Membros estáticos e construtores estáticos  
 Uma classe não é inicializada até que o construtor de classe (construtor `static` em C#, `Shared Sub New` no Visual Basic) tenha terminado de ser executado. Para impedir a execução de código em um tipo não inicializado, o Common Language Runtime bloqueia todas as chamadas de outros threads para membros `static` da classe (membros `Shared` no Visual Basic) até que o construtor da classe tenha concluído sua execução.  
  
 Por exemplo, se um construtor de classe iniciar um novo thread, e o procedimento do thread chamar um membro `static` da classe, o novo thread bloqueia até que o construtor da classe seja concluído.  
  
 Isso se aplica a qualquer tipo que possa ter um construtor `static`.  

## <a name="number-of-processors"></a>Número de processadores

A existência de apenas um ou de vários processadores disponíveis em um sistema pode influenciar a arquitetura de vários threads. Para obter mais informações, veja [Número de processadores](https://docs.microsoft.com/previous-versions/dotnet/netframework-1.1/1c9txz50(v%3dvs.71)#number-of-processors).

Use a <xref:System.Environment.ProcessorCount?displayProperty=nameWithType> propriedade para determinar o número de processadores disponíveis em tempo de execução.
  
## <a name="general-recommendations"></a>Recomendações gerais  
 Ao usar vários threads, considere as seguintes diretrizes:  
  
- Não use <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> para encerrar outros threads. Chamar **Abort** em outro thread equivale a gerar uma exceção nesse thread sem saber qual ponto ele alcançou nesse processamento.  
  
- Não use <xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType> e <xref:System.Threading.Thread.Resume%2A?displayProperty=nameWithType> para sincronizar as atividades de vários threads. Use <xref:System.Threading.Mutex>, <xref:System.Threading.ManualResetEvent>, <xref:System.Threading.AutoResetEvent>, e <xref:System.Threading.Monitor>.  
  
- Não controle a execução de threads de trabalho do seu programa principal (usando eventos, por exemplo). Em vez disso, projete seu programa de forma que os threads de trabalho sejam responsáveis por esperar até que o trabalho esteja disponível, executá-lo e notificar outras partes do seu programa quando terminar. Se seus threads de trabalho não bloquearem, considere o uso de threads de pool. <xref:System.Threading.Monitor.PulseAll%2A?displayProperty=nameWithType> é útil em situações em que os threads de trabalho bloqueiam.  
  
- Não use tipos como objetos de bloqueio. Isto é, evite códigos como `lock(typeof(X))` em C# ou `SyncLock(GetType(X))` no Visual Basic ou o uso de <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> com objetos <xref:System.Type>. Para um determinado tipo, há apenas uma instância de <xref:System.Type?displayProperty=nameWithType> por domínio de aplicativo. Se o tipo no qual você usar um bloqueio for público, o código que não for o seu próprio poderá assumir bloqueios, levando a deadlocks. Para problemas adicionais, consulte [Práticas recomendadas de confiabilidade](../../framework/performance/reliability-best-practices.md).  
  
- Tenha cuidado ao bloquear em instâncias, por exemplo `lock(this)` em C# ou `SyncLock(Me)` no Visual Basic. Se outro código no seu aplicativo, externo ao tipo, assumir um bloqueio no objeto, podem ocorrer deadlocks.  
  
- Certifique-se de que um thread que tenha entrado em um monitor sempre o deixe, mesmo que uma exceção ocorra enquanto o thread estiver no monitor. A instrução [lock](../../csharp/language-reference/keywords/lock-statement.md) do C# e a instrução [SyncLock](../../visual-basic/language-reference/statements/synclock-statement.md) do Visual Basic oferece esse comportamento automaticamente, empregando um bloco **finally** para garantir que <xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType> seja chamado. Se você não puder garantir que **Exit** seja chamado, considere alterar o design para usar **Mutex**. Um mutex é liberado automaticamente quando o thread que o possui atualmente for encerrado.  
  
- Usar vários threads para tarefas que exigem recursos diferentes e evite atribuir vários threads para um único recurso. Por exemplo, qualquer tarefa que envolva E/S se beneficia em ter seu próprio thread, porque esse thread bloqueará durante as operações de E/S e, assim, permitirá que outros threads sejam executados. A entrada do usuário é outro recurso que se beneficia com um thread dedicado. Em um computador de um processador, uma tarefa que envolve a computação intensiva coexiste com a entrada do usuário e com tarefas que envolvem E/S, mas várias tarefas competem umas com as outras.  
  
- Considere o uso de métodos da classe <xref:System.Threading.Interlocked> para alterações de estado simples, em vez de usar a instrução `lock` (`SyncLock` no Visual Basic). A instrução `lock` é uma boa ferramenta para fins gerais, mas a classe <xref:System.Threading.Interlocked> oferece um melhor desempenho para atualizações que devem ser atômicas. Internamente, ela executa um único prefixo de bloqueio se não houver nenhuma contenção. Em revisões de código, atente para códigos semelhantes aos mostrados nos exemplos a seguir. No primeiro exemplo, uma variável de estado é incrementada:  
  
    ```vb  
    SyncLock lockObject  
        myField += 1  
    End SyncLock  
    ```  
  
    ```csharp  
    lock(lockObject)
    {  
        myField++;  
    }  
    ```  
  
     Você pode melhorar o desempenho usando o método <xref:System.Threading.Interlocked.Increment%2A> em vez da instrução `lock`, da seguinte maneira:  
  
    ```vb  
    System.Threading.Interlocked.Increment(myField)  
    ```  
  
    ```csharp  
    System.Threading.Interlocked.Increment(myField);  
    ```  
  
    > [!NOTE]
    > No .NET Framework 2.0 e posteriores, use o método <xref:System.Threading.Interlocked.Add%2A> para incrementos atômicos maiores do que 1.  
  
     No segundo exemplo, uma variável de tipo de referência é atualizada somente se ela for uma referência nula (`Nothing` no Visual Basic).  
  
    ```vb  
    If x Is Nothing Then  
        SyncLock lockObject  
            If x Is Nothing Then  
                x = y  
            End If  
        End SyncLock  
    End If  
    ```  
  
    ```csharp  
    if (x == null)  
    {  
        lock (lockObject)  
        {  
            x ??= y;
        }  
    }  
    ```  
  
     É possível melhorar o desempenho usando o método <xref:System.Threading.Interlocked.CompareExchange%2A>, da seguinte maneira:  
  
    ```vb  
    System.Threading.Interlocked.CompareExchange(x, y, Nothing)  
    ```  
  
    ```csharp  
    System.Threading.Interlocked.CompareExchange(ref x, y, null);  
    ```  
  
    > [!NOTE]
    > Começando com o .NET Framework 2.0, a sobrecarga do método <xref:System.Threading.Interlocked.CompareExchange%60%601%28%60%600%40%2C%60%600%2C%60%600%29> fornece uma alternativa fortemente tipada para tipos de referência.
  
## <a name="recommendations-for-class-libraries"></a>Recomendações para bibliotecas de classes  
 Considere as seguintes diretrizes ao criar bibliotecas de classes para multithreading:  
  
- Evite a necessidade de sincronização, se possível. Isso é especialmente válido para o código de uso intensivo. Por exemplo, um algoritmo pode ser ajustado para tolerar uma condição de corrida em vez de eliminá-la. Sincronização desnecessária reduz o desempenho e cria a possibilidade de deadlocks e condições de corrida.  
  
- Torne os dados estáticos (`Shared` no Visual Basic) thread-safe por padrão.  
  
- Não torne dados de instância thread-safe por padrão. Adicionar bloqueios para criar códigos de thread-safe reduz o desempenho, aumenta a contenção de bloqueios e cria a possibilidade de deadlocks. Em modelos de aplicativo comuns, somente um thread por vez executa código do usuário, o que minimiza a necessidade de acesso thread-safe. Por esse motivo, as bibliotecas de classes do .NET Framework não são thread-safe por padrão.  
  
- Evite fornecer métodos estáticos que alteram o estado estático. Em cenários de servidor comuns, o estado estático é compartilhado entre as solicitações, que significa que vários threads podem executar esse código ao mesmo tempo. Isso abre a possibilidade de bugs de threading. Considere usar um padrão de design que encapsule dados em instâncias que não sejam compartilhadas entre solicitações. Além disso, se os dados estáticos forem sincronizados, chamadas entre os métodos estáticos que alteram o estado podem resultar em deadlocks ou em sincronização redundante, afetando negativamente o desempenho.  
  
## <a name="see-also"></a>Confira também

- [Threading](index.md)
- [Threads e threading](threads-and-threading.md)
