---
title: Assemblies de referência
description: Saiba mais sobre assemblies de referência, um tipo especial de assemblies no .NET que contém apenas a superfície da API pública da biblioteca
author: MSDN-WhiteKnight
ms.date: 09/12/2019
ms.technology: dotnet-standard
ms.openlocfilehash: 43a9dab037f4d0f1926ff67f8f38eaa6734a6d67
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164526"
---
# <a name="reference-assemblies"></a>Assemblies de referência

Os *assemblies de referência* são um tipo especial de assembly que contém apenas a quantidade mínima de metadados necessários para representar a superfície da API pública da biblioteca. Eles incluem declarações para todos os membros que são significativos ao referenciar um assembly em ferramentas de compilação, mas excluem todas as implementações de membro e declarações de membros privados que não têm impacto observável em seu contrato de API. Por outro lado, os assemblies regulares são chamados de *assemblies de implementação*.

Os assemblies de referência não podem ser carregados para execução, mas podem ser passados como entrada do compilador da mesma maneira que os assemblies de implementação. Normalmente, os assemblies de referência são distribuídos com o Software Development Kit (SDK) de uma plataforma ou biblioteca específica.

O uso de um assembly de referência permite que os desenvolvedores criem programas direcionados a uma versão de biblioteca específica sem ter o assembly de implementação completa para essa versão. Suponha que você tenha apenas a versão mais recente de algumas bibliotecas em seu computador, mas deseja criar um programa que tenha como alvo uma versão anterior dessa biblioteca. Se você compilar diretamente no assembly de implementação, poderá usar inadvertidamente membros de API que não estão disponíveis na versão anterior. Você só encontrará esse erro ao testar o programa no computador de destino. Se você compilar em relação ao assembly de referência para a versão anterior, obterá imediatamente um erro de tempo de compilação.

Um assembly de referência também pode representar um contrato, ou seja, um conjunto de APIs que não correspondem ao assembly de implementação concreto. Esses assemblies de referência, chamados de *assembly de contrato*, podem ser usados para direcionar várias plataformas que dão suporte ao mesmo conjunto de APIs. Por exemplo, .NET Standard fornece o assembly de contrato, *netstandard.dll*, que representa o conjunto de APIs comuns compartilhadas entre diferentes plataformas .net. As implementações dessas APIs estão contidas em diferentes assemblies em diferentes plataformas, como *mscorlib.dll* em .NET Framework ou *System.Private.CoreLib.dll* no .NET Core. Uma biblioteca que tem como alvo .NET Standard pode ser executada em todas as plataformas que dão suporte a .NET Standard.

## <a name="using-reference-assemblies"></a>Usando assemblies de referência

Para usar determinadas APIs do seu projeto, você deve adicionar referências a seus assemblies. Você pode adicionar referências a assemblies de implementação ou a assemblies de referência. É recomendável que você use assemblies de referência sempre que eles estiverem disponíveis. Isso garante que você esteja usando apenas os membros de API com suporte na versão de destino, destinada a ser usada pelos designers de API. Usar o assembly de referência garante que você não está tomando uma dependência dos detalhes da implementação.

