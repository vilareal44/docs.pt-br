---
title: Glossário .NET
description: Descubra o significado de termos selecionados usados na documentação do .NET.
ms.date: 01/22/2019
ms.technology: dotnet-standard
ms.openlocfilehash: 529b1d9142ddf7982a6712c355c10666f0414d73
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163112"
---
# <a name="net-glossary"></a>Glossário .NET

A principal meta deste glossário é esclarecer os significados de termos e acrônimos selecionados que aparecem com frequência na documentação do .NET sem definições.

## <a name="aot"></a>AOT

Compilador Ahead-of-Time.

Semelhante ao [JIT](#jit), esse compilador também converte [IL](#il) em código de máquina. Diferentemente da compilação JIT, a compilação AOT acontece antes que o aplicativo seja executado e normalmente é executada em um computador diferente. Como as cadeias de ferramentas da AOT não são compiladas em tempo de execução, elas não precisam minimizar o tempo gasto na compilação. Isso significa que elas podem gastar mais tempo em otimização. Como o contexto da AOT é o aplicativo inteiro, o compilador AOT também executa a vinculação de módulo cruzado e a análise de programa inteiro, o que significa que todas as referências são seguidas e um único executável é produzido.

Confira [CoreRT](#corert) e [.NET Native](#net-native).

## <a name="aspnet"></a>ASP.NET

A implementação do ASP.NET original que é fornecida com o .NET Framework.

Às vezes, o ASP.NET é um termo abrangente que se refere a ambas as implementações de ASP.NET, incluindo o ASP.NET Core. O significado que o termo carrega em uma determinada instância é determinado pelo contexto. Consulte ASP.NET 4. x quando quiser deixar claro que você não está usando o ASP.NET para significar ambas as implementações.

Confira [Documentação do ASP.NET](/aspnet/#pivot=aspnet).

## <a name="aspnet-core"></a>ASP.NET Core

Uma implementação de plataforma cruzada, de alto desempenho e de software livre do ASP.NET criada no .NET Core.

Confira [Documentação do ASP.NET Core](/aspnet/#pivot=core).

## <a name="assembly"></a>assembly

Um arquivo *. dll* / *. exe* que pode conter uma coleção de APIs que podem ser chamadas por aplicativos ou outros assemblies.

Um assembly pode incluir tipos como interfaces, classes, estruturas, enumerações e delegados. Às vezes, os assemblies em uma pasta *bin* de projeto são chamados de *binários*. Consulte também [biblioteca](#library).

## <a name="clr"></a>CLR

Common Language Runtime.

O significado exato depende do contexto, mas o tempo de execução de linguagem comum geralmente se refere ao tempo de execução de .NET Framework. O CLR manipula a alocação e o gerenciamento de memória. O CLR também é uma máquina virtual que não só executa aplicativos, mas também gera e compila código imediatamente usando um compilador [JIT](#jit) . A implementação atual do CLR da Microsoft é somente para Windows.

## <a name="coreclr"></a>CoreCLR

Common Language Runtime do .NET Core.

Esse CLR é criado com a mesma base de código que o CLR. Originalmente, o CoreCLR era o runtime do Silverlight e foi projetado para ser executado em várias plataformas, especificamente o Windows e OS X. Agora o CoreCLR faz parte do .NET Core e representa uma versão simplificada do CLR. Ainda é um runtime [multiplataforma](#cross-platform), incluindo também suporte para várias distribuições do Linux. O CoreCLR também é uma máquina virtual com recursos JIT e de execução de código.

## <a name="corefx"></a>CoreFx

BCL (biblioteca de classes base) do .NET Core

> [!TIP]
> *FX* significa *estrutura*.

Um conjunto de bibliotecas que compõem o sistema. \* (e com uma extensão limitada da Microsoft. \* ) namespaces. A BCL é uma estrutura de nível inferior e de uso geral, base para a criação de estruturas de aplicativo de nível mais alto, como o ASP.NET Core. O código-fonte da BCL do .NET Core está contido no [repositório do .NET Core Runtime](https://github.com/dotnet/runtime). No entanto, a maioria das APIs do .NET Core também estão disponíveis no .NET Framework, portanto você pode pensar no CoreFX como um fork da BCL do .NET Framework.

## <a name="corert"></a>CoreRT

runtime do .NET Core.

Ao contrário do CLR/CoreCLR, o CoreRT não é uma máquina virtual, o que significa que ele não inclui os recursos para gerar e executar código dinamicamente, já que não inclui um [JIT](#jit). No entanto, ele inclui o [GC](#gc) e a capacidade de RTTI (identificação de tipo de tempo de execução) e reflexão. Contudo, seu sistema de tipos é projetado para que os metadados para reflexão não sejam necessários. Não exigir metadados permite que uma cadeia de ferramentas [AOT](#aot) possa se vincular a metadados supérfluos e (o mais importante) identifique o código que o aplicativo não usa. O CoreRT está em desenvolvimento.

Consulte [introdução a .net Native e CoreRT](https://github.com/dotnet/corert/blob/master/Documentation/intro-to-corert.md).

## <a name="cross-platform"></a>várias plataformas

A capacidade de desenvolver e executar um aplicativo que pode ser usado em vários sistemas operacionais diferentes, como Linux, Windows e iOS, sem a necessidade de reescrever especificamente para cada um. Isso permite a reutilização de código e a consistência entre aplicativos em diferentes plataformas.

## <a name="ecosystem"></a>ecossistema

Todos os softwares de runtime, as ferramentas de desenvolvimento e os recursos da comunidade que são usados para compilar e executar aplicativos de uma determinada tecnologia.

O termo "Ecossistema do .NET" difere de termos semelhantes, como "Pilha do .NET", em relação à inclusão de bibliotecas e aplicativos de terceiros. Veja um exemplo em uma frase:

- "A motivação por trás do [.NET Standard](#net-standard) é estabelecer maior uniformidade no ecossistema do .NET."

## <a name="framework"></a>estrutura

Em geral, uma coleção abrangente de APIs que facilita o desenvolvimento e a implantação de aplicativos que são baseados em uma tecnologia específica. Nesse sentido geral, o ASP.NET Core e o Windows Forms são exemplos de estruturas de aplicativo. Consulte também [biblioteca](#library).

A palavra "estrutura" tem um significado técnico mais específico nos seguintes termos:

- [.NET Framework](#net-framework)
- [estrutura de destino](#target-framework)
- [TFM (Moniker de Estrutura de Destino)](#tfm)

Na documentação existente, "estrutura" às vezes se refere a uma [implementação do .NET](#implementation-of-net). Por exemplo, um artigo pode chamar o .NET Core de uma estrutura. Planejamos eliminar da documentação esse uso confuso da palavra.

## <a name="gc"></a>GC

Coletor de lixo.

O coletor de lixo é uma implementação do gerenciamento automático de memória.  O GC libera a memória ocupada por objetos que não estejam mais em uso.

Consulte [Coleta de lixo](garbage-collection/index.md).

## <a name="il"></a>IL

Linguagem intermediária.

As linguagens .NET de nível mais alto , como C#, são compiladas em um conjunto de instruções independente de hardware, que é chamado de IL (linguagem intermediária). A IL às vezes é conhecida como MSIL (Microsoft Intermediate Language) ou CIL (Common Intermediate Language).

## <a name="jit"></a>JIT

Compilador Just-In-Time.

Semelhante ao [AOT](#aot), esse compilador converte a [IL](#il) em um código de máquina que o processador entenda. Ao contrário da AOT, a compilação JIT acontece sob demanda e é executada no mesmo computador em que o código precisa ser executado. Como a compilação JIT ocorre durante a execução do aplicativo, o tempo de compilação faz parte do tempo de execução. Assim, os compiladores JIT precisam equilibrar o tempo gasto com a otimização do código em relação à economia que o código resultante pode produzir. Mas um JIT reconhece o hardware e pode evitar que os desenvolvedores tenham que fornecer implementações diferentes.

## <a name="implementation-of-net"></a>implementação do .NET

Uma implementação do .NET inclui:

- Um ou mais runtimes. Exemplos: CLR, CoreCLR e CoreRT.
- Uma biblioteca de classes que implemente uma versão do .NET Standard, podendo incluir APIs adicionais. Exemplos: biblioteca de classes base do NET Framework, biblioteca de classes base do .NET Core.
- Opcionalmente, uma ou mais estruturas de aplicativo. Exemplos: ASP.NET, Windows Forms e WPF estão incluídos no .NET Framework.
- Opcionalmente, ferramentas de desenvolvimento. Algumas ferramentas de desenvolvimento são compartilhadas entre várias implementações.

Exemplos de implementações do .NET:

- [.NET Framework](#net-framework)
- [.NET Core](#net-core)
- [UWP (Plataforma Universal do Windows)](#uwp)

## <a name="library"></a>biblioteca

Uma coleção de APIs que podem ser chamadas por aplicativos ou outras bibliotecas. Uma biblioteca do .NET é composta de um ou mais [assemblies](#assembly).

A biblioteca de palavras e a [estrutura](#framework) geralmente são usadas como sinônimos.

## <a name="metapackage"></a>metapacote

Um pacote NuGet que não tem nenhuma biblioteca própria, mas é apenas uma lista de dependências. Os pacotes incluídos podem, opcionalmente, estabelecer a API para uma estrutura de destino.

## <a name="mono"></a>Mono

Mono é uma implementação do .NET [multiplataforma](#cross-platform) de software livre usada principalmente quando é necessário um pequeno runtime. É o tempo de execução que capacita aplicativos Xamarin no Android, Mac, iOS, tvOS e watchOS e concentra-se principalmente em aplicativos que exigem uma pequena superfície.

Ele dá suporte a todas as versões do .NET Standard publicadas atualmente.

Historicamente, o Mono implementava a maior API do .NET Framework e emulava alguns dos recursos mais populares do Unix. Às vezes, ele é usado para executar aplicativos .NET que dependem desses recursos no Unix.

O Mono normalmente é usado com um compilador Just-In-Time, mas ele também apresenta um compilador estático completo (compilação Ahead Of Time) que é usado em plataformas como iOS.

Para saber mais sobre o Mono, consulte a [Documentação do Mono](https://www.mono-project.com/docs/).

## <a name="net"></a>.NET

O termo coletivo para [.NET Standard](#net-standard) e todas as [implementações de .NET](#implementation-of-net), bem como as cargas de trabalho. Sempre totalmente em maiúsculas, nunca ".net".

Consulte o [guia .net](index.yml)

## <a name="net-core"></a>.NET Core

Uma implementação de software livre de várias plataformas, de alto desempenho e de software livre do .NET. Inclui o CoreCLR (Core Common Language Runtime), o CoreRT (tempo de execução Core AOT, em desenvolvimento), a biblioteca de classes base do Core e o SDK do Core.

Consulte [.NET Core](../core/index.yml).

## <a name="net-core-cli"></a>CLI do .NET Core

Uma cadeia de ferramentas multiplataforma para o desenvolvimento de aplicativos .NET Core.

Consulte [CLI do .NET Core](../core/tools/index.md).

## <a name="net-core-sdk"></a>SDK do .Net Core

Um conjunto de bibliotecas e ferramentas que permitem aos desenvolvedores criar bibliotecas e aplicativos do .NET Core. Inclui a [CLI do .NET Core](#net-core-cli) para a criação de aplicativos, bibliotecas e runtime do .NET Core para criar e executar aplicativos e o executável do dotnet (*dotnet.exe*) que executa comandos de CLI e executa aplicativos.

Consulte a [Visão geral do SDK do .NET Core](../core/sdk.md).

## <a name="net-framework"></a>.NET Framework

Uma implementação do .NET que é executado somente no Windows. Inclui o CLR (Common Language Runtime), a Biblioteca de classes base e as bibliotecas de estrutura do aplicativo, como ASP.NET, Windows Forms e WPF.

Consulte o [Guia do .NET Framework](../framework/index.yml).

## <a name="net-native"></a>.NET Nativo

Uma cadeia de ferramentas de compilador que gera código nativo AOT (Ahead Of Time), em vez de JIT (Just-In-Time).

A compilação acontece no computador do desenvolvedor, semelhante à maneira como um compilador e vinculador C++ funciona. Ela remove código não utilizado e gasta mais tempo otimizando-o. Ele extrai o código de bibliotecas e os mescla no executável. O resultado é um módulo único que representa o aplicativo inteiro.

A UWP foi a primeira estrutura de aplicativo com suporte pelo .NET Native. Agora, há suporte para criação de aplicativos de console nativo para macOS, Windows e Linux.

Consulte [Introdução ao .NET Native e ao CoreRT](https://github.com/dotnet/corert/blob/master/Documentation/intro-to-corert.md)

## <a name="net-standard"></a>.NET Standard

Uma especificação formal das APIs do .NET que estão disponíveis em cada implementação do .NET.

Às vezes, a especificação do .NET Standard também é chamada de biblioteca na documentação. Como uma biblioteca inclui implementações de API e não apenas especificações (interfaces), é um equívoco chamar .NET Standard de "biblioteca". Planejamos eliminar esse uso da documentação, exceto em referência ao nome do metapacote do .NET Standard (`NETStandard.Library`).

Consulte [.NET Standard](net-standard.md).

## <a name="ngen"></a>NGEN

Geração (de imagem) nativa.

Você pode pensar nessa tecnologia como um compilador JIT persistente. Ela geralmente compila o código no computador em que o código é executado, mas a compilação normalmente ocorre no momento da instalação.

## <a name="package"></a>pacote

Um pacote NuGet &mdash; ou apenas um pacote &mdash; é um arquivo *.zip* com um ou mais assemblies de mesmo nome, junto com metadados adicionais, como o nome do autor.

O arquivo *.zip* tem uma extensão *.nupkg* e pode conter ativos, como arquivos *.dll* e arquivos *.xml*, para uso com várias versões e estruturas de destino. Quando instalado em um aplicativo ou uma biblioteca, os ativos apropriados são selecionados com base na estrutura de destino especificada pelo aplicativo ou pela biblioteca. Os ativos que definem a interface estão na pasta *ref* e os ativos que definem a implementação estão na pasta *lib*.

## <a name="platform"></a>plataforma

Um sistema operacional e o hardware em que ele é executado, como macOS, Windows, Linux, iOS e Android.

Veja alguns exemplos de uso nessas frases:

- "O .NET Core é uma implementação multiplataforma do .NET."
- "Os perfis de PCL representam plataformas da Microsoft enquanto que o .NET Standard é independente de plataforma."

A documentação do .NET frequentemente usa "plataforma .NET" para significar uma implementação do .NET ou a pilha do .NET, incluindo todas as implementações. Os dois usos tendem a ser confundidos com o significado (SO/hardware) principal, portanto planejamos eliminar esses usos da documentação.

## <a name="runtime"></a>runtime

O ambiente de execução de um programa gerenciado.

O SO faz parte do ambiente do runtime, mas não faz parte do runtime do .NET. Aqui estão alguns exemplos de runtimes do .NET:

- CLR (Common Language Runtime)
- Core Common Language Runtime (CoreCLR)
- .NET Native (para UWP)
- runtime Mono

Às vezes, a documentação do .NET usa "runtime" para se referir a uma implementação do .NET. Por exemplo, nas seguintes sentenças "runtime" deve ser substituído por "implementação":

- "Os diversos runtimes do .NET implementam versões específicas do .NET Standard."
- "Bibliotecas destinadas à execução em vários runtimes devem ter essa estrutura como destino." (referindo-se ao .NET Standard)
- "Os diversos runtimes do .NET implementam versões específicas do .NET Standard. … Cada versão de runtime do .NET anuncia a última versão do .NET Standard à qual ele dá suporte..."

Planejamos eliminar esse uso inconsistente.

## <a name="stack"></a>stack

Um conjunto de tecnologias de programação que são usadas para compilar e executar aplicativos.

"A pilha do .NET" refere-se ao .NET Standard e a todas as implementações do .NET. A frase "uma pilha do .NET" pode se referir a uma implementação do .NET.

## <a name="target-framework"></a>estrutura de destino

A coleção de APIs da qual um aplicativo ou biblioteca do .NET depende.

Um aplicativo ou uma biblioteca pode se destinar a uma versão do .NET Standard (por exemplo, .NET Standard 2.0), que é a especificação de um conjunto padronizado de APIs entre todas as implementações de .NET. Um aplicativo ou uma biblioteca também pode se destinar a uma versão de uma implementação específica do .NET, obtendo acesso a APIs específicas da implementação. Por exemplo, um aplicativo que tem como destino o Xamarin.iOS obtém acesso aos wrappers da API fornecidos para Xamarin iOS.

Para algumas estruturas de destino (por exemplo, o .NET Framework) as APIs disponíveis são definidas pelos assemblies que uma implementação do .NET instala em um sistema, que podem incluir as APIs de estrutura do aplicativo (por exemplo, ASP.NET, WinForms). Para estruturas de destino baseadas em pacote (como .NET Standard e .NET Core), a APIs de estrutura são definidas por pacotes instalados no aplicativo ou na biblioteca. Nesse caso, a estrutura de destino especifica implicitamente um metapacote que faz referência a todos os pacotes que, juntos, compõem a estrutura.

Consulte [Estruturas de destino](frameworks.md).

## <a name="tfm"></a>TFM

Moniker da estrutura de destino.

Um formato de token padronizado para especificar a estrutura de destino de um aplicativo ou uma biblioteca do .NET. As estruturas de destino são geralmente referenciadas por um nome curto, como `net462`. Os TFMs de formato longo (como. .NETFramework,Version=4.6.2) existem, mas não são geralmente usados para especificar uma estrutura de destino.

Consulte [Estruturas de destino](frameworks.md).

## <a name="uwp"></a>UWP

Plataforma Universal do Windows.

Uma implementação do .NET que é usada para criar aplicativos do Windows modernos e sensíveis ao toque, bem como software para a IoT (Internet das Coisas). Ele foi projetado para unificar os diferentes tipos de dispositivos que você talvez queira direcionar, incluindo PCs, tablets, telefones e até mesmo o Xbox. A UWP fornece muitos serviços, como um repositório centralizado de aplicativos, um ambiente de execução (AppContainer) e um conjunto de APIs do Windows para usar em vez das APIS do Win32 (WinRT). Os aplicativos podem ser escritos em C++, C#, Visual Basic e JavaScript. Ao usar C# e Visual Basic, as APIs do .NET são fornecidas pelo .NET Core.

## <a name="see-also"></a>Confira também

- [Guia do .NET](index.yml)
- [Guia de .NET Framework](../framework/index.yml)
- [.NET Core](../core/index.yml)
- [Visão geral do ASP.NET](/aspnet/index#pivot=aspnet)
- [Visão geral de ASP.NET Core](/aspnet/index#pivot=core)
