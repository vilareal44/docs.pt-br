---
title: 'Como: Conectar um delegado usando a reflexão'
description: Veja como conectar um delegado usando reflexão no .NET. Conecte um método existente a um evento obtendo os tipos necessários por meio de reflexão.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- events [.NET Framework], adding event handlers with reflection
- reflection, adding event-handler delegates
- delegates [.NET Framework], adding event handlers with reflection
ms.assetid: 076ee62d-a964-449e-a447-c31b33518b81
ms.openlocfilehash: b5d93efd278a53a4e6382f2321918e58ead55899
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865080"
---
# <a name="how-to-hook-up-a-delegate-using-reflection"></a>Como: Conectar um delegado usando a reflexão
Quando você usa a reflexão para carregar e executar assemblies, não pode usar recursos de linguagem como o operador `+=` do C# ou a [instrução AddHandler](../../visual-basic/language-reference/statements/addhandler-statement.md) do Visual Basic para conectar eventos. Os procedimentos a seguir mostram como conectar um método existente a um evento obtendo todos os tipos necessários por meio de reflexão e como criar um método dinâmico usando a emissão de reflexão e conectá-lo a um evento.  
  
> [!NOTE]
> Para encontrar outra maneira de conectar um delegado manipulador de evento, consulte o exemplo de código para o método <xref:System.Reflection.EventInfo.AddEventHandler%2A> da classe <xref:System.Reflection.EventInfo>.  
  
### <a name="to-hook-up-a-delegate-using-reflection"></a>Para conectar um delegado usando reflexão  
  
