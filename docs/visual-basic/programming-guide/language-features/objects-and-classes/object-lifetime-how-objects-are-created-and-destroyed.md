---
title: 'Tempo de vida do objeto: como os objetos são criados e destruídos'
ms.date: 07/20/2015
f1_keywords:
- vb.Constructor
helpviewer_keywords:
- destructors, object lifetime
- Sub Finalize destructor
- objects [Visual Basic], destroying
- lifetime [Visual Basic], objects
- Sub New constructor, object lifetime
- Finalize method [Visual Basic], object lifetime
- objects [Visual Basic], creating
- Class_Terminate
- Dispose method [Visual Basic], object lifetime
- Class_Initialize
- object creation [Visual Basic], object lifetime
- parameterized constructors
- objects [Visual Basic], lifetime
- objects [Visual Basic], garbage collection
- constructors [Visual Basic], object lifetime
- Sub Dispose destructor
- garbage collection [Visual Basic], Visual Basic
ms.assetid: f1ee8458-b156-44e0-9a8a-5dd171648cd8
ms.openlocfilehash: e81e131933e0997756ed4185a3ceb12ad19b78de
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392877"
---
# <a name="object-lifetime-how-objects-are-created-and-destroyed-visual-basic"></a>Tempo de vida do objeto: como os objetos são criados e destruídos (Visual Basic)

Uma instância de uma classe, um objeto, é criada usando a palavra-chave `New`. Tarefas de inicialização geralmente devem ser executadas em novos objetos antes de serem usadas. Tarefas comuns de inicialização incluem abrir arquivos, conectar-se aos bancos de dados e ler os valores das chaves do registro. Visual Basic controla a inicialização de novos objetos usando procedimentos chamados *construtores* (métodos especiais que permitem o controle sobre a inicialização).

Depois que um objeto sai do escopo, ele é liberado pelo common language runtime (CLR). Visual Basic controla a versão dos recursos do sistema usando procedimentos chamados *destruidores*. Juntos, os construtores e os destruidores oferecem suporte à criação de bibliotecas de classe robusta e previsível.

## <a name="using-constructors-and-destructors"></a>Usando construtores e destruidores

Os construtores e os destruidores controlam a criação e a destruição de objetos. Os `Sub New` `Sub Finalize` procedimentos e em Visual Basic inicializar e destruir objetos; eles substituem `Class_Initialize` os `Class_Terminate` métodos e usados em Visual Basic 6,0 e versões anteriores.

### <a name="sub-new"></a>Sub New

O construtor `Sub New` pode ser executado apenas uma vez quando uma classe é criada. Não pode ser chamado explicitamente em qualquer lugar que não seja na primeira linha do código de outro construtor da mesma classe ou de uma classe derivada. Além disso, o código no método `Sub New` sempre é executado antes de qualquer outro código em uma classe. Visual Basic cria implicitamente um `Sub New` Construtor em tempo de execução se você não definir explicitamente um `Sub New` procedimento para uma classe.

Para criar um construtor para uma classe, crie um procedimento denominado `Sub New` em qualquer lugar na definição de classe. Para criar um construtor com parâmetros, especifique os nomes e tipos de dados dos argumentos para `Sub New` exatamente como você faria para qualquer outro procedimento, como no seguinte código:

