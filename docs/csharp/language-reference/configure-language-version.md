---
title: Controle de versão da linguagem C# – Guia de C#
description: Saiba mais sobre como a versão da linguagem C# é determinada com base no seu projeto e os motivos por trás dessa escolha. Saiba como substituir o padrão manualmente.
ms.custom: updateeachrelease
ms.date: 05/20/2020
ms.openlocfilehash: a27f3210f399f1bed190c18d778cf3824772d576
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656841"
---
# <a name="c-language-versioning"></a>Controle de versão da linguagem C#

O compilador do C# mais recente determina uma versão da linguagem padrão com base nas estruturas de destino do projeto. O Visual Studio não fornece uma interface do usuário para alterar o valor, mas você pode alterá-lo editando o arquivo *csproj* . A escolha do padrão garante que você use a versão de idioma mais recente compatível com a estrutura de destino. Você se beneficia do acesso aos recursos de linguagem mais recentes compatíveis com o destino do seu projeto. Essa opção padrão também garante que você não use uma linguagem que exija tipos ou comportamento de tempo de execução não disponível em sua estrutura de destino. Escolher uma versão de idioma mais recente do que o padrão pode causar dificuldade para diagnosticar erros de tempo de compilação e Runtime.

As regras neste artigo se aplicam ao compilador fornecido com o Visual Studio 2019 ou o SDK do .NET Core 3,0. Os compiladores do C# que fazem parte da instalação do Visual Studio 2017 ou de versões anteriores do SDK do .NET Core são direcionados ao C# 7.0 por padrão.

O C# 8,0 (e superior) tem suporte apenas no .NET Core 3. x e em versões mais recentes. Muitos dos recursos mais recentes exigem recursos de biblioteca e tempo de execução introduzidos no .NET Core 3. x:

- A [implementação de interface padrão](../whats-new/csharp-8.md#default-interface-methods) requer novos recursos no .net Core 3,0 CLR.
- Os [fluxos assíncronos](../whats-new/csharp-8.md#asynchronous-streams) exigem os novos tipos <xref:System.IAsyncDisposable?displayProperty=nameWithType> , <xref:System.Collections.Generic.IAsyncEnumerable%601?displayProperty=nameWithType> e <xref:System.Collections.Generic.IAsyncEnumerator%601?displayProperty=nameWithType> .
- [Índices e intervalos](../whats-new/csharp-8.md#indices-and-ranges) exigem os novos tipos <xref:System.Index?displayProperty=nameWithType> e <xref:System.Range?displayProperty=nameWithType> .
- Os [tipos de referência anuláveis](../whats-new/csharp-8.md#nullable-reference-types) fazem uso de vários [atributos](attributes/nullable-analysis.md) para fornecer avisos melhores. Esses atributos foram adicionados no .NET Core 3,0. Outras estruturas de destino não foram anotadas com nenhum desses atributos. Isso significa que os avisos anuláveis podem não refletir com precisão possíveis problemas.

## <a name="defaults"></a>Padrões

O compilador determina um padrão com base nestas regras:

| Estrutura de destino | version | Padrão da versão da linguagem C# |
|------------------|---------|-----------------------------|
| .NET Core        | 3.x     | C# 8.0                      |
| .NET Core        | 2. x     | C# 7.3                      |
| .NET Standard    | 2.1     | C# 8.0                      |
| .NET Standard    | 2.0     | C# 7.3                      |
| .NET Standard    | 1.x     | C# 7.3                      |
| .NET Framework   | all     | C# 7.3                      |

Quando seu projeto se destina a uma estrutura de visualização que tem uma versão da linguagem correspondente da visualização, a versão de linguagem usada é a de visualização. Você usa os recursos mais recentes com essa visualização em qualquer ambiente, sem afetar projetos direcionados a uma versão lançada do .NET Core.

## <a name="override-a-default"></a>Substituir um padrão

Se precisar especificar sua versão do C# explicitamente, poderá fazer isso de várias maneiras:

- Edite manualmente o [arquivo de projeto](#edit-the-project-file).
- Definir a versão da linguagem [para vários projetos em um subdiretório](#configure-multiple-projects).
- Configure a [ `-langversion` opção do compilador](compiler-options/langversion-compiler-option.md).

### <a name="edit-the-project-file"></a>Editar o arquivo de projeto

É possível definir a versão da linguagem em seu arquivo de projeto. Por exemplo, se você quiser explicitamente acesso às versões prévias dos recursos, adicione um elemento como este:

```xml
<PropertyGroup>
   <LangVersion>preview</LangVersion>
</PropertyGroup>
```

O valor `preview` usa a versão prévia mais recente da linguagem C# compatível com seu compilador.

### <a name="configure-multiple-projects"></a>Configurar vários projetos

Para configurar vários projetos, você pode criar um arquivo **Directory. Build. props** que contém o `<LangVersion>` elemento. Normalmente, você faz isso no diretório da solução. Adicione o seguinte a um arquivo **Directory. Build. props** no diretório da solução:

```xml
<Project>
 <PropertyGroup>
   <LangVersion>preview</LangVersion>
 </PropertyGroup>
</Project>
```

As compilações em todos os subdiretórios do diretório que contém esse arquivo usarão a versão preview C#. Para obter mais informações, confira o artigo sobre como [personalizar o build](/visualstudio/msbuild/customize-your-build).

## <a name="c-language-version-reference"></a>Referência à versão da linguagem C#

A tabela a seguir mostra todas as versões atuais da linguagem C#. Seu compilador pode não entender necessariamente todos os valores se for mais antigo. Se você instalar o .NET Core 3,0 ou posterior, terá acesso a todos os itens listados.

[!INCLUDE [langversion-table](includes/langversion-table.md)]

> [!TIP]
> Abra o [prompt de comando do desenvolvedor para o Visual Studio](../../framework/tools/developer-command-prompt-for-vs.md)e execute o comando a seguir para ver a lista de versões de idioma disponíveis em seu computador.
>
> ```CMD
> csc -langversion:?
> ```
>
> Questionando a opção de compilação [-langversion](compiler-options/langversion-compiler-option.md) como essa, imprimirá algo semelhante ao seguinte:
>
> ```CMD
> Supported language versions:
> default
> 1
> 2
> 3
> 4
> 5
> 6
> 7.0
> 7.1
> 7.2
> 7.3
> 8.0 (default)
> latestmajor
> preview
> latest
> ```