1. Carregue um assembly que contém o tipo que gera eventos. Os assemblies normalmente são carregados com o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>. Para manter esse exemplo simples, um formulário derivado no assembly atual é usado, portanto, o método <xref:System.Reflection.Assembly.GetExecutingAssembly%2A> é usado para carregar o assembly atual.  
  
     [!code-cpp[HookUpDelegate#3](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#3)]
     [!code-csharp[HookUpDelegate#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#3)]
     [!code-vb[HookUpDelegate#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#3)]  
  
2. Obtenha um objeto <xref:System.Type> que representa o tipo e crie uma instância do tipo. O método <xref:System.Activator.CreateInstance%28System.Type%29> é usado no código a seguir porque o formulário tem um construtor sem parâmetros. Há várias outras sobrecargas do método <xref:System.Activator.CreateInstance%2A> que poderão ser usadas se o tipo sendo criado não tiver um construtor sem parâmetros. A nova instância é armazenada como o tipo <xref:System.Object> para manter a ficção de que nada é conhecido sobre o assembly. (A reflexão permite que você obtenha os tipos em um assembly sem saber seus nomes com antecedência.)  
  
     [!code-cpp[HookUpDelegate#4](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#4)]
     [!code-csharp[HookUpDelegate#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#4)]
     [!code-vb[HookUpDelegate#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#4)]  
  
3. Obtenha um objeto <xref:System.Reflection.EventInfo> que representa o evento e use a propriedade <xref:System.Reflection.EventInfo.EventHandlerType%2A> para obter o tipo de delegado usado para manipular o evento. No código a seguir, é obtido um <xref:System.Reflection.EventInfo> para o evento <xref:System.Windows.Forms.Control.Click>.  
  
     [!code-cpp[HookUpDelegate#5](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#5)]
     [!code-csharp[HookUpDelegate#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#5)]
     [!code-vb[HookUpDelegate#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#5)]  
  
4. Obtenha um objeto <xref:System.Reflection.MethodInfo> que representa o método que manipula o evento. O código de programa completo na seção Exemplo posteriormente neste tópico contém um método que corresponde à assinatura do delegado <xref:System.EventHandler>, que manipula o evento <xref:System.Windows.Forms.Control.Click>, mas você também pode gerar métodos dinâmicos no tempo de execução. Para obter detalhes, consulte o procedimento que acompanha esse artigo para gerar um manipulador de eventos em tempo de execução usando um método dinâmico.  
  
     [!code-cpp[HookUpDelegate#6](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#6)]
     [!code-csharp[HookUpDelegate#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#6)]
     [!code-vb[HookUpDelegate#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#6)]  
  
5. Crie uma instância do delegado usando o método <xref:System.Delegate.CreateDelegate%2A>. Esse método é estático (`Shared` no Visual Basic), portanto, o tipo de delegado deve ser fornecido. O uso das sobrecargas de <xref:System.Delegate.CreateDelegate%2A> que utilizam um <xref:System.Reflection.MethodInfo> é recomendado.  
  
     [!code-cpp[HookUpDelegate#7](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#7)]
     [!code-csharp[HookUpDelegate#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#7)]
     [!code-vb[HookUpDelegate#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#7)]  
  
6. Obtenha o método do acessador `add` e invoque-o para conectar o evento. Todos os eventos têm um acessador `add` e um acessador `remove`, que estão ocultos pela sintaxe de linguagens de alto nível. Por exemplo, o C# usa o operador `+=` para conectar eventos e o Visual Basic usa a [instrução AddHandler](../../visual-basic/language-reference/statements/addhandler-statement.md). O código a seguir obtém o acessador `add` do evento <xref:System.Windows.Forms.Control.Click> e o invoca com associação tardia, passando a instância do delegado. Os argumentos devem ser passados como uma matriz.  
  
     [!code-cpp[HookUpDelegate#8](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#8)]
     [!code-csharp[HookUpDelegate#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#8)]
     [!code-vb[HookUpDelegate#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#8)]  
  
7. Teste o evento. O código a seguir mostra o formulário definido no exemplo de código. Clicar no formulário invoca o manipulador de eventos.  
  
     [!code-cpp[HookUpDelegate#12](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#12)]
     [!code-csharp[HookUpDelegate#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#12)]
     [!code-vb[HookUpDelegate#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#12)]  
  
<a name="procedureSection1"></a>
### <a name="to-generate-an-event-handler-at-run-time-by-using-a-dynamic-method"></a>Para gerar um manipulador de eventos em tempo de execução usando um método dinâmico  
  
1. Os métodos do manipulador de eventos podem ser gerados em tempo de execução, usando métodos dinâmicos leves e a emissão de reflexão. Para criar um manipulador de eventos, são necessários o tipo de retorno e os tipos de parâmetro do delegado. Eles podem ser obtidos examinando o método `Invoke` do delegado. O código a seguir usa os métodos `GetDelegateReturnType` e `GetDelegateParameterTypes` para obter essas informações. O código para esses métodos pode ser encontrado na seção Exemplo neste tópico.  
  
     Não é necessário nomear um <xref:System.Reflection.Emit.DynamicMethod>, portanto, a cadeia de caracteres vazia pode ser usada. No código a seguir, o último argumento associa o método dinâmico ao tipo atual, concedendo ao delegado o acesso a todos os membros públicos e privados da classe `Example`.  
  
     [!code-cpp[HookUpDelegate#9](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#9)]
     [!code-csharp[HookUpDelegate#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#9)]
     [!code-vb[HookUpDelegate#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#9)]  
  
2. Gere um corpo de método. Esse método carrega uma cadeia de caracteres, chama a sobrecarga do método <xref:System.Windows.Forms.MessageBox.Show%2A?displayProperty=nameWithType> que usa uma cadeia de caracteres, exibe o valor retornado da pilha (porque o manipulador não tem nenhum tipo de retorno) e retorna. Para saber mais sobre a emissão de métodos dinâmicos, consulte [Como definir e executar métodos dinâmicos](how-to-define-and-execute-dynamic-methods.md).  
  
     [!code-cpp[HookUpDelegate#10](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#10)]
     [!code-csharp[HookUpDelegate#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#10)]
     [!code-vb[HookUpDelegate#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#10)]  
  
3. Conclua o método dinâmico chamando seu método <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A>. Use o acessador `add` para adicionar o delegado à lista de invocação para o evento.  
  
     [!code-cpp[HookUpDelegate#11](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#11)]
     [!code-csharp[HookUpDelegate#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#11)]
     [!code-vb[HookUpDelegate#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#11)]  
  
4. Teste o evento. O código a seguir carrega o formulário definido no exemplo de código. Clicar no formulário invoca o manipulador de eventos predefinido e o manipulador de eventos emitido.  
  
     [!code-cpp[HookUpDelegate#12](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#12)]
     [!code-csharp[HookUpDelegate#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#12)]
     [!code-vb[HookUpDelegate#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#12)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra como conectar um método existente a um evento usando a reflexão e também como usar a classe <xref:System.Reflection.Emit.DynamicMethod> para emitir um método em tempo de execução e conectá-lo a um evento.  
  
 [!code-cpp[HookUpDelegate#1](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#1)]
 [!code-csharp[HookUpDelegate#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#1)]
 [!code-vb[HookUpDelegate#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#1)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Emit.DynamicMethod>
- <xref:System.Activator.CreateInstance%2A>
- <xref:System.Delegate.CreateDelegate%2A>
- [Como: Definir e executar métodos dinâmicos](how-to-define-and-execute-dynamic-methods.md)
- [Reflexão](reflection.md)
