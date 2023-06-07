---
title: System. buffers-.NET
ms.date: 12/05/2019
ms.technology: dotnet-standard
helpviewer_keywords:
- buffers [.NET]
- I/O [.NET], buffers
author: rick-anderson
ms.author: riande
ms.openlocfilehash: d113def0182dc6a5bcea6c18b2d0e4b475946e31
ms.sourcegitcommit: 465547886a1224a5435c3ac349c805e39ce77706
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81739616"
---
# <a name="work-with-buffers-in-net"></a>Trabalhar com buffers no .NET

Este artigo fornece uma visão geral dos tipos que ajudam a ler dados que são executados em vários buffers. Elas são usadas principalmente para dar suporte a <xref:System.IO.Pipelines.PipeReader> objetos.

## <a name="ibufferwritert"></a>IBufferWriter \< T\>

<xref:System.Buffers.IBufferWriter%601?displayProperty=fullName>é um contrato para gravação síncrona em buffer. No nível mais baixo, a interface:

- É básico e não difícil de usar.
- Permite o acesso a um <xref:System.Memory%601> ou <xref:System.Span%601> . O `Memory<T>` ou `Span<T>` pode ser gravado e você pode determinar quantos `T` itens foram gravados.

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet)]

O método anterior:

- Solicita um buffer de pelo menos 5 bytes do `IBufferWriter<byte>` uso do `GetSpan(5)` .
- Grava bytes para a cadeia de caracteres ASCII "Olá" para o retornado `Span<byte>` .
- Chama <xref:System.Buffers.IBufferWriter%601> para indicar quantos bytes foram gravados no buffer.

Esse método de gravação usa o `Memory<T>` / `Span<T>` buffer fornecido pelo `IBufferWriter<T>` . Como alternativa, o <xref:System.Buffers.BuffersExtensions.Write%2A> método de extensão pode ser usado para copiar um buffer existente para o `IBufferWriter<T>` . `Write`faz o trabalho de chamar `GetSpan` / `Advance` conforme apropriado, portanto, não há necessidade de chamar `Advance` após escrever:

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet2)]

<xref:System.Buffers.ArrayBufferWriter%601>é uma implementação de `IBufferWriter<T>` cujo armazenamento de backup é uma única matriz contígua.

### <a name="ibufferwriter-common-problems"></a>Problemas comuns do IBufferWriter

- `GetSpan`e `GetMemory` retornam um buffer com pelo menos a quantidade solicitada de memória. Não presuma os tamanhos de buffer exatos.
- Não há nenhuma garantia de que as chamadas sucessivas retornarão o mesmo buffer ou o buffer de mesmo tamanho.
- Um novo buffer deve ser solicitado após `Advance` a chamada para continuar gravando mais dados. Um buffer adquirido anteriormente não pode ser gravado depois de `Advance` ter sido chamado.

## <a name="readonlysequencet"></a>ReadOnlySequence \< T\>

![ReadOnlySequence mostrando a memória no pipe e abaixo da posição da sequência de memória somente leitura](media/buffers/ro-sequence.png)

<xref:System.Buffers.ReadOnlySequence%601>é uma struct que pode representar uma sequência contígua ou não contígua de `T` . Ele pode ser construído a partir de:

1. Uma `T[]`
1. Uma `ReadOnlyMemory<T>`
1. Um par de nó de lista vinculada <xref:System.Buffers.ReadOnlySequenceSegment%601> e índice para representar a posição inicial e final da sequência.

A terceira representação é a mais interessante, uma vez que tem implicações de desempenho em várias operações no `ReadOnlySequence<T>` :

|Representação|Operação|Complexidade|
---|---|---|
|`T[]`/`ReadOnlyMemory<T>`|`Length`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`GetPosition(long)`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`Slice(int, int)`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`Slice(SequencePostion,  SequencePostion)`|`O(1)`|
|`ReadOnlySequenceSegment<T>`|`Length`|`O(1)`|
|`ReadOnlySequenceSegment<T>`|`GetPosition(long)`|`O(number of segments)`|
|`ReadOnlySequenceSegment<T>`|`Slice(int, int)`|`O(number of segments)`|
|`ReadOnlySequenceSegment<T>`|`Slice(SequencePostion, SequencePostion)`|`O(1)`|

Devido a essa representação mista, o `ReadOnlySequence<T>` expõe índices como `SequencePosition` em vez de um inteiro. R `SequencePosition` :

