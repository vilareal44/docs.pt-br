---
title: Implementar um método Dispose
description: Neste artigo, aprenda a implementar o método Dispose, que libera recursos não gerenciados usados pelo seu código no .NET.
ms.date: 05/27/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Dispose method
- garbage collection, Dispose method
ms.assetid: eb4e1af0-3b48-4fbc-ad4e-fc2f64138bf9
ms.openlocfilehash: 4f0cc9b88947d60638057ca83adb7f2e141c5d14
ms.sourcegitcommit: 7499bdb428d63ed0e19e97f54d3d576c41598659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87455733"
---
# <a name="implement-a-dispose-method"></a>Implementar um método Dispose

A implementação do <xref:System.IDisposable.Dispose%2A> método destina-se principalmente à liberação de recursos não gerenciados. Ao trabalhar com membros de instância que são <xref:System.IDisposable> implementações, é comum fazer chamadas em cascata <xref:System.IDisposable.Dispose%2A> . Há motivos adicionais para implementar <xref:System.IDisposable.Dispose%2A> , por exemplo, para liberar memória que foi alocada, remover um item que foi adicionado a uma coleção ou sinalizar a liberação de um bloqueio que foi adquirido.

O [coletor de lixo do .net](index.md) não aloca ou libera memória não gerenciada. O padrão para descartar um objeto, chamado de padrão Dispose, impõe a ordem no tempo de vida de um objeto. O padrão Dispose é usado para objetos que implementam a <xref:System.IDisposable> interface e é comum ao interagir com identificadores de arquivo e pipe, identificadores de registro, identificadores de espera ou ponteiros para blocos de memória não gerenciada. Isso ocorre porque o coletor de lixo não pode recuperar objetos não gerenciados.

Para ajudar a garantir que os recursos sempre sejam limpos adequadamente, um <xref:System.IDisposable.Dispose%2A> método deve ser idempotente, de modo que ele possa ser chamado várias vezes sem gerar uma exceção. Além disso, as invocações subsequentes do <xref:System.IDisposable.Dispose%2A> não devem fazer nada.

O exemplo de código fornecido para o <xref:System.GC.KeepAlive%2A?displayProperty=nameWithType> método mostra como a coleta de lixo pode fazer com que um finalizador seja executado, enquanto uma referência não gerenciada para o objeto ou seus membros ainda está em uso. Pode fazer sentido utilizar <xref:System.GC.KeepAlive%2A?displayProperty=nameWithType> para tornar o objeto inelegível para coleta de lixo do início da rotina atual até o ponto em que esse método é chamado.

## <a name="safe-handles"></a>Identificadores seguros

Escrever código para o finalizador de um objeto é uma tarefa complexa que poderá causar problemas se não for feito corretamente. Assim, recomendamos que você construa objetos <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType>, em vez de implementar um finalizador.

Um <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> é um tipo gerenciado abstrato que encapsula um <xref:System.IntPtr?displayProperty=nameWithType> que identifica um recurso não gerenciado. No Windows, ele pode identificar um identificador no UNIX, um descritor de arquivo. Ele fornece toda a lógica necessária para garantir que esse recurso seja liberado apenas uma vez, quando o `SafeHandle` é Descartado ou quando todas as referências a foram `SafeHandle` removidas e a `SafeHandle` instância é finalizada.

O <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> é uma classe base abstrata. As classes derivadas fornecem instâncias específicas para diferentes tipos de identificador. Essas classes derivadas validam quais valores do <xref:System.IntPtr?displayProperty=nameWithType> são considerados inválidos e como realmente liberar o identificador. Por exemplo, <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> deriva de `SafeHandle` para encapsular `IntPtrs` que identifica identificadores/descritores de arquivos abertos e substitui seu <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle?displayProperty=nameWithType> método para fechá-lo (por meio da `close` função no UNIX ou na `CloseHandle` função no Windows). A maioria das APIs em bibliotecas do .NET que criam um recurso não gerenciado o encapsulará em um `SafeHandle` e retornará isso `SafeHandle` para você conforme necessário, em vez de redistribuir o ponteiro bruto. Em situações em que você interage com um componente não gerenciado e Obtém um `IntPtr` para um recurso não gerenciado, você pode criar seu próprio `SafeHandle` tipo para encapsulá-lo. Como resultado, poucos não `SafeHandle` tipos precisam implementar finalizadores; a maioria das implementações de padrão descartáveis acaba com o encapsulamento de outros recursos gerenciados, alguns dos quais podem ser `SafeHandle` s.

