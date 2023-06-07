---
title: Ativar o recurso auto-completar com TAB
description: Este artigo ensina você a habilitar o preenchimento com TAB na CLI do .NET Core para o PowerShell, o Bash e o zsh.
author: adegeo
ms.author: adegeo
ms.topic: how-to
ms.date: 11/03/2019
ms.openlocfilehash: cd46305b8cd82825671a3a1568e8b93de1bbab26
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062802"
---
# <a name="how-to-enable-tab-completion-for-the-net-core-cli"></a>Como habilitar o preenchimento com TAB na CLI do .NET Core

**Este artigo aplica-se a:** ✔️ SDK do .net Core 2,1 e versões posteriores

Este artigo descreve como configurar o preenchimento com TAB para três shells: PowerShell, Bash e zsh. Para outros shells, consulte a documentação sobre como configurar a conclusão da guia.

Uma vez configurado, o preenchimento com Tab para o CLI do .NET Core é disparado digitando um `dotnet` comando no Shell e, em seguida, pressionando a tecla Tab. A linha de comando atual é enviada para o comando `dotnet complete`, e os resultados são processados pelo shell. Teste os resultados sem habilitar o preenchimento com TAB enviando algo diretamente para o comando `dotnet complete`. Por exemplo:

```console
> dotnet complete "dotnet a"
add
clean
--diagnostics
migrate
pack
```

Se esse comando não funcionar, verifique se esse SDK do .NET Core 2.0 ou superior está instalado. Se ele está instalado, mas esse comando ainda não funciona, verifique se o comando `dotnet` é resolvido para uma versão do SDK do .NET Core 2.0 e superior. Use o comando `dotnet --version` para ver para qual versão do `dotnet` o caminho atual está sendo resolvido. Para obter mais informações, confira [Selecionar a versão do .NET Core a ser usada](../versions/selection.md).

### <a name="examples"></a>Exemplos

Estes são alguns exemplos do que o preenchimento com TAB fornece:

Entrada                                | se torna                                                                     | porque
:------------------------------------|:----------------------------------------------------------------------------|:--------------------------------
`dotnet a⇥`                          | `dotnet add`                                                                 | `add` é o primeiro subcomando, em ordem alfabética.
`dotnet add p⇥`                      | `dotnet add --help`                                                          | O preenchimento com TAB faz a correspondência de subcadeias de caracteres e `--help` vem em primeiro lugar em ordem alfabética.
`dotnet add p⇥⇥`                    | `dotnet add package`                                                          | Na segunda vez que a tecla TAB é pressionada, a próxima sugestão é exibida.
`dotnet add package Microsoft⇥`      | `dotnet add package Microsoft.ApplicationInsights.Web`                      | Os resultados são retornados em ordem alfabética.
`dotnet remove reference ⇥`          | `dotnet remove reference ..\..\src\OmniSharp.DotNet\OmniSharp.DotNet.csproj` | O preenchimento com TAB reconhece o arquivo de projeto.

## <a name="powershell"></a>PowerShell

Para adicionar o preenchimento com TAB ao **PowerShell** na CLI do .NET Core, crie ou edite o perfil armazenado na variável `$PROFILE`. Para obter mais informações, confira [Como criar seu perfil](/powershell/module/microsoft.powershell.core/about/about_profiles#how-to-create-a-profile) e [Perfis e política de execução](/powershell/module/microsoft.powershell.core/about/about_profiles#profiles-and-execution-policy).

Adicione o seguinte código ao seu perfil:

```powershell
# PowerShell parameter completion shim for the dotnet CLI
Register-ArgumentCompleter -Native -CommandName dotnet -ScriptBlock {
     param($commandName, $wordToComplete, $cursorPosition)
         dotnet complete --position $cursorPosition "$wordToComplete" | ForEach-Object {
            [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
         }
 }
```

## <a name="bash"></a>bash

Para adicionar o preenchimento com TAB ao shell do **Bash** na CLI do .NET Core, adicione o seguinte código ao arquivo `.bashrc`:

```bash
# bash parameter completion for the dotnet CLI

_dotnet_bash_complete()
{
  local word=${COMP_WORDS[COMP_CWORD]}

  local completions
  completions="$(dotnet complete --position "${COMP_POINT}" "${COMP_LINE}" 2>/dev/null)"
  if [ $? -ne 0 ]; then
    completions=""
  fi

  COMPREPLY=( $(compgen -W "$completions" -- "$word") )
}

complete -f -F _dotnet_bash_complete dotnet
```

## <a name="zsh"></a>zsh

Para adicionar o preenchimento com TAB ao shell do **zsh** na CLI do .NET Core, adicione o seguinte código ao arquivo `.zshrc`:

```zsh
# zsh parameter completion for the dotnet CLI

_dotnet_zsh_complete()
{
  local completions=("$(dotnet complete "$words")")

  reply=( "${(ps:\n:)completions}" )
}

compctl -K _dotnet_zsh_complete dotnet
```
