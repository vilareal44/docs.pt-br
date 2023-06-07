---
title: Novidades do .NET Core 3.1
description: Saiba mais sobre os novos recursos encontrados no .NET Core 3,1.
dev_langs:
- csharp
author: adegeo
ms.author: adegeo
ms.date: 12/04/2019
ms.openlocfilehash: 42d4f7e8800bf2d13d584084f8a41bad2ada534f
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608121"
---
# <a name="whats-new-in-net-core-31"></a>Novidades do .NET Core 3.1

Este artigo descreve o que há de novo no .NET Core 3,1. Esta versão contém pequenas melhorias no .NET Core 3,0, concentrando-se em correções pequenas, mas importantes. O recurso mais importante sobre o .NET Core 3,1 é que ele é uma versão de [LTS (suporte a longo prazo)](#long-term-support) .

Se você estiver usando o Visual Studio 2019, deverá atualizar para o [visual studio 2019 versão 16,4 ou posterior](https://visualstudio.microsoft.com/downloads/) para trabalhar com projetos do .net Core 3,1. Para obter informações sobre as novidades do Visual Studio versão 16,4, consulte [What ' s New in Visual studio 2019 versão 16,4](/visualstudio/releases/2019/release-notes-v16.4#whats-new-in-visual-studio-2019-version-164).

O Visual Studio para Mac também dá suporte a e inclui o .NET Core 3,1 no Visual Studio para Mac 8,4.

Para obter mais informações sobre a versão, consulte o [anúncio do .NET Core 3,1](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-1/).

- [Baixe e comece a usar o .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1) no Windows, no MacOS ou no Linux.

## <a name="long-term-support"></a>Suporte de longo prazo

O .NET Core 3,1 é uma versão LTS com suporte da Microsoft nos próximos três anos. É altamente recomendável que você mova seus aplicativos para o .NET Core 3,1. O ciclo de vida atual de outras versões principais é o seguinte:

| Versão | Observação |
| ------- | ---- |
| .NET Core 3.0 | Fim da vida útil em 3 de março de 2020.     |
| .NET Core 2.2 | Fim da vida útil em 23 de dezembro de 2019. |
| .NET Core 2.1 | Fim da vida útil em 21 de agosto de 2021.    |

Para obter mais informações, consulte a [política de suporte do .NET Core](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

## <a name="macos-apphost-and-notarization"></a>macOS appHost e notarization

*somente macOS*

A partir do notarized SDK do .NET Core 3,1 para macOS, a configuração appHost é desabilitada por padrão. Para obter mais informações, consulte [o notarization Catalina do MacOS e o impacto sobre os downloads e projetos do .NET Core](../install/macos-notarization-issues.md).

Quando a configuração appHost está habilitada, o .NET Core gera um executável de Mach-O nativo quando você cria ou publica. Seu aplicativo é executado no contexto do appHost quando ele é executado do código-fonte com o `dotnet run` comando ou iniciando o executável de Mach-o diretamente.

Sem o appHost, a única maneira como um usuário pode iniciar um aplicativo [dependente da estrutura](../deploying/index.md#publish-framework-dependent) é com o `dotnet <filename.dll>` comando. Um appHost é sempre criado quando você publica [seu aplicativo independente](../deploying/index.md#publish-self-contained).

Você pode configurar o appHost no nível do projeto ou alternar o appHost para um `dotnet` comando específico com o `-p:UseAppHost` parâmetro:

- Arquivo de projeto

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- Parâmetro de linha de comando

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

Para obter mais informações sobre a `UseAppHost` configuração, consulte [Propriedades do MSBuild para Microsoft. net. SDK](../project-sdk/msbuild-props.md#useapphost).

## <a name="windows-forms"></a>Windows Forms

*Somente Windows*

> [!WARNING]
> Há alterações significativas no Windows Forms.

Os controles herdados foram incluídos em Windows Forms que não estão disponíveis na caixa de ferramentas do Visual Studio designer por algum tempo. Eles foram substituídos por novos controles de volta no .NET Framework 2,0. Elas foram removidas do SDK de desktop para .NET Core 3,1.

| Controle removido | Substituição recomendada | APIs associadas removidas |
| --------------- | ----------------------- | ----------------------- |
| DataGrid        | <xref:System.Windows.Forms.DataGridView>      | DataGridCell<br/>DataGridRow<br/>DataGridTableCollection<br/>DataGridColumnCollection<br/>DataGridTableStyle<br/>DataGridColumnStyle<br/>Datagridlinesstyle<br/>DataGridParentRowsLabel<br/>DataGridParentRowsLabelStyle<br/>DataGridBoolColumn<br/>DataGridTextBox<br/>GridColumnStylesCollection<br/>GridTableStylesCollection<br/>HitTesttype |
| ToolBar         | <xref:System.Windows.Forms.ToolStrip>         | ToolBarAppearance |
| ToolBarButton   | <xref:System.Windows.Forms.ToolStripButton>   | ToolBarButtonClickEventArgs<br/>ToolBarButtonClickEventHandler<br/>ToolBarButtonStyle<br/>ToolBarTextAlign |
| ContextMenu     | <xref:System.Windows.Forms.ContextMenuStrip>  |  |
| :::no-loc text="Menu"::: | <xref:System.Windows.Forms.ToolStripDropDown><br/><xref:System.Windows.Forms.ToolStripDropDownMenu> | MenuItemCollection |
| MainMenu        | <xref:System.Windows.Forms.MenuStrip>         |  |
| MenuItem        | <xref:System.Windows.Forms.ToolStripMenuItem> |  |

Recomendamos que você atualize seus aplicativos para o .NET Core 3,1 e vá para os controles de substituição. Substituir os controles é um processo simples, essencialmente "localizar e substituir" no tipo.

## <a name="ccli"></a>C++/CLI

*Somente Windows*

Foi adicionado suporte para a criação de projetos C++/CLI (também conhecidos como "C++ gerenciado"). Os binários produzidos a partir desses projetos são compatíveis com o .NET Core 3,0 e versões posteriores.

Para adicionar suporte para C++/CLI no Visual Studio 2019 versão 16,4, instale o [desenvolvimento de desktop com carga de trabalho do C++](/cpp/build/vscpp-step-0-installation?view=vs-2019#step-4---choose-workloads). Essa carga de trabalho adiciona dois modelos ao Visual Studio:

- Biblioteca de classes CLR (.NET Core)
- Projeto CLR vazio (.NET Core)

## <a name="next-steps"></a>Próximas etapas

- [Examine as alterações significativas entre o .NET Core 3,0 e 3,1.](../compatibility/3.0-3.1.md)
- [Examine as alterações significativas no .NET Core 3,1 para aplicativos Windows Forms.](../compatibility/winforms.md#net-core-31)