- É um valor opaco que representa um índice no `ReadOnlySequence<T>` local onde ele foi originado.
- Consiste em duas partes, um inteiro e um objeto. O que esses dois valores representam são vinculados à implementação de `ReadOnlySequence<T>` .

### <a name="access-data"></a>Acessar dados

O `ReadOnlySequence<T>` expõe dados como um enumerável de `ReadOnlyMemory<T>` . A enumeração de cada um dos segmentos pode ser feita usando um foreach básico:

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet3)]

O método anterior pesquisa cada segmento em busca de um byte específico. Se você precisar manter o controle de cada segmento `SequencePosition` , <xref:System.Buffers.ReadOnlySequence%601.TryGet%2A?displayProperty=nameWithType> é mais apropriado. O próximo exemplo altera o código anterior para retornar um `SequencePosition` , em vez de um inteiro. Retornar um `SequencePosition` tem a vantagem de permitir que o chamador Evite uma segunda verificação para obter os dados em um índice específico.

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet4)]

A combinação de `SequencePosition` e `TryGet` atua como um enumerador. O campo posição é modificado no início de cada iteração para ser iniciado de cada segmento dentro do `ReadOnlySequence<T>` .

O método anterior existe como um método de extensão em `ReadOnlySequence<T>` . <xref:System.Buffers.BuffersExtensions.PositionOf%2A>pode ser usado para simplificar o código anterior:

```csharp
SequencePosition? FindIndexOf(in ReadOnlySequence<byte> buffer, byte data) => buffer.PositionOf(data);
```

#### <a name="process-a-readonlysequencet"></a>Processar um ReadOnlySequence \< T\>

O processamento de um `ReadOnlySequence<T>` pode ser desafiador, pois os dados podem ser divididos em vários segmentos dentro da sequência. Para obter o melhor desempenho, divida o código em dois caminhos:

- Um caminho rápido que lida com o caso de segmento único.
- Um caminho lento que lida com os dados divididos entre segmentos.

Há algumas abordagens que podem ser usadas para processar dados em sequências com vários segmentos:

- Use o [`SequenceReader<T>`](#sequencereadert) .
- Analisar o segmento de dados por segmento, controlando o `SequencePosition` índice e dentro do segmento analisado. Isso evita as alocações desnecessárias, mas pode ser ineficiente, especialmente para buffers pequenos.
- Copie o `ReadOnlySequence<T>` para uma matriz contígua e trate-o como um único buffer:
  - Se o tamanho do `ReadOnlySequence<T>` for pequeno, talvez seja razoável copiar os dados em um buffer alocado por pilha usando o operador [stackalloc](../../csharp/language-reference/operators/stackalloc.md) .
  - Copie o `ReadOnlySequence<T>` em uma matriz em pool usando <xref:System.Buffers.ArrayPool%601.Shared%2A?displayProperty=nameWithType> .
  - Use [`ReadOnlySequence<T>.ToArray()`](xref:System.Buffers.BuffersExtensions.ToArray%2A). Isso não é recomendado em caminhos ativos, pois aloca um novo `T[]` no heap.

Os exemplos a seguir demonstram alguns casos comuns para processamento `ReadOnlySequence<byte>` :

##### <a name="process-binary-data"></a>Processar dados binários

O exemplo a seguir analisa um tamanho inteiro big-endian de 4 bytes desde o início do `ReadOnlySequence<byte>` .

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet5)]

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

##### <a name="process-text-data"></a>Processar dados de texto

O exemplo a seguir:

- Localiza a primeira nova linha ( `\r\n` ) no `ReadOnlySequence<byte>` e retorna-a por meio do parâmetro ' line ' out.
- Corta essa linha, excluindo o `\r\n` do buffer de entrada.

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet6)]

##### <a name="empty-segments"></a>Segmentos vazios

É válido armazenar segmentos vazios dentro de um `ReadOnlySequence<T>` . Segmentos vazios podem ocorrer ao enumerar segmentos explicitamente:

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet7)]

O código anterior cria um `ReadOnlySequence<byte>` com segmentos vazios e mostra como esses segmentos vazios afetam as várias APIs:

- `ReadOnlySequence<T>.Slice`com um `SequencePosition` apontando para um segmento vazio preserva esse segmento.
- `ReadOnlySequence<T>.Slice`com um int ignora os segmentos vazios.
- Enumerar o `ReadOnlySequence<T>` enumera os segmentos vazios.