Os assemblies de referência para as bibliotecas de .NET Framework são distribuídos com pacotes de direcionamento. Você pode obtê-los baixando um instalador autônomo ou selecionando um componente no instalador do Visual Studio. Para obter mais informações, consulte [instalar o .NET Framework para desenvolvedores](../../framework/install/guide-for-developers.md). Para .NET Core e .NET Standard, os assemblies de referência são baixados automaticamente conforme necessário (via NuGet) e referenciados. Para o .NET Core 3,0 e superior, os assemblies de referência para a estrutura principal estão no pacote [Microsoft. NetCore. app. ref](https://www.nuget.org/packages/Microsoft.NETCore.App.Ref) (o pacote [Microsoft. NetCore. app](https://www.nuget.org/packages/Microsoft.NETCore.App) é usado em vez de versões anteriores a 3,0).

Quando você adiciona referências a .NET Framework assemblies no Visual Studio usando a caixa de diálogo **Adicionar referência** , seleciona um assembly na lista e o Visual Studio localiza automaticamente os assemblies de referência que correspondem à versão da estrutura de destino selecionada em seu projeto. O mesmo se aplica à adição de referências diretamente no projeto do MSBuild usando o item de projeto de [referência](/visualstudio/msbuild/common-msbuild-project-items#reference) : você só precisa especificar o nome do assembly, não o caminho completo do arquivo. Quando você adiciona referências a esses assemblies na linha de comando usando a `-reference` opção do compilador ([em C#](../../csharp/language-reference/compiler-options/reference-compiler-option.md) e no [Visual Basic](../../visual-basic/reference/command-line-compiler/reference.md)) ou usando o <xref:Microsoft.CodeAnalysis.Compilation.AddReferences%2A?displayProperty=nameWithType> método na API Roslyn, você deve especificar manualmente os arquivos do assembly de referência para a versão correta da plataforma de destino. .NET Framework arquivos de assembly de referência estão localizados no *% ProgramFiles (x86)% \\ Reference Assemblies \\ Microsoft \\ Framework \\ . Diretório NETFramework* Para o .NET Core, você pode forçar a operação de publicação a copiar assemblies de referência para sua plataforma de destino no subdiretório *Publish/refs* do diretório de saída definindo a `PreserveCompilationContext` Propriedade Project como `true` . Em seguida, você pode passar esses arquivos de assembly de referência para o compilador. `DependencyContext`O uso do pacote [Microsoft. Extensions. DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/) pode ajudar a localizar seus caminhos.

Como não contêm nenhuma implementação, os assemblies de referência não podem ser carregados para execução. Tentar fazer isso resulta em um <xref:System.BadImageFormatException?displayProperty=nameWithType> . Se você quiser examinar o conteúdo de um assembly de referência, poderá carregá-lo no contexto somente de reflexão no .NET Framework (usando o <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A?displayProperty=nameWithType> método) ou no <xref:System.Reflection.MetadataLoadContext> no .NET Core.

## <a name="generating-reference-assemblies"></a>Gerando assemblies de referência

Gerar assemblies de referência para suas bibliotecas pode ser útil quando os consumidores da biblioteca precisam criar seus programas em várias versões diferentes da biblioteca. A distribuição de assemblies de implementação para todas essas versões pode ser impraticável por causa de seu grande tamanho. Os assemblies de referência têm tamanho menor e distribuí-los como parte do SDK da biblioteca reduzem o tamanho do download e economizam espaço em disco.

IDEs e ferramentas de Build também podem aproveitar os assemblies de referência para reduzir os tempos de compilação no caso de soluções grandes que consistem em várias bibliotecas de classes. Normalmente, em cenários de compilação incremental, um projeto é recriado quando qualquer um de seus arquivos de entrada é alterado, incluindo os assemblies dos quais ele depende. O assembly de implementação é alterado sempre que o programador altera a implementação de qualquer membro. O assembly de referência só é alterado quando sua API pública é afetada. Portanto, o uso do assembly de referência como um arquivo de entrada em vez do assembly de implementação permite ignorar a compilação do projeto dependente em alguns casos.

Você pode gerar assemblies de referência:

- Em um projeto do MSBuild, usando a [ `ProduceReferenceAssembly` Propriedade Project](/visualstudio/msbuild/common-msbuild-project-properties).
- Ao compilar o programa da linha de comando, por `-refonly` Opções de compilador especificar ([c#](../../csharp/language-reference/compiler-options/refonly-compiler-option.md)  /  [Visual Basic](../../visual-basic/reference/command-line-compiler/refonly-compiler-option.md) ) ou `-refout` ([c#](../../csharp/language-reference/compiler-options/refout-compiler-option.md)  /  [Visual Basic](../../visual-basic/reference/command-line-compiler/refout-compiler-option.md)).
- Ao usar a API Roslyn, definindo <xref:Microsoft.CodeAnalysis.Emit.EmitOptions.EmitMetadataOnly?displayProperty=nameWithType> como `true` e <xref:Microsoft.CodeAnalysis.Emit.EmitOptions.IncludePrivateMembers?displayProperty=nameWithType> como `false` em um objeto passado para o <xref:Microsoft.CodeAnalysis.Compilation.Emit%2A?displayProperty=nameWithType> método.

Se você quiser distribuir assemblies de referência com pacotes NuGet, você deve incluí-los no subdiretório *ref \\ * no diretório do pacote, em vez de no subdiretório *lib \\ * usado para assemblies de implementação.

## <a name="reference-assemblies-structure"></a>Estrutura de assemblies de referência

Os assemblies de referência são uma expansão do conceito relacionado, *assemblies somente de metadados*. Os assemblies somente de metadados têm seus corpos de método substituídos por um único corpo `throw null`, mas incluem todos os membros, exceto tipos anônimos. O motivo para usar `throw null` corpos (em oposição a nenhum corpo) é que o **PEVerify** possa ser executado e aprovado (Validando assim a integridade dos metadados).

Os assemblies de referência removem ainda mais metadados (membros particulares) de assemblies somente de metadados:

- Um assembly de referência tem somente referências para o que ele precisa na superfície de API. Talvez o assembly real tenha outras referências relacionadas a implementações específicas. Por exemplo, o assembly de referência para `class C { private void M() { dynamic d = 1; ... } }` não faz referência a nenhum tipo necessário para `dynamic` .
- Membros de função privados (métodos, propriedades e eventos) são removidos nos casos em que sua remoção não afeta nitidamente a compilação. Se não houver nenhum atributo [InternalsVisibleTo](xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute) , os membros da função interna também serão removidos.

Os metadados em assemblies de referência continuam a manter as seguintes informações:

- Todos os tipos, incluindo tipos privados e aninhados.
- Todos os atributos, mesmo os internos.
- Todos os métodos virtuais.
- Implementações de interface explícitas.
- Propriedades e eventos implementados explicitamente, pois seus acessadores são virtuais.
- Todos os campos de estruturas.

Os assemblies de referência incluem um atributo [ReferenceAssembly](xref:System.Runtime.CompilerServices.ReferenceAssemblyAttribute) no nível do assembly. Esse atributo pode ser especificado na origem; em seguida, o compilador não precisará resumir. Devido a esse atributo, os tempos de execução se recusarão a carregar assemblies de referência para execução (mas podem ser carregados no modo somente de reflexão).

Os detalhes da estrutura do assembly de referência exata dependem da versão do compilador. As versões mais recentes podem optar por excluir mais metadados se ele for determinado como não afetando a superfície da API pública.

> [!NOTE]
> As informações nesta seção são aplicáveis somente a assemblies de referência gerados por compiladores do Roslyn a partir do C# versão 7,1 ou Visual Basic versão 15,3. A estrutura de assemblies de referência para as bibliotecas .NET Framework e .NET Core pode ser diferente em alguns detalhes, pois elas usam seu próprio mecanismo de geração de assemblies de referência. Por exemplo, eles podem ter corpos de métodos totalmente vazios em vez do `throw null` corpo. Mas o princípio geral ainda se aplica: eles não têm implementações de método utilizáveis e contêm metadados somente para membros que têm um impacto observável de uma perspectiva de API pública.

## <a name="see-also"></a>Confira também

- [Assemblies no .NET](index.md)
- [Visão geral do direcionamento de estrutura](/visualstudio/ide/visual-studio-multi-targeting-overview)
- [Como: Adicionar ou remover referências usando o Gerenciador de referências](/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager)
