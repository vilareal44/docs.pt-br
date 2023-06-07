---
title: 'Tutorial: criar um provedor de tipos'
description: 'Saiba como criar seus próprios provedores de tipo F # no F # 3,0 examinando vários provedores de tipo simples para ilustrar os conceitos básicos.'
ms.date: 11/04/2019
ms.openlocfilehash: 67ebd91007ff814370573ebc1a65b2c7a8399f7d
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202139"
---
# <a name="tutorial-create-a-type-provider"></a>Tutorial: criar um provedor de tipos

O mecanismo do provedor de tipos no F # é uma parte significativa de seu suporte para a programação avançada de informações. Este tutorial explica como criar seus próprios provedores de tipo orientando você pelo desenvolvimento de vários provedores de tipo simples para ilustrar os conceitos básicos. Para obter mais informações sobre o mecanismo do provedor de tipos em F #, consulte [provedores de tipos](index.md).

O ecossistema F # contém um intervalo de provedores de tipos para serviços de dados empresariais e da Internet usados com frequência. Por exemplo:

- O [FSharp. Data](https://fsharp.github.io/FSharp.Data/) inclui provedores de tipos para formatos de documento JSON, XML, CSV e HTML.

- O [Sqlfornecetor](https://fsprojects.github.io/SQLProvider/) fornece acesso fortemente tipado a bancos de dados SQL por meio de um mapeamento de objeto e consultas F # LINQ em relação a essas fontes.

- O [FSharp. Data. SqlClient](https://fsprojects.github.io/FSharp.Data.SqlClient/) tem um conjunto de provedores de tipos para inserção verificada em tempo de compilação de T-SQL em F #.

- [FSharp. Data. TypeProviders](https://fsprojects.github.io/FSharp.Data.TypeProviders/) é um conjunto mais antigo de provedores de tipo para uso somente com .NET Framework programação para acessar serviços de dados SQL, Entity Framework, ODATA e WSDL.

Quando necessário, você pode criar provedores de tipo personalizados ou pode referenciar provedores de tipos que outras pessoas criaram. Por exemplo, sua organização pode ter um serviço de dados que fornece um número grande e crescente de conjuntos de dados nomeados, cada um com seu próprio esquema de dados estável. Você pode criar um provedor de tipos que lê os esquemas e apresenta os conjuntos de dados atuais para o programador de uma maneira fortemente tipada.

## <a name="before-you-start"></a>Antes de iniciar

O mecanismo do provedor de tipos é projetado principalmente para injetar dados estáveis e espaços de informações de serviço na experiência de programação em F #.

Esse mecanismo não é projetado para injetar espaços de informações cujo esquema muda durante a execução do programa de maneiras relevantes para a lógica do programa. Além disso, o mecanismo não é projetado para a metaprogramação entre idiomas, mesmo que esse domínio contenha alguns usos válidos. Você deve usar esse mecanismo somente quando necessário e onde o desenvolvimento de um provedor de tipo produz um valor muito alto.

Você deve evitar escrever um provedor de tipos em que um esquema não esteja disponível. Da mesma forma, você deve evitar escrever um provedor de tipos em que uma biblioteca .NET comum (ou até mesmo uma existente) seria suficiente.

Antes de começar, você pode fazer as seguintes perguntas:

- Você tem um esquema para sua fonte de informações? Nesse caso, qual é o mapeamento para o sistema de tipos F # e .NET?

- Você pode usar uma API existente (digitada dinamicamente) como um ponto de partida para sua implementação?

- Você e sua organização têm usos suficientes do provedor de tipos para tornar a sua leitura vale a pena? Uma biblioteca .NET normal atende às suas necessidades?

- Quanto seu esquema vai mudar?

- Ele será alterado durante a codificação?

- Ele será alterado entre as sessões de codificação?

- Ele será alterado durante a execução do programa?

Os provedores de tipos são mais adequados para situações em que o esquema é estável em tempo de execução e durante a vida útil do código compilado.

## <a name="a-simple-type-provider"></a>Um provedor de tipo simples

Este exemplo é Samples. HelloWorldTypeProvider, semelhante aos exemplos do `examples` diretório do SDK do [tipo F #](https://github.com/fsprojects/FSharp.TypeProviders.SDK/). O provedor disponibiliza um "espaço de tipo" que contém os tipos apagados 100, como mostra o código a seguir, usando a sintaxe de assinatura F # e omitindo os detalhes para todos, exceto `Type1` . Para obter mais informações sobre tipos apagados, consulte [detalhes sobre os tipos fornecidos e apagados](#details-about-erased-provided-types) mais adiante neste tópico.

```fsharp
namespace Samples.HelloWorldTypeProvider

type Type1 =
    /// This is a static property.
    static member StaticProperty : string

    /// This constructor takes no arguments.
    new : unit -> Type1

    /// This constructor takes one argument.
    new : data:string -> Type1

    /// This is an instance property.
    member InstanceProperty : int

    /// This is an instance method.
    member InstanceMethod : x:int -> char

    nested type NestedType =
        /// This is StaticProperty1 on NestedType.
        static member StaticProperty1 : string
        …
        /// This is StaticProperty100 on NestedType.
        static member StaticProperty100 : string

type Type2 =
…
…

type Type100 =
…
```

Observe que o conjunto de tipos e membros fornecidos é estaticamente conhecido. Este exemplo não aproveita a capacidade dos provedores de fornecer tipos que dependem de um esquema. A implementação do provedor de tipos é descrita no código a seguir, e os detalhes são abordados nas seções posteriores deste tópico.

> [!WARNING]
> Pode haver diferenças entre esse código e os exemplos online.

```fsharp
namespace Samples.FSharp.HelloWorldTypeProvider

open System
open System.Reflection
open ProviderImplementation.ProvidedTypes
open FSharp.Core.CompilerServices
open FSharp.Quotations

// This type defines the type provider. When compiled to a DLL, it can be added
// as a reference to an F# command-line compilation, script, or project.
[<TypeProvider>]
type SampleTypeProvider(config: TypeProviderConfig) as this =

  // Inheriting from this type provides implementations of ITypeProvider
  // in terms of the provided types below.
  inherit TypeProviderForNamespaces(config)

  let namespaceName = "Samples.HelloWorldTypeProvider"
  let thisAssembly = Assembly.GetExecutingAssembly()

  // Make one provided type, called TypeN.
  let makeOneProvidedType (n:int) =
  …
  // Now generate 100 types
  let types = [ for i in 1 .. 100 -> makeOneProvidedType i ]

  // And add them to the namespace
  do this.AddNamespace(namespaceName, types)

[<assembly:TypeProviderAssembly>]
do()
```

Para usar esse provedor, abra uma instância separada do Visual Studio, crie um script F # e, em seguida, adicione uma referência ao provedor do seu script usando #r como mostra o código a seguir:

```fsharp
#r @".\bin\Debug\Samples.HelloWorldTypeProvider.dll"

let obj1 = Samples.HelloWorldTypeProvider.Type1("some data")

let obj2 = Samples.HelloWorldTypeProvider.Type1("some other data")

obj1.InstanceProperty
obj2.InstanceProperty

[ for index in 0 .. obj1.InstanceProperty-1 -> obj1.InstanceMethod(index) ]
[ for index in 0 .. obj2.InstanceProperty-1 -> obj2.InstanceMethod(index) ]

let data1 = Samples.HelloWorldTypeProvider.Type1.NestedType.StaticProperty35
```

Em seguida, procure os tipos no `Samples.HelloWorldTypeProvider` namespace que o provedor de tipos gerou.

Antes de recompilar o provedor, verifique se você fechou todas as instâncias do Visual Studio e F# Interativo que estão usando a DLL do provedor. Caso contrário, ocorrerá um erro de compilação porque a DLL de saída será bloqueada.

Para depurar esse provedor usando instruções PRINT, crie um script que expõe um problema com o provedor e, em seguida, use o seguinte código:

```console
fsc.exe -r:bin\Debug\HelloWorldTypeProvider.dll script.fsx
```

Para depurar esse provedor usando o Visual Studio, abra o Prompt de Comando do Desenvolvedor para Visual Studio com credenciais administrativas e execute o seguinte comando:

```console
devenv.exe /debugexe fsc.exe -r:bin\Debug\HelloWorldTypeProvider.dll script.fsx
```

Como alternativa, abra o Visual Studio, abra o menu Depurar, escolha `Debug/Attach to process…` e anexe a outro `devenv` processo em que você está editando o script. Usando esse método, você pode direcionar com mais facilidade uma lógica específica no provedor de tipos, digitando expressões interativamente na segunda instância (com IntelliSense completo e outros recursos).

Você pode desabilitar a depuração de Apenas Meu Código para identificar melhor os erros no código gerado. Para obter informações sobre como habilitar ou desabilitar esse recurso, consulte [navegando pelo código com o depurador](/visualstudio/debugger/navigating-through-code-with-the-debugger). Além disso, você também pode definir a captura de exceção de primeira chance abrindo o `Debug` menu e escolhendo `Exceptions` ou escolhendo as teclas CTRL + ALT + E para abrir a `Exceptions` caixa de diálogo. Nessa caixa de diálogo, em `Common Language Runtime Exceptions` , marque a `Thrown` caixa de seleção.

### <a name="implementation-of-the-type-provider"></a>Implementação do provedor de tipos

Esta seção orienta você pelas seções principais da implementação do provedor de tipos. Primeiro, você define o tipo para o próprio provedor de tipo personalizado:

```fsharp
[<TypeProvider>]
type SampleTypeProvider(config: TypeProviderConfig) as this =
```

Esse tipo deve ser público, e você deve marcá-lo com o atributo [TypeProvider](https://msdn.microsoft.com/library/bdf7b036-7490-4ace-b79f-c5f1b1b37947) para que o compilador reconheça o provedor de tipos quando um projeto F # separado referencia o assembly que contém o tipo. O parâmetro *config* é opcional e, se presente, contém informações de configuração contextuais para a instância do provedor de tipos que o compilador do F # cria.

Em seguida, implemente a interface [ITypeProvider](https://msdn.microsoft.com/library/2c2b0571-843d-4a7d-95d4-0a7510ed5e2f) . Nesse caso, você usa o `TypeProviderForNamespaces` tipo da `ProvidedTypes` API como um tipo base. Esse tipo de auxiliar pode fornecer uma coleção finita de namespaces fornecidos com mais adiantamento, cada um dos quais contém diretamente um número finito de tipos fixos e fornecidos com mais adiantamento. Nesse contexto, *o provedor gera* com mais regularidade tipos, mesmo que eles não sejam necessários ou usados.

```fsharp
inherit TypeProviderForNamespaces(config)
```

Em seguida, defina valores privados locais que especificam o namespace para os tipos fornecidos e localize o próprio assembly do provedor de tipos. Esse assembly é usado posteriormente como o tipo de pai lógico dos tipos apagados que são fornecidos.

```fsharp
let namespaceName = "Samples.HelloWorldTypeProvider"
let thisAssembly = Assembly.GetExecutingAssembly()
```

Em seguida, crie uma função para fornecer cada um dos tipos type1... Type100. Essa função é explicada em mais detalhes posteriormente neste tópico.

```fsharp
let makeOneProvidedType (n:int) = …
```

Em seguida, gere os tipos fornecidos 100:

```fsharp
let types = [ for i in 1 .. 100 -> makeOneProvidedType i ]
```

Em seguida, adicione os tipos como um namespace fornecido:

```fsharp
do this.AddNamespace(namespaceName, types)
```

Por fim, adicione um atributo de assembly que indica que você está criando uma DLL de provedor de tipos:

```fsharp
[<assembly:TypeProviderAssembly>]
do()
```

### <a name="providing-one-type-and-its-members"></a>Fornecendo um tipo e seus membros

A `makeOneProvidedType` função faz o trabalho real de fornecer um dos tipos.

```fsharp
let makeOneProvidedType (n:int) =
…
```

Esta etapa explica a implementação dessa função. Primeiro, crie o tipo fornecido (por exemplo, type1, quando n = 1 ou Type57, quando n = 57).

```fsharp
// This is the provided type. It is an erased provided type and, in compiled code,
// will appear as type 'obj'.
let t = ProvidedTypeDefinition(thisAssembly, namespaceName,
                               "Type" + string n,
                               baseType = Some typeof<obj>)
```

Você deve observar os seguintes pontos:

- Esse tipo fornecido é apagado.  Como você indica que o tipo base é `obj` , as instâncias serão exibidas como valores do tipo [obj](https://msdn.microsoft.com/library/dcf2430f-702b-40e5-a0a1-97518bf137f7) no código compilado.

- Ao especificar um tipo não aninhado, você deve especificar o assembly e o namespace. Para tipos apagados, o assembly deve ser o próprio assembly do provedor de tipos.

Em seguida, adicione a documentação XML ao tipo. Esta documentação está atrasada, ou seja, calculada sob demanda se o compilador do host precisar dela.

```fsharp
t.AddXmlDocDelayed (fun () -> sprintf "This provided type %s" ("Type" + string n))
```

Em seguida, você adiciona uma propriedade estática fornecida ao tipo:

```fsharp
let staticProp = ProvidedProperty(propertyName = "StaticProperty",
                                  propertyType = typeof<string>,
                                  isStatic = true,
                                  getterCode = (fun args -> <@@ "Hello!" @@>))
```

Obter essa propriedade sempre será avaliada como a cadeia de caracteres "Olá!". O `GetterCode` para a propriedade usa uma cotação de F #, que representa o código que o compilador host gera para obter a propriedade. Para obter mais informações sobre cotações, consulte [Code Requotas (F #)](https://msdn.microsoft.com/library/6f055397-a1f0-4f9a-927c-f0d7c6951155).

Adicione a documentação XML à propriedade.

```fsharp
staticProp.AddXmlDocDelayed(fun () -> "This is a static property")
```

Agora, anexe a propriedade fornecida ao tipo fornecido. Você deve anexar um membro fornecido a um e apenas um tipo. Caso contrário, o membro nunca estará acessível.

```fsharp
t.AddMember staticProp
```

Agora, crie um construtor fornecido que não usa parâmetros.

```fsharp
let ctor = ProvidedConstructor(parameters = [ ],
                               invokeCode = (fun args -> <@@ "The object data" :> obj @@>))
```

O `InvokeCode` para o construtor retorna uma cotação de F #, que representa o código que o compilador host gera quando o construtor é chamado. Por exemplo, você pode usar o seguinte construtor:

```fsharp
new Type10()
```

Uma instância do tipo fornecido será criada com os dados subjacentes "os dados do objeto". O código entre aspas inclui uma conversão para [obj](https://msdn.microsoft.com/library/dcf2430f-702b-40e5-a0a1-97518bf137f7) porque esse tipo é a eliminação desse tipo fornecido (como especificado quando você declarou o tipo fornecido).

Adicione a documentação XML ao construtor e adicione o construtor fornecido ao tipo fornecido:

```fsharp
ctor.AddXmlDocDelayed(fun () -> "This is a constructor")

t.AddMember ctor
```

Crie um segundo construtor fornecido que aceite um parâmetro:

```fsharp
let ctor2 =
ProvidedConstructor(parameters = [ ProvidedParameter("data",typeof<string>) ],
                    invokeCode = (fun args -> <@@ (%%(args.[0]) : string) :> obj @@>))
```

`InvokeCode`Novamente, o para o construtor retorna uma aspa de F #, que representa o código gerado pelo compilador de host para uma chamada para o método. Por exemplo, você pode usar o seguinte construtor:

```fsharp
new Type10("ten")
```

Uma instância do tipo fornecido é criada com os dados subjacentes "dez". Talvez você já tenha notado que a `InvokeCode` função retorna uma cotação. A entrada para essa função é uma lista de expressões, uma por parâmetro de construtor. Nesse caso, uma expressão que representa o valor de parâmetro único está disponível em `args.[0]` . O código para uma chamada para o Construtor impõe o valor de retorno para o tipo apagado `obj` . Depois de adicionar o segundo construtor fornecido ao tipo, você cria uma propriedade de instância fornecida:

```fsharp
let instanceProp =
    ProvidedProperty(propertyName = "InstanceProperty",
                     propertyType = typeof<int>,
                     getterCode= (fun args ->
                        <@@ ((%%(args.[0]) : obj) :?> string).Length @@>))
instanceProp.AddXmlDocDelayed(fun () -> "This is an instance property")
t.AddMember instanceProp
```

Obter essa propriedade retornará o comprimento da cadeia de caracteres, que é o objeto de representação. A `GetterCode` propriedade retorna uma cotação de F # que especifica o código que o compilador host gera para obter a propriedade. Como `InvokeCode` , a `GetterCode` função retorna uma cotação. O compilador do host chama essa função com uma lista de argumentos. Nesse caso, os argumentos incluem apenas a única expressão que representa a instância sobre a qual o getter está sendo chamado, que você pode acessar usando `args.[0]` . A implementação de `GetterCode` , em seguida, se unirá à cotação resultante no tipo apagado `obj` , e uma conversão é usada para satisfazer o mecanismo do compilador para verificar os tipos que o objeto é uma cadeia de caracteres. A próxima parte de `makeOneProvidedType` fornece um método de instância com um parâmetro.

```fsharp
let instanceMeth =
    ProvidedMethod(methodName = "InstanceMethod",
                   parameters = [ProvidedParameter("x",typeof<int>)],
                   returnType = typeof<char>,
                   invokeCode = (fun args ->
                       <@@ ((%%(args.[0]) : obj) :?> string).Chars(%%(args.[1]) : int) @@>))

instanceMeth.AddXmlDocDelayed(fun () -> "This is an instance method")
// Add the instance method to the type.
t.AddMember instanceMeth
```

Por fim, crie um tipo aninhado que contenha 100 propriedades aninhadas. A criação desse tipo aninhado e suas propriedades são atrasadas, ou seja, calculadas sob demanda.

```fsharp
t.AddMembersDelayed(fun () ->
  let nestedType = ProvidedTypeDefinition("NestedType", Some typeof<obj>)

  nestedType.AddMembersDelayed (fun () ->
    let staticPropsInNestedType =
      [
          for i in 1 .. 100 ->
              let valueOfTheProperty = "I am string "  + string i

              let p =
                ProvidedProperty(propertyName = "StaticProperty" + string i,
                  propertyType = typeof<string>,
                  isStatic = true,
                  getterCode= (fun args -> <@@ valueOfTheProperty @@>))

              p.AddXmlDocDelayed(fun () ->
                  sprintf "This is StaticProperty%d on NestedType" i)

              p
      ]

    staticPropsInNestedType)

  [nestedType])
```

### <a name="details-about-erased-provided-types"></a>Detalhes sobre os tipos fornecidos e apagados

O exemplo nesta seção fornece apenas os *tipos fornecidos e apagados*, que são particularmente úteis nas seguintes situações:

- Quando você está escrevendo um provedor para um espaço de informações que contém apenas dados e métodos.

- Quando você está escrevendo um provedor onde a semântica de tipo de tempo de execução precisa não é essencial para o uso prático do espaço de informações.

- Quando você está escrevendo um provedor para um espaço de informações tão grande e interconectado, não é tecnicamente viável gerar tipos .NET reais para o espaço de informações.

Neste exemplo, cada tipo fornecido é apagado para o tipo `obj` , e todos os usos do tipo aparecerão como tipo `obj` no código compilado. Na verdade, os objetos subjacentes nesses exemplos são cadeias de caracteres, mas o tipo será exibido como `System.Object` no código compilado do .net. Assim como acontece com todos os usos do tipo erasureing, você pode usar boxing, unboxing e Cast explícitos para os tipos de subtextos apagados. Nesse caso, uma exceção de conversão que não é válida pode resultar quando o objeto é usado. Um tempo de execução do provedor pode definir seu próprio tipo de representação particular para ajudar a proteger contra representações falsas. Você não pode definir tipos apagados no próprio F #. Somente tipos fornecidos podem ser apagados. Você deve entender as ramificações, práticas e semânticas, do uso de tipos apagados para seu provedor de tipos ou um provedor que fornece tipos apagados. Um tipo apagado não tem nenhum tipo .NET real. Portanto, você não pode fazer uma reflexão precisa sobre o tipo, e você pode subverter tipos apagados se usar conversões de tempo de execução e outras técnicas que dependem da semântica de tipo de tempo de execução exato. A subversão de tipos apagados frequentemente resulta em exceções de conversão de tipo em tempo de execução.

### <a name="choosing-representations-for-erased-provided-types"></a>Escolhendo representações para tipos fornecidos e apagados

Para alguns usos de tipos fornecidos e apagados, nenhuma representação é necessária. Por exemplo, o tipo fornecido apagado pode conter apenas propriedades e membros estáticos e nenhum construtor, e nenhum método ou propriedade retornaria uma instância do tipo. Se você puder acessar instâncias de um tipo fornecido, você deve considerar as seguintes perguntas:

**Qual é a eliminação de um tipo fornecido?**

- A eliminação de um tipo fornecido é como o tipo aparece no código .NET compilado.

- A eliminação de um tipo de classe apagado fornecido é sempre o primeiro tipo base não apagado na cadeia de herança do tipo.

- A eliminação de um tipo de interface apagado fornecido é sempre `System.Object` .

**Quais são as representações de um tipo fornecido?**

- O conjunto de objetos possíveis para um tipo fornecido apagado é chamado de suas representações. No exemplo deste documento, as representações de todos os tipos fornecidos apagados `Type1..Type100` são sempre objetos de cadeia de caracteres.

Todas as representações de um tipo fornecido devem ser compatíveis com a eliminação do tipo fornecido. (Caso contrário, o compilador F # apresentará um erro para o uso do provedor de tipos ou o código .NET não verificável que não for válido será gerado. Um provedor de tipos não será válido se retornar um código que forneça uma representação que não seja válida.)

Você pode escolher uma representação para os objetos fornecidos usando uma das abordagens a seguir, os quais são muito comuns:

- Se você estiver simplesmente fornecendo um wrapper fortemente tipado em um tipo .NET existente, geralmente faz sentido para que o tipo seja apagado para esse tipo, use instâncias desse tipo como representações, ou ambas. Essa abordagem é apropriada quando a maioria dos métodos existentes nesse tipo ainda faz sentido ao usar a versão com rigidez de tipos.

- Se você quiser criar uma API que difere significativamente de qualquer API .NET existente, faz sentido criar tipos de tempo de execução que serão a eliminação de tipos e representações para os tipos fornecidos.

O exemplo neste documento usa cadeias de caracteres como representações de objetos fornecidos. Frequentemente, pode ser apropriado usar outros objetos para representações. Por exemplo, você pode usar um dicionário como um recipiente de propriedades:

```fsharp
ProvidedConstructor(parameters = [],
    invokeCode= (fun args -> <@@ (new Dictionary<string,obj>()) :> obj @@>))
```

Como alternativa, você pode definir um tipo em seu provedor de tipos que será usado em tempo de execução para formar a representação, juntamente com uma ou mais operações de tempo de execução:

```fsharp
type DataObject() =
    let data = Dictionary<string,obj>()
    member x.RuntimeOperation() = data.Count
```

Os membros fornecidos podem então construir instâncias desse tipo de objeto:

```fsharp
ProvidedConstructor(parameters = [],
    invokeCode= (fun args -> <@@ (new DataObject()) :> obj @@>))
```

Nesse caso, você pode (opcionalmente) usar esse tipo como a eliminação de tipo, especificando esse tipo como o `baseType` ao construir o `ProvidedTypeDefinition` :

```fsharp
ProvidedTypeDefinition(…, baseType = Some typeof<DataObject> )
…
ProvidedConstructor(…, InvokeCode = (fun args -> <@@ new DataObject() @@>), …)
```

### <a name="key-lessons"></a>Principais lições

A seção anterior explicou como criar um provedor de tipo de apagamento simples que fornece uma variedade de tipos, propriedades e métodos. Esta seção também explicou o conceito de eliminação de tipo, incluindo algumas das vantagens e desvantagens de fornecer tipos apagados de um provedor de tipos e discutiu representações de tipos apagados.

## <a name="a-type-provider-that-uses-static-parameters"></a>Um provedor de tipos que usa parâmetros estáticos

A capacidade de parametrizar provedores de tipo por dados estáticos permite muitos cenários interessantes, mesmo em casos em que o provedor não precisa acessar dados locais ou remotos. Nesta seção, você aprenderá algumas técnicas básicas para reunir esse provedor.

### <a name="type-checked-regex-provider"></a>Provedor de Regex de tipo verificado

Imagine que você queira implementar um provedor de tipos para expressões regulares que encapsula as bibliotecas .NET <xref:System.Text.RegularExpressions.Regex> em uma interface que fornece as seguintes garantias de tempo de compilação:

- Verificando se uma expressão regular é válida.

- Fornecer propriedades nomeadas em correspondências que se baseiam em qualquer nome de grupo na expressão regular.

Esta seção mostra como usar provedores de tipos para criar um `RegexTyped` tipo que o padrão de expressão regular parametriza para fornecer esses benefícios. O compilador relatará um erro se o padrão fornecido não for válido e o provedor de tipos puder extrair os grupos do padrão para que você possa acessá-los usando propriedades nomeadas em correspondências. Quando você cria um provedor de tipos, deve considerar como sua API exposta deve procurar os usuários finais e como esse design será traduzido para o código .NET. O exemplo a seguir mostra como usar essa API para obter os componentes do código de área:

```fsharp
type T = RegexTyped< @"(?<AreaCode>^\d{3})-(?<PhoneNumber>\d{3}-\d{4}$)">
let reg = T()
let result = T.IsMatch("425-555-2345")
let r = reg.Match("425-555-2345").Group_AreaCode.Value //r equals "425"
```

O exemplo a seguir mostra como o provedor de tipos traduz essas chamadas:

```fsharp
let reg = new Regex(@"(?<AreaCode>^\d{3})-(?<PhoneNumber>\d{3}-\d{4}$)")
let result = reg.IsMatch("425-123-2345")
let r = reg.Match("425-123-2345").Groups.["AreaCode"].Value //r equals "425"
```

Observe os seguintes pontos:

- O tipo Regex padrão representa o tipo parametrizado `RegexTyped` .

- O `RegexTyped` Construtor resulta em uma chamada para o construtor Regex, passando o argumento de tipo estático para o padrão.

- Os resultados do `Match` método são representados pelo tipo padrão <xref:System.Text.RegularExpressions.Match> .

- Cada grupo nomeado resulta em uma propriedade fornecida e o acesso à propriedade resulta em um uso de um indexador na coleção de uma correspondência `Groups` .

O código a seguir é o núcleo da lógica para implementar tal provedor, e este exemplo omite a adição de todos os membros ao tipo fornecido. Para obter informações sobre cada membro adicionado, consulte a seção apropriada mais adiante neste tópico. Para o código completo, baixe o exemplo do [pacote de exemplo F # 3,0](https://archive.codeplex.com/?p=fsharp3sample) no site do CodePlex.

```fsharp
namespace Samples.FSharp.RegexTypeProvider

open System.Reflection
open Microsoft.FSharp.Core.CompilerServices
open Samples.FSharp.ProvidedTypes
open System.Text.RegularExpressions

[<TypeProvider>]
type public CheckedRegexProvider() as this =
    inherit TypeProviderForNamespaces()

    // Get the assembly and namespace used to house the provided types
    let thisAssembly = Assembly.GetExecutingAssembly()
    let rootNamespace = "Samples.FSharp.RegexTypeProvider"
    let baseTy = typeof<obj>
    let staticParams = [ProvidedStaticParameter("pattern", typeof<string>)]

    let regexTy = ProvidedTypeDefinition(thisAssembly, rootNamespace, "RegexTyped", Some baseTy)

    do regexTy.DefineStaticParameters(
        parameters=staticParams,
        instantiationFunction=(fun typeName parameterValues ->

          match parameterValues with
          | [| :? string as pattern|] ->

            // Create an instance of the regular expression.
            //
            // This will fail with System.ArgumentException if the regular expression is not valid.
            // The exception will escape the type provider and be reported in client code.
            let r = System.Text.RegularExpressions.Regex(pattern)

            // Declare the typed regex provided type.
            // The type erasure of this type is 'obj', even though the representation will always be a Regex
            // This, combined with hiding the object methods, makes the IntelliSense experience simpler.
            let ty =
              ProvidedTypeDefinition(
                thisAssembly,
                rootNamespace,
                typeName,
                baseType = Some baseTy)

            ...

            ty
          | _ -> failwith "unexpected parameter values"))

    do this.AddNamespace(rootNamespace, [regexTy])

[<TypeProviderAssembly>]
do ()
```

Observe os seguintes pontos:

- O provedor de tipos usa dois parâmetros estáticos: o `pattern` , que é obrigatório e o `options` , que são opcionais (porque um valor padrão é fornecido).

- Depois que os argumentos estáticos são fornecidos, você cria uma instância da expressão regular. Essa instância gerará uma exceção se o Regex estiver malformado e esse erro será relatado aos usuários.

- No `DefineStaticParameters` retorno de chamada, você define o tipo que será retornado depois que os argumentos forem fornecidos.

- Esse código define `HideObjectMethods` como true para que a experiência do IntelliSense permaneça simplificada. Esse atributo faz com que os `Equals` Membros,, `GetHashCode` `Finalize` e `GetType` sejam suprimidos das listas do IntelliSense para um objeto fornecido.

- Você usa `obj` como o tipo base do método, mas usará um `Regex` objeto como a representação de tempo de execução desse tipo, como mostra o exemplo a seguir.

- A chamada para o `Regex` Construtor gera um <xref:System.ArgumentException> quando uma expressão regular não é válida. O compilador captura essa exceção e relata uma mensagem de erro para o usuário no momento da compilação ou no editor do Visual Studio. Essa exceção permite que expressões regulares sejam validadas sem executar um aplicativo.

O tipo definido acima não é útil ainda porque ele não contém nenhum método ou Propriedade significativo. Primeiro, adicione um `IsMatch` método estático:

```fsharp
let isMatch =
    ProvidedMethod(
        methodName = "IsMatch",
        parameters = [ProvidedParameter("input", typeof<string>)],
        returnType = typeof<bool>,
        isStatic = true,
        invokeCode = fun args -> <@@ Regex.IsMatch(%%args.[0], pattern) @@>)

isMatch.AddXmlDoc "Indicates whether the regular expression finds a match in the specified input string."
ty.AddMember isMatch
```

O código anterior define um método `IsMatch` , que usa uma cadeia de caracteres como entrada e retorna um `bool` . A única parte complicada é o uso do `args` argumento dentro da `InvokeCode` definição. Neste exemplo, `args` é uma lista de Cotações que representa os argumentos para esse método. Se o método for um método de instância, o primeiro argumento representará o `this` argumento. No entanto, para um método estático, os argumentos são apenas os argumentos explícitos para o método. Observe que o tipo de valor entre aspas deve corresponder ao tipo de retorno especificado (nesse caso, `bool` ). Observe também que esse código usa o `AddXmlDoc` método para garantir que o método fornecido também tenha documentação útil, que você pode fornecer por meio do IntelliSense.

Em seguida, adicione um método de correspondência de instância. No entanto, esse método deve retornar um valor de um `Match` tipo fornecido para que os grupos possam ser acessados de maneira fortemente tipada. Portanto, primeiro você declara o `Match` tipo. Como esse tipo depende do padrão que foi fornecido como um argumento estático, esse tipo deve ser aninhado dentro da definição de tipo com parâmetros:

```fsharp
let matchTy =
    ProvidedTypeDefinition(
        "MatchType",
        baseType = Some baseTy,
        hideObjectMethods = true)

ty.AddMember matchTy
```

Em seguida, você adiciona uma propriedade ao tipo de correspondência para cada grupo. Em tempo de execução, uma correspondência é representada como um <xref:System.Text.RegularExpressions.Match> valor, portanto, a cotação que define a propriedade deve usar a <xref:System.Text.RegularExpressions.Match.Groups> propriedade indexada para obter o grupo relevante.

```fsharp
for group in r.GetGroupNames() do
    // Ignore the group named 0, which represents all input.
    if group <> "0" then
    let prop =
      ProvidedProperty(
        propertyName = group,
        propertyType = typeof<Group>,
        getterCode = fun args -> <@@ ((%%args.[0]:obj) :?> Match).Groups.[group] @@>)
        prop.AddXmlDoc(sprintf @"Gets the ""%s"" group from this match" group)
    matchTy.AddMember prop
```

Novamente, observe que você está adicionando a documentação XML à propriedade fornecida. Observe também que uma propriedade pode ser lida se uma `GetterCode` função for fornecida e a propriedade puder ser gravada se uma `SetterCode` função for fornecida, portanto, a propriedade resultante será somente leitura.

Agora você pode criar um método de instância que retorna um valor desse `Match` tipo:

```fsharp
let matchMethod =
    ProvidedMethod(
        methodName = "Match",
        parameters = [ProvidedParameter("input", typeof<string>)],
        returnType = matchTy,
        invokeCode = fun args -> <@@ ((%%args.[0]:obj) :?> Regex).Match(%%args.[1]) :> obj @@>)

matchMeth.AddXmlDoc "Searches the specified input string for the first occurrence of this regular expression"

ty.AddMember matchMeth
```

Como você está criando um método de instância, `args.[0]` representa a `RegexTyped` instância na qual o método está sendo chamado e `args.[1]` é o argumento de entrada.

Por fim, forneça um construtor para que as instâncias do tipo fornecido possam ser criadas.

```fsharp
let ctor =
    ProvidedConstructor(
        parameters = [],
        invokeCode = fun args -> <@@ Regex(pattern, options) :> obj @@>)

ctor.AddXmlDoc("Initializes a regular expression instance.")

ty.AddMember ctor
```

O Construtor simplesmente apaga a criação de uma instância Regex .NET padrão, que é novamente emoldurada em um objeto porque `obj` é a eliminação do tipo fornecido. Com essa alteração, o uso da API de exemplo especificado anteriormente no tópico funciona conforme o esperado. O código a seguir está completo e final:

```fsharp
namespace Samples.FSharp.RegexTypeProvider

open System.Reflection
open Microsoft.FSharp.Core.CompilerServices
open Samples.FSharp.ProvidedTypes
open System.Text.RegularExpressions

[<TypeProvider>]
type public CheckedRegexProvider() as this =
    inherit TypeProviderForNamespaces()

    // Get the assembly and namespace used to house the provided types.
    let thisAssembly = Assembly.GetExecutingAssembly()
    let rootNamespace = "Samples.FSharp.RegexTypeProvider"
    let baseTy = typeof<obj>
    let staticParams = [ProvidedStaticParameter("pattern", typeof<string>)]

    let regexTy = ProvidedTypeDefinition(thisAssembly, rootNamespace, "RegexTyped", Some baseTy)

    do regexTy.DefineStaticParameters(
        parameters=staticParams,
        instantiationFunction=(fun typeName parameterValues ->

            match parameterValues with
            | [| :? string as pattern|] ->

                // Create an instance of the regular expression.

                let r = System.Text.RegularExpressions.Regex(pattern)

                // Declare the typed regex provided type.

                let ty =
                    ProvidedTypeDefinition(
                        thisAssembly,
                        rootNamespace,
                        typeName,
                        baseType = Some baseTy)

                ty.AddXmlDoc "A strongly typed interface to the regular expression '%s'"

                // Provide strongly typed version of Regex.IsMatch static method.
                let isMatch =
                    ProvidedMethod(
                        methodName = "IsMatch",
                        parameters = [ProvidedParameter("input", typeof<string>)],
                        returnType = typeof<bool>,
                        isStatic = true,
                        invokeCode = fun args -> <@@ Regex.IsMatch(%%args.[0], pattern) @@>)

                isMatch.AddXmlDoc "Indicates whether the regular expression finds a match in the specified input string"

                ty.AddMember isMatch

                // Provided type for matches
                // Again, erase to obj even though the representation will always be a Match
                let matchTy =
                    ProvidedTypeDefinition(
                        "MatchType",
                        baseType = Some baseTy,
                        hideObjectMethods = true)

                // Nest the match type within parameterized Regex type.
                ty.AddMember matchTy

                // Add group properties to match type
                for group in r.GetGroupNames() do
                    // Ignore the group named 0, which represents all input.
                    if group <> "0" then
                        let prop =
                          ProvidedProperty(
                            propertyName = group,
                            propertyType = typeof<Group>,
                            getterCode = fun args -> <@@ ((%%args.[0]:obj) :?> Match).Groups.[group] @@>)
                        prop.AddXmlDoc(sprintf @"Gets the ""%s"" group from this match" group)
                        matchTy.AddMember(prop)

                // Provide strongly typed version of Regex.Match instance method.
                let matchMeth =
                  ProvidedMethod(
                    methodName = "Match",
                    parameters = [ProvidedParameter("input", typeof<string>)],
                    returnType = matchTy,
                    invokeCode = fun args -> <@@ ((%%args.[0]:obj) :?> Regex).Match(%%args.[1]) :> obj @@>)
                matchMeth.AddXmlDoc "Searches the specified input string for the first occurrence of this regular expression"

                ty.AddMember matchMeth

                // Declare a constructor.
                let ctor =
                  ProvidedConstructor(
                    parameters = [],
                    invokeCode = fun args -> <@@ Regex(pattern) :> obj @@>)

                // Add documentation to the constructor.
                ctor.AddXmlDoc "Initializes a regular expression instance"

                ty.AddMember ctor

                ty
            | _ -> failwith "unexpected parameter values"))

    do this.AddNamespace(rootNamespace, [regexTy])

[<TypeProviderAssembly>]
do ()
```

### <a name="key-lessons"></a>Principais lições

Esta seção explicou como criar um provedor de tipos que opera em seus parâmetros estáticos. O provedor verifica o parâmetro estático e fornece operações com base em seu valor.

## <a name="a-type-provider-that-is-backed-by-local-data"></a>Um provedor de tipos que é apoiado por dados locais

Frequentemente, você pode querer que os provedores de tipos apresentem APIs com base em parâmetros estáticos e também informações de sistemas locais ou remotos. Esta seção discute os provedores de tipos baseados em dados locais, como arquivos de dados locais.

### <a name="simple-csv-file-provider"></a>Provedor de arquivo CSV simples

Como um exemplo simples, considere um provedor de tipos para acessar dados científicos em formato CSV (valores separados por vírgula). Esta seção pressupõe que os arquivos CSV contêm uma linha de cabeçalho seguida por dados de ponto flutuante, como mostra a tabela a seguir:

|Distância (medidor)|Tempo (segundo)|
|----------------|-------------|
|50,0|3.7|
|100.0|5.2|
|150,0|6.4|

Esta seção mostra como fornecer um tipo que você pode usar para obter linhas com uma `Distance` Propriedade do tipo `float<meter>` e uma `Time` Propriedade do tipo `float<second>` . Para simplificar, são feitas as seguintes suposições:

- Os nomes de cabeçalho são menos unitários ou têm a forma "nome (unidade)" e não contêm vírgulas.

- As unidades são todas as unidades internacionais (SI) do sistema, como o módulo [Microsoft. FSharp. Data. UnitSystems. si. unitnames (F #)](https://msdn.microsoft.com/library/3cb43485-11f5-4aa7-a779-558f19d4013b) define.

- As unidades são todas simples (por exemplo, medidor) em vez de compostas (por exemplo, medidor/segundo).

- Todas as colunas contêm dados de ponto flutuante.

Um provedor mais completo desafrouxaria essas restrições.

Novamente, a primeira etapa é considerar a aparência da API. Dado um `info.csv` arquivo com o conteúdo da tabela anterior (em formato separado por vírgula), os usuários do provedor devem ser capazes de escrever um código semelhante ao exemplo a seguir:

```fsharp
let info = new MiniCsv<"info.csv">()
for row in info.Data do
let time = row.Time
printfn "%f" (float time)
```

Nesse caso, o compilador deve converter essas chamadas em algo semelhante ao exemplo a seguir:

```fsharp
let info = new CsvFile("info.csv")
for row in info.Data do
let (time:float) = row.[1]
printfn "%f" (float time)
```

A tradução ideal exigirá que o provedor de tipos defina um `CsvFile` tipo real no assembly do provedor de tipos. Provedores de tipo geralmente dependem de alguns tipos auxiliares e métodos para encapsular lógica importante. Como as medidas são apagadas no tempo de execução, você pode usar um `float[]` como o tipo apagado para uma linha. O compilador tratará colunas diferentes como tendo tipos de medidas diferentes. Por exemplo, a primeira coluna em nosso exemplo tem tipo `float<meter>` e a segunda tem `float<second>` . No entanto, a representação apagada pode permanecer bem simples.

O código a seguir mostra o núcleo da implementação.

```fsharp
// Simple type wrapping CSV data
type CsvFile(filename) =
    // Cache the sequence of all data lines (all lines but the first)
    let data =
        seq {
            for line in File.ReadAllLines(filename) |> Seq.skip 1 ->
                line.Split(',') |> Array.map float
        }
        |> Seq.cache
    member _.Data = data

[<TypeProvider>]
type public MiniCsvProvider(cfg:TypeProviderConfig) as this =
    inherit TypeProviderForNamespaces(cfg)

    // Get the assembly and namespace used to house the provided types.
    let asm = System.Reflection.Assembly.GetExecutingAssembly()
    let ns = "Samples.FSharp.MiniCsvProvider"

    // Create the main provided type.
    let csvTy = ProvidedTypeDefinition(asm, ns, "MiniCsv", Some(typeof<obj>))

    // Parameterize the type by the file to use as a template.
    let filename = ProvidedStaticParameter("filename", typeof<string>)
    do csvTy.DefineStaticParameters([filename], fun tyName [| :? string as filename |] ->

        // Resolve the filename relative to the resolution folder.
        let resolvedFilename = Path.Combine(cfg.ResolutionFolder, filename)

        // Get the first line from the file.
        let headerLine = File.ReadLines(resolvedFilename) |> Seq.head

        // Define a provided type for each row, erasing to a float[].
        let rowTy = ProvidedTypeDefinition("Row", Some(typeof<float[]>))

        // Extract header names from the file, splitting on commas.
        // use Regex matching to get the position in the row at which the field occurs
        let headers = Regex.Matches(headerLine, "[^,]+")

        // Add one property per CSV field.
        for i in 0 .. headers.Count - 1 do
            let headerText = headers.[i].Value

            // Try to decompose this header into a name and unit.
            let fieldName, fieldTy =
                let m = Regex.Match(headerText, @"(?<field>.+) \((?<unit>.+)\)")
                if m.Success then

                    let unitName = m.Groups.["unit"].Value
                    let units = ProvidedMeasureBuilder.Default.SI unitName
                    m.Groups.["field"].Value, ProvidedMeasureBuilder.Default.AnnotateType(typeof<float>,[units])

                else
                    // no units, just treat it as a normal float
                    headerText, typeof<float>

            let prop =
                ProvidedProperty(fieldName, fieldTy,
                    getterCode = fun [row] -> <@@ (%%row:float[]).[i] @@>)

            // Add metadata that defines the property's location in the referenced file.
            prop.AddDefinitionLocation(1, headers.[i].Index + 1, filename)
            rowTy.AddMember(prop)

        // Define the provided type, erasing to CsvFile.
        let ty = ProvidedTypeDefinition(asm, ns, tyName, Some(typeof<CsvFile>))

        // Add a parameterless constructor that loads the file that was used to define the schema.
        let ctor0 =
            ProvidedConstructor([],
                invokeCode = fun [] -> <@@ CsvFile(resolvedFilename) @@>)
        ty.AddMember ctor0

        // Add a constructor that takes the file name to load.
        let ctor1 = ProvidedConstructor([ProvidedParameter("filename", typeof<string>)],
            invokeCode = fun [filename] -> <@@ CsvFile(%%filename) @@>)
        ty.AddMember ctor1

        // Add a more strongly typed Data property, which uses the existing property at runtime.
        let prop =
            ProvidedProperty("Data", typedefof<seq<_>>.MakeGenericType(rowTy),
                getterCode = fun [csvFile] -> <@@ (%%csvFile:CsvFile).Data @@>)
        ty.AddMember prop

        // Add the row type as a nested type.
        ty.AddMember rowTy
        ty)

    // Add the type to the namespace.
    do this.AddNamespace(ns, [csvTy])
```

Observe os seguintes pontos sobre a implementação:

- Construtores sobrecarregados permitem que o arquivo original ou um que tenha um esquema idêntico seja lido. Esse padrão é comum quando você escreve um provedor de tipos para fontes de dados locais ou remotas, e esse padrão permite que um arquivo local seja usado como modelo para dados remotos.

- Você pode usar o valor [TypeProviderConfig](https://msdn.microsoft.com/library/1cda7b9a-3d07-475d-9315-d65e1c97eb44) que é passado para o construtor do provedor de tipos para resolver nomes de arquivo relativos.

- Você pode usar o `AddDefinitionLocation` método para definir o local das propriedades fornecidas. Portanto, se você usar `Go To Definition` o em uma propriedade fornecida, o arquivo CSV será aberto no Visual Studio.

- Você pode usar o `ProvidedMeasureBuilder` tipo para pesquisar as unidades de si e gerar os tipos relevantes `float<_>` .

### <a name="key-lessons"></a>Principais lições

Esta seção explicou como criar um provedor de tipos para uma fonte de dados local com um esquema simples contido na própria fonte de dados.

## <a name="going-further"></a>Aprofundamento

As seções a seguir incluem sugestões para um estudo adicional.

### <a name="a-look-at-the-compiled-code-for-erased-types"></a>Uma olhada no código compilado para tipos apagados

Para lhe dar uma ideia de como o uso do provedor de tipos corresponde ao código emitido, examine a seguinte função usando o `HelloWorldTypeProvider` que é usado anteriormente neste tópico.

```fsharp
let function1 () =
    let obj1 = Samples.HelloWorldTypeProvider.Type1("some data")
    obj1.InstanceProperty
```

Aqui está uma imagem do código resultante descompilado usando ildasm. exe:

```il
.class public abstract auto ansi sealed Module1
extends [mscorlib]System.Object
{
.custom instance void [FSharp.Core]Microsoft.FSharp.Core.CompilationMappingAtt
ribute::.ctor(valuetype [FSharp.Core]Microsoft.FSharp.Core.SourceConstructFlags)
= ( 01 00 07 00 00 00 00 00 )
.method public static int32  function1() cil managed
{
// Code size       24 (0x18)
.maxstack  3
.locals init ([0] object obj1)
IL_0000:  nop
IL_0001:  ldstr      "some data"
IL_0006:  unbox.any  [mscorlib]System.Object
IL_000b:  stloc.0
IL_000c:  ldloc.0
IL_000d:  call       !!0 [FSharp.Core_2]Microsoft.FSharp.Core.LanguagePrimit
ives/IntrinsicFunctions::UnboxGeneric<string>(object)
IL_0012:  callvirt   instance int32 [mscorlib_3]System.String::get_Length()
IL_0017:  ret
} // end of method Module1::function1

} // end of class Module1
```

Como mostra o exemplo, todas as menção do tipo `Type1` e da `InstanceProperty` Propriedade foram apagadas, deixando apenas as operações nos tipos de tempo de execução envolvidos.

### <a name="design-and-naming-conventions-for-type-providers"></a>Design e convenções de nomenclatura para provedores de tipo

Observe as seguintes convenções ao criar provedores de tipos.

**Provedores para protocolos de conectividade** Em geral, os nomes da maioria das DLLs de provedor para dados e protocolos de conectividade de serviço, como conexões OData ou SQL, devem terminar no `TypeProvider` ou no `TypeProviders` . Por exemplo, use um nome de DLL que se assemelha à seguinte cadeia de caracteres:

`Fabrikam.Management.BasicTypeProviders.dll`

Verifique se os tipos fornecidos são membros do namespace correspondente e indique o protocolo de conectividade que você implementou:

```fsharp
  Fabrikam.Management.BasicTypeProviders.WmiConnection<…>
  Fabrikam.Management.BasicTypeProviders.DataProtocolConnection<…>
```

**Provedores de utilitários para codificação geral**.  Para um provedor de tipos de utilitário como, para expressões regulares, o provedor de tipos pode fazer parte de uma biblioteca base, como mostra o exemplo a seguir:

```fsharp
#r "Fabrikam.Core.Text.Utilities.dll"
```

Nesse caso, o tipo fornecido apareceria em um ponto apropriado de acordo com as convenções normais de design do .NET:

```fsharp
  open Fabrikam.Core.Text.RegexTyped

  let regex = new RegexTyped<"a+b+a+b+">()
```

**Fontes de dados singleton**. Alguns provedores de tipos se conectam a uma única fonte de dados dedicada e fornecem apenas dados. Nesse caso, você deve remover o `TypeProvider` sufixo e usar convenções normais de nomenclatura do .net:

```fsharp
#r "Fabrikam.Data.Freebase.dll"

let data = Fabrikam.Data.Freebase.Astronomy.Asteroids
```

Para obter mais informações, consulte a `GetConnection` Convenção de design descrita mais adiante neste tópico.

### <a name="design-patterns-for-type-providers"></a>Padrões de design para provedores de tipo

As seções a seguir descrevem os padrões de design que você pode usar ao criar provedores de tipo.

#### <a name="the-getconnection-design-pattern"></a>O padrão de design getConnection

A maioria dos provedores de tipo deve ser escrita para usar o `GetConnection` padrão usado pelos provedores de tipo em FSharp. Data. TypeProviders. dll, como mostra o exemplo a seguir:

```fsharp
#r "Fabrikam.Data.WebDataStore.dll"

type Service = Fabrikam.Data.WebDataStore<…static connection parameters…>

let connection = Service.GetConnection(…dynamic connection parameters…)

let data = connection.Astronomy.Asteroids
```

#### <a name="type-providers-backed-by-remote-data-and-services"></a>Provedores de tipos apoiados por dados e serviços remotos

Antes de criar um provedor de tipos que é apoiado por dados e serviços remotos, você deve considerar uma variedade de problemas inerentes à programação conectada. Esses problemas incluem as seguintes considerações:

- mapeamento de esquema

- vida e invalidação na presença de alteração de esquema

- cache de esquema

- implementações assíncronas de operações de acesso a dados

- suporte a consultas, incluindo consultas LINQ

- credenciais e autenticação

Este tópico não explora mais esses problemas.

### <a name="additional-authoring-techniques"></a>Técnicas de criação adicionais

Ao escrever seus próprios provedores de tipo, talvez você queira usar as técnicas adicionais a seguir.

### <a name="creating-types-and-members-on-demand"></a>Criando tipos e membros sob demanda

A API fornecida tem versões atrasadas de AddMember.

```fsharp
  type ProvidedType =
      member AddMemberDelayed  : (unit -> MemberInfo)      -> unit
      member AddMembersDelayed : (unit -> MemberInfo list) -> unit
```

Essas versões são usadas para criar espaços de tipos sob demanda.

### <a name="providing-array-types-and-generic-type-instantiations"></a>Fornecendo tipos de matriz e instanciações de tipo genérico

Você faz com que os membros fornecidos (cujas assinaturas incluem tipos de matriz, tipos ByRef e instanciações de tipos genéricos) usando o normal `MakeArrayType` , `MakePointerType` e `MakeGenericType` em qualquer instância do <xref:System.Type> , incluindo `ProvidedTypeDefinitions` .

> [!NOTE]
> Em alguns casos, talvez seja necessário usar o auxiliar no `ProvidedTypeBuilder.MakeGenericType` .  Consulte a [documentação do SDK do provedor de tipos](https://github.com/fsprojects/FSharp.TypeProviders.SDK/blob/master/README.md#explicit-construction-of-code-makegenerictype-makegenericmethod-and-uncheckedquotations) para obter mais detalhes.

### <a name="providing-unit-of-measure-annotations"></a>Fornecendo anotações de unidade de medida

A API ProvidedTypes fornece auxiliares para fornecer anotações de medida. Por exemplo, para fornecer o tipo `float<kg>` , use o seguinte código:

```fsharp
  let measures = ProvidedMeasureBuilder.Default
  let kg = measures.SI "kilogram"
  let m = measures.SI "meter"
  let float_kg = measures.AnnotateType(typeof<float>,[kg])
```

  Para fornecer o tipo `Nullable<decimal<kg/m^2>>` , use o seguinte código:

```fsharp
  let kgpm2 = measures.Ratio(kg, measures.Square m)
  let dkgpm2 = measures.AnnotateType(typeof<decimal>,[kgpm2])
  let nullableDecimal_kgpm2 = typedefof<System.Nullable<_>>.MakeGenericType [|dkgpm2 |]
```

### <a name="accessing-project-local-or-script-local-resources"></a>Acessando projetos-recursos locais ou de script local

Cada instância de um provedor de tipos pode receber um `TypeProviderConfig` valor durante a construção. Esse valor contém a "pasta de resolução" para o provedor (ou seja, a pasta do projeto para a compilação ou o diretório que contém um script), a lista de assemblies referenciados e outras informações.

### <a name="invalidation"></a>Invalidação

Os provedores podem gerar sinais de invalidação para notificar o serviço de linguagem F # que as suposições de esquema podem ter alterado. Quando ocorrer invalidação, um typecheck será refeito se o provedor estiver sendo hospedado no Visual Studio. Esse sinal será ignorado quando o provedor for hospedado em F# Interativo ou pelo compilador F # (FSC. exe).

### <a name="caching-schema-information"></a>Armazenando em cache informações de esquema

Os provedores geralmente devem armazenar em cache o acesso a informações de esquema. Os dados armazenados em cache devem ser armazenados usando um nome de arquivo que é fornecido como um parâmetro estático ou como dados do usuário. Um exemplo de cache de esquema é o `LocalSchemaFile` parâmetro nos provedores de tipos no `FSharp.Data.TypeProviders` assembly. Na implementação desses provedores, esse parâmetro estático direciona o provedor de tipos para usar as informações de esquema no arquivo local especificado em vez de acessar a fonte de dados pela rede. Para usar as informações de esquema em cache, você também deve definir o parâmetro estático `ForceUpdate` como `false` . Você pode usar uma técnica semelhante para habilitar o acesso a dados online e offline.

### <a name="backing-assembly"></a>Assembly de backup

Quando você compila um `.dll` `.exe` arquivo ou, o arquivo. dll de backup para tipos gerados é vinculado estaticamente ao assembly resultante. Esse link é criado copiando as definições de tipo IL (linguagem intermediária) e todos os recursos gerenciados do assembly de backup para o assembly final. Quando você usa F# Interativo, o arquivo. dll de backup não é copiado e, em vez disso, é carregado diretamente no processo de F# Interativo.

### <a name="exceptions-and-diagnostics-from-type-providers"></a>Exceções e diagnósticos de provedores de tipos

Todos os usos de todos os membros de tipos fornecidos podem gerar exceções. Em todos os casos, se um provedor de tipos lançar uma exceção, o compilador do host irá reattributeá-lo para um provedor de tipos específico.

- Exceções de provedor de tipo nunca devem resultar em erros de compilador interno.

- Os provedores de tipos não podem relatar avisos.

- Quando um provedor de tipos é hospedado no compilador F #, em um ambiente de desenvolvimento F # ou F# Interativo, todas as exceções desse provedor são detectadas. A propriedade Message sempre é o texto de erro e nenhum rastreamento de pilha é exibido. Se você for lançar uma exceção, poderá lançar os seguintes exemplos: `System.NotSupportedException` , `System.IO.IOException` , `System.Exception` .

#### <a name="providing-generated-types"></a>Fornecendo tipos gerados

Até agora, este documento explicou como fornecer tipos apagados. Você também pode usar o mecanismo do provedor de tipos em F # para fornecer tipos gerados, que são adicionados como definições de tipo .NET reais no programa dos usuários. Você deve referir-se aos tipos fornecidos gerados usando uma definição de tipo.

```fsharp
open Microsoft.FSharp.TypeProviders

type Service = ODataService<"http://services.odata.org/Northwind/Northwind.svc/">
```

O código auxiliar ProvidedTypes-0,2 que faz parte da versão F # 3,0 tem apenas suporte limitado para fornecer tipos gerados. As instruções a seguir devem ser verdadeiras para uma definição de tipo gerada:

- `isErased` deve ser definido como `false`.

- O tipo gerado deve ser adicionado a um recém-criado `ProvidedAssembly()` , que representa um contêiner para fragmentos de código gerados.

- O provedor deve ter um assembly que tenha um arquivo .NET. dll de backup real com um arquivo. dll correspondente no disco.

## <a name="rules-and-limitations"></a>Regras e limitações

Ao escrever provedores de tipo, mantenha as seguintes regras e limitações em mente.

### <a name="provided-types-must-be-reachable"></a>Os tipos fornecidos devem estar acessíveis

Todos os tipos fornecidos devem ser acessíveis a partir de tipos não aninhados. Os tipos não aninhados são fornecidos na chamada ao `TypeProviderForNamespaces` Construtor ou a uma chamada para `AddNamespace` . Por exemplo, se o provedor fornecer um tipo `StaticClass.P : T` , você deve garantir que T seja um tipo não aninhado ou aninhado em um.

Por exemplo, alguns provedores têm uma classe estática, como `DataTypes` a que contém esses `T1, T2, T3, ...` tipos. Caso contrário, o erro indica que uma referência ao tipo T no assembly A foi encontrada, mas o tipo não pôde ser encontrado nesse assembly. Se esse erro for exibido, verifique se todos os seus subtipos podem ser acessados dos tipos de provedor. Observação: esses `T1, T2, T3...` tipos são chamados de tipos *em funcionamento* . Lembre-se de colocá-los em um namespace acessível ou em um tipo pai.

### <a name="limitations-of-the-type-provider-mechanism"></a>Limitações do mecanismo do provedor de tipos

O mecanismo do provedor de tipos no F # tem as seguintes limitações:

- A infraestrutura subjacente para provedores de tipo no F # não dá suporte a tipos genéricos fornecidos ou métodos genéricos fornecidos.

- O mecanismo não dá suporte a tipos aninhados com parâmetros estáticos.

## <a name="development-tips"></a>Dicas de desenvolvimento

Você pode encontrar as seguintes dicas úteis durante o processo de desenvolvimento:

### <a name="run-two-instances-of-visual-studio"></a>Executar duas instâncias do Visual Studio

Você pode desenvolver o provedor de tipos em uma instância e testar o provedor no outro porque o IDE de teste usará um bloqueio no arquivo. dll que impede que o provedor de tipos seja recriado. Portanto, você deve fechar a segunda instância do Visual Studio enquanto o provedor é criado na primeira instância e, em seguida, deve reabrir a segunda instância depois que o provedor é compilado.

### <a name="debug-type-providers-by-using-invocations-of-fscexe"></a>Depurar provedores de tipo usando invocações de FSC. exe

Você pode invocar provedores de tipo usando as seguintes ferramentas:

- FSC. exe (compilador de linha de comando F #)

- FSI. exe (o compilador de F# Interativo)

- devenv. exe (Visual Studio)

Geralmente, é possível depurar provedores de tipos com mais facilidade usando o FSC. exe em um arquivo de script de teste (por exemplo, script. fsx). Você pode iniciar um depurador a partir de um prompt de comando.

```console
devenv /debugexe fsc.exe script.fsx
```

  Você pode usar o log de impressão para stdout.

## <a name="see-also"></a>Veja também

- [Provedores de Tipos](index.md)
- [O SDK do provedor de tipos](https://github.com/fsprojects/FSharp.TypeProviders.SDK)
