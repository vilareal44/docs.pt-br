---
title: Inicialização lenta
description: Explore a inicialização lenta no .NET, uma melhoria de desempenho que significa que a criação de um objeto é adiada até que o objeto seja usado pela primeira vez.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- lazy initialization in .NET, introduction
ms.assetid: 56b4ae5c-4745-44ff-ad78-ffe4fcde6b9b
ms.openlocfilehash: 355fa326fc19e9a50a74e21ace0a6353f5c740c5
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904228"
---
# <a name="lazy-initialization"></a>Inicialização lenta
*Inicialização lenta* de um objeto significa que a criação dele é adiada até que ele seja usado pela primeira vez. (Para este tópico, os termos *inicialização lenta* e *instanciação lenta* são sinônimos.) A inicialização lenta é usada principalmente para melhorar o desempenho, evitar o desperdício de computação e reduzir os requisitos de memória do programa. Estes são os cenários mais comuns:  
  
- Quando você tem um objeto de criação dispendiosa e o programa pode não vir a usá-lo. Por exemplo, suponha que você tenha na memória um objeto `Customer` que tem uma propriedade `Orders` contendo uma grande variedade de objetos `Order` que, ao serem inicializados, requerem uma conexão de banco de dados. Se o usuário nunca solicita a exibição das Ordens nem usa os dados em uma computação, não há motivo para usar a memória do sistema ou ciclos de computação para criá-lo. Ao usar `Lazy<Orders>` para declarar o objeto `Orders` para inicialização lenta, você pode evitar desperdício de recursos de sistema quando o objeto não é usado.  
  
- Quando você tem um objeto cuja criação é dispendiosa e você deseja adiar a criação até após a conclusão de outras operações dispendiosas. Por exemplo, suponha que seu programa carrega várias instâncias de objeto quando ele é iniciado, mas apenas algumas delas são necessárias imediatamente. Você pode melhorar o desempenho de inicialização do programa, adiando a inicialização dos objetos que não são necessários até que os objetos necessários tenham sido criados.  
  
 Embora você possa escrever seu próprio código para executar a inicialização lenta, recomendamos que você use <xref:System.Lazy%601> em vez disso. <xref:System.Lazy%601> e seus tipos relacionados também dão suporte a segurança do thread e fornecem uma política de propagação de exceção consistente.  
  
 A tabela a seguir lista os tipos que o .NET Framework versão 4 fornece para permitir a inicialização lenta em cenários diferentes.  
  
|Type|Description|  
|----------|-----------------|  
|<xref:System.Lazy%601>|Uma classe wrapper que fornece a semântica de inicialização lenta para qualquer biblioteca de classes ou tipo definido pelo usuário.|  
|<xref:System.Threading.ThreadLocal%601>|Ela é semelhante a <xref:System.Lazy%601>, exceto pelo fato de que ela fornece a semântica de inicialização lenta por local de thread. Cada thread tem acesso ao seu próprio valor exclusivo.|  
|<xref:System.Threading.LazyInitializer>|Fornece métodos `static` avançados (`Shared` no Visual Basic) para inicialização lenta de objetos sem a sobrecarga de uma classe.|  
  
