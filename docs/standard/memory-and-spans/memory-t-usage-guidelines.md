---
title: Diretrizes de uso de Memory<T> e Span<T>
description: Este artigo descreve a memória <T> e o span <T> , que são buffers de dados estruturados no .NET Core que podem ser usados em pipelines.
ms.date: 10/01/2018
helpviewer_keywords:
- Memory&lt;T&gt; and Span&lt;T&gt; best practices
- using Memory&lt;T&gt; and Span&lt;T&gt;
ms.openlocfilehash: d9a50fa18e027b6df7415438e1a5584003f7a094
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245590"
---
# <a name="memoryt-and-spant-usage-guidelines"></a>Diretrizes de uso de Memory\<T> e Span\<T>

O .NET Core inclui vários tipos que representam uma região contígua arbitrária de memória. O .NET Core 2.0 introduziu <xref:System.Span%601> e <xref:System.ReadOnlySpan%601>, que são buffers de memória leves que podem ter suporte de memória gerenciada ou não gerenciada. Como esses tipos só podem ser armazenados na pilha, eles são inadequados para vários cenários, incluindo chamadas de método assíncronas. O .NET Core 2.1 adiciona vários outros tipos, inclusive <xref:System.Memory%601>, <xref:System.ReadOnlyMemory%601>, <xref:System.Buffers.IMemoryOwner%601> e <xref:System.Buffers.MemoryPool%601>. Assim como <xref:System.Span%601>, <xref:System.Memory%601> e os tipos relacionados podem ter suporte de memória gerenciada e não gerenciada. Diferentemente de <xref:System.Span%601>, <xref:System.Memory%601> pode ser armazenado no heap gerenciado.

Tanto <xref:System.Span%601> como <xref:System.Memory%601> representam buffers de dados estruturados que podem ser usados em pipelines. Ou seja, eles são projetados para que alguns ou todos os dados possam ser passados com eficiência para os componentes no pipeline que pode processá-los e, opcionalmente, modificar o buffer. Como <xref:System.Memory%601> e os tipos relacionados podem ser acessados por vários componentes ou threads, é importante que os desenvolvedores sigam algumas diretrizes de uso padrão para criar um código robusto.

## <a name="owners-consumers-and-lifetime-management"></a>Gerenciamento de proprietários, consumidores e vida útil

Como os buffers podem ser passados entre APIs e, às vezes, ser acessados de vários threads, é importante levar em consideração o gerenciamento da vida útil. Os três principais conceitos são:

- **Propriedade**. O proprietário de uma instância de buffer é responsável pelo gerenciamento da vida útil, inclusive pela destruição do buffer quando ele não está mais em uso. Todos os buffers pertencem a um único proprietário. Geralmente, o proprietário é o componente que criou o buffer ou que recebeu o buffer de fábrica. É possível também transferir a propriedade. O **Componente-A** poderá ceder o controle do buffer ao **Componente-B**, passando o **Componente-A** a não poder mais usar o buffer. A partir daí, o **Componente-B** se tornará responsável por destruir o buffer quando ele não estiver mais em uso.

- **Consumo**. O consumidor de uma instância do buffer pode usar essa instância, lendo e possivelmente gravando nela. Os buffers podem ter um consumidor por vez, a menos que algum mecanismo de sincronização externo seja fornecido. O consumidor ativo de um buffer não é necessariamente o proprietário do buffer.

- **Concessão**. A concessão é o período de tempo em que um componente específico pode ser o consumidor do buffer.

O exemplo de pseudocódigo a seguir ilustra esses três conceitos. Ele inclui um método `Main` que cria uma instância para o buffer <xref:System.Memory%601> de tipo <xref:System.Char>, chama o método `WriteInt32ToBuffer` para gravar a representação da cadeia de caracteres de um número inteiro no buffer e, em seguida, chama o método `DisplayBufferToConsole` para exibir o valor do buffer.

