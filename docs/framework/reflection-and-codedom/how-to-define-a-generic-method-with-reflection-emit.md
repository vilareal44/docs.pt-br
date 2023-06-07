---
title: 'Como: Definir um método genérico com a emissão de reflexão'
description: Defina um método genérico com emissão de reflexão. Um exemplo cria um método genérico com dois parâmetros de tipo. Um segundo exemplo mostra como emitir o corpo do método.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- generics [.NET Framework], reflection emit
- reflection emit, generic methods
- generics [.NET Framework], dynamic types
ms.assetid: 93892fa4-90b3-4ec4-b147-4bec9880de2b
ms.openlocfilehash: 3b85fb480e5862daa3b2800f75392adbe92348f2
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865132"
---
# <a name="how-to-define-a-generic-method-with-reflection-emit"></a>Como: Definir um método genérico com a emissão de reflexão

O primeiro procedimento mostra como criar um método genérico simples com dois parâmetros de tipo e como aplicar restrições de classe, restrições de interface e restrições especiais aos parâmetros de tipo.

O segundo procedimento mostra como emitir o corpo do método e como usar os parâmetros de tipo do método genérico para criar instâncias de tipos genéricos e chamar seus métodos.

O terceiro procedimento mostra como invocar o método genérico.

> [!IMPORTANT]
> Um método não é genérico apenas porque pertence a um tipo genérico e usa os parâmetros de tipo desse tipo. Um método será genérico somente se ele tiver sua própria lista de parâmetros de tipo. Um método genérico pode aparecer em um tipo não genérico, como neste exemplo. Para obter um exemplo de um método não genérico em um tipo genérico, consulte [Como definir um tipo genérico com a emissão de reflexão](how-to-define-a-generic-type-with-reflection-emit.md).

### <a name="to-define-a-generic-method"></a>Para definir um método genérico