## <a name="basic-lazy-initialization"></a>Inicialização lenta básica  
 Para definir um tipo de inicialização lenta, por exemplo, `MyType`, use `Lazy<MyType>` (`Lazy(Of MyType)` no Visual Basic), conforme mostrado no exemplo a seguir. Se nenhum delegado é passado ao construtor <xref:System.Lazy%601>, o tipo encapsulado é criado usando <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> quando a propriedade de valor é acessada pela primeira vez. Se o tipo não tiver um construtor sem parâmetros, uma exceção de tempo de execução será lançada.  
  
 No exemplo a seguir, suponha que `Orders` é uma classe que contém uma matriz de objetos `Order` recuperados de um banco de dados. Um objeto `Customer` contém uma instância de `Orders`, mas, dependendo de ações do usuário, os dados do objeto `Orders` talvez não sejam necessário.  
  
 [!code-csharp[Lazy#1](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#1)]
 [!code-vb[Lazy#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#1)]  
  
 Você também pode passar um delegado no construtor <xref:System.Lazy%601> que invoca uma sobrecarga de construtor específica no tipo encapsulado no momento da criação e pode executar outras etapas de inicialização necessárias, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[Lazy#2](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#2)]
 [!code-vb[Lazy#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#2)]  
  
 Depois que o objeto lento é criado, nenhuma instância de `Orders` é criada até a propriedade <xref:System.Lazy%601.Value%2A> da variável lenta ser acessada pela primeira vez. No primeiro acesso, o tipo encapsulado é criado e retornado e então é armazenado para eventuais acessos futuros.  
  
 [!code-csharp[Lazy#3](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#3)]
 [!code-vb[Lazy#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#3)]  
  
 Um objeto <xref:System.Lazy%601> sempre retorna o mesmo objeto ou valor com o qual ele foi inicializado. Portanto, a propriedade <xref:System.Lazy%601.Value%2A> é somente leitura. Se <xref:System.Lazy%601.Value%2A> armazena um tipo de referência, você não pode atribuir um novo objeto a ela. (No entanto, você pode alterar o valor de seus campos públicos e propriedades de tabela.) Se <xref:System.Lazy%601.Value%2A> o armazenar um tipo de valor, você não poderá modificar seu valor. No entanto, você pode criar uma nova variável, invocando o construtor de variável novamente usando novos argumentos.  
  
 [!code-csharp[Lazy#4](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#4)]
 [!code-vb[Lazy#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#4)]  
  
 A nova instância lenta, semelhante à anterior, não instanciará `Orders` até que propriedade <xref:System.Lazy%601.Value%2A> dele seja acessada pela primeira vez.  
  
### <a name="thread-safe-initialization"></a>Inicialização thread-safe  
 Por padrão, objetos <xref:System.Lazy%601> são thread-safe. Ou seja, se o construtor não especificar o tipo de acesso thread-safe, os objetos <xref:System.Lazy%601> que ele criar serão thread-safe. Em cenários com vários threads, o primeiro thread para acessar a propriedade <xref:System.Lazy%601.Value%2A> de um objeto <xref:System.Lazy%601> thread-safe inicializa-o para todos os acessos subsequentes em todos os threads e todos os threads compartilham os mesmos dados. Portanto, não importa qual thread inicializa o objeto e as condições de corrida são benignas.  
  
> [!NOTE]
> Você pode estender essa consistência às condições de erro usando o cache de exceções. Para obter mais informações, consulte a próxima seção, [Exceções em objetos lentos](lazy-initialization.md#ExceptionsInLazyObjects).  
  
 O exemplo a seguir mostra que a mesma instância `Lazy<int>` tem o mesmo valor para três threads separados.  
  
 [!code-csharp[Lazy#8](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#8)]
 [!code-vb[Lazy#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#8)]  
  
 Se você precisar de dados separados em cada thread, use o tipo <xref:System.Threading.ThreadLocal%601>, conforme descrito posteriormente neste tópico.  
  
 Alguns construtores <xref:System.Lazy%601> têm um parâmetro booliano denominado `isThreadSafe` que é usado para especificar se a propriedade <xref:System.Lazy%601.Value%2A> será acessada de vários threads. Se você pretende acessar a propriedade de apenas um thread, passe `false` para obter uma discreta melhora no desempenho. Se você pretende acessar a propriedade de vários threads, passe `true` para instruir a instância <xref:System.Lazy%601> para lidar corretamente com condições de corrida em que um thread gera uma exceção em tempo de inicialização.  
  
 Alguns construtores <xref:System.Lazy%601> têm um parâmetro <xref:System.Threading.LazyThreadSafetyMode> chamado `mode`. Esses construtores fornecem um modo adicional de acesso thread-safe. A tabela a seguir mostra como o acesso thread-safe de um objeto <xref:System.Lazy%601> é afetado pelos parâmetros de construtor que especificam o acesso thread-safe. Cada construtor tem no máximo um desses parâmetros.  
  
|Acesso thread-safe do objeto|`LazyThreadSafetyMode``mode`parâmetro do|Parâmetro `isThreadSafe` booliano|Sem parâmetros de acesso thread-safe|  
|---------------------------------|---------------------------------------------|--------------------------------------|---------------------------------|  
|Totalmente thread-safe; apenas um thread por vez tenta inicializar o valor.|<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>|`true`|Sim.|  
|Não é thread-safe.|<xref:System.Threading.LazyThreadSafetyMode.None>|`false`|Não aplicável.|  
|Totalmente thread-safe; os threads fazem corrida para inicializar o valor.|<xref:System.Threading.LazyThreadSafetyMode.PublicationOnly>|Não aplicável.|Não aplicável.|  
  
 Conforme demonstrado na tabela, especificar <xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication?displayProperty=nameWithType> para o parâmetro `mode` é o mesmo que especificar `true` para o parâmetro `isThreadSafe` e especificar <xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType> é o mesmo que especificar `false`.  
  
 Especificar <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly?displayProperty=nameWithType> permite que vários threads tentar inicializar a instância <xref:System.Lazy%601>. Apenas um thread pode vencer essa corrida e todos os outros threads recebem o valor que foi inicializado pelo thread bem-sucedido. Se uma exceção for lançada em um thread durante a inicialização, esse thread não receberá o valor definido pelo thread bem-sucedido. Exceções não são armazenadas em cache, portanto, uma tentativa subsequente de acessar a propriedade <xref:System.Lazy%601.Value%2A> pode resultar em inicialização bem-sucedida. Isso é diferente da maneira como exceções são tratadas em outros modos, o que é descrito na seção a seguir. Para obter mais informações, consulte a enumeração <xref:System.Threading.LazyThreadSafetyMode>.  
  
<a name="ExceptionsInLazyObjects"></a>
## <a name="exceptions-in-lazy-objects"></a>Exceções em objetos lentos  
 Conforme mencionado anteriormente, um objeto <xref:System.Lazy%601> sempre retorna o mesmo objeto ou valor com o qual ele foi inicializado e, portanto, a propriedade <xref:System.Lazy%601.Value%2A> é somente leitura. Se você habilitar o cache de exceção, essa imutabilidade também se estenderá ao comportamento de exceção. Se um objeto inicializado por Lazy tiver o cache de exceção habilitado e lançar uma exceção a partir de seu método de inicialização quando a <xref:System.Lazy%601.Value%2A> propriedade for acessada pela primeira vez, essa mesma exceção será lançada em todas as tentativas subsequentes de acessar a <xref:System.Lazy%601.Value%2A> propriedade. Em outras palavras, o construtor do tipo encapsulado nunca será invocado novamente, mesmo em cenários com vários threads. Portanto, o objeto <xref:System.Lazy%601> não é capaz de gerar uma exceção em um acesso e retorna um valor em um acesso subsequente.  
  
 O cache de exceção é habilitado quando você usar qualquer construtor <xref:System.Lazy%601?displayProperty=nameWithType> que usa um método de inicialização (parâmetro `valueFactory`); por exemplo, ele é habilitado quando você usa o construtor `Lazy(T)(Func(T))`. Se o construtor também usa um valor <xref:System.Threading.LazyThreadSafetyMode> (parâmetro `mode`), especifique <xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication?displayProperty=nameWithType> ou <xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType>. Especificar um método de inicialização permite o cache de exceções para esses dois modos. O método de inicialização pode ser muito simples. Por exemplo, ele pode chamar o construtor sem parâmetros para `T` : `new Lazy<Contents>(() => new Contents(), mode)` em C# ou `New Lazy(Of Contents)(Function() New Contents())` em Visual Basic. Se você usar um construtor <xref:System.Lazy%601?displayProperty=nameWithType> que não especifique um método de inicialização, as exceções geradas pelo construtor sem parâmetros para `T` não serão armazenadas em cache. Para obter mais informações, consulte a enumeração <xref:System.Threading.LazyThreadSafetyMode>.  
  
> [!NOTE]
> Se você criar um objeto <xref:System.Lazy%601> com o parâmetro de construtor `isThreadSafe` definido como `false` ou o parâmetro de construtor `mode` definido como <xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType>, você deverá acessar o objeto <xref:System.Lazy%601> de um único thread ou fornecer sua própria sincronização. Isso se aplica a todos os aspectos do objeto, incluindo o cache de exceção.  
  
 Conforme observado na seção anterior, objetos <xref:System.Lazy%601> criados com a especificação <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly?displayProperty=nameWithType> tratam exceções de forma diferente. Com <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly>, vários threads podem competir para inicializar a instância <xref:System.Lazy%601>. Nesse caso, as exceções não são armazenadas em cache e tentativas de acessar a propriedade <xref:System.Lazy%601.Value%2A> podem continuar até que a inicialização seja bem-sucedida.  
  
 A tabela a seguir resume a maneira como os construtores <xref:System.Lazy%601> controlam o cache de exceção.  
  
|Construtor|Modo de acesso thread-safe|Usa o método de inicialização|Exceções são armazenadas em cache|  
|-----------------|------------------------|--------------------------------|---------------------------|  
|Lazy(T)()|(<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>)|Não|Não|  
|Lazy(T)(Func(T))|(<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>)|Sim|Sim|  
|Lazy(T)(Boolean)|`True` (<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>) ou `false` (<xref:System.Threading.LazyThreadSafetyMode.None>)|Não|Não|  
|Lazy(T)(Func(T), Boolean)|`True` (<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>) ou `false` (<xref:System.Threading.LazyThreadSafetyMode.None>)|Sim|Sim|  
|Lazy(T)(LazyThreadSafetyMode)|Especificado pelo usuário|Não|Não|  
|Lazy(T)(Func(T), LazyThreadSafetyMode)|Especificado pelo usuário|Yes|Não se o usuário especifica <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly>; caso contrário, sim.|  
  
## <a name="implementing-a-lazy-initialized-property"></a>Implementando uma propriedade de inicialização lenta  
 Para implementar uma propriedade pública usando inicialização lenta, defina o campo de suporte da propriedade como um <xref:System.Lazy%601> e retorne a propriedade <xref:System.Lazy%601.Value%2A> do acessador `get` da propriedade.  
  
 [!code-csharp[Lazy#5](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#5)]
 [!code-vb[Lazy#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#5)]  
  
 A propriedade <xref:System.Lazy%601.Value%2A> é somente leitura; portanto, a propriedade que a expõe não tem nenhum acessador `set`. Se você precisar de uma propriedade de leitura/gravação com o apoio de um objeto <xref:System.Lazy%601>, o acessador `set` deverá criar um novo objeto <xref:System.Lazy%601> e atribuí-lo ao repositório de backup. O acessador `set` deve criar uma expressão lambda que retorna o novo valor da propriedade que foi passado para o acessador `set` e passar essa expressão lambda para o construtor para o novo objeto <xref:System.Lazy%601>. O próximo acesso à propriedade <xref:System.Lazy%601.Value%2A> causará a inicialização do novo <xref:System.Lazy%601> e a propriedade <xref:System.Lazy%601.Value%2A> dele retornará posteriormente o novo valor atribuído à propriedade. A razão para essa organização complicada é preservar as proteções de multithreading como internas em <xref:System.Lazy%601>. Caso contrário, os acessadores de propriedade precisariam armazenar em cache o primeiro valor retornado pela propriedade <xref:System.Lazy%601.Value%2A> e modificar apenas o valor armazenado em cache e você precisa escrever seu próprio código thread-safe para fazer isso. Devido às inicializações adicionais requeridas por uma propriedade de leitura/gravação com o suporte de um objeto <xref:System.Lazy%601>, o desempenho pode não ser aceitável. Além disso, dependendo do cenário específico, pode ser necessária coordenação adicional para evitar condições de corrida entre setters e getters.  
  
## <a name="thread-local-lazy-initialization"></a>Inicialização lenta de local de thread  
 Em alguns cenários com vários threads, você talvez queira dar a cada thread seus próprios dados privados. Esses dados são chamados de *dados locais de thread*. No .NET Framework versão 3.5 e anteriores, você pode aplicar o atributo `ThreadStatic` a uma variável estática para torná-la local de thread. No entanto, usar o atributo `ThreadStatic` pode levar a erros. Por exemplo, até mesmo instruções de inicialização básicas farão com que a variável a seja inicializada somente no primeiro thread que a acessar, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[Lazy#6](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#6)]
 [!code-vb[Lazy#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#6)]  
  
 Em todos os outros threads, a variável será inicializada usando seu valor padrão (zero). Como alternativa no .NET Framework versão 4, você pode usar o tipo <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> para criar uma variável local de thread baseada em instância, que é inicializada em todos os threads pelo delegado <xref:System.Action%601> que você fornece. No exemplo a seguir, todos os threads que acessarem `counter` verão seu valor inicial como 1.  
  
 [!code-csharp[Lazy#7](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#7)]
 [!code-vb[Lazy#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#7)]  
  
 <xref:System.Threading.ThreadLocal%601> encapsula seu objeto da mesma maneira que <xref:System.Lazy%601>, com estas diferenças essenciais:  
  
- Cada thread inicializa a variável local de thread usando seus próprios dados privados que não são acessíveis de outros threads.  
  
- A propriedade <xref:System.Threading.ThreadLocal%601.Value%2A?displayProperty=nameWithType> é de leitura-gravação e pode ser modificada qualquer número de vezes. Isso pode afetar a propagação de exceção, por exemplo, uma operação `get` pode gerar uma exceção, mas a próxima pode inicializar o valor com êxito.  
  
- Se nenhum delegado de inicialização for fornecido, <xref:System.Threading.ThreadLocal%601> inicializará seu tipo encapsulado usando o valor padrão do tipo. Nesse sentido, <xref:System.Threading.ThreadLocal%601> é consistente com o atributo <xref:System.ThreadStaticAttribute>.  
  
 O exemplo a seguir demonstra que cada thread que acessa a instância de `ThreadLocal<int>` obtém sua própria cópia exclusiva dos dados.  
  
 [!code-csharp[Lazy#9](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#9)]
 [!code-vb[Lazy#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#9)]  
  
## <a name="thread-local-variables-in-parallelfor-and-foreach"></a>Variáveis locais de thread em Parallel.For e ForEach  
 Quando você usa o método <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> ou o método <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> para iterar por fontes de dados em paralelo, você pode usar as sobrecargas que têm suporte interno para dados locais de thread. Esses métodos, a localidade do thread é obtida usando delegados locais para criar, acessar e limpar os dados. Para obter mais informações, consulte [Como gravar um loop Parallel.For com variáveis locais de thread](../../standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md) e [Como gravar um loop Parallel.ForEach com variáveis locais de partição](../../standard/parallel-programming/how-to-write-a-parallel-foreach-loop-with-partition-local-variables.md).  
  
## <a name="using-lazy-initialization-for-low-overhead-scenarios"></a>Usando inicialização lenta para cenários de baixa sobrecarga  
 Em cenários em que você precisa fazer inicialização lenta de um grande número de objetos, você pode decidir que encapsular cada objeto em um <xref:System.Lazy%601> requer memória ou recursos de computação demais. Ou então, você pode ter requisitos rigorosos para como a inicialização lenta é exposta. Nesses casos, você pode usar os métodos `static` (`Shared` no Visual Basic) da classe <xref:System.Threading.LazyInitializer?displayProperty=nameWithType> para fazer inicialização lenta de cada objeto sem encapsulá-lo em uma instância de <xref:System.Lazy%601>.  
  
 No exemplo a seguir, suponha que, em vez de encapsular todo um objeto `Orders` em um objeto <xref:System.Lazy%601>, você inicializou lentamente objetos `Order` individuais somente se eles são necessários.  
  
 [!code-csharp[Lazy#10](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#10)]
 [!code-vb[Lazy#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#10)]  
  
 Neste exemplo, observe que o procedimento de inicialização é invocado em cada iteração do loop. Em cenários com vários threads, o primeiro thread a invocar o procedimento de inicialização é aquele cujo valor é visto por todos os threads. Threads posteriores também chamam o procedimento de inicialização, mas os resultados não são usados. Se esse tipo de condição de corrida potencial não é aceitável, use a sobrecarga de <xref:System.Threading.LazyInitializer.EnsureInitialized%2A?displayProperty=nameWithType> que usa um argumento booliano e um objeto de sincronização.  
  
## <a name="see-also"></a>Veja também

- [Noções básicas sobre Threading gerenciado](../../standard/threading/managed-threading-basics.md)
- [Threads e threading](../../standard/threading/threads-and-threading.md)
- [Biblioteca de tarefas paralelas (TPL)](../../standard/parallel-programming/task-parallel-library-tpl.md)
- [Como executar a inicialização lenta de objetos](how-to-perform-lazy-initialization-of-objects.md)