### <a name="potential-problems-with-readonlysequencet-and-sequenceposition"></a>Problemas potenciais com ReadOnlySequence \< T> e SequencePosition

Há vários resultados incomuns ao lidar com um `ReadOnlySequence<T>` / `SequencePosition` normal `ReadOnlySpan<T>` / `ReadOnlyMemory<T>` / `T[]` / `int` :

- `SequencePosition`é um marcador de posição para uma `ReadOnlySequence<T>` posição específica, e não absoluta. Como ele é relativo a um específico `ReadOnlySequence<T>` , ele não tem significado se for usado fora do `ReadOnlySequence<T>` local onde ele foi originado.
- A aritmética não pode ser executada em `SequencePosition` sem o `ReadOnlySequence<T>` . Isso significa que fazer coisas básicas como `position++` é escrito `ReadOnlySequence<T>.GetPosition(position, 1)` .
- `GetPosition(long)`Não **oferece suporte** a índices negativos. Isso significa que é impossível obter o segundo para o último caractere sem percorrer todos os segmentos.
- Dois `SequencePosition` não podem ser comparados, dificultando:
  - Saiba se uma posição é maior ou menor que outra posição.
  - Escreva alguns algoritmos de análise.
- `ReadOnlySequence<T>`é maior que uma referência de objeto e deve ser passada por [em](../../csharp/language-reference/keywords/in-parameter-modifier.md) ou [ref](../../csharp/language-reference/keywords/ref.md) sempre que possível. Passando `ReadOnlySequence<T>` por `in` ou `ref` reduz cópias da [estrutura](../../csharp/language-reference/builtin-types/struct.md).
- Segmentos vazios:
  - São válidas dentro de um `ReadOnlySequence<T>` .
  - Pode aparecer ao iterar usando o `ReadOnlySequence<T>.TryGet` método.
  - Pode aparecer de divisão da sequência usando o `ReadOnlySequence<T>.Slice()` método com `SequencePosition` objetos.

## <a name="sequencereadert"></a>SequenceReader \< T\>

<xref:System.Buffers.SequenceReader%601>:

- É um novo tipo que foi introduzido no .NET Core 3,0 para simplificar o processamento de um `ReadOnlySequence<T>` .
- Unifica as diferenças entre um único segmento `ReadOnlySequence<T>` e vários segmentos `ReadOnlySequence<T>` .
- Fornece ajuda para a leitura de dados binários e de texto ( `byte` e `char` ) que podem ou não ser divididos entre segmentos.

Há métodos internos para lidar com o processamento de dados binários e delimitados. A seção a seguir demonstra como os mesmos métodos se parecem com `SequenceReader<T>` :

### <a name="access-data"></a>Acessar dados

`SequenceReader<T>`tem métodos para enumerar dados dentro do `ReadOnlySequence<T>` diretamente. O código a seguir é um exemplo de processamento de um de `ReadOnlySequence<byte>` `byte` cada vez:

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet8)]

O `CurrentSpan` expõe o segmento atual `Span` , que é semelhante ao que foi feito no método manualmente.

### <a name="use-position"></a>Usar posição

O código a seguir é uma implementação de exemplo do `FindIndexOf` usando o `SequenceReader<T>` :

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet9)]

### <a name="process-binary-data"></a>Processar dados binários

O exemplo a seguir analisa um tamanho inteiro big-endian de 4 bytes desde o início do `ReadOnlySequence<byte>` .

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet11)]

### <a name="process-text-data"></a>Processar dados de texto

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet10)]

### <a name="sequencereadert-common-problems"></a>SequenceReader \< \> problemas comuns

- Como `SequenceReader<T>` é uma struct mutável, ela sempre deve ser passada por [referência](../../csharp/language-reference/keywords/ref.md).
- `SequenceReader<T>`é uma [struct de referência](../../csharp/language-reference/builtin-types/struct.md#ref-struct) para que ela possa ser usada apenas em métodos síncronos e não pode ser armazenada em campos. Para obter mais informações, consulte [escrever código C# seguro e eficiente](../../csharp/write-safe-efficient-code.md).
- `SequenceReader<T>`é otimizado para uso como um leitor somente de encaminhamento. `Rewind`o destina-se a backups pequenos que não podem ser resolvidos utilizando outras `Read` `Peek` APIs, e `IsNext` .
