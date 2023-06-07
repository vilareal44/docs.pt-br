---
title: Gacutil.exe (Ferramenta do Cache de Assemblies Global)
description: Obtenha detalhes sobre Gacutil.exe, a ferramenta cache de assembly global. Essa ferramenta permite exibir e manipular o cache de assembly global e o cache de download.
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- global assembly cache, viewing contents
- viewing assemblies in global assembly cache
- global assembly cache, manipulating contents
- GAC (global assembly cache), Gacutil.exe
- Gacutil.exe
- GAC (global assembly cache), viewing contents
- installing assemblies into global assembly cache
- removing assemblies from global assembly cache
- list of assemblies in global assembly cache
- cache [.NET Framework], global assembly cache
- GAC (global assembly cache), manipulating contents
- global assembly cache, Gacutil.exe
- Global Assembly Cache tool
ms.assetid: 4c7be9c8-72ae-481f-a01c-1a4716806e99
ms.openlocfilehash: 1fe79a09aa99c9d91d8223e7642408a56da20dfe
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166944"
---
# <a name="gacutilexe-global-assembly-cache-tool"></a>Gacutil.exe (Ferramenta do Cache de Assemblies Global)

A ferramenta de Cache de Assembly Global permite exibir e manipular o conteúdo do cache de assembly global e do cache de download.

Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).

No prompt de comando, digite o seguinte:

## <a name="syntax"></a>Sintaxe

```console
gacutil [options] [assemblyName | assemblyPath | assemblyListFile]
```

## <a name="parameters"></a>parâmetros

|Argumento|Descrição|
|--------------|-----------------|
|*assemblyName*|O nome de um assembly. É possível fornecer um nome de assembly especificado parcialmente como, por exemplo, `myAssembly` ou um nome de assembly totalmente especificado como, por exemplo, `myAssembly, Version=2.0.0.0, Culture=neutral, PublicKeyToken=0038abc9deabfle5`.|
|*assemblyPath*|O nome de um arquivo que contém um manifesto de assembly.|
|*assemblyListFile*|O caminho para um arquivo de texto ANSI que lista assemblies para instalação ou desinstalação. Para usar um arquivo de texto para instalar assemblies, especifique o caminho para cada assembly em uma linha separada no arquivo. A ferramenta interpreta caminhos relativos em relação ao local do *assemblyListFile*. Para usar um arquivo de texto para desinstalar assemblies, especifique o nome de assembly totalmente qualificado para cada assembly em uma linha separada no arquivo. Consulte os exemplos de conteúdo *assemblyListFile* adiante neste tópico.|

