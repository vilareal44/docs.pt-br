---
title: Tipos de alterações da falha
description: Saiba como o .NET Core tenta manter a compatibilidade para desenvolvedores em versões do .NET e que tipo de alteração é considerada uma alteração significativa.
ms.date: 06/10/2019
ms.openlocfilehash: bc93316141ae99d8cfedc5e6d88a9e91216f9c6e
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415739"
---
# <a name="changes-that-affect-compatibility"></a>Alterações que afetam a compatibilidade

Ao longo de sua história, o .NET tentou manter um alto nível de compatibilidade de versão para versão e em todos os tipos de .NET. Isso também ocorre no .NET Core. Embora o .NET Core possa ser considerado uma nova tecnologia independente do .NET Framework, dois fatores principais limitam a capacidade do .NET Core de divergir do .NET Framework:

- Um grande número de desenvolvedores originalmente desenvolveu ou continua a desenvolver aplicativos .NET Framework. Eles esperam um comportamento consistente nas implementações do .NET.

- Os projetos da biblioteca .NET Standard permitem que os desenvolvedores criem bibliotecas voltadas a APIs comuns compartilhadas pelo .NET Core e pelo .NET Framework. Os desenvolvedores esperam que uma biblioteca usada em um aplicativo .NET Core se comporte de maneira idêntica à mesma biblioteca usada em um aplicativo .NET Framework.

Além da compatibilidade entre as implementações do .NET, os desenvolvedores esperam um alto nível de compatibilidade nas versões do .NET Core. Em particular, o código escrito para uma versão anterior do .NET Core precisam ser executado sem problemas em uma versão posterior do .NET Core. Na verdade, muitos desenvolvedores esperam que as novas APIs encontradas em versões recém-lançadas do .NET Core também sejam compatíveis com as versões de pré-lançamento em que essas APIs foram apresentadas.