As seguintes classes derivadas no namespace <xref:Microsoft.Win32.SafeHandles> fornecem os identificadores seguros:

- As classes <xref:Microsoft.Win32.SafeHandles.SafeFileHandle>, <xref:Microsoft.Win32.SafeHandles.SafeMemoryMappedFileHandle> e <xref:Microsoft.Win32.SafeHandles.SafePipeHandle> para arquivos, arquivos mapeados na memória e pipes.
- A classe <xref:Microsoft.Win32.SafeHandles.SafeMemoryMappedViewHandle> para exibições de memória.
- As classes <xref:Microsoft.Win32.SafeHandles.SafeNCryptKeyHandle>, <xref:Microsoft.Win32.SafeHandles.SafeNCryptProviderHandle> e <xref:Microsoft.Win32.SafeHandles.SafeNCryptSecretHandle> para construtores de criptografia.
- A classe <xref:Microsoft.Win32.SafeHandles.SafeRegistryHandle> para chaves do registro.
- A classe <xref:Microsoft.Win32.SafeHandles.SafeWaitHandle> para identificadores de espera.

## <a name="dispose-and-disposebool"></a>Dispose () e Dispose (bool)

A interface <xref:System.IDisposable> requer a implementação de um único método sem parâmetros, <xref:System.IDisposable.Dispose%2A>. Além disso, qualquer classe não selada deve ter um `Dispose(bool)` método de sobrecarga adicional a ser implementado:

- Uma `public` implementação não virtual ( `NonInheritable` em Visual Basic) <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> que não tem parâmetros.

