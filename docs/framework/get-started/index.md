---
title: Introdução ao .NET Framework
description: Comece a usar o .NET, que é um ambiente de execução de tempo de execução que gerencia aplicativos. Ele contém um Common Language Runtime (CLR) e uma extensa biblioteca de classes.
ms.date: 04/02/2019
helpviewer_keywords:
- .NET Framework, getting started
- getting started [.NET Framework]
ms.assetid: c693fd34-88fe-4d90-b332-19eeadf3b7e7
ms.openlocfilehash: 1d6b1fccd9751180ee096531a34b2afb60547072
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557315"
---
# <a name="get-started-with-net-framework"></a>Introdução ao .NET Framework

.NET Framework é um ambiente de execução de tempo de execução que gerencia os aplicativos direcionados .NET Framework. Ele consiste no Common Language Runtime, que fornece gerenciamento de memória e outros serviços do sistema, além de em uma biblioteca de classes extensa, o que permite que programadores usem o código robusto e confiável para todas as áreas principais do desenvolvimento de aplicativos.

> [!NOTE]
> .NET Framework está disponível somente em sistemas Windows. Você pode usar o [.NET Core](../../core/index.yml) para desenvolver e executar aplicativos no Windows, no MacOS e no Linux.

## <a name="what-is-net-framework"></a>O que é .NET Framework?

.NET Framework é um ambiente de execução gerenciado para Windows que fornece uma variedade de serviços para seus aplicativos em execução. Ele consiste em dois componentes principais: o CLR (Common Language Runtime), o mecanismo de execução que manipula aplicativos em execução, e a biblioteca de classes .NET Framework, que oferece uma biblioteca de códigos testados e reutilizáveis que os desenvolvedores podem chamar de seus próprios aplicativos. Os serviços que .NET Framework fornece aos aplicativos em execução incluem o seguinte:

- Gerenciamento de memória. Em muitas linguagens de programação, os programadores são responsáveis por alocar e liberar memória e por identificar o tempo de vida do objeto. Em aplicativos do .NET Framework, o CLR fornece esses serviços em nome do aplicativo.

- Um Common Type System. Em linguagens de programação tradicionais, os tipos básicos são definidos pelo compilador, que complica a interoperabilidade entre linguagens. No .NET Framework, os tipos básicos são definidos pelo sistema de tipo de .NET Framework e são comuns a todas as linguagens que se destinam .NET Framework.

- Uma biblioteca de classes abrangente. Em vez de precisar gravar grandes volumes de código para manipular operações de programação comuns de baixo nível, os programadores usam uma biblioteca de tipos facilmente acessível e seus membros da biblioteca de classes .NET Framework.

- Estruturas e tecnologias de desenvolvimento. O .NET Framework inclui bibliotecas para áreas específicas de desenvolvimento de aplicativos, como ASP.NET para aplicativos Web, ADO.NET para acesso a dados, Windows Communication Foundation para aplicativos orientados a serviços e Windows Presentation Foundation para aplicativos da área de trabalho do Windows.

- Interoperabilidade da linguagem. Os compiladores de linguagem que visam .NET Framework emitem um código intermediário chamado Common Intermediate Language (CIL), que, por sua vez, é compilado no tempo de execução pelo Common Language Runtime. Com esse recurso, as rotinas gravadas em uma linguagem são acessíveis a outras linguagens, e os programadores privilegiam a criação de aplicativos em suas linguagens preferidas.

- Compatibilidade de versões. Com exceções raras, os aplicativos que são desenvolvidos usando uma versão específica do .NET Framework são executados sem modificação em uma versão posterior.

- Execução lado a lado. .NET Framework ajuda a resolver conflitos de versão, permitindo que várias versões do Common Language Runtime existam no mesmo computador. Isso significa que várias versões de aplicativos podem coexistir e que um aplicativo pode ser executado na versão do .NET Framework com a qual foi criado. A execução lado a lado aplica-se aos grupos de versão do .NET Framework 1.0/1.1, 2.0/3.0/3.5 e 4/4.5.x/4.6.x/4.7.x/4.8.

- Multiplataforma. Ao direcionar o [.NET Standard](../../standard/net-standard.md), desenvolvedores criam bibliotecas de classes que funcionam em várias plataformas do .NET Framework com suporte por essa versão do padrão. Por exemplo, as bibliotecas que visam .NET Standard 2,0 podem ser usadas por aplicativos direcionados .NET Framework 4.6.1, .NET Core 2,0 e UWP 10.0.16299.

<a name="ForUsers"></a>
## <a name="the-net-framework-for-users"></a>O .NET Framework para usuários

Se você não desenvolver .NET Framework aplicativos, mas usá-los, não será necessário ter conhecimento específico sobre .NET Framework ou sua operação. Na maior parte, a estrutura é completamente transparente para os usuários.

Se você estiver usando o sistema operacional Windows, .NET Framework talvez já esteja instalado no seu computador. Além disso, se você instalar um aplicativo que requer .NET Framework, o programa de instalação do aplicativo poderá instalar uma versão específica da estrutura em seu computador. Em alguns casos, você poderá ver uma caixa de diálogo solicitando a instalação do .NET Framework. Se você já tentou executar um aplicativo quando essa caixa de diálogo é exibida e se o computador tem acesso à Internet, você pode ir para uma página da Web que permite instalar a versão ausente do .NET Framework. Para obter mais informações, consulte o [Guia de instalação](../install/index.md).