Este artigo descreve as alterações que afetam a compatibilidade e a maneira como a equipe do .NET avalia cada tipo de alteração. Entender como a equipe .NET se aproxima das possíveis alterações significativas é particularmente útil para desenvolvedores que abrem solicitações de pull que modificam o comportamento de [APIs existentes do .net](https://github.com/dotnet/runtime).

As seções a seguir descrevem as categorias de alterações feitas nas APIs do .NET Core e seu impacto na compatibilidade de aplicativos. As alterações são permitidas ✔️, não permitido ❌ ou exigem julgamento e uma avaliação de como a previsibilidade, óbvia e consistente o comportamento anterior foi ❓.

> [!NOTE]
>
> - Além de servir como um guia de como as alterações nas bibliotecas do .NET são avaliadas, os desenvolvedores de biblioteca também podem usar esses critérios para avaliar as alterações em suas bibliotecas direcionadas a várias versões e implementações do .NET.
> - Para obter informações sobre as categorias de compatibilidade, por exemplo, compatibilidade com versões posteriores e anteriores, consulte [compatibilidade](categories.md).

## <a name="modifications-to-the-public-contract"></a>Modificações no contrato público

Alterações nesta categoria modificam a área de superfície pública de um tipo. A maioria das alterações nesta categoria não é permitida porque viola a *compatibilidade com versões anteriores* (a capacidade de um aplicativo desenvolvido com a versão anterior de uma API ser executado sem recompilação em uma versão posterior).

### <a name="types"></a>Tipos

- ✔️ **permitido: removendo uma implementação de interface de um tipo quando a interface já está implementada por um tipo base**

- ❓ **Requer julgamento: adicionar uma nova implementação de interface a um tipo**

  Essa é uma alteração aceitável, pois não afeta negativamente os clientes existentes. Quaisquer alterações no tipo precisam funcionar dentro dos limites das alterações aceitáveis ​​definidas aqui para que a nova implementação permaneça aceitável. É necessário ter um extremo cuidado ao adicionar interfaces que afetam diretamente a capacidade de um designer ou serializador de gerar código ou dados que não podem ser consumidos em um nível anterior. Um exemplo é a interface <xref:System.Runtime.Serialization.ISerializable>.

- ❓ **Requer julgamento: introduzindo uma nova classe base**

  Um tipo pode ser introduzido em uma hierarquia entre dois tipos existentes se ele não introduzir nenhum novo membro [abstrato](../../csharp/language-reference/keywords/abstract.md) ou alterar a semântica ou o comportamento de tipos existentes. Por exemplo, no .NET Framework 2.0, a classe <xref:System.Data.Common.DbConnection> tornou-se uma nova classe base para <xref:System.Data.SqlClient.SqlConnection>, que antes era derivada diretamente de <xref:System.ComponentModel.Component>.

- ✔️ **permitido: mover um tipo de um assembly para outro**

  O assembly *antigo* deve ser marcado com o <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute> que aponta para o novo assembly.

- ✔️ **permitido: alterar um tipo de [struct](../../csharp/language-reference/builtin-types/struct.md) para um `readonly struct` tipo**

  `readonly struct`Não é permitido alterar um tipo para um `struct` tipo.

- ✔️ **permitido: adicionar a palavra-chave [sealed](../../csharp/language-reference/keywords/sealed.md) ou [abstract](../../csharp/language-reference/keywords/abstract.md) a um tipo quando não há construtores *acessíveis* (públicos ou protegidos)**

- ✔️ **permitido: expandindo a visibilidade de um tipo**

- ❌Não **permitido: alterando o namespace ou o nome de um tipo**

- ❌Não **permitido: renomear ou remover um tipo público**

   Isso interrompe todo o código que usa o tipo renomeado ou removido.

- ❌Não **permitido: alterando o tipo subjacente de uma enumeração**

   Esta é uma alteração significativa de tempo de compilação e comportamental, bem como uma alteração significativa binária que pode tornar os argumentos de atributo inseparáveis.

- ❌Não **permitido: lacrar um tipo que foi anteriormente não lacrado**

- ❌Não **permitido: adicionando uma interface ao conjunto de tipos de base de uma interface**

   Se uma interface implementar uma interface não implementada anteriormente, todos os tipos que implementaram a versão original da interface serão corrompidos.

- ❓ **Requer julgamento: removendo uma classe do conjunto de classes base ou de uma interface do conjunto de interfaces implementadas**

  Há uma exceção à regra para remoção de interface: é possível adicionar a implementação de uma interface derivada da interface removida. Por exemplo, você pode remover <xref:System.IDisposable> se o tipo ou interface agora implementa <xref:System.ComponentModel.IComponent>, que implementa <xref:System.IDisposable>.

- ❌Não **permitido: alteração de um `readonly struct` tipo para um tipo de [struct](../../csharp/language-reference/builtin-types/struct.md) **

  `struct`No entanto, a alteração de um tipo para um `readonly struct` tipo é permitida.

- ❌Não **permitido: alteração de um tipo de [struct](../../csharp/language-reference/builtin-types/struct.md) para um `ref struct` tipo, e vice-versa**

- ❌Não **permitido: reduzindo a visibilidade de um tipo**

   No entanto, aumentar a visibilidade de um tipo é permitido.

### <a name="members"></a>Membros

- ✔️ **permitido: expandindo a visibilidade de um membro que não é [virtual](../../csharp/language-reference/keywords/sealed.md) **

- ✔️ **permitido: adicionar um membro abstrato a um tipo público que não tem construtores (públicos ou protegidos) *acessíveis* ou o tipo é [lacrado](../../csharp/language-reference/keywords/sealed.md) **

  No entanto, adicionar um membro abstrato a um tipo que tenha construtores acessíveis (públicos ou protegidos) e não seja `sealed` não é permitido.

- ✔️ **permitido: restringir a visibilidade de um membro [protegido](../../csharp/language-reference/keywords/protected.md) quando o tipo não tem construtores (públicos ou protegidos) acessíveis ou o tipo é [lacrado](../../csharp/language-reference/keywords/sealed.md) **

- ✔️ **permitido: mover um membro para uma classe acima na hierarquia do que o tipo do qual ele foi removido**

- ✔️ **permitido: Adicionar ou remover uma substituição**

  A introdução de uma substituição pode fazer com que os consumidores anteriores ignorem a substituição ao chamar [base](../../csharp/language-reference/keywords/base.md).

- ✔️ **permitido: adicionar um construtor a uma classe, juntamente com um construtor sem parâmetros, se a classe anteriormente não tivesse construtores**

   No entanto, não é permitido adicionar um construtor a uma classe que anteriormente não tinha construtores *sem* adicionar o construtor sem parâmetros.

- ✔️ **permitido: alterar um membro de [abstract](../../csharp/language-reference/keywords/abstract.md) para [virtual](../../csharp/language-reference/keywords/virtual.md) **

- ✔️ **permitido: alteração de um `ref readonly` para um `ref` valor de retorno (exceto para métodos ou interfaces virtuais)**

- ✔️ **permitido: removendo [ReadOnly](../../csharp/language-reference/keywords/readonly.md) de um campo, a menos que o tipo estático do campo seja um tipo de valor mutável**

- ✔️ **permitido: chamando um novo evento que não foi definido anteriormente**

- ❓ **Requer julgamento: adicionar um novo campo de instância a um tipo**

   Essa alteração afeta a serialização.

- ❌Não **permitido: renomear ou remover um membro ou parâmetro público**

   Isso interrompe todo o código que usa o membro ou parâmetro renomeado ou removido.

   Isso inclui remover ou renomear um getter ou setter de uma propriedade, bem como renomear ou remover membros de enumeração.

- ❌Não **permitido: adicionando um membro a uma interface**

- ❌Não **permitido: alterando o valor de uma constante pública ou membro de enumeração**

- ❌Não **permitido: alteração do tipo de uma propriedade, campo, parâmetro ou valor de retorno**

- ❌Não **permitido: adição, remoção ou alteração da ordem dos parâmetros**

- ❌Não **permitido: adição ou remoção da palavra-chave [in](../../csharp/language-reference/keywords/in.md), [out](../../csharp/language-reference/keywords/out.md) ou [ref](../../csharp/language-reference/keywords/ref.md) de um parâmetro**

- ❌Não **permitido: renomear um parâmetro (incluindo a alteração de seu caso)**

  Isso é considerado significativo por dois motivos:

  - Interrompe cenários de associação tardia, como o recurso de associação tardia no Visual Basic e [dinâmico](../../csharp/language-reference/builtin-types/reference-types.md#the-dynamic-type) no C#.

  - Interrompe a [compatibilidade de origem](categories.md#source-compatibility) quando os desenvolvedores usam [argumentos nomeados](../../csharp/programming-guide/classes-and-structs/named-and-optional-arguments.md#named-arguments).

- ❌Não **permitido: alteração de um `ref` valor de retorno para um `ref readonly` valor de retorno**

- ❌️Não **permitido: alteração de um `ref readonly` para um `ref` valor de retorno em um método ou interface virtual**

- ❌Não **permitido: adição ou remoção de [Resumo](../../csharp/language-reference/keywords/abstract.md) de um membro**

- ❌Não **permitido: removendo a palavra-chave [virtual](../../csharp/language-reference/keywords/virtual.md) de um membro**

  Embora isso geralmente não seja uma alteração significativa, pois o compilador C# tende a emitir instruções [callvirt](<xref:System.Reflection.Emit.OpCodes.Callvirt>) de IL (Intermediate Language) para chamar métodos não virtuais (`callvirt` executa uma verificação nula, enquanto uma chamada normal não), esse comportamento não é invariável por vários motivos:
  - C# não é a única linguagem de destino do .NET.

  - O compilador C# tenta otimizar cada vez mais `callvirt` a uma chamada normal sempre que o método de destino é não virtual e provavelmente não é nulo (como um método acessado por meio de operador de propagação nula [?.](../../csharp/language-reference/operators/member-access-operators.md#null-conditional-operators--and-)).

  Tornar um método virtual significa que o código do consumidor em geral acabaria chamando-o não virtualmente.

- ❌Não **permitido: adicionando a palavra-chave [virtual](../../csharp/language-reference/keywords/virtual.md) a um membro**

- ❌Não **permitido: tornar um membro Virtual abstrato**

  Um [membro virtual](../../csharp/language-reference/keywords/virtual.md) fornece uma implementação de método que *pode ser* substituída por uma classe derivada. Um [membro abstrato](../../csharp/language-reference/keywords/abstract.md) não fornece nenhuma implementação e *precisa ser* substituído.

- ❌Não **permitido: adicionar um membro abstrato a um tipo público que tenha construtores acessíveis (públicos ou protegidos) e que não esteja [lacrado](../../csharp/language-reference/keywords/sealed.md) **

- ❌Não **permitido: adicionando ou removendo a palavra-chave [static](../../csharp/language-reference/keywords/static.md) de um membro**

- ❌Não **permitido: adicionar uma sobrecarga que impede uma sobrecarga existente e define um comportamento diferente**

  Isso interrompe os clientes existentes que estavam vinculados à sobrecarga anterior. Por exemplo, se uma classe tiver uma única versão de um método que aceite um <xref:System.UInt32>, um consumidor existente será vinculado a essa sobrecarga ao passar um valor <xref:System.Int32>. No entanto, se você adicionar uma sobrecarga que aceita um <xref:System.Int32>, ao recompilar ou usar associação tardia, o compilador agora se associa à nova sobrecarga. Se resultar em um comportamento diferente, significa que essa é uma alteração significativa.

- ❌**Não permitido: adição de um construtor a uma classe que anteriormente não tinha nenhum construtor sem adicionar o construtor com parâmetros**

- ❌️Não **permitido: adicionando [ReadOnly](../../csharp/language-reference/keywords/readonly.md) a um campo**

- ❌Não **permitido: reduzindo a visibilidade de um membro**

   Isso inclui reduzir a visibilidade de um membro [protegido](../../csharp/language-reference/keywords/protected.md) quando há construtores *acessíveis* ( `public` ou `protected` ) e o tipo *não* é [lacrado](../../csharp/language-reference/keywords/sealed.md). Se esse não for o caso, será permitido reduzir a visibilidade de um membro protegido.

   É permitido aumentar a visibilidade de um membro.

- ❌Não **permitido: alterando o tipo de um membro**

   Não é possível modificar o valor de retorno de um método ou o tipo de uma propriedade ou campo. Por exemplo, a assinatura de um método que retorna um <xref:System.Object> não pode ser alterada para retornar um <xref:System.String> ou vice-versa.

- ❌**Não permitido: adicionando um campo a uma struct que anteriormente não tinha estado**

  As regras de atribuição definidas permitem o uso de variáveis ​​não inicializadas, desde que o tipo de variável seja um struct sem estado. Se o struct for alterado para “com estado”, o código poderá acabar com dados não inicializados. Isso é potencialmente uma interrupção de fonte e uma alteração binária significativa.

- ❌Não **permitido: acionamento de um evento existente quando nunca foi acionado antes**

## <a name="behavioral-changes"></a>Alterações de comportamento

### <a name="assemblies"></a>Assemblies

- ✔️ **permitido: tornar um assembly portátil quando as mesmas plataformas ainda têm suporte**

- ❌Não **permitido: alterando o nome de um assembly**
- ❌Não **permitido: alterando a chave pública de um assembly**

### <a name="properties-fields-parameters-and-return-values"></a>Propriedades, campos, parâmetros e valores retornados

- ✔️ **permitido: alterar o valor de uma propriedade, campo, valor de retorno ou parâmetro de [saída](../../csharp/language-reference/keywords/out-parameter-modifier.md) para um tipo mais derivado**

  Por exemplo, um método que retorna um tipo de <xref:System.Object> pode retornar uma instância <xref:System.String>. (No entanto, a assinatura do método não pode ser alterada.)

- ✔️ **permitido: aumentar o intervalo de valores aceitos para uma propriedade ou parâmetro se o membro não for [virtual](../../csharp/language-reference/keywords/virtual.md) **

  Embora o intervalo de valores que pode ser passado para o método ou seja retornado pelo membro possa ser expandido, o parâmetro ou o tipo de membro não pode. Por exemplo, enquanto os valores passados ​​para um método podem ser expandidos de 0-124 para 0-255, o tipo de parâmetro não pode ser alterado de <xref:System.Byte> para <xref:System.Int32>.

- ❌Não **permitido: aumentando o intervalo de valores aceitos para uma propriedade ou parâmetro se o membro for [virtual](../../csharp/language-reference/keywords/virtual.md) **

   Essa alteração interrompe os membros substituídos existentes, que não funcionarão corretamente para o intervalo estendido de valores.

- ❌Não **permitido: diminuindo o intervalo de valores aceitos para uma propriedade ou parâmetro**

- ❌Não **permitido: aumentando o intervalo de valores retornados para uma propriedade, campo, valor de retorno ou parâmetro de [saída](../../csharp/language-reference/keywords/out-parameter-modifier.md) **

- ❌Não **permitido: alteração dos valores retornados para uma propriedade, campo, valor de retorno de método ou parâmetro de [saída](../../csharp/language-reference/keywords/out-parameter-modifier.md) **

- ❌Não **permitido: alterando o valor padrão de uma propriedade, campo ou parâmetro**

- ❌Não **permitido: alterando a precisão de um valor numérico de retorno**

- ❓ **Requer julgamento: uma alteração na análise de entrada e lançamento de novas exceções (mesmo se o comportamento de análise não for especificado na documentação**

### <a name="exceptions"></a>Exceções

- ✔️ **permitido: lançando uma exceção mais derivada do que uma exceção existente**

  Como a nova exceção é uma subclasse de uma exceção existente, o código de manipulação de exceção anterior continua a manipular a exceção. Por exemplo, no .NET Framework 4, os métodos de criação e recuperação de cultura começariam a lançar um <xref:System.Globalization.CultureNotFoundException> em vez de um <xref:System.ArgumentException> se a cultura não pudesse ser encontrada. Como <xref:System.Globalization.CultureNotFoundException> deriva de <xref:System.ArgumentException>, essa é uma alteração aceitável.

- ✔️ **permitido: lançando uma exceção mais específica <xref:System.NotSupportedException> que <xref:System.NotImplementedException> , <xref:System.NullReferenceException> ,**

- ✔️ **permitido: lançando uma exceção que é considerada irrecuperável**

  Exceções irrecuperáveis ​​não devem ser capturadas, mas precisam ser manipuladas por um manipulador de nível alto. Portanto, não se espera que os usuários tenham código que capture essas exceções explícitas. As exceções irrecuperáveis são:

  - <xref:System.AccessViolationException>
  - <xref:System.ExecutionEngineException>
  - <xref:System.Runtime.InteropServices.SEHException>
  - <xref:System.StackOverflowException>

- ✔️ **permitido: lançando uma nova exceção em um novo caminho de código**

  A exceção deve se aplicar somente a um novo caminho de código executado com novos valores de parâmetro ou estado e que não pode ser executado por código existente direcionado à versão anterior.

- ✔️ **permitido: removendo uma exceção para habilitar um comportamento mais robusto ou novos cenários**

  Por exemplo, um método `Divide` que anteriormente apenas manipulou valores positivos e lançou um <xref:System.ArgumentOutOfRangeException> pode ser alterado para dar suporte a valores negativos e positivos sem gerar uma exceção.

- ✔️ **permitido: alteração do texto de uma mensagem de erro**

  Os desenvolvedores não devem se basear no texto de mensagens de erro, que também mudam com base na cultura do usuário.

- ❌**Não permitido: lançando uma exceção em qualquer outro caso não listado acima**

- ❌**Não permitido: removendo uma exceção em qualquer outro caso não listado acima**

### <a name="attributes"></a>Atributos

- ✔️ **permitido: alterar o valor de um atributo que *não* é observável**

- ❌**Não permitido: alterando o valor de um atributo que *é* observável**

- ❓ **Requer julgamento: removendo um atributo**

  Na maioria dos casos, a remoção de um atributo (como <xref:System.NonSerializedAttribute>) é uma alteração significativa.

## <a name="platform-support"></a>Suporte a plataforma

- ✔️ **permitido: suporte a uma operação em uma plataforma que não era suportada anteriormente**

- ❌**Não permitido: sem suporte ou agora exigindo uma Service Pack específica para uma operação que tinha suporte anteriormente em uma plataforma**

## <a name="internal-implementation-changes"></a>Alterações na implementação interna

- ❓ **Requer julgamento: alterando a área da superfície de um tipo interno**

   Tais mudanças geralmente são permitidas, embora interrompam a reflexão privada. Em alguns casos, em que bibliotecas populares de terceiros ou um grande número de desenvolvedores dependem das APIs internas, essas alterações podem não ser permitidas.

- ❓ **Requer julgamento: alterando a implementação interna de um membro**

  Essas mudanças geralmente são permitidas, embora interrompam a reflexão privada. Em alguns casos, em que o código do cliente frequentemente depende da reflexão privada ou em que a alteração introduz efeitos colaterais indesejados, essas alterações podem não ser permitidas.

- ✔️ **permitido: melhorando o desempenho de uma operação**

   A capacidade de modificar o desempenho de uma operação é essencial, mas essas alterações podem quebrar o código que depende da velocidade atual de uma operação. Isso é particularmente verdadeiro no código que depende do tempo de operações assíncronas. A alteração de desempenho não deve ter nenhum efeito sobre outro comportamento da API em questão; caso contrário, a alteração será interrompida.

- ✔️ **permitido: alterar indiretamente (e, com freqüência, negativamente) o desempenho de uma operação**

  Se a alteração em questão não for categorizada como significativa por algum outro motivo, isso será aceitável. Geralmente, ações precisam ser realizadas, o que pode incluir operações extras ou a adição de novas funcionalidades. Isso quase sempre afetará o desempenho, mas pode ser essencial para que a API em questão funcione conforme o esperado.

- ❌Não **permitido: alteração de uma API síncrona para assíncrona (e vice-versa)**

## <a name="code-changes"></a>Alterações de código

- ✔️ **permitido: adicionando [params](../../csharp/language-reference/keywords/params.md) a um parâmetro**

- ❌Não **permitido: alteração de uma [struct](../../csharp/language-reference/builtin-types/struct.md) para uma [classe](../../csharp/language-reference/keywords/class.md) e vice-versa**

- ❌Não **permitido: adicionando a palavra-chave [verificada](../../csharp/language-reference/keywords/virtual.md) a um bloco de código**

   Essa alteração pode fazer com que um código que foi executado anteriormente lance um <xref:System.OverflowException>, o que é inaceitável.

- ❌Não **permitido: remoção de [params](../../csharp/language-reference/keywords/params.md) de um parâmetro**

- ❌Não **permitido: alterar a ordem na qual os eventos são acionados**

  Os desenvolvedores podem razoavelmente esperar que os eventos sejam disparados na mesma ordem, e o código do desenvolvedor frequentemente depende da ordem em que os eventos são disparados.

- ❌Não **permitido: removendo a geração de um evento em uma determinada ação**

- ❌Não **permitido: a alteração do número de vezes que os eventos fornecidos são chamados**

- ❌Não **permitido: adicionando o <xref:System.FlagsAttribute> a um tipo de enumeração**
