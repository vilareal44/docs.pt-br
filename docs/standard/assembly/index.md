---
title: Assemblies no .NET
description: Os assemblies são unidades fundamentais de implantação, controle de versão, reutilização, escopo de ativação e permissões de segurança para o. Aplicativos baseados em rede.
ms.date: 08/15/2019
ms.assetid: 149f5ca5-5b34-4746-9542-1ae43b2d0256
helpviewer_keywords:
- dynamic assemblies
- security [.NET Framework], boundaries
- boundaries of assemblies
- assemblies [.NET Framework], about
- assemblies [.NET Framework], boundaries
- reference scope boundaries
- assemblies [.NET Framework]
- version boundaries
- type boundaries
ms.openlocfilehash: 364a1a8c0fbaae93a02495aaf2e8c519ffb46451
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290935"
---
# <a name="assemblies-in-net"></a>Assemblies no .NET

Os assemblies formam as unidades fundamentais de implantação, controle de versão, reutilização, escopo de ativação e permissões de segurança para o. Aplicativos baseados em rede. Um assembly é uma coleção de tipos e recursos compilados para funcionar juntos e formar uma unidade lógica de funcionalidade. Os assemblies assumem a forma de arquivos executáveis (*. exe*) ou Dynamic Link Library (*. dll*) e são os blocos de construção de aplicativos .net. Eles oferecem ao Common Language Runtime as informações de que ele precisa para estar ciente das implementações de tipo.

No .NET Core e .NET Framework, você pode criar um assembly a partir de um ou mais arquivos de código-fonte. No .NET Framework, os assemblies podem conter um ou mais módulos. Isso permite que projetos maiores sejam planejados para que vários desenvolvedores possam trabalhar em módulos ou arquivos de código-fonte separados, que são combinados para criar um único assembly. Para obter mais informações sobre módulos, consulte [como compilar um assembly de multiarquivos](../../framework/app-domains/build-multifile-assembly.md).

Os assemblies têm as seguintes propriedades:

- Os assemblies são implementados como arquivos *. exe* ou *. dll* .

- Para bibliotecas direcionadas ao .NET Framework, você pode compartilhar assemblies entre aplicativos, colocando-os no [cache de assembly global (GAC)](../../framework/app-domains/gac.md). Você deve obter os assemblies de nome forte antes de poder incluí-los no GAC. Para obter mais informações, consulte [assemblies com nomes fortes](strong-named.md).

- Os assemblies serão carregados na memória somente se forem necessários. Se eles não forem usados, eles não serão carregados. Isso significa que os assemblies podem ser uma maneira eficiente de gerenciar recursos em projetos grandes.

