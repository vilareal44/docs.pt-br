---
title: Installutil.exe (Ferramenta de Instalação)
description: Use Installutil.exe, o Ferramenta de Instalação. Essa ferramenta permite que você instale ou desinstale recursos do servidor executando os componentes do instalador em assemblies especificados.
ms.date: 03/30/2017
helpviewer_keywords:
- uninstalling server resources
- removing server resources
- status information for installation
- installation progress reports
- installing server resources
- Installer tool
- Installutil.exe
- files, Installer tool
- progress information for installation
- reporting installation progress
ms.assetid: 3f9d0533-f895-4897-b4ea-528284e0241d
ms.openlocfilehash: 042e5f64a7a863173db9c4e601d3152b0df46d97
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164442"
---
# <a name="installutilexe-installer-tool"></a>Installutil.exe (Ferramenta de Instalação)

A ferramenta Instalador é um utilitário de linha de comando que permite instalar e desinstalar recursos de servidor executando-se os componentes do instalador em assemblies especificados. Essa ferramenta funciona com classes no namespace <xref:System.Configuration.Install>.

Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).

No prompt de comando, digite o seguinte:

## <a name="syntax"></a>Sintaxe

```console
installutil [/u[ninstall]] [options] assembly [[options] assembly] ...
```

## <a name="parameters"></a>parâmetros

|Argumento|Descrição|
|--------------|-----------------|
|`assembly`|O nome do arquivo do assembly no qual os componentes do instalador devem ser executados. Omita esse parâmetro se você quiser especificar o nome forte do assembly usando a opção `/AssemblyName`.|

<a name="options"></a>

## <a name="options"></a>Opções