Em geral, você não deve desinstalar versões do .NET Framework que estão instaladas em seu computador. Há dois motivos para isso:

- Se um aplicativo que você usa depende de uma versão específica do .NET Framework, esse aplicativo poderá ser interrompido se essa versão for removida.

- Algumas versões do .NET Framework são atualizações in-loco para versões anteriores. Por exemplo, .NET Framework 3,5 é uma atualização in-loco para a versão 2,0 e .NET Framework 4,8 é uma atualização in-loco para as versões 4 por meio do 4.7.2. Para obter mais informações, confira [Versões e dependências do .NET Framework](../migration-guide/versions-and-dependencies.md).

Em versões do Windows anteriores ao Windows 8, se você optar por remover .NET Framework, sempre use **programas e recursos** do painel de controle para desinstalá-lo. Nunca remova uma versão de .NET Framework manualmente. No Windows 8 e versões posteriores, .NET Framework é um componente do sistema operacional e não pode ser desinstalado independentemente.

Várias versões do .NET Framework podem coexistir em um único computador ao mesmo tempo. Isso significa que você não precisa desinstalar as versões anteriores para instalar uma versão posterior.

## <a name="net-framework-for-developers"></a>.NET Framework para desenvolvedores

Se você for um desenvolvedor, escolha qualquer linguagem de programação que dê suporte a .NET Framework para criar seus aplicativos. Como .NET Framework fornece independência de linguagem e interoperabilidade, você interage com outros aplicativos e componentes de .NET Framework, independentemente da linguagem com a qual foram desenvolvidos.

Para desenvolver aplicativos ou componentes do .NET Framework, faça o seguinte:

1. Se não for pré-instalado no seu sistema operacional, instale a versão do .NET Framework que seu aplicativo irá direcionar. A versão de produção mais recente é .NET Framework 4,8. Ele é pré-instalado no Windows 10 pode ser atualizado para 2019 e está disponível para download em versões anteriores do sistema operacional Windows. Para conhecer os requisitos de sistema do .NET Framework, confira [Requisitos de sistema](system-requirements.md). Para obter informações sobre como instalar outras versões do .NET Framework, consulte o [Guia de instalação](../install/guide-for-developers.md). Os pacotes adicionais do .NET Framework são lançados fora de banda, o que significa que eles são lançados de forma contínua fora de um ciclo de lançamento regular ou agendado. Para obter informações sobre esses pacotes, consulte [.NET Framework e versões fora de banda](the-net-framework-and-out-of-band-releases.md).

2. Selecione o idioma ou os idiomas com suporte na versão .NET Framework que você pretende usar para desenvolver seus aplicativos. Há um grande número de linguagens disponível, incluindo [Visual Basic](../../visual-basic/index.yml), [C#](../../csharp/index.yml), [F#](../../fsharp/index.yml) e [C++/CLI](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp) da Microsoft. (Uma linguagem de programação que permite desenvolver aplicativos para .NET Framework segue a [especificação da CLI (Common Language Infrastructure)](https://visualstudio.microsoft.com/license-terms/ecma-c-common-language-infrastructure-standards/).)

3. Para criar seus aplicativos , selecione e instale o ambiente de desenvolvimento a ser usado que dê suporte à linguagem ou às linguagens de programação selecionadas. O IDE (ambiente de desenvolvimento integrado) da Microsoft para aplicativos .NET Framework é o [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link). Está disponível em várias edições.

Para obter mais informações sobre como desenvolver aplicativos direcionados .NET Framework, consulte o [Guia de desenvolvimento](../development-guide.md).

## <a name="related-articles"></a>Artigos relacionados

| Title | Descrição |
| ----- |------------ |
| [Visão geral](overview.md) | Fornece informações detalhadas para os desenvolvedores que criam aplicativos direcionados .NET Framework. |
| [Guia de instalação](../install/index.md) | Fornece informações sobre como instalar .NET Framework. |  
| [.NET Framework e versões fora de banda](the-net-framework-and-out-of-band-releases.md) | Descreve as versões fora de faixa do .NET Framework e como usá-las em seu aplicativo. |
| [Requisitos do sistema](system-requirements.md) | Lista os requisitos de hardware e software para executar o .NET Framework. |
| [.NET Core e software livre](net-core-and-open-source.md) | Descreve o .NET Core em relação a .NET Framework e como acessar os projetos .NET Core de código-fonte aberto. |
| [Documentação do .NET Core](../../core/index.yml) | Fornece a documentação conceitual e de referência de API para .NET Core. |
| [.NET Standard](../../standard/net-standard.md) | Discute .NET Standard, uma especificação com controle de versão com suporte das implementações individuais do .NET para garantir que um conjunto consistente de APIs esteja disponível em várias plataformas.

## <a name="see-also"></a>Confira também

- [Guia do .NET Framework](../index.yml)
- [Novidades](../whats-new/index.md)
- [Navegador de API do .NET](../../../api/index.md)
- [Guia de desenvolvimento](../development-guide.md)