- Você pode obter informações programaticamente sobre um assembly usando reflexão. Para obter mais informações, confira [Reflexão (C#)](../../csharp/programming-guide/concepts/reflection.md) ou [Reflexão (Visual Basic)](../../visual-basic/programming-guide/concepts/reflection.md).

- Você pode carregar um assembly apenas para inspecioná-lo usando a <xref:System.Reflection.MetadataLoadContext> classe no .NET Core e os <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A?displayProperty=nameWithType> <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A?displayProperty=nameWithType> métodos ou no .net Core e .NET Framework.

## <a name="assemblies-in-the-common-language-runtime"></a>Assemblies no Common Language Runtime

Os assemblies fornecem o Common Language Runtime com as informações de que ele precisa para estar ciente das implementações de tipo. Para o runtime, um tipo não existe fora do contexto de um assembly.

Um assembly define as seguintes informações:

- Código que o Common Language Runtime é executado. Observe que cada assembly pode ter apenas um ponto de entrada: `DllMain` , `WinMain` ou `Main` .

- Limite de segurança. Um assembly é a unidade na qual as permissões são solicitadas e concedidas. Para obter mais informações sobre limites de segurança em assemblies, consulte [considerações de segurança do assembly](security-considerations.md).

- Limite de tipo. A identidade de cada tipo inclui o nome do assembly no qual ele reside. Um tipo chamado `MyType`, carregado no escopo de um assembly, não é o mesmo de um tipo chamado `MyType`, carregado no escopo de outro assembly.

- Limite de escopo de referência. O [manifesto do assembly](#assembly-manifest) tem metadados que são usados para resolver tipos e satisfazer solicitações de recursos. O manifesto especifica os tipos e recursos a serem expostos fora do assembly e enumera outros assemblies dos quais ele depende. O código MSIL (Microsoft Intermediate Language) em um arquivo executável portátil (PE) não será executado, a menos que tenha um [manifesto de assembly](#assembly-manifest)associado.

- Limite de versão. O assembly é a menor unidade com controle de versão no Common Language Runtime. Todos os tipos e recursos no mesmo assembly têm controle de versão como uma unidade. O [manifesto do assembly](#assembly-manifest) descreve as dependências de versão que você especifica para quaisquer assemblies dependentes. Para obter mais informações sobre o controle de versão, consulte [controle de versão do assembly](versioning.md).

- Unidade de implantação. Quando um aplicativo é iniciado, somente os assemblies que o aplicativo chama inicialmente devem estar presentes. Outros assemblies, como assemblies que contêm recursos de localização ou classes utilitárias, podem ser recuperados sob demanda. Isso permite que os aplicativos sejam simples e finos quando baixados pela primeira vez. Para obter mais informações sobre a implantação de assemblies, consulte [implantar aplicativos](../../framework/deployment/index.md).

- Unidade de execução lado a lado. Para obter mais informações sobre como executar várias versões de um assembly, consulte [assemblies e execução lado a lado](side-by-side-execution.md).

## <a name="create-an-assembly"></a>Criar um assembly

Assemblies podem ser estáticos ou dinâmicos. Assemblies estáticos são armazenados em disco em arquivos PE. Os assemblies estáticos podem incluir interfaces, classes e recursos como bitmaps, arquivos JPEG e outros arquivos de recurso. Você também pode criar assemblies dinâmicos, que são executados diretamente da memória e não são salvos em disco antes da execução. Você pode salvar assemblies dinâmicos em disco após sua execução.

Existem várias maneiras de criar assemblies. Você pode usar as ferramentas de desenvolvimento, como o Visual Studio, que podem criar arquivos *. dll* ou *. exe* . Você pode usar ferramentas no SDK do Windows para criar assemblies com módulos de outros ambientes de desenvolvimento. Você também pode usar APIs do Common Language Runtime, como <xref:System.Reflection.Emit?displayProperty=nameWithType>, a fim de criar assemblies dinâmicos.

Compile assemblies Criando-os no Visual Studio, criando-os com ferramentas de interface de linha de comando do .NET Core ou compilando assemblies de .NET Framework com um compilador de linha de comando. Para obter mais informações sobre como criar assemblies usando CLI do .NET Core, consulte [CLI do .NET Core visão geral](../../core/tools/index.md). Para compilar assemblies com os compiladores de linha de comando, consulte [compilação de linha de comando com CSC. exe](../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md) para C# ou [Build na linha de comando](../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md) para Visual Basic.

> [!NOTE]
> Para criar um assembly no Visual Studio, no menu **Compilar** , selecione **Compilar**.

## <a name="assembly-manifest"></a>Manifesto do assembly

Cada assembly tem um arquivo de *manifesto do assembly* . Semelhante a um sumário, o manifesto do assembly contém:

- A identidade do assembly (seu nome e versão).

- Uma tabela de arquivos que descreve todos os outros arquivos que compõem o assembly, como outros assemblies que você criou que o arquivo *. exe* ou *. dll* depende, arquivos de bitmap ou arquivos Leiame.

- Uma *lista de referências de assembly*, que é uma lista de todas as dependências externas, como *. dll*s ou outros arquivos. As referências de assembly contêm referências a objetos globais e privados. Os objetos globais estão disponíveis para todos os outros aplicativos. No .NET Core, os objetos globais são acoplados a um tempo de execução específico do .NET Core. No .NET Framework, os objetos globais residem no GAC (cache de assembly global). *System. IO. dll* é um exemplo de um assembly no GAC. Os objetos privados devem estar em um nível de diretório no ou abaixo do diretório no qual seu aplicativo está instalado.

Como os assemblies contêm informações sobre conteúdo, controle de versão e dependências, os aplicativos que os usam não precisam depender de fontes externas, como o registro em sistemas Windows, para funcionar corretamente. Os assemblies reduzem conflitos *. dll* e tornam seus aplicativos mais confiáveis e fáceis de implantar. Em muitos casos, você pode instalar um aplicativo baseado em .NET simplesmente copiando seus arquivos para o computador de destino. Para obter mais informações, consulte [manifesto do assembly](manifest.md).

## <a name="add-a-reference-to-an-assembly"></a>Adicionar uma referência a um assembly

Para usar um assembly em um aplicativo, você deve adicionar uma referência a ele. Depois que um assembly é referenciado, todos os tipos, propriedades, métodos e outros membros acessíveis de seus namespaces estão disponíveis para seu aplicativo como se o código fosse parte de seu arquivo de origem.

> [!NOTE]
> A maioria dos assemblies da Biblioteca de Classes .NET é referenciada automaticamente. Se um assembly do sistema não for referenciado automaticamente, para o .NET Core, você poderá adicionar uma referência ao pacote NuGet que contém o assembly. Use o Gerenciador de pacotes NuGet no Visual Studio ou adicione um [\<PackageReference>](../../core/tools/dependencies.md#the-packagereference-element) elemento para o assembly ao projeto *. csproj* ou *. vbproj* . No .NET Framework, você pode adicionar uma referência ao assembly usando a caixa de diálogo **Adicionar referência** no Visual Studio ou usando a opção de `-reference` linha de comando para compiladores [C#](../../csharp/language-reference/compiler-options/reference-compiler-option.md) ou [Visual Basic](../../visual-basic/reference/command-line-compiler/reference.md) .

No C#, você pode usar duas versões do mesmo assembly em um único aplicativo. Para obter mais informações, consulte [alias externo](../../csharp/language-reference/keywords/extern-alias.md).

## <a name="related-content"></a>Conteúdo relacionado

|Title|Descrição|
|-----------|-----------------|
|[Conteúdos do assembly](contents.md)|Elementos que compõem um assembly.|
|[Manifesto do assembly](manifest.md)|Dados no manifesto do assembly e como eles são armazenados em assemblies.|
|[Cache de assembly global](../../framework/app-domains/gac.md)|Como o GAC armazena e usa assemblies.|
|[Assemblies de nome forte](strong-named.md)|Características de assemblies de nome forte.|
|[Considerações sobre a segurança do assembly](security-considerations.md)|Como funciona a segurança com assemblies.|
|[Controle de versão do assembly](versioning.md)|Visão geral da política de controle de versão do .NET Framework.|
|[Posicionamento do assembly](../../framework/app-domains/assembly-placement.md)|Onde localizar os assemblies.|
|[Assemblies e execução lado a lado](side-by-side-execution.md)|Use várias versões do tempo de execução ou um assembly simultaneamente.|
|[Emissão de métodos e assemblies dinâmicos](../../framework/reflection-and-codedom/emitting-dynamic-methods-and-assemblies.md)|Como criar assemblies dinâmicos.|
|[Como o tempo de execução localiza assemblies](../../framework/deployment/how-the-runtime-locates-assemblies.md)|Como o .NET Framework resolve referências de assembly em tempo de execução.|

## <a name="reference"></a>Referência

<xref:System.Reflection.Assembly?displayProperty=nameWithType>

## <a name="see-also"></a>Veja também

- [Formato de arquivo do assembly .NET](file-format.md)
- [Assemblies amigáveis](friend.md)
- [Assemblies de referência](reference-assemblies.md)
- [Como carregar e descarregar assemblies](load-unload.md)
- [Como usar e depurar a descargabilidade do assembly no .NET Core](unloadability.md)
- [Como determinar se um arquivo é um assembly](identify.md)
- [Como inspecionar o conteúdo do assembly usando MetadataLoadContext](inspect-contents-using-metadataloadcontext.md)