```csharp
using System;

class Program
{
    // Write 'value' as a human-readable string to the output buffer.
    void WriteInt32ToBuffer(int value, Buffer buffer);

    // Display the contents of the buffer to the console.
    void DisplayBufferToConsole(Buffer buffer);

    // Application code
    static void Main()
    {
        var buffer = CreateBuffer();
        try
        {
            int value = Int32.Parse(Console.ReadLine());
            WriteInt32ToBuffer(value, buffer);
            DisplayBufferToConsole(buffer);
        }
        finally
        {
            buffer.Destroy();
        }
    }
}
```

O método `Main` cria o buffer (neste caso, uma instância de <xref:System.Span%601>), portanto, ele é o respectivo proprietário. Dessa forma, `Main` é responsável por destruir o buffer quando ele não está mais em uso. Ele faz isso chamando o método <xref:System.Span%601.Clear?displayProperty=nameWithType> do buffer. O método <xref:System.Span%601.Clear> realmente limpa a memória do buffer. A estrutura de <xref:System.Span%601> não tem um método que destrua o buffer.

O buffer tem dois consumidores: `WriteInt32ToBuffer` e `DisplayBufferToConsole`. Há apenas um consumidor por vez (primeiro `WriteInt32ToBuffer`, depois `DisplayBufferToConsole`), e nenhum dos consumidores é proprietário do buffer. Ainda, "consumidor" neste contexto não implica uma exibição somente leitura do buffer. Os consumidores podem modificar o conteúdo do buffer, assim como `WriteInt32ToBuffer` o faz, se for fornecida uma exibição de leitura/gravação do buffer.

O método `WriteInt32ToBuffer` tem uma concessão (pode consumir) o buffer entre o início da chamada do método e a hora em que o método é retornado. Da mesma maneira, `DisplayBufferToConsole` tem uma concessão no buffer enquanto ele está em execução, e a concessão é liberada quando o método é desenrolado. Não há APIs para gerenciamento de concessão. Uma "concessão" é uma questão conceitual.

## <a name="memoryt-and-the-ownerconsumer-model"></a>Memória \<T> e o modelo de proprietário/consumidor

