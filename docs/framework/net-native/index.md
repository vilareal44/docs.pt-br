---
title: Compilando aplicativos com o .NET Nativo
ms.date: 03/30/2017
helpviewer_keywords:
- native compilation
- .NET and native code
- compilation with .NET Native
- .NET Native
- C# and native compilation
ms.assetid: 47cd5648-9469-4b1d-804c-43cc04384045
ms.openlocfilehash: 1f176e81905fe68c6d740a13240fe814659a7a59
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128384"
---
# <a name="compiling-apps-with-net-native"></a>Compilando aplicativos com o .NET Nativo

.NET Native é uma tecnologia de pré-compilação para a criação e implantação de aplicativos do Windows que estão incluídos no Visual Studio 2015 e versões posteriores. Ele compila automaticamente a versão de lançamento de aplicativos escritos em código gerenciado (C# ou Visual Basic) e que são destinados ao .NET Framework e ao Windows 10 para código nativo.

Normalmente, os aplicativos com destino para o .NET Framework são compilados para IL (linguagem intermediária). No tempo de execução, uma compilação JIT (just-in-time) converte o IL em código nativo. Por outro lado, .NET Native compila os aplicativos do Windows diretamente para o código nativo. Para desenvolvedores, isso significa que:

- Seus aplicativos apresentam o desempenho do código nativo. Normalmente, o desempenho será superior ao código que é compilado primeiro para IL e compilado para código nativo pelo compilador JIT.

- Você pode continuar a programar em C# ou em Visual Basic.

- Você pode continuar a aproveitar os recursos fornecidos pelo .NET Framework, incluindo sua biblioteca de classes, coleta de lixo, gerenciamento automático de memória e manipulação de exceção.

Para usuários de seus aplicativos, o .NET Native oferece essas vantagens:

- Tempos de execução mais rápidos para a maioria dos aplicativos e cenários.

- Tempos de inicialização mais rápidos para a maioria dos aplicativos e cenários.

- Baixo custo de implantação e atualização.

- Uso otimizado de memória do aplicativo.

> [!IMPORTANT]
> Para a grande maioria dos aplicativos e cenários, o .NET Native oferece tempos de inicialização significativamente mais rápidos e desempenho superior quando comparado a um aplicativo compilado para IL ou a uma imagem NGEN. No entanto, os resultados podem variar. Para garantir que seu aplicativo tenha se beneficiado dos aprimoramentos de desempenho do .NET Native, você deve comparar seu desempenho com o da versão nativa non-.NET do seu aplicativo. Para obter mais informações, consulte [visão geral da sessão de desempenho](https://docs.microsoft.com/visualstudio/profiling/performance-session-overview).

Mas .NET Native envolve mais do que uma compilação para código nativo. Ele transforma a maneira que os aplicativos .NET Framework são criados e executados. Especialmente:

- Durante a pré-compilação, partes necessárias do .NET Framework são vinculadas estaticamente ao seu aplicativo. Isso permite que o aplicativo seja executado com as bibliotecas de aplicativo local do .NET Framework e o compilador realize análises globais para proporcionar vantagens de desempenho. Como resultado, os aplicativos são inicializados consistentemente mais rápido mesmo depois de atualizações do .NET Framework.

- O tempo de execução de .NET Native é otimizado para pré-compilação estática e, na grande maioria dos casos, oferece desempenho superior. Ao mesmo tempo, ele mantém os recursos de reflexão principais que os desenvolvedores acham tão produtivos.

- .NET Native usa o mesmo back-end que o compilador C++, que é otimizado para cenários de pré-compilação estática.

.NET Native é capaz de trazer os benefícios de desempenho do C++ para os desenvolvedores de código gerenciado, pois ele usa as mesmas ou ferramentas semelhantes que o C++ nos bastidores, conforme mostrado nesta tabela.

||.NET Nativo|C++|
|-|----------------------------------------------------------------|-----------|
|Bibliotecas|O .NET Framework + Windows Runtime|Win32 + Windows Runtime|
|Compilador|Compilador de otimização de UTC|Compilador de otimização de UTC|
|Implantado|Binários prontos para execução|Binários prontos para execução (ASM)|
|Runtime|MRT.dll (runtime de CLR mínimo)|CRT.dll (runtime C)|

Para aplicativos do Windows para Windows 10, carregue os binários de compilação de código do .NET Native nos pacotes do aplicativo (arquivos .appx) para a Windows Store.

## <a name="in-this-section"></a>Nesta seção

Para obter mais informações sobre o desenvolvimento de aplicativos com compilação de código do .NET Nativo, consulte estes tópicos:

- [Guia de Introdução à compilação de código do .NET Native: passo a passo da experiência de desenvolvedor](getting-started-with-net-native.md)

- [Compilação e .NET Native:](net-native-and-compilation.md) como o .NET Native compila seu projeto para código nativo.

- [Reflexão e .NET Nativo](reflection-and-net-native.md)

  - [APIs que dependem de reflexão](apis-that-rely-on-reflection.md)

  - [Referência da API de Reflexão](net-native-reflection-api-reference.md)

  - [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)

- [Serialização e metadados](serialization-and-metadata.md)

- [Migrando seu aplicativo da Windows Store para .NET Nativo](migrating-your-windows-store-app-to-net-native.md)

- [Solução de problemas gerais do .NET Nativo](net-native-general-troubleshooting.md)