[!code-vb[VbVbalrOOP#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/WhidbeyStuff.vb#42)]

Os construtores são frequentemente sobrecarregados, como no seguinte código:

[!code-vb[VbVbalrOOP#116](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/WhidbeyStuff.vb#116)]

Quando você define uma classe derivada de outra classe, a primeira linha do construtor deve ser uma chamada para o construtor da classe base, a menos que a classe base tenha um construtor acessível que não utiliza parâmetros. Uma chamada para a classe base que contém o construtor acima, por exemplo, seria `MyBase.New(s)`. Caso contrário, `MyBase.New` é opcional e o tempo de execução de Visual Basic o chama implicitamente.

Depois de escrever o código para chamar o construtor do objeto pai, você pode adicionar qualquer código de inicialização adicional para o procedimento `Sub New`. `Sub New` pode aceitar argumentos quando chamado como um construtor com parâmetros. Esses parâmetros são passados do procedimento que está chamando o construtor, por exemplo, `Dim AnObject As New ThisClass(X)`.

### <a name="sub-finalize"></a>Sub Finalize

Antes da liberação de objetos, o CLR chama automaticamente o método `Finalize` para objetos que definem um procedimento `Sub Finalize`. O método `Finalize` pode conter códigos que precisam ser executados antes que um objeto seja destruído, como o código para fechar arquivos e salvar as informações de estado. Há uma pequena perda de desempenho para executar `Sub Finalize`, portanto, você deve definir um método `Sub Finalize` somente quando precisar liberar objetos explicitamente.

> [!NOTE]
> O coletor de lixo no CLR não (e não pode) descartar *objetos não gerenciados*, objetos que o sistema operacional executa diretamente, fora do ambiente CLR. Isso ocorre porque diferentes objetos não gerenciados devem ser descartados de maneiras diferentes. Essas informações não estão diretamente associadas ao objeto não gerenciado; devem ser encontradas na documentação do objeto. Uma classe que usa objetos não gerenciados deve descartá-los em seu método `Finalize`.

O destruidor `Finalize` é um método protegido que pode ser chamado somente da classe à qual pertence ou de classes derivadas. O sistema automaticamente chama `Finalize` quando um objeto é destruído, assim, você não deve chamar explicitamente o `Finalize` de fora de uma implementação `Finalize` de classe derivada.

Diferentemente de `Class_Terminate`, que é executado assim que um objeto é definido como nada, normalmente existe um atraso entre o momento em que um objeto perde o escopo e o momento em que o Visual Basic chama o destruidor `Finalize`. Visual Basic .NET permite um segundo tipo de destruidor, <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> , que pode ser chamado explicitamente a qualquer momento para liberar recursos imediatamente.

> [!NOTE]
> Um destruidor `Finalize` não deve lançar exceções, já que elas não podem ser tratadas pelo aplicativo e podem fazer com que o aplicativo seja finalizado.

### <a name="how-new-and-finalize-methods-work-in-a-class-hierarchy"></a>Como os métodos novos e de finalização funcionam em uma hierarquia de classe

Sempre que uma instância de uma classe é criada, o common language runtime (CLR) tenta executar um procedimento denominado `New`, se ele existir nesse objeto. `New` é um tipo de procedimento denominado `constructor` que é usado para inicializar novos objetos antes que qualquer outro código em um objeto seja executado. Um construtor `New` pode ser usado para abrir arquivos, conectar-se aos bancos de dados, inicializar variáveis e cuidar de quaisquer outras tarefas que precisam ser feitas antes que um objeto pode ser usado.

Quando uma instância de uma classe derivada é criada, o construtor `Sub New` da classe base é executado primeiro, seguido pelos construtores em classes derivadas. Isso acontece porque a primeira linha do código em um construtor `Sub New` usa a sintaxe `MyBase.New()` para chamar o construtor da classe imediatamente acima de si próprio na hierarquia de classe. O construtor `Sub New` é chamado para cada classe na hierarquia de classe, até que o construtor de classe base seja atingido. Nesse ponto, o código no construtor para a classe base é executado, seguido pelo código em cada construtor em todas as classes derivadas e o código na maioria das classes derivadas é executado por último.

![Captura de tela mostrando construtores e herança da hierarquia de classes.](./media/object-lifetime-how-objects-are-created-and-destroyed/subnew-constructor-inheritance.gif)

Quando um objeto não é mais necessário, o CLR chama o método <xref:System.Object.Finalize%2A> para esse objeto antes de liberar sua memória. O método <xref:System.Object.Finalize%2A> é denominado de `destructor` porque executa tarefas de limpeza, como salvar informações do estado, fechar arquivos e conexões ao bancos de dados e outras tarefas que devem ser executadas antes de liberar o objeto.

![Captura de tela mostrando o destruidor de método Finalize.](./media/object-lifetime-how-objects-are-created-and-destroyed/finalize-method-destructor.gif)

## <a name="idisposable-interface"></a>Interface IDisposable

Instâncias de classe geralmente controlam os recursos não gerenciados pelo CLR, assim como identificadores do Windows e conexões de banco de dados. Esses recursos devem ser descartados no método `Finalize` da classe, para que eles sejam liberados quando o objeto for destruído pelo coletor de lixo. No entanto, o coletor de lixo destrói objetos somente quando o CLR exige mais memória livre. Isso significa que os recursos podem não ser liberados até muito tempo depois que o objeto sair do escopo.

Para complementar a coleta de lixo, suas classes podem fornecer um mecanismo para gerenciar ativamente os recursos do sistema se eles implementarem a interface <xref:System.IDisposable>. <xref:System.IDisposable> possui um método, <xref:System.IDisposable.Dispose%2A>, o qual os clientes devem chamar ao concluir a utilização de um objeto. Você pode usar o método <xref:System.IDisposable.Dispose%2A> para liberar recursos imediatamente e executar tarefas como fechar arquivos e conexões de banco de dados. Diferentemente do destruidor `Finalize`, o método <xref:System.IDisposable.Dispose%2A> não é chamado automaticamente. Os clientes de uma classe devem chamar explicitamente o <xref:System.IDisposable.Dispose%2A> quando você desejar liberar recursos imediatamente.

### <a name="implementing-idisposable"></a>Implementando IDisposable

Uma classe que implementa a interface <xref:System.IDisposable> deve incluir essas seções de código:

- Um campo para controlar se o objeto foi descartado:

  ```vb
  Protected disposed As Boolean = False
  ```

- Uma sobrecarga de <xref:System.IDisposable.Dispose%2A> que libera recursos da classe. Esse método deve ser chamado pelos métodos <xref:System.IDisposable.Dispose%2A> e `Finalize` da classe base:

  ```vb
  Protected Overridable Sub Dispose(ByVal disposing As Boolean)
      If Not Me.disposed Then
          If disposing Then
              ' Insert code to free managed resources.
          End If
          ' Insert code to free unmanaged resources.
      End If
      Me.disposed = True
  End Sub
  ```

- Uma implementação de <xref:System.IDisposable.Dispose%2A> que contém apenas o código a seguir:

  ```vb
  Public Sub Dispose() Implements IDisposable.Dispose
      Dispose(True)
      GC.SuppressFinalize(Me)
  End Sub
  ```

- Uma substituição do método `Finalize` que contém apenas o código a seguir:

  ```vb
  Protected Overrides Sub Finalize()
      Dispose(False)
      MyBase.Finalize()
  End Sub
  ```

### <a name="deriving-from-a-class-that-implements-idisposable"></a>Derivação de uma classe que implementa IDisposable

Uma classe que é derivada de uma classe base que implementa a interface <xref:System.IDisposable> não precisa substituir nenhum dos métodos base, a menos que ele utilize recursos adicionais que precisam ser descartados. Nessa situação, a classe derivada deve substituir o método `Dispose(disposing)` de classe base para descartar os recursos da classe derivada. Essa substituição deve chamar o método `Dispose(disposing)` da classe base.

```vb
Protected Overrides Sub Dispose(ByVal disposing As Boolean)
    If Not Me.disposed Then
        If disposing Then
            ' Insert code to free managed resources.
        End If
        ' Insert code to free unmanaged resources.
    End If
    MyBase.Dispose(disposing)
End Sub
```

Uma classe derivada não deve substituir os métodos <xref:System.IDisposable.Dispose%2A> e `Finalize` da classe base. Quando esses métodos são chamados de uma instância da classe derivada, a implementação da classe base desses métodos chamam a substituição da classe derivada do método `Dispose(disposing)`.

## <a name="garbage-collection-and-the-finalize-destructor"></a>A coleta de lixo e o destruidor de finalização

O .NET Framework usa o sistema de *coleta de lixo de rastreamento de referência* para liberar periodicamente recursos não utilizados. Visual Basic 6,0 e versões anteriores usaram um sistema diferente chamado *contagem de referência* para gerenciar recursos. Embora os dois sistemas executem a mesma função automaticamente, existem algumas diferenças importantes.

O CLR destrói periodicamente objetos quando o sistema determina que esses objetos não são mais necessários. Os objetos são liberados mais rapidamente quando os recursos de sistema estão em fornecimento pequeno e, em caso contrário, menos frequente. O atraso entre quando um objeto perde o escopo e quando o CLR libera, significa que, diferente dos objetos no Visual Basic 6.0 e versões anteriores, você não pode determinar exatamente quando o objeto será destruído. Nessa situação, os objetos são considerados um tempo de *vida não determinístico*. Na maioria dos casos, o tempo de vida não determinístico não altera o modo como você grava aplicativos, desde que você se lembre de que o destruidor `Finalize` pode não ser executado imediatamente quando um objeto perde o escopo.

Outra diferença entre os sistemas de coleta de lixo envolve o uso de `Nothing`. Para aproveitar a contagem de referência no Visual Basic 6.0 e em versões anteriores, os programadores às vezes atribuem `Nothing` para as variáveis de objeto para liberar as referências que essas variáveis mantinham. Se a variável mantiver a última referência ao objeto, os recursos do objeto são lançados imediatamente. Em versões posteriores do Visual Basic, embora haja casos em que esse procedimento ainda é importante, executá-lo nunca faz com que o objeto referenciado libere seus recursos imediatamente. Para liberar recursos imediatamente, use o método <xref:System.IDisposable.Dispose%2A> do objeto, se disponível. A única vez em que você deve definir uma variável para `Nothing` é quando seu tempo de vida é longo em relação ao tempo que o coletor de lixo leva para detectar objetos órfãos.

## <a name="see-also"></a>Confira também

- <xref:System.IDisposable.Dispose%2A>
- [Inicialização e término de componentes](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/ws9dc6t6(v=vs.120))
- [Novo operador](../../../language-reference/operators/new-operator.md)
- [Limpando recursos não gerenciados](../../../../standard/garbage-collection/unmanaged.md)
- [Nada](../../../language-reference/nothing.md)