Conforme a seção [Gerenciamento de proprietários, consumidores e vida útil](#owners-consumers-and-lifetime-management) descreve, um buffer tem sempre um proprietário. O .NET Core tem suporte para dois modelos de propriedade:

- Um modelo com suporte para propriedade única. Um buffer tem um único proprietário por toda a vida útil.

- Um modelo com suporte para transferência de propriedade. É possível transferir a propriedade de um buffer do proprietário original (o respectivo criador) para outro componente que se torna responsável pelo gerenciamento da vida útil do buffer. Esse proprietário pode, por sua vez, transferir a propriedade para outro componente, e assim por diante.

Use a interface <xref:System.Buffers.IMemoryOwner%601?displayProperty=nameWithType> para gerenciar explicitamente a propriedade de um buffer. <xref:System.Buffers.IMemoryOwner%601> tem suporte para os dois modelos de propriedade. O componente que possui uma referência de <xref:System.Buffers.IMemoryOwner%601> possui o buffer. O exemplo a seguir usa uma <xref:System.Buffers.IMemoryOwner%601?> instância para refletir a propriedade de um <xref:System.Memory%601> buffer.

[!code-csharp[ownership](~/samples/snippets/standard/buffers/memory-t/owner/owner.cs)]

Também podemos escrever este exemplo com [`using`](../../csharp/language-reference/keywords/using-statement.md) :

[!code-csharp[ownership-using](~/samples/snippets/standard/buffers/memory-t/owner-using/owner-using.cs)]

Neste código:

- O método `Main` contém a referência à instância de <xref:System.Buffers.IMemoryOwner%601>, portanto, o método `Main` é o proprietário do buffer.

- Os métodos `WriteInt32ToBuffer` e `DisplayBufferToConsole` aceitam <xref:System.Memory%601> como uma API pública. Portanto, eles são consumidores do buffer, e o consomem somente um de cada vez.

Embora o método `WriteInt32ToBuffer` seja destinado a gravar um valor no buffer, isso não se aplica ao método `DisplayBufferToConsole`. Para refletir isso, ele poderia aceitar um argumento de tipo <xref:System.ReadOnlyMemory%601>. Para obter mais informações sobre o <xref:System.ReadOnlyMemory%601> , consulte [#2 de regras: Use ReadOnlySpan \<T> ou ReadOnlyMemory \<T> se o buffer deve ser somente leitura](#rule-2).

### <a name="ownerless-memoryt-instances"></a>Instâncias de memória "com proprietário" \<T>

É possível criar uma instância de <xref:System.Memory%601> sem usar <xref:System.Buffers.IMemoryOwner%601>. Nesse caso, a propriedade do buffer é implícita, em vez de explícita, e tem suporte apenas para o modelo de proprietário único. É possível fazer isso da seguinte maneira:

- Chamar um dos construtores de <xref:System.Memory%601> diretamente, passando um `T[]`, assim como no exemplo a seguir.

- Chamar o método de extensão [String.AsMemory](xref:System.MemoryExtensions.AsMemory(System.String)) para criar uma instância de `ReadOnlyMemory<char>`.

[!code-csharp[ownerless-memory](~/samples/snippets/standard/buffers/memory-t/ownerless/ownerless.cs)]

O método que inicialmente cria a instância de <xref:System.Memory%601> é o proprietário implícito do buffer. Não é possível transferir a propriedade para nenhum outro componente porque não há instâncias de <xref:System.Buffers.IMemoryOwner%601> para facilitar a transferência. Como alternativa, imagine que o coletor de lixo do runtime possui o buffer, e que todos os métodos apenas o consomem.

## <a name="usage-guidelines"></a>Diretrizes de uso

Como um bloco de memória pertence, mas se destina a ser passado para vários componentes, alguns dos quais podem operar em um determinado bloco de memória simultaneamente, é importante estabelecer diretrizes para o uso de <xref:System.Memory%601> e <xref:System.Span%601>.  Diretrizes são necessárias porque:

- É possível que um componente retenha uma referência a um bloco de memória depois que o respectivo proprietário o liberar.

- É possível que um componente opere em um buffer, ao mesmo tempo em que outro componente esteja operando nele, corrompendo os dados no buffer durante o processo.

- Embora a natureza alocada na pilha do <xref:System.Span%601> otimize o desempenho e torne <xref:System.Span%601> o tipo preferencial para a operação em um bloco de memória, ele também submete <xref:System.Span%601> para algumas restrições importantes. É importante saber quando usar <xref:System.Span%601> e quando usar <xref:System.Memory%601>.

A seguir estão nossas recomendações para usar com êxito o <xref:System.Memory%601> e os tipos relacionados. Diretrizes que se aplicam a e <xref:System.Memory%601> <xref:System.Span%601> também se aplicam a <xref:System.ReadOnlyMemory%601> e <xref:System.ReadOnlySpan%601> , a menos que Notemos explicitamente o contrário.

**#1 de regra: para uma API síncrona, use span \<T> em vez de memória \<T> como um parâmetro, se possível.**

<xref:System.Span%601> é mais versátil do que <xref:System.Memory%601> e pode representar uma variedade maior de buffers de memória contíguos. <xref:System.Span%601> também oferece um melhor desempenho que <xref:System.Memory%601>. Por fim, você pode usar a <xref:System.Memory%601.Span?displayProperty=nameWithType> propriedade para converter uma <xref:System.Memory%601> instância em um <xref:System.Span%601> , embora a conversão de span \<T> para memória \<T> não seja possível. Portanto, se os chamadores tiverem uma instância de <xref:System.Memory%601>, eles poderão chamar seus métodos com os parâmetros <xref:System.Span%601> de qualquer maneira.

Usando um parâmetro de tipo <xref:System.Span%601> em vez de o tipo <xref:System.Memory%601>, você pode também escrever uma implementação correta do método de consumo. Você receberá automaticamente verificações de tempo de compilação para garantir que não esteja tentando acessar o buffer, além da concessão do método (saiba mais sobre isso a seguir).

Às vezes, você terá que usar um parâmetro <xref:System.Memory%601> em vez de um parâmetro <xref:System.Span%601>, mesmo que esteja totalmente síncrono. Talvez uma API da qual você dependa aceite apenas argumentos <xref:System.Memory%601>. Isso é possível, mas conheça as vantagens e desvantagens envolvidas com o uso de <xref:System.Memory%601> de maneira síncrona.

<a name="rule-2"></a>

**#2 de regra: Use ReadOnlySpan \<T> ou ReadOnlyMemory \<T> se o buffer deve ser somente leitura.**

Nos exemplos anteriores, o método `DisplayBufferToConsole` apenas lê no buffer, sem modificar o respectivo conteúdo. A assinatura do método deve ser alterada para a seguinte.

```csharp
void DisplayBufferToConsole(ReadOnlyMemory<char> buffer);
```

Na verdade, se combinarmos esta regra com a Regra 1, poderemos fazer ainda melhor e reescrever a assinatura do método da seguinte forma:

```csharp
void DisplayBufferToConsole(ReadOnlySpan<char> buffer);
```

O método `DisplayBufferToConsole` agora funciona praticamente com todos os tipos de buffer possíveis: `T[]`, armazenamento alocado com [stackalloc](../../csharp/language-reference/operators/stackalloc.md), e assim por diante. Você pode, inclusive, passar um <xref:System.String> diretamente nele!

**#3 de regra: se o método aceitar memória \<T> e retornar `void` , você não deverá usar a \<T> instância de memória depois que o método retornar.**

Isso está relacionado ao conceito de "concessão" mencionado anteriormente. A concessão de um retorno de void do método na instância de <xref:System.Memory%601> começa quando o método é inserido e termina quando o método é encerrado. Considere o exemplo a seguir, que chama `Log` em um loop com base na entrada do console.

[!code-csharp[void-returning](~/samples/snippets/standard/buffers/memory-t/void-returning/void-returning.cs#1)]

Se `Log` for um método totalmente síncrono, esse código se comportará como esperado porque há apenas um consumidor ativo da instância de memória a qualquer momento.
Mas imagine que `Log` tenha essa implementação.

```csharp
// !!! INCORRECT IMPLEMENTATION !!!
static void Log(ReadOnlyMemory<char> message)
{
    // Run in background so that we don't block the main thread while performing IO.
    Task.Run(() =>
    {
        StreamWriter sw = File.AppendText(@".\input-numbers.dat");
        sw.WriteLine(message);
    });
}
```

Nesta implementação, `Log` viola a respectiva concessão porque ela ainda tenta usar a instância de <xref:System.Memory%601> em segundo plano, após o método original ter retornado. O método `Main` pode alterar o buffer enquanto `Log` tenta ler nele, o que pode resultar em dados corrompidos.

Há várias maneiras de resolver isso:

- O método `Log` pode retornar uma classe <xref:System.Threading.Tasks.Task> em vez de `void`, assim como faz a seguinte implementação do método `Log`.

   [!code-csharp[task-returning](~/samples/snippets/standard/buffers/memory-t/task-returning2/task-returning2.cs#1)]

- É possível implementar `Log` da seguinte maneira:

   [!code-csharp[defensive-copy](~/samples/snippets/standard/buffers/memory-t/task-returning/task-returning.cs#1)]

**#4 de regra: se o método aceitar uma memória \<T> e retornar uma tarefa, você não deverá usar a \<T> instância de memória após a transição da tarefa para um estado de terminal.**

Esta é apenas a variante assíncrona da Regra 3. O método `Log` do exemplo anterior pode ser escrito da seguinte forma para cumprir esta regra:

[!code-csharp[task-returning-async](~/samples/snippets/standard/buffers/memory-t/void-returning-async/void-returning-async.cs#1)]

Nesse caso, "estado terminal" significa que a tarefa passa para um estado concluído, com falha ou cancelado. Em outras palavras, "estado terminal" significa "qualquer coisa que cause atraso de lançamento ou continuação de execução".

Essa orientação se aplica a métodos que retornam <xref:System.Threading.Tasks.Task>, <xref:System.Threading.Tasks.Task%601>, <xref:System.Threading.Tasks.ValueTask%601> ou outros tipos semelhantes.

**#5 de regra: se o Construtor aceitar memória \<T> como um parâmetro, os métodos de instância no objeto construído serão considerados consumidores da instância de memória \<T> .**

Considere o exemplo a seguir:

```csharp
class OddValueExtractor
{
    public OddValueExtractor(ReadOnlyMemory<int> input);
    public bool TryReadNextOddValue(out int value);
}

void PrintAllOddValues(ReadOnlyMemory<int> input)
{
    var extractor = new OddValueExtractor(input);
    while (extractor.TryReadNextOddValue(out int value))
    {
      Console.WriteLine(value);
    }
}
```

Nesse caso, o construtor `OddValueExtractor` aceita um `ReadOnlyMemory<int>` como parâmetro, para que o próprio construtor seja um consumidor da instância de `ReadOnlyMemory<int>`, e todos os métodos da instância no valor retornado também sejam consumidores da instância original de `ReadOnlyMemory<int>`. Isso significa que `TryReadNextOddValue` consome a instância de `ReadOnlyMemory<int>`, mesmo que a instância não seja passada diretamente para o método `TryReadNextOddValue`.

**#6 de regra: se você tiver uma propriedade configurável com tipo de memória \<T> (ou um método de instância equivalente) em seu tipo, os métodos de instância nesse objeto serão considerados consumidores da instância de memória \<T> .**

Na verdade, esta é apenas uma variante da Regra 5. Esta regra existe porque presume-se que os setters de propriedade ou métodos equivalentes devam capturar e persistir as respectivas entradas, de modo que os métodos da instância no mesmo objeto possam usar o estado capturado.

O exemplo a seguir aciona esta regra:

```csharp
class Person
{
    // Settable property.
    public Memory<char> FirstName { get; set; }

    // alternatively, equivalent "setter" method
    public SetFirstName(Memory<char> value);

    // alternatively, a public settable field
    public Memory<char> FirstName;
}
```

**Regra #7: se você tiver uma referência de IMemoryOwner \<T> , você deverá, em algum momento, descartar ou transferir sua propriedade (mas não ambas).**

Como uma instância de <xref:System.Memory%601> pode ter suporte de memória gerenciada e não gerenciada, o proprietário deve chamar <xref:System.Buffers.MemoryPool%601.Dispose%2A?displayProperty=nameWithType> quando concluir o trabalho realizado na instância de <xref:System.Memory%601>. Como alternativa, o proprietário pode transferir a propriedade da instância de <xref:System.Buffers.IMemoryOwner%601> para um componente diferente, até que o componente receptor se torne responsável por chamar <xref:System.Buffers.MemoryPool%601.Dispose%2A?displayProperty=nameWithType> no tempo adequado (saiba mais sobre isso a seguir).

Uma falha ao chamar o método <xref:System.Buffers.MemoryPool%601.Dispose%2A> pode causar perdas de memória não gerenciada ou outra degradação do desempenho.

Esta regra também se aplica ao código que chama métodos de fábrica, como <xref:System.Buffers.MemoryPool%601.Rent%2A?displayProperty=nameWithType>. O chamador se torna proprietário do <xref:System.Buffers.IMemoryOwner%601> retornado e fica responsável pelo descarte da instância após concluída.

**#8 de regra: se você tiver um \<T> parâmetro IMemoryOwner em sua superfície de API, você estará aceitando a propriedade dessa instância.**

Aceitar uma instância desse tipo indica que o componente pretende se apropriar dessa instância. O componente se torna responsável pelo descarte adequado de acordo com a Regra 7.

Os componentes que transferem a propriedade da instância de <xref:System.Buffers.IMemoryOwner%601> para um componente diferente não devem mais usar essa instância após a conclusão da chamada do método.

> [!IMPORTANT]
> Se o construtor aceitar <xref:System.Buffers.IMemoryOwner%601> como parâmetro, o respectivo tipo deverá implementar <xref:System.IDisposable>, e o método <xref:System.IDisposable.Dispose%2A> deverá chamar <xref:System.Buffers.MemoryPool%601.Dispose%2A?displayProperty=nameWithType>.

**Regra #9: se você estiver encapsulando um método p/invoke síncrono, sua API deverá aceitar span \<T> como um parâmetro.**

De acordo com a Regra 1, <xref:System.Span%601> geralmente é o tipo correto a ser usado para APIs síncronas. Você pode fixar instâncias de <xref:System.Span%601> por meio da palavra-chave [`fixed`](../../csharp/language-reference/keywords/fixed-statement.md), como no exemplo a seguir.

```csharp
using System.Runtime.InteropServices;

[DllImport(...)]
private static extern unsafe int ExportedMethod(byte* pbData, int cbData);

public unsafe int ManagedWrapper(Span<byte> data)
{
    fixed (byte* pbData = &MemoryMarshal.GetReference(data))
    {
        int retVal = ExportedMethod(pbData, data.Length);

        /* error checking retVal goes here */

        return retVal;
    }
}
```

No exemplo anterior, `pbData` poderá ser nulo, se o alcance da entrada estiver vazio. Se o método exportado exigir que `pbData` seja não nulo, mesmo que `cbData` seja 0, o método poderá ser implementado da seguinte maneira:

```csharp
public unsafe int ManagedWrapper(Span<byte> data)
{
    fixed (byte* pbData = &MemoryMarshal.GetReference(data))
    {
        byte dummy = 0;
        int retVal = ExportedMethod((pbData != null) ? pbData : &dummy, data.Length);

        /* error checking retVal goes here */

        return retVal;
    }
}
```

**Regra #10: se você estiver encapsulando um método p/invoke assíncrono, sua API deverá aceitar memória \<T> como um parâmetro.**

Como você não pode usar a [`fixed`](../../csharp/language-reference/keywords/fixed-statement.md) palavra-chave entre operações assíncronas, use o <xref:System.Memory%601.Pin%2A?displayProperty=nameWithType> método para fixar <xref:System.Memory%601> instâncias, independentemente do tipo de memória contígua que a instância representa. O exemplo a seguir mostra como usar esta API para executar uma chamada de P/Invoke assíncrona.

```csharp
using System.Runtime.InteropServices;

[UnmanagedFunctionPointer(...)]
private delegate void OnCompletedCallback(IntPtr state, int result);

[DllImport(...)]
private static extern unsafe int ExportedAsyncMethod(byte* pbData, int cbData, IntPtr pState, IntPtr lpfnOnCompletedCallback);

private static readonly IntPtr _callbackPtr = GetCompletionCallbackPointer();

public unsafe Task<int> ManagedWrapperAsync(Memory<byte> data)
{
    // setup
    var tcs = new TaskCompletionSource<int>();
    var state = new MyCompletedCallbackState
    {
        Tcs = tcs
    };
    var pState = (IntPtr)GCHandle.Alloc(state);

    var memoryHandle = data.Pin();
    state.MemoryHandle = memoryHandle;

    // make the call
    int result;
    try
    {
        result = ExportedAsyncMethod((byte*)memoryHandle.Pointer, data.Length, pState, _callbackPtr);
    }
    catch
    {
        ((GCHandle)pState).Free(); // cleanup since callback won't be invoked
        memoryHandle.Dispose();
        throw;
    }

    if (result != PENDING)
    {
        // Operation completed synchronously; invoke callback manually
        // for result processing and cleanup.
        MyCompletedCallbackImplementation(pState, result);
    }

    return tcs.Task;
}

private static void MyCompletedCallbackImplementation(IntPtr state, int result)
{
    GCHandle handle = (GCHandle)state;
    var actualState = (MyCompletedCallbackState)(handle.Target);
    handle.Free();
    actualState.MemoryHandle.Dispose();

    /* error checking result goes here */

    if (error)
    {
        actualState.Tcs.SetException(...);
    }
    else
    {
        actualState.Tcs.SetResult(result);
    }
}

private static IntPtr GetCompletionCallbackPointer()
{
    OnCompletedCallback callback = MyCompletedCallbackImplementation;
    GCHandle.Alloc(callback); // keep alive for lifetime of application
    return Marshal.GetFunctionPointerForDelegate(callback);
}

private class MyCompletedCallbackState
{
    public TaskCompletionSource<int> Tcs;
    public MemoryHandle MemoryHandle;
}
```

## <a name="see-also"></a>Confira também

- <xref:System.Memory%601?displayProperty=nameWithType>
- <xref:System.Buffers.IMemoryOwner%601?displayProperty=nameWithType>
- <xref:System.Span%601?displayProperty=nameWithType>