|Opção|DESCRIÇÃO|
|------------|-----------------|
|`/h[elp]`<br /><br /> -ou-<br /><br /> `/?`|Exibe sintaxe de comando e opções para a ferramenta.|
|`/help`*assembly* do<br /><br /> -ou-<br /><br /> `/?`*assembly* do|Exibe opções adicionais reconhecidas por instaladores individuais dentro do assembly especificado, com a sintaxe do comando e as opções de InstallUtil.exe. Essa opção adiciona o texto retornado pela propriedade <xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType> de cada componente do instalador para o texto de ajuda de InstallUtil.exe.|
|`/AssemblyName`"*AssemblyName*<br /><br /> ,Version=*major.minor.build.revision*<br /><br /> ,Culture=*locale*<br /><br /> ,PublicKeyToken=*publicKeyToken*"|Especifica o nome forte de um assembly, que deve ser registrado no cache de assembly global. O nome do assembly deve ser totalmente qualificado com a versão, a cultura e o token de chave pública do assembly. O nome totalmente qualificado deve estar entre aspas.<br /><br /> Por exemplo, "myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0" é um nome de assembly totalmente qualificado.|
|`/InstallStateDir=[`*DirectoryName*`]`|Especifica o diretório do arquivo .InstallState que contém os dados usados para desinstalar o assembly. O padrão é o diretório que contém o assembly.|
|`/LogFile=`[*nome do arquivo*]|Especifica o nome do arquivo de log em que o andamento da instalação é registrado. Por padrão, se a opção `/LogFile` for omitida, um arquivo de log chamado *assemblyname*.InstallLog será criado. Se *filename* for omitido, nenhum arquivo de log será gerado.|
|`/LogToConsole`={`true`&#124;`false`}|Se `true`, exibirá a saída no console. Se `false` (o padrão), suprimirá a saída no console.|
|`/ShowCallStack`|A saída da pilha de chamadas será para o arquivo de log se ocorrer uma exceção a qualquer momento durante a instalação.|
|`/u`[`ninstall`]|Desinstala os assemblies especificados. Diferentemente das outras opções, `/u` se aplica a todos os assemblies, independentemente de onde a opção seja exibida na linha de comando.|

<a name="cmdline"></a>

## <a name="additional-installer-options"></a>Opções Adicionais do Instalador

Os instaladores individuais usados em um assembly podem reconhecer opções além das listadas na seção [Opções](#options). Para saber mais sobre essas opções, execute InstallUtil.exe com os caminhos dos assemblies na linha de comando com a opção `/?` ou `/help`. Para especificar essas opções, você as inclui na linha de comando com as opções reconhecidas por InstallUtil.exe.

> [!NOTE]
> O texto da ajuda nas opções compatíveis com componentes do instalador individuais é retornado pela propriedade <xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType>. As opções individuais que foram inseridas na linha de comando são acessíveis programaticamente com base na propriedade <xref:System.Configuration.Install.Installer.Context%2A?displayProperty=nameWithType>.

Todas as opções e os parâmetros de linha de comando são gravados no arquivo de log da instalação. No entanto, se você usar o parâmetro `/Password`, reconhecido por alguns componentes do instalador, as informações da senha serão substituídas por oito asteriscos (*) e não serão exibidas no arquivo de log.

> [!IMPORTANT]
> Em alguns casos, os parâmetros passados para o instalador podem incluir informações confidenciais ou de identificação pessoal que, por padrão, são gravadas em um arquivo de log de texto sem formatação. Para evitar esse comportamento, é possível suprimir o arquivo de log especificando `/LogFile=` (sem nenhum argumento *filename*) após Installutil.exe na linha de comando.

## <a name="remarks"></a>Comentários

Os aplicativos do .NET Framework consistem em arquivos de programa tradicionais e recursos associados como, por exemplo, filas de mensagens, logs de eventos e contadores de desempenho que devem ser criados quando o aplicativo é implantado. É possível usar componentes do instalador de um assembly para criar esses recursos quando o aplicativo é instalado e para removê-los quando o aplicativo é desinstalado. Installutil.exe detecta e executa esses componentes do instalador.

É possível especificar vários assemblies na mesma linha de comando. Qualquer opção ocorrida antes de um nome de assembly se aplica à instalação desse assembly. Com exceção de `/u` e `/AssemblyName`, as opções são cumulativas, mas substituíveis. Ou seja, as opções especificadas para um assembly se aplicam a todos os assemblies subsequentes, a menos que a opção seja especificada com um valor novo.

Se você executar Installutil.exe em um assembly sem especificar nenhuma opção, ele colocará estes três arquivos no diretório do assembly:

- InstallUtil.InstallLog - Contém uma descrição geral do andamento da instalação.

- *assemblyname*.InstallLog – Contém informações específicas para a fase de confirmação do processo de instalação. Para obter mais informações sobre a fase de confirmação, consulte o método <xref:System.Configuration.Install.Installer.Commit%2A>.

- *assemblyname*.InstallState – Contém dados usados para desinstalar o assembly.

Installutil.exe usa reflexão para inspecionar os assemblies especificados e encontrar todos os tipos de <xref:System.Configuration.Install.Installer> que têm o atributo <xref:System.ComponentModel.RunInstallerAttribute?displayProperty=nameWithType> definido como `true`. Em seguida, a ferramenta executa o método <xref:System.Configuration.Install.Installer.Install%2A?displayProperty=nameWithType> ou <xref:System.Configuration.Install.Installer.Uninstall%2A?displayProperty=nameWithType> em cada instância do tipo de <xref:System.Configuration.Install.Installer>. Installutil.exe realiza a instalação de maneira transacional; ou seja, se um dos assemblies não for instalado, ele reverterá as instalações de todos os outros assemblies. A desinstalação não é transacional.

Installutil.exe não pode instalar ou desinstalar assemblies assinados com atraso, mas pode instalar ou desinstalar assemblies de nome forte.

Desde o .NET Framework versão 2.0, a versão 32 bits do CLR (Common Language Runtime) acompanha apenas a versão 32 bits da ferramenta Instalador, mas a versão 64 bits do CLR acompanha as versões 32 e 64 bits da ferramenta Instalador. Ao usar o CLR 64 bits, use a ferramenta Instalador 32 bits para instalar assemblies 32 bits, e a ferramenta Instalador 64 bits para instalar assemblies 64 bits e MSIL (Microsoft Intermediate Language). Ambas as versões da ferramenta Instalador se comportam da mesma forma.

Não é possível usar Installutil.exe para implantar um serviço do Windows criado usando-se o C++, porque Installutil.exe pode não reconhecer o código nativo inserido produzido pelo compilador do C++. Se você tentar implantar um serviço do Windows do C++ com Installutil.exe como, por exemplo, <xref:System.BadImageFormatException> uma exceção será acionada. Para trabalhar com esse cenário, mova o código de serviço para o módulo do C++ e, em seguida, grave o objeto do instalador no C# ou no Visual Basic.

## <a name="examples"></a>Exemplos

O comando a seguir exibe uma descrição da sintaxe e das opções de comando para InstallUtil.exe.

```console
installutil /?
```

O comando a seguir exibe uma descrição da sintaxe e das opções de comando para InstallUtil.exe. Ele também exibirá uma descrição e uma lista de opções com suporte pelos componentes do instalador em `myAssembly.exe` se o texto de ajuda tiver sido atribuído à propriedade <xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType> do instalador.

```console
installutil /? myAssembly.exe
```

O comando a seguir executa os componentes do instalador no assembly `myAssembly.exe`.

```console
installutil myAssembly.exe
```

O comando a seguir executa os componentes do instalador em um assembly usando-se a opção `/AssemblyName` e um nome totalmente qualificado.

```console
installutil /AssemblyName "myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0"
```

O comando a seguir executa os componentes do instalador em um assembly especificado pelo nome de arquivo e em um assembly especificado pelo nome forte. Todos os assemblies especificados pelo nome de arquivo devem anteceder assemblies especificados pelo nome forte na linha de comando, porque a opção `/AssemblyName` não pode ser substituída.

```console
installutil myAssembly.exe /AssemblyName "myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0"
```

O comando a seguir executa os componentes do desinstalador no assembly `myAssembly.exe`.

```console
installutil /u myAssembly.exe
```

O comando a seguir executa os componentes do desinstalador nos assemblies `myAssembly1.exe` e `myAssembly2.exe`.

```console
installutil myAssembly1.exe /u myAssembly2.exe
```

Como a posição da opção `/u` na linha de comando não é importante, ela equivale ao comando a seguir.

```console
installutil /u myAssembly1.exe myAssembly2.exe
```

O comando a seguir executa os instaladores no assembly `myAssembly.exe` e especifica que as informações de andamento serão gravadas em `myLog.InstallLog`.

```console
installutil /LogFile=myLog.InstallLog myAssembly.exe
```

O comando a seguir executa os instaladores no assembly `myAssembly.exe`, especifica que as informações de andamento devem ser gravadas em `myLog.InstallLog` e usa a opção `/reg` personalizada dos instaladores para especificar que as atualizações devem ser feitas no Registro do sistema.

```console
installutil /LogFile=myLog.InstallLog /reg=true myAssembly.exe
```

O comando a seguir executa os instaladores no assembly `myAssembly.exe`, usa a opção `/email` personalizada do instalador para especificar o endereço de email do usuário e suprime saída para o arquivo de log.

```console
installutil /LogFile= /email=admin@mycompany.com myAssembly.exe
```

O comando a seguir grava o andamento da instalação de `myAssembly.exe` em `myLog.InstallLog` e grava o andamento de `myTestAssembly.exe` em `myTestLog.InstallLog`.

```console
installutil /LogFile=myLog.InstallLog myAssembly.exe /LogFile=myTestLog.InstallLog myTestAssembly.exe
```

## <a name="see-also"></a>Confira também

- <xref:System.Configuration.Install>
- [Ferramentas](index.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