- Um `protected virtual` `Overridable` método (no Visual Basic) `Dispose` cuja assinatura é:

  [!code-csharp[Conceptual.Disposable#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/dispose1.cs#8)]
  [!code-vb[Conceptual.Disposable#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/dispose1.vb#8)]

  > [!IMPORTANT]
  > O `disposing` parâmetro deve ser `false` quando chamado de um finalizador e `true` quando chamado a partir do <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> método. Em outras palavras, é `true` quando chamado de forma determinística e `false` quando chamado de forma não determinística.

### <a name="the-dispose-method"></a>O método Dispose ()

Como o `public` , que não é virtual ( `NonInheritable` em Visual Basic), o `Dispose` método sem parâmetros é chamado por um consumidor do tipo, sua finalidade é liberar recursos não gerenciados, executar a limpeza geral e indicar que o finalizador, se houver, não precisa ser executado. Liberar a memória real associada a um objeto gerenciado é sempre o domínio do coletor de [lixo](index.md). Por isso, ele tem uma implementação padrão:

[!code-csharp[Conceptual.Disposable#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/dispose1.cs#7)]
[!code-vb[Conceptual.Disposable#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/dispose1.vb#7)]

O método `Dispose` executa toda a limpeza do objeto, de modo que o coletor de lixo não precisa mais chamar a substituição dos objetos <xref:System.Object.Finalize%2A?displayProperty=nameWithType>. Assim, a chamada para o método <xref:System.GC.SuppressFinalize%2A> impede que o coletor de lixo execute o finalizador. Se o tipo não possuir um finalizador, a chamada para <xref:System.GC.SuppressFinalize%2A?displayProperty=nameWithType> não terá efeito. Observe que a limpeza real é executada pela `Dispose(bool)` sobrecarga do método.

### <a name="the-disposebool-method-overload"></a>A sobrecarga do método Dispose (bool)

Na sobrecarga, o `disposing` parâmetro é um <xref:System.Boolean> que indica se a chamada do método vem de um <xref:System.IDisposable.Dispose%2A> método (seu valor é `true` ) ou de um finalizador (seu valor é `false` ).

O corpo do método consiste em dois blocos de código:

- Um bloco que libera recursos não gerenciados. Este bloco é executado independentemente do valor do parâmetro `disposing`.
- Um bloco condicional que libera recursos gerenciados. Este bloco será executado se o valor de `disposing` for `true`. Os recursos gerenciados que ele libera podem incluir:

  - **Objetos gerenciados que implementam <xref:System.IDisposable>.** O bloco condicional pode ser usado para chamar sua <xref:System.IDisposable.Dispose%2A> implementação (descarte em cascata). Se você tiver usado uma classe derivada do <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> para encapsular seu recurso não gerenciado, deverá chamar a <xref:System.Runtime.InteropServices.SafeHandle.Dispose?displayProperty=nameWithType> implementação aqui.

  - **Objetos gerenciados que consomem muita memória ou consomem recursos escassos.** Atribua grandes referências de objeto gerenciado ao `null` para torná-las mais prováveis de serem inacessíveis. Isso libera mais rápido do que se eles fossem recuperados de forma não determinística.

Se a chamada do método vier de um finalizador, somente o código que libera recursos não gerenciados deve ser executado. O implementador é responsável por garantir que o caminho falso não interaja com objetos gerenciados que podem ter sido recuperados. Isso é importante porque a ordem na qual o coletor de lixo destrói os objetos gerenciados durante a finalização é não determinística.

## <a name="cascade-dispose-calls"></a>Propagar chamadas de Dispose

Se sua classe possui um campo ou propriedade, e seu tipo implementa <xref:System.IDisposable> , a própria classe recipiente também deve implementar <xref:System.IDisposable> . Uma classe que instancia uma <xref:System.IDisposable> implementação e o armazena como um membro de instância também é responsável por sua limpeza. Isso é para ajudar a garantir que os tipos descartáveis referenciados recebam a oportunidade de realizar a limpeza de forma determinística através do <xref:System.IDisposable.Dispose%2A> método. Neste exemplo, a classe é `sealed` (ou `NotInheritable` em Visual Basic).

[!code-csharp[Conceptual.Disposable#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/disposable1.cs#1)]
[!code-vb[Conceptual.Disposable#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/disposable1.vb#1)]

## <a name="implement-the-dispose-pattern"></a>Implementar o padrão Dispose

Todas as classes não seladas ou (Visual Basic classes não modificadas como `NotInheritable` ) devem ser consideradas uma classe base em potencial, porque elas podem ser herdadas. Se você implementar o padrão Dispose para qualquer classe base em potencial, deverá fornecer o seguinte:

- Uma implementação de <xref:System.IDisposable.Dispose%2A> que chame o método `Dispose(bool)`.
- Um `Dispose(bool)` método que executa a limpeza real.
- Uma classe derivada de <xref:System.Runtime.InteropServices.SafeHandle> que envolva o recurso não gerenciado (recomendado) ou uma substituição para o método <xref:System.Object.Finalize%2A?displayProperty=nameWithType>. A <xref:System.Runtime.InteropServices.SafeHandle> classe fornece um finalizador, portanto, você não precisa escrever um por conta própria.

> [!IMPORTANT]
> É possível que uma classe base referencie apenas objetos gerenciados e implemente o padrão Dispose. Nesses casos, um finalizador é desnecessário. Um finalizador só será necessário se você fizer referência direta A recursos não gerenciados.

Aqui está o padrão geral para implementar o padrão de descarte para uma classe base que usa um identificador seguro.

[!code-csharp[System.IDisposable#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.idisposable/cs/base1.cs#3)]
[!code-vb[System.IDisposable#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.idisposable/vb/base1.vb#3)]

> [!NOTE]
> O exemplo anterior usa um objeto <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> para ilustrar o padrão; qualquer objeto derivado de <xref:System.Runtime.InteropServices.SafeHandle> poderia ser usado em vez disso. Observe que o exemplo não cria corretamente uma instância de seu objeto <xref:Microsoft.Win32.SafeHandles.SafeFileHandle>.

Aqui está o padrão geral para implementar o padrão de descarte para uma classe base que substitui <xref:System.Object.Finalize%2A?displayProperty=nameWithType>.

[!code-csharp[System.IDisposable#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.idisposable/cs/base2.cs#5)]
[!code-vb[System.IDisposable#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.idisposable/vb/base2.vb#5)]

> [!TIP]
> Em C#, você cria um [finalizador](../../csharp/programming-guide/classes-and-structs/destructors.md) substituindo <xref:System.Object.Finalize%2A?displayProperty=nameWithType> . No Visual Basic, isso é feito com `Protected Overrides Sub Finalize()` .

## <a name="implement-the-dispose-pattern-for-a-derived-class"></a>Implementar o padrão Dispose para uma classe derivada

Uma classe derivada de uma classe que implementa a interface <xref:System.IDisposable> não deve implementar <xref:System.IDisposable> porque a implementação da classe base <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> é herdada pelas classes derivadas. Em vez disso, para limpar uma classe derivada, você fornece o seguinte:

- Um `protected override void Dispose(bool)` método que substitui o método da classe base e executa a limpeza real da classe derivada. Esse método também deve chamar o `base.Dispose(bool)` `MyBase.Dispose(bool)` método (no Visual Basic) da classe base e passar seu status de descarte para o argumento.
- Uma classe derivada de <xref:System.Runtime.InteropServices.SafeHandle> que envolva o recurso não gerenciado (recomendado) ou uma substituição para o método <xref:System.Object.Finalize%2A?displayProperty=nameWithType>. A classe <xref:System.Runtime.InteropServices.SafeHandle> fornece um finalizador que o libera de ter que codificar um. Se você fornecer um finalizador, ele deverá chamar a `Dispose(bool)` sobrecarga com um `disposing` argumento de `false` .

Aqui está o padrão geral para implementar o padrão de descarte para uma classe derivada que usa um identificador seguro:

[!code-csharp[System.IDisposable#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.idisposable/cs/derived1.cs#4)]
[!code-vb[System.IDisposable#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.idisposable/vb/derived1.vb#4)]

> [!NOTE]
> O exemplo anterior usa um objeto <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> para ilustrar o padrão; qualquer objeto derivado de <xref:System.Runtime.InteropServices.SafeHandle> poderia ser usado em vez disso. Observe que o exemplo não cria corretamente uma instância de seu objeto <xref:Microsoft.Win32.SafeHandles.SafeFileHandle>.

Aqui está o padrão geral para implementar o padrão de descarte para uma classe derivada que substitui <xref:System.Object.Finalize%2A?displayProperty=nameWithType>:

[!code-csharp[System.IDisposable#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.idisposable/cs/derived2.cs#6)]
[!code-vb[System.IDisposable#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.idisposable/vb/derived2.vb#6)]

## <a name="implement-the-dispose-pattern-with-safe-handles"></a>Implementar o padrão Dispose com identificadores seguros

O exemplo a seguir ilustra o padrão de descarte para uma classe base, `DisposableStreamResource`, a qual usa um identificador seguro para encapsular recursos não gerenciados. Define uma classe `DisposableStreamResource` que usa <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> para encapsular um objeto <xref:System.IO.Stream> que representa um arquivo aberto. A classe também inclui uma única propriedade, `Size` , que retorna o número total de bytes no fluxo de arquivos.

[!code-csharp[Conceptual.Disposable#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/base1.cs#9)]
[!code-vb[Conceptual.Disposable#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/base1.vb#9)]

## <a name="implement-the-dispose-pattern-for-a-derived-class-with-safe-handles"></a>Implementar o padrão Dispose para uma classe derivada com identificadores seguros

O exemplo a seguir ilustra o padrão de descarte para uma classe derivada, `DisposableStreamResource2`, que herda da classe `DisposableStreamResource` apresentada no exemplo anterior. A classe acrescenta um método adicional, `WriteFileInfo`, e usa um objeto <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> para encapsular o identificador do arquivo gravável.

[!code-csharp[Conceptual.Disposable#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/derived1.cs#10)]
[!code-vb[Conceptual.Disposable#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/derived1.vb#10)]

## <a name="see-also"></a>Veja também

- <xref:System.GC.SuppressFinalize%2A>
- <xref:System.IDisposable>
- <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType>
- <xref:Microsoft.Win32.SafeHandles>
- <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType>
- <xref:System.Object.Finalize%2A?displayProperty=nameWithType>
- [Definir e consumir classes e estruturas (C++/CLI)](/cpp/dotnet/how-to-define-and-consume-classes-and-structs-cpp-cli)
