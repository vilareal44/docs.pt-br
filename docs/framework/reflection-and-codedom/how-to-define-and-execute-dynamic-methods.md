---
title: 'Como: Definir e executar métodos dinâmicos'
description: Consulte como definir e executar métodos dinâmicos no .NET. Exibir exemplos de um método dinâmico simples e um método dinâmico associado a uma instância de uma classe.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reflection emit, dynamic methods
- dynamic methods
ms.assetid: 07d08a99-62c5-4254-bce2-2a75e55a18ab
ms.openlocfilehash: 7c68be91deb59ea9439e81561f50b7cc40766a45
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865106"
---
# <a name="how-to-define-and-execute-dynamic-methods"></a>Como: Definir e executar métodos dinâmicos
Os procedimentos a seguir mostram como definir e executar um método dinâmico simples e um método dinâmico ligado a uma instância de uma classe. Para obter mais informações sobre métodos dinâmicos, consulte a classe <xref:System.Reflection.Emit.DynamicMethod> e [Cenários de métodos dinâmicos para a emissão de reflexão](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/sfk2s47t(v=vs.100)).  
  
### <a name="to-define-and-execute-a-dynamic-method"></a>Para definir e executar um método dinâmico  
  
1. Declare um tipo de delegado para executar o método. Considere usar um delegado genérico para minimizar o número de tipos de delegado que você precisa declarar. O código a seguir declara dois tipos de delegado que podem ser usados para o método `SquareIt` e um deles é genérico.  
  
     [!code-cpp[DynamicMethodHowTo#2](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#2)]
     [!code-csharp[DynamicMethodHowTo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#2)]
     [!code-vb[DynamicMethodHowTo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#2)]  
  
2. Crie uma matriz que especifica os tipos de parâmetro para o método dinâmico. Neste exemplo, o único parâmetro é um `int` (`Integer` no Visual Basic), portanto, a matriz tem apenas um elemento.  
  
     [!code-cpp[DynamicMethodHowTo#3](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#3)]
     [!code-csharp[DynamicMethodHowTo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#3)]
     [!code-vb[DynamicMethodHowTo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#3)]  
  
3. Crie um <xref:System.Reflection.Emit.DynamicMethod>. Neste exemplo, o método é chamado `SquareIt`.  
  
    > [!NOTE]
    > Não é necessário atribuir nomes aos métodos dinâmicos e eles não podem ser invocados por nome. Vários métodos dinâmicos podem ter o mesmo nome. No entanto, o nome aparece em pilhas de chamadas e pode ser útil para depuração.  
  
     O tipo do valor retornado é especificado como `long`. O método está associado com o módulo que contém a classe `Example`, que contém o código de exemplo. Qualquer módulo carregado pode ser especificado. O método dinâmico age como um método `static` de nível de módulo (`Shared` no Visual Basic).  
  
     [!code-cpp[DynamicMethodHowTo#4](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#4)]
     [!code-csharp[DynamicMethodHowTo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#4)]
     [!code-vb[DynamicMethodHowTo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#4)]  
  
4. Emita o corpo do método. Neste exemplo, um objeto <xref:System.Reflection.Emit.ILGenerator> é usado para emitir o MSIL (Microsoft Intermediate Language). Como alternativa, um objeto <xref:System.Reflection.Emit.DynamicILInfo> pode ser usado em conjunto com geradores de código não gerenciado para emitir o corpo do método para um <xref:System.Reflection.Emit.DynamicMethod>.  
  
     O MSIL neste exemplo carrega o argumento, que é um `int`, para a pilha, converte-o em um `long`, duplica o `long` e multiplica os dois números. Isso deixa o resultado ao quadrado na pilha e tudo o que o método precisa fazer é retornar.  
  
     [!code-cpp[DynamicMethodHowTo#5](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#5)]
     [!code-csharp[DynamicMethodHowTo#5](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#5)]
     [!code-vb[DynamicMethodHowTo#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#5)]  
  
5. Crie uma instância do delegado (declarado na etapa 1) que representa o método dinâmico chamando o método <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A>. Criar o delegado conclui o método e quaisquer tentativas adicionais de alterar o método — por exemplo, adicionando mais MSIL — são ignoradas. O código a seguir cria o delegado e o invoca, usando um delegado genérico.  
  
     [!code-cpp[DynamicMethodHowTo#6](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#6)]
     [!code-csharp[DynamicMethodHowTo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#6)]
     [!code-vb[DynamicMethodHowTo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#6)]  
  
### <a name="to-define-and-execute-a-dynamic-method-that-is-bound-to-an-object"></a>Para definir e executar um método dinâmico associado a um objeto  
  
1. Declare um tipo de delegado para executar o método. Considere usar um delegado genérico para minimizar o número de tipos de delegado que você precisa declarar. O código a seguir declara um tipo de delegado genérico que pode ser usado para executar qualquer método com um parâmetro e um valor retornado ou um método com dois parâmetros e um valor retornado se o delegado estiver associado a um objeto.  
  
     [!code-cpp[DynamicMethodHowTo#12](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#12)]
     [!code-csharp[DynamicMethodHowTo#12](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#12)]
     [!code-vb[DynamicMethodHowTo#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#12)]  
  
2. Crie uma matriz que especifica os tipos de parâmetro para o método dinâmico. Se o delegado que representa o método deve ser associado a um objeto, o primeiro parâmetro deve corresponder ao tipo ao qual o delegado está associado. Neste exemplo, há dois parâmetros, do tipo `Example` e do tipo `int` (`Integer` no Visual Basic).  
  
     [!code-cpp[DynamicMethodHowTo#13](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#13)]
     [!code-csharp[DynamicMethodHowTo#13](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#13)]
     [!code-vb[DynamicMethodHowTo#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#13)]  
  
3. Crie um <xref:System.Reflection.Emit.DynamicMethod>. Neste exemplo, o método não tem nenhum nome. O tipo do valor retornado é especificado como `int` (`Integer` no Visual Basic). O método tem acesso aos membros particulares e protegidos da classe `Example`.  
  
     [!code-cpp[DynamicMethodHowTo#14](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#14)]
     [!code-csharp[DynamicMethodHowTo#14](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#14)]
     [!code-vb[DynamicMethodHowTo#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#14)]  
  
4. Emita o corpo do método. Neste exemplo, um objeto <xref:System.Reflection.Emit.ILGenerator> é usado para emitir o MSIL (Microsoft Intermediate Language). Como alternativa, um objeto <xref:System.Reflection.Emit.DynamicILInfo> pode ser usado em conjunto com geradores de código não gerenciado para emitir o corpo do método para um <xref:System.Reflection.Emit.DynamicMethod>.  
  
     O MSIL neste exemplo carrega o primeiro argumento, que é uma instância da classe `Example` e o usa para carregar o valor de um campo de instância privada do tipo `int`. O segundo argumento é carregado e os dois números são multiplicados. Se o resultado for maior do que `int`, o valor será truncado e os bits mais significativos serão descartados. O método retorna, com o valor retornado na pilha.  
  
     [!code-cpp[DynamicMethodHowTo#15](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#15)]
     [!code-csharp[DynamicMethodHowTo#15](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#15)]
     [!code-vb[DynamicMethodHowTo#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#15)]  
  
5. Crie uma instância do delegado (declarado na etapa 1) que representa o método dinâmico chamando a sobrecarga do método <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%28System.Type%2CSystem.Object%29>. Criar o delegado conclui o método e quaisquer tentativas adicionais de alterar o método — por exemplo, adicionando mais MSIL — são ignoradas.  
  
    > [!NOTE]
    > Você pode chamar o método <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A> várias vezes para criar delegados associados a outras instâncias do tipo de destino.  
  
     O código a seguir associa o método a uma nova instância da classe `Example` cujo campo de teste privado está definido como 42. Isto é, cada vez que o delegado é invocado, a instância de `Example` é passada para o primeiro parâmetro do método.  
  
     O delegado `OneParameter` é usado porque o primeiro parâmetro do método sempre recebe a instância do `Example`. Quando o delegado é invocado, apenas o segundo parâmetro é necessário.  
  
     [!code-cpp[DynamicMethodHowTo#16](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#16)]
     [!code-csharp[DynamicMethodHowTo#16](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#16)]
     [!code-vb[DynamicMethodHowTo#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#16)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra um método dinâmico simples e um método dinâmico associado a uma instância de uma classe.  
  
 O método dinâmico simples tem um argumento, um inteiro de 32 bits e retorna o quadrado de 64 bits desse inteiro. Um delegado genérico é usado para invocar o método.  
  
 O segundo método dinâmico tem dois parâmetros de tipo `Example` e de tipo `int` (`Integer` no Visual Basic). Quando o método dinâmico tiver sido criado, ele será associado a uma instância de `Example`, usando um delegado genérico que tem um argumento do tipo `int`. O delegado não tem um argumento do tipo `Example` porque o primeiro parâmetro do método sempre recebe a instância associada do `Example`. Quando o delegado é invocado, somente o argumento `int` é fornecido. Esse método dinâmico acessa um campo particular da classe `Example` e retorna o produto do campo particular e o argumento `int`.  
  
 O exemplo de código define delegados que podem ser usados para executar os métodos.  
  
 [!code-cpp[DynamicMethodHowTo#1](../../../samples/snippets/cpp/VS_Snippets_CLR/DynamicMethodHowTo/cpp/source.cpp#1)]
 [!code-csharp[DynamicMethodHowTo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/DynamicMethodHowTo/cs/source.cs#1)]
 [!code-vb[DynamicMethodHowTo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DynamicMethodHowTo/vb/source.vb#1)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Reflection.Emit.DynamicMethod>
- [Usando a emissão de reflexão](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y322t50(v=vs.100))
- [Cenários de métodos dinâmicos para a emissão de reflexão](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/sfk2s47t(v=vs.100))