|Opção|DESCRIÇÃO|
|------------|-----------------|
|**/cdl**|Exclui o conteúdo do cache de download.|
|**f**|Especifique essa opção com as opções **/i** ou **/il** para forçar a reinstalação de um assembly. Se um assembly com o mesmo nome já estiver no cache de assembly global, a ferramenta o substituirá.|
|**/h**[**elp**]|Exibe sintaxe de comando e opções para a ferramenta.|
|**/i** *assemblyPath*|Instala um assembly no cache de assembly global.|
|**/if**  *assemblyPath*|Instala um assembly no cache de assembly global. Se um assembly com o mesmo nome já estiver no cache de assembly global, a ferramenta o substituirá.<br /><br /> Especificar essa opção equivale a definir as opções **/i** e **/f** juntas.|
|**/il** *assemblyListFile*|Instala um ou mais assemblies especificados em *assemblyListFile* no cache de assembly global.|
|**/ir**  *assemblyPath*<br /><br /> *esquema*<br /><br /> *id*<br /><br /> *descrição*|Instala um assembly no cache de assembly global e adiciona uma referência para contar o assembly. Você deve especificar os parâmetros *assemblyPath*, *scheme*, *id* e *description* com essa opção. Para obter uma descrição dos valores válidos que podem ser especificados nesses parâmetros, consulte a opção **/r**.<br /><br /> Especificar essa opção equivale a definir as opções **/i** e **/r** juntas.|
|**/l** [*assemblyName*]|Lista o conteúdo do cache de assembly global. Se você especificar o parâmetro *assemblyName*, a ferramenta listará apenas os assemblies que correspondem a esse nome.|
|**/ldl**|Lista o conteúdo do cache de arquivos baixados.|
|**/lr** [*assemblyName*]|Lista todos os assemblies e suas contagens de referência correspondentes. Se você especificar o parâmetro *assemblyName*, a ferramenta listará apenas os assemblies que correspondem a esse nome e suas contagens de referência correspondentes.|
|**/nologo**|Suprime a exibição do banner de inicialização da Microsoft.|
|**/r** [*assemblyName &#124; assemblyPath*]<br /><br /> *esquema*<br /><br /> *id*<br /><br /> *descrição*|Especifica uma referência rastreada para um assembly ou assemblies para instalação ou desinstalação. Especifique essa opção com as opções **/i**, **/il**, **/u** ou **/ul**.<br /><br /> Para instalar um assembly, especifique os parâmetros *assemblyPath*, *scheme*, *id* e *description* com essa opção. Para desinstalar um assembly, especifique os parâmetros *assemblyName*, *scheme*, *id* e *description*.<br /><br /> Para remover uma referência a um assembly, você deve especificar os mesmos parâmetros *scheme*, *id* e *description* que foram especificados com as opções **/i** e **/r** (ou **/ir**) quando o assembly foi instalado. Se você estiver desinstalando um assembly, a ferramenta também removerá o assembly do cache de assembly global se for a última referência a ser removida e se o Windows Installer não tiver referências pendentes para o assembly.<br /><br /> O parâmetro *scheme* especifica o tipo de esquema de instalação. É possível especificar um dos seguintes valores:<br /><br /> -UNINSTALL_KEY: Especifique esse valor se o instalador adicionar o aplicativo para adicionar ou remover programas no Microsoft Windows. Os aplicativos se adicionam a Adicionar/Remover Programas incluindo uma chave do Registro a HKLM\Software\Microsoft\Windows\CurrentVersion.<br />-FILEPATH: Especifique esse valor se o instalador não adicionar o aplicativo para adicionar ou remover programas.<br />-OPACO: Especifique esse valor se fornecer uma chave do registro ou um caminho de arquivo não se aplicar ao seu cenário de instalação. Esse valor permite especificar informações personalizadas para o parâmetro *id*.<br /><br /> O valor a ser especificado para o parâmetro *id* depende do valor especificado para o parâmetro *scheme*:<br /><br /> -Se você especificar UNINSTALL_KEY para o parâmetro de *esquema* , especifique o nome do aplicativo definido na chave do registro HKLM\Software\Microsoft\Windows\CurrentVersion. Por exemplo, se a chave do Registro for HKLM\Software\Microsoft\Windows\CurrentVersion\MyApp, especifique MyApp para o parâmetro *id*.<br />– Se você especificar FILEPATH para o parâmetro *scheme*, especifique o caminho completo para o arquivo executável que instala o assembly como o parâmetro *id*.<br />– Se você especificar OPAQUE para o parâmetro *scheme*, poderá fornecer qualquer dado como o parâmetro *id*. Os dados especificados devem estar entre aspas ("").<br /><br /> O parâmetro *description* permite especificar o texto descritivo sobre o aplicativo a ser instalado. Essas informações são exibidas quando as referências são enumeradas.|
|**/Silent**|Suprime a exibição de toda a saída.|
|**/u**  *assemblyName*|Desinstala um assembly do cache de assembly global.|
|**/uf**  *assemblyName*|Força um assembly especificado para desinstalar removendo-se todas as referências para o assembly.<br /><br /> Especificar essa opção equivale a definir as opções **/u** e **/f** juntas. **Observação**: não é possível usar essa opção para remover um assembly que foi instalado usando o Microsoft Windows Installer. Se você tentar essa operação, a ferramenta exibirá uma mensagem de erro.|
|**/ul** *assemblyListFile*|Desinstala um ou mais assemblies especificados em *assemblyListFile* do cache de assembly global.|
|**/u**[**ngen**] *assemblyName*|Desinstala um assembly especificado do cache de assembly global. Se o assembly especificado tiver contagens de referência existentes, a ferramenta exibirá as contagens de referência e não removerá o assembly do cache de assembly global. **Observação:** no .NET Framework versão 2.0, não há suporte para `/ungen`. Em vez disso, use o comando `uninstall` do [Ngen.exe (Gerador de Imagens Nativas)](ngen-exe-native-image-generator.md). <br /><br /> No .NET Framework versões 1.0 e 1.1, especificar **/ungen** faz Gacutil.exe remover o assembly do cache de imagens nativas. Esse cache armazena as imagens nativas para assemblies que foram criados usando o [Ngen.exe (Gerador de Imagens Nativas)](ngen-exe-native-image-generator.md).|
|**/ur**  *assemblyName*<br /><br /> *esquema*<br /><br /> *id*<br /><br /> *descrição*|Desinstala uma referência de um assembly especificado do cache de assembly global. Para remover uma referência a um assembly, você deve especificar os mesmos parâmetros de *esquema*, *ID*e *Descrição* que foram especificados com as opções **/i** e **/r** (ou **/ir)** quando o assembly foi instalado. Para obter uma descrição dos valores válidos que podem ser especificados nesses parâmetros, consulte a opção **/r**.<br /><br /> Especificar essa opção equivale a definir as opções **/u** e **/r** juntas.|
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|

## <a name="remarks"></a>Comentários

> [!NOTE]
> Você deve ter privilégios de administrador para usar Gacutil.exe.

Mais especificamente, Gacutil.exe permite instalar assemblies no cache, removê-los de cache e listar o conteúdo do cache.

Gacutil.exe oferece opções que dão suporte à contagem de referências semelhante ao esquema da contagem de referências compatível com o Windows Installer. É possível usar Gacutil.exe para instalar dois aplicativos que instalam o mesmo assembly; a ferramenta acompanha o número de referências para o assembly. Assim, o assembly permanecerá no computador até ambos os aplicativos serem desinstalados. Se você estiver usando Gacutil.exe em instalações de produto reais, use as opções que dão suporte à contagem de referências. Use as opções **/i** e **/r** juntas para instalar um assembly e adicionar uma referência para contá-lo. Use as opções **/u** e **/r** juntas para remover uma contagem de referência para um assembly. Lembre que o uso das opções **/i** e **/u** sozinhas não dá suporte à contagem de referência. Essas opções são apropriadas ao uso durante o desenvolvimento de produtos, mas não para instalações de produto reais.

Use as opções **/il** ou **/ul** para instalar ou desinstalar uma lista de assemblies armazenadas em um arquivo de texto ANSI. O conteúdo do arquivo de texto deve ser formatado corretamente. Para usar um arquivo de texto para instalar assemblies, especifique o caminho para cada assembly em uma linha separada no arquivo. O exemplo a seguir demonstra o conteúdo de um arquivo que contém assemblies para instalação.

```text
myAssembly1.dll
myAssembly2.dll
myAssembly3.dll
```

Para usar um arquivo de texto para desinstalar assemblies, especifique o nome de assembly totalmente qualificado para cada assembly em uma linha separada no arquivo. O exemplo a seguir demonstra o conteúdo de um arquivo que contém assemblies para desinstalação.

```text
myAssembly1,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
myAssembly2,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
myAssembly3,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
```

> [!NOTE]
> A tentativa de instalar um assembly com um nome de arquivo co mais de 91 a 79 caracteres (excluindo a extensão de arquivo) pode resultar no seguinte erro:
>
> ```output
> Failure adding assembly to the cache:   The file name is too long.
> ```
>
> Isso ocorre porque, internamente, Gacutil.exe constrói um caminho de até MAX_PATH caracteres, composto pelos seguintes elementos:
>
> - Raiz do GAC: 34 caracteres (`C:\Windows\Microsoft.NET\assembly\`)
> - Arquitetura: 7 ou 9 caracteres (`GAC_32\`, `GAC_64\`, `GAC_MSIL`)
> - AssemblyName: até 91 caracteres, dependendo do tamanho dos outros elementos (por exemplo. `System.Xml.Linq\`)
> - AssemblyInfo: 31 a 48 caracteres ou mais, composto por:
>   - Framework: 5 caracteres (por exemplo, `v4.0_`)
>   - AssemblyVersion: 8 a 24 caracteres (por exemplo, `9.0.1000.0_`)
>   - AssemblyLanguage: 1 a 8 caracteres (por exemplo, `de_`, `sr-Cyrl_`)
>   - PublicKey: 17 caracteres (por exemplo, `31bf3856ad364e35\`)
> - DllFileName: até 91 + 4 caracteres (`<AssemblyName>.dll`)

## <a name="examples"></a>Exemplos

O comando a seguir instala o assembly `mydll.dll` no cache de assembly global.

```console
gacutil /i mydll.dll
```

O comando a seguir remove o assembly `hello` do cache de assembly global, desde que não haja contagens de referência para o assembly.

```console
gacutil /u hello
```

O comando anterior pode remover mais de um assembly do cache de assembly porque o nome do assembly não está totalmente especificado. Por exemplo, se as versões 1.0.0.0 e 3.2.2.1 de `hello` forem instaladas no cache, o comando `gacutil /u hello` removerá os assemblies.

Use o exemplo a seguir para evitar a remoção de mais de um assembly. Esse comando remove apenas o assembly `hello` correspondente ao número de versão totalmente especificado, à cultura e à chave pública.

```console
gacutil /u hello, Version=1.0.0.1, Culture="de",PublicKeyToken=45e343aae32233ca
```

O comando a seguir instala os assemblies especificados no arquivo `assemblyList.txt` no cache de assembly global.

```console
gacutil /il assemblyList.txt
```

O comando a seguir remove os assemblies especificados no arquivo `assemblyList.txt` do cache de assembly global.

```console
gacutil /ul assemblyList.txt
```

O comando a seguir instala `myDll.dll` no cache de assembly global e adiciona uma referência para contá-lo. O assembly `myDll.dll` é usado pelo aplicativo `MyApp`. O parâmetro `UNINSTALL_KEY MyApp` especifica a chave do Registro que adiciona `MyApp` a Adicionar/Remover Programas no Windows. O parâmetro de descrição é especificado como `My Application Description`.

```console
gacutil /i /r myDll.dll UNINSTALL_KEY MyApp "My Application Description"
```

O comando a seguir instala `myDll.dll` no cache de assembly global e adiciona uma referência para contá-lo. O parâmetro de esquema, `FILEPATH` e o parâmetro de ID, `c:\applications\myApp\myApp.exe`, especificam o caminho para o aplicativo que está instalando `myDll.dll.`. O parâmetro de descrição é especificado como `MyApp`.

```console
gacutil /i /r myDll.dll FILEPATH c:\applications\myApp\myApp.exe MyApp
```

O comando a seguir instala `myDll.dll` no cache de assembly global e adiciona uma referência para contá-lo. O parâmetro de esquema, `OPAQUE`, permite personalizar os parâmetros de ID e descrição.

```console
gacutil /i /r mydll.dll OPAQUE "Insert custom application details here" "Insert Custom description information here"
```

O comando a seguir remove a referência para `myDll.dll` pelo aplicativo `myApp`. Se essa for a última referência para o assembly, ela também removerá o assembly do cache de assembly global.

```console
gacutil /u /r myDll.dll FILEPATH c:\applications\myApp\myApp.exe MyApp
```

O comando a seguir lista o conteúdo do cache de assembly global.

```console
gacutil /l
```

## <a name="see-also"></a>Confira também

- [Ferramentas](index.md)
- [Cache de assemblies global](../app-domains/gac.md)
- [Regasm.exe (ferramenta de registro de assembly)](regasm-exe-assembly-registration-tool.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