1. Antes de começar, é útil observar como o método genérico aparece quando escrito usando uma linguagem de alto nível. O código a seguir está incluído no código de exemplo deste tópico, juntamente com o código para chamar o método genérico. O método tem dois parâmetros de tipo, `TInput` e `TOutput`, o segundo dos quais deve ser um tipo de referência (`class`), deve ter um construtor sem parâmetros (`new`) e deve implementar `ICollection(Of TInput)` (`ICollection<TInput>` em C#). Essa restrição de interface garante que o método <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType> pode ser usado para adicionar elementos à coleção `TOutput` que o método cria. O método tem um parâmetro formal, `input`, que é uma matriz de `TInput`. O método cria uma coleção do tipo `TOutput` e copia os elementos de `input` para a coleção.

    [!code-csharp[GenericMethodHowTo#20](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#20)]
    [!code-vb[GenericMethodHowTo#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#20)]

2. Defina um assembly dinâmico e um módulo dinâmico para conter o tipo ao qual o método genérico pertence. Nesse caso, o assembly tem apenas um módulo, chamado `DemoMethodBuilder1` e o nome do módulo é o mesmo nome do assembly com a adição de uma extensão. Neste exemplo, o assembly é salvo no disco e também executado, portanto, <xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndSave?displayProperty=nameWithType> é especificado. Você pode usar o [Ildasm.exe (IL Disassembler)](../tools/ildasm-exe-il-disassembler.md) para examinar o DemoMethodBuilder1.dll e compará-lo à MSIL (	Microsoft Intermediate Language) para o método mostrado na etapa 1.

    [!code-csharp[GenericMethodHowTo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#2)]
    [!code-vb[GenericMethodHowTo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#2)]

3. Defina o tipo ao qual o método genérico pertence. O tipo não precisa ser genérico. Um método genérico pode pertencer a um tipo genérico ou não genérico. Neste exemplo, o tipo é uma classe, não é genérico e é chamado `DemoType`.

    [!code-csharp[GenericMethodHowTo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#3)]
    [!code-vb[GenericMethodHowTo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#3)]

4. Defina o método genérico. Se os tipos dos parâmetros formais de um método genérico forem especificados por parâmetros de tipo genérico do método genérico, use a sobrecarga do método <xref:System.Reflection.Emit.TypeBuilder.DefineMethod%28System.String%2CSystem.Reflection.MethodAttributes%29> para definir o método. Os parâmetros de tipo genérico do método ainda não foram definidos, portanto você não pode especificar os tipos dos parâmetros formais do método na chamada para <xref:System.Reflection.Emit.TypeBuilder.DefineMethod%2A>. Neste exemplo, o método é chamado `Factory`. O método é público e `static` (`Shared` no Visual Basic).

    [!code-csharp[GenericMethodHowTo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#4)]
    [!code-vb[GenericMethodHowTo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#4)]

5. Defina os parâmetros de tipo genérico de `DemoMethod` passando uma matriz de cadeias de caracteres que contém os nomes dos parâmetros para o método <xref:System.Reflection.Emit.MethodBuilder.DefineGenericParameters%2A?displayProperty=nameWithType>. Isso torna o método um método genérico. O código a seguir torna `Factory` um método genérico com parâmetros de tipo `TInput` e `TOutput`. Para tornar o código mais fácil de ler, variáveis com esses nomes são criadas para conter os objetos <xref:System.Reflection.Emit.GenericTypeParameterBuilder> que representam os dois tipos parâmetros.

    [!code-csharp[GenericMethodHowTo#5](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#5)]
    [!code-vb[GenericMethodHowTo#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#5)]

6. Opcionalmente, adicione restrições especiais aos parâmetros de tipo. As restrições especiais são adicionadas usando o método <xref:System.Reflection.Emit.GenericTypeParameterBuilder.SetGenericParameterAttributes%2A>. Neste exemplo, `TOutput` é restrito para ser um tipo de referência e para ter um construtor sem parâmetros.

    [!code-csharp[GenericMethodHowTo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#6)]
    [!code-vb[GenericMethodHowTo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#6)]

7. Opcionalmente, adicione restrições de classe e interface aos parâmetros de tipo. Neste exemplo, o parâmetro de tipo `TOutput` é restrito a tipos que implementam a interface `ICollection(Of TInput)` (`ICollection<TInput>` em C#). Isso garante que o método <xref:System.Collections.Generic.ICollection%601.Add%2A> pode ser usado para adicionar elementos.

    [!code-csharp[GenericMethodHowTo#7](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#7)]
    [!code-vb[GenericMethodHowTo#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#7)]

8. Defina os parâmetros formais do método, usando o método <xref:System.Reflection.Emit.MethodBuilder.SetParameters%2A>. Neste exemplo, o método `Factory` tem um parâmetro, uma matriz de `TInput`. Esse tipo é criado chamando o método <xref:System.Type.MakeArrayType%2A> no <xref:System.Reflection.Emit.GenericTypeParameterBuilder> que representa `TInput`. O argumento de <xref:System.Reflection.Emit.MethodBuilder.SetParameters%2A> é uma matriz de objetos <xref:System.Type>.

    [!code-csharp[GenericMethodHowTo#8](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#8)]
    [!code-vb[GenericMethodHowTo#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#8)]

9. Defina o tipo de retorno para o método, usando o método <xref:System.Reflection.Emit.MethodBuilder.SetReturnType%2A>. Neste exemplo, uma instância de `TOutput` é retornada.

    [!code-csharp[GenericMethodHowTo#9](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#9)]
    [!code-vb[GenericMethodHowTo#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#9)]

10. Emita o corpo do método usando <xref:System.Reflection.Emit.ILGenerator>. Para obter detalhes, consulte o procedimento que acompanha este artigo para emitir o corpo do método.

    > [!IMPORTANT]
    > Quando emitir chamadas para métodos de tipos genéricos e os argumentos de tipo desses tipos forem parâmetros de tipo do método genérico, você deverá usar as sobrecargas de método `static`<xref:System.Reflection.Emit.TypeBuilder.GetConstructor%28System.Type%2CSystem.Reflection.ConstructorInfo%29>, <xref:System.Reflection.Emit.TypeBuilder.GetMethod%28System.Type%2CSystem.Reflection.MethodInfo%29> e <xref:System.Reflection.Emit.TypeBuilder.GetField%28System.Type%2CSystem.Reflection.FieldInfo%29> da classe <xref:System.Reflection.Emit.TypeBuilder> para obter formulários construídos dos métodos. O procedimento que acompanha este artigo para emitir o corpo do método demonstra isso.

11. Complete o tipo que contém o método e salve o assembly. O procedimento que acompanha este artigo para invocar o método genérico mostra duas maneiras de invocar o método concluído.

    [!code-csharp[GenericMethodHowTo#14](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#14)]
    [!code-vb[GenericMethodHowTo#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#14)]

<a name="procedureSection1"></a>

### <a name="to-emit-the-method-body"></a>Para emitir o corpo do método

1. Obtenha um gerador de código e declare rótulos e variáveis locais. O método <xref:System.Reflection.Emit.ILGenerator.DeclareLocal%2A> é usado para declarar variáveis locais. O método `Factory` tem quatro variáveis locais: `retVal` para manter o novo `TOutput` que é retornado pelo método, `ic` para manter o `TOutput` quando ele é convertido em `ICollection(Of TInput)` (`ICollection<TInput>` em C#), `input` para manter a matriz de entrada de objetos `TInput` e `index` para iterar por meio da matriz. O método também tem dois rótulos, um para inserir o loop (`enterLoop`) e outro para a parte superior do loop (`loopAgain`), definida usando o método <xref:System.Reflection.Emit.ILGenerator.DefineLabel%2A>.

    A primeira coisa que o método faz é carregar seu argumento usando o opcode <xref:System.Reflection.Emit.OpCodes.Ldarg_0> e armazená-los na variável local `input` usando o opcode <xref:System.Reflection.Emit.OpCodes.Stloc_S>.

    [!code-csharp[GenericMethodHowTo#10](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#10)]
    [!code-vb[GenericMethodHowTo#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#10)]

2. Emita o código para criar uma instância de `TOutput`, usando a sobrecarga de método genérico do método <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType>. Usar essa sobrecarga requer que o tipo especificado tenha um construtor sem parâmetros, que é o motivo para adicionar essa restrição a `TOutput`. Crie o método genérico construído passando `TOutput` para <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A>. Depois de emitir o código para chamar o método, emita o código para armazená-lo na variável local `retVal` usando <xref:System.Reflection.Emit.OpCodes.Stloc_S>

    [!code-csharp[GenericMethodHowTo#11](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#11)]
    [!code-vb[GenericMethodHowTo#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#11)]

3. Emita o código para converter o novo objeto `TOutput` em `ICollection(Of TInput)` e armazená-lo na variável local `ic`.

    [!code-csharp[GenericMethodHowTo#31](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#31)]
    [!code-vb[GenericMethodHowTo#31](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#31)]

4. Obtenha um <xref:System.Reflection.MethodInfo> que representa o método <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType>. O método está atuando em um `ICollection(Of TInput)` (`ICollection<TInput>` em C#), portanto, é necessário obter o método `Add` específico para o tipo construído. Não é possível usar o método <xref:System.Type.GetMethod%2A> para obter esse <xref:System.Reflection.MethodInfo> diretamente de `icollOfTInput`, pois <xref:System.Type.GetMethod%2A> não tem suporte em um tipo que foi construído com um <xref:System.Reflection.Emit.GenericTypeParameterBuilder>. Em vez disso, chame <xref:System.Type.GetMethod%2A> em `icoll`, que contém a definição de tipo genérico para a interface genérica <xref:System.Collections.Generic.ICollection%601>. Em seguida, use o método <xref:System.Reflection.Emit.TypeBuilder.GetMethod%28System.Type%2CSystem.Reflection.MethodInfo%29>`static` para produzir o <xref:System.Reflection.MethodInfo> para o tipo construído. O código a seguir demonstra isso.

    [!code-csharp[GenericMethodHowTo#12](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#12)]
    [!code-vb[GenericMethodHowTo#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#12)]

5. Emita o código para inicializar a variável `index`, carregando um inteiro 0 de 32 bits e armazenando-o na variável. Emita o código para fazer o branch para o rótulo `enterLoop`. Esse rótulo ainda não foi marcado, pois ele está dentro do loop. O código para o loop é emitido na próxima etapa.

    [!code-csharp[GenericMethodHowTo#32](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#32)]
    [!code-vb[GenericMethodHowTo#32](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#32)]

6. Emita o código para o loop. A primeira etapa é marcar a parte superior do loop, chamando <xref:System.Reflection.Emit.ILGenerator.MarkLabel%2A> com o rótulo `loopAgain`. Instruções de branch que usam o rótulo agora farão o branch para esse ponto no código. A próxima etapa é enviar por push o objeto `TOutput`, convertido em `ICollection(Of TInput)`, para a pilha. Ele não é necessário imediatamente, mas precisa estar na posição para chamar o método `Add`. Em seguida a matriz de entrada é enviada por push para a pilha e, em seguida, a variável `index` que contém o índice atual para a matriz. O opcode <xref:System.Reflection.Emit.OpCodes.Ldelem> retira o índice e a matriz da pilha e envia por push o elemento de matriz indexado para a pilha. A pilha agora está pronta para a chamada para o método <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType>, que retira a coleção e o novo elemento da pilha e adiciona o elemento à coleção.

    O restante do código no loop incrementa o índice e testa para ver se o loop é concluído: o índice e um inteiro 1 de 32 bits são enviados por push para a pilha e adicionados, deixando a soma na pilha, a soma é armazenada no `index`. <xref:System.Reflection.Emit.ILGenerator.MarkLabel%2A> é chamado para definir esse ponto como o ponto de entrada para o loop. O índice é carregado novamente. A matriz de entrada é enviada por push na pilha e <xref:System.Reflection.Emit.OpCodes.Ldlen> é emitido para obter seu comprimento. O índice e o comprimento agora estão na pilha e <xref:System.Reflection.Emit.OpCodes.Clt> é emitido para compará-los. Se o índice for menor que o comprimento, <xref:System.Reflection.Emit.OpCodes.Brtrue_S> realiza o branch de volta para o início do loop.

    [!code-csharp[GenericMethodHowTo#13](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#13)]
    [!code-vb[GenericMethodHowTo#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#13)]

7. Emita o código para enviar por push o objeto `TOutput` para a pilha e retornar do método. As variáveis locais `retVal` e `ic` contêm referências para o novo `TOutput`. `ic` é usado somente para acessar o método <xref:System.Collections.Generic.ICollection%601.Add%2A?displayProperty=nameWithType>.

    [!code-csharp[GenericMethodHowTo#33](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#33)]
    [!code-vb[GenericMethodHowTo#33](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#33)]

<a name="procedureSection2"></a>

### <a name="to-invoke-the-generic-method"></a>Para invocar o método genérico

1. `Factory` é uma definição de método genérico. Para invocá-lo, você deve atribuir tipos para seus parâmetros de tipo genérico. Use o método <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A> para fazer isso. O código a seguir cria um método genérico construído, especificando <xref:System.String> para `TInput` e `List(Of String)` (`List<string>` em C#) para `TOutput` e exibe uma representação de cadeia de caracteres do método.

    [!code-csharp[GenericMethodHowTo#21](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#21)]
    [!code-vb[GenericMethodHowTo#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#21)]

2. Para invocar a método com associação tardia, use o método <xref:System.Reflection.MethodBase.Invoke%2A>. O código a seguir cria uma matriz de <xref:System.Object>, contendo como seu único elemento uma matriz de cadeias de caracteres a passa como a lista de argumentos para o método genérico. O primeiro parâmetro de <xref:System.Reflection.MethodBase.Invoke%2A> é uma referência nula, pois o método é `static`. O valor retornado é convertido em `List(Of String)` e o primeiro elemento é exibido.

    [!code-csharp[GenericMethodHowTo#22](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#22)]
    [!code-vb[GenericMethodHowTo#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#22)]

3. Para invocar o método usando um delegado, você deve ter um delegado que corresponda à assinatura do método genérico construído. Uma maneira fácil de fazer isso é criar um delegado genérico. O código a seguir cria uma instância do delegado genérico `D` definida no código de exemplo, usando a sobrecarga de método <xref:System.Delegate.CreateDelegate%28System.Type%2CSystem.Reflection.MethodInfo%29?displayProperty=nameWithType> e invoca o delegado. Os delegados funcionam melhor que as chamadas com associação tardia.

    [!code-csharp[GenericMethodHowTo#23](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#23)]
    [!code-vb[GenericMethodHowTo#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#23)]

4. O método emitido também pode ser chamado de um programa que referencia o assembly salvo.

## <a name="example"></a>Exemplo

O exemplo de código a seguir cria um tipo não genérico, `DemoType`, com um método genérico, `Factory`. Esse método tem dois parâmetros de tipo genérico, `TInput` para especificar um tipo de entrada e `TOutput` para especificar um tipo de saída. O parâmetro de tipo `TOutput` é restrito para implementar `ICollection<TInput>` (`ICollection(Of TInput)` no Visual Basic), para ser um tipo de referência e para ter um construtor sem parâmetros.

O método tem um parâmetro formal, que é uma matriz de `TInput`. O método retorna uma instância de `TOutput` que contém todos os elementos da matriz de entrada. `TOutput` pode ser qualquer tipo de coleção genérica que implementa a interface genérica <xref:System.Collections.Generic.ICollection%601>.

Quando o código é executado, o assembly dinâmico é salvo como DemoGenericMethod1.dll e pode ser examinado usando o [Ildasm.exe (IL Disassembler)](../tools/ildasm-exe-il-disassembler.md).

> [!NOTE]
> Uma boa maneira de aprender como emitir o código é escrever um programa Visual Basic, C# ou Visual C++ que executa a tarefa que você está tentando emitir e usa o desmontador para examinar o MSIL produzido pelo compilador.

O exemplo de código inclui o código-fonte que é equivalente ao método emitido. O método emitido é invocado com associação tardia e também usando um delegado genérico declarado no exemplo de código.

[!code-csharp[GenericMethodHowTo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/GenericMethodHowTo/CS/source.cs#1)]
[!code-vb[GenericMethodHowTo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GenericMethodHowTo/VB/source.vb#1)]

## <a name="see-also"></a>Veja também

- <xref:System.Reflection.Emit.MethodBuilder>
- [Como: Definir um tipo genérico com a emissão de reflexão](how-to-define-a-generic-type-with-reflection-emit.md)
