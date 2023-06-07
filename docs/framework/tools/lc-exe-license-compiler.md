---
title: Lc.exe (Compilador de Licença)
description: Use Lc.exe, o compilador de licença. Essa ferramenta lê os arquivos de texto que têm informações de licenciamento e torna um arquivo binário a ser inserido em um executável CLR como um recurso.
ms.date: 03/30/2017
helpviewer_keywords:
- Lc.exe
- .licx file
- License Compiler
- control licenses [Windows Forms]
- compiling licenses file
- license file
- .licenses file
- Windows Forms, control licenses
- licensed controls [Windows Forms]
ms.assetid: 2de803b8-495e-4982-b209-19a72aba0460
ms.openlocfilehash: 45a80ba7c3e24c0f419758315b2d2daafd3890f4
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164252"
---
# <a name="lcexe-license-compiler"></a>Lc.exe (Compilador de Licença)
O Compilador de Licença lê arquivos de texto que contêm informações de licenciamento e produz um arquivo binário que pode ser inserido em um executável do Common Language Runtime como um recurso.  
  
 Um arquivo de texto .licx é gerado automaticamente ou atualizado pelo Designer de Formulários do Windows sempre que um controle licenciado é adicionado ao formulário. Como parte da compilação, o sistema do projeto transformará o arquivo de texto .licx em um recurso binário .licenses que dá suporte ao licenciamento de controle do .NET. Em seguida, o recurso binário será inserido na saída do projeto.  
  
 A compilação cruzada entre 32 e 64 bits não é compatível quando você usa o Compilador de Licença ao compilar o projeto. Isso porque o Compilador de Licença precisa carregar assemblies, e o carregamento de assemblies 64 bits de um aplicativo 32 bits não é permitido, e vice-versa. Nesse caso, use o Compilador de Licença na linha de comando para compilar manualmente a licença e especificar a arquitetura correspondente.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console
      lc /target:  
targetPE /complist:filename [-outdir:path]  
/i:modules [/nologo] [/v]  
```  
  
|Opção|Descrição|  
|------------|-----------------|  
|**/complist:** *filename*|Especifica o nome de um arquivo que contém a lista de componentes licenciados que serão incluídos no arquivo .licenses. Cada componente é referenciado usando-se seu nome completo com apenas um componente por linha.<br /><br /> Os usuários de linha de comando podem especificar um arquivo separado para cada formulário no projeto. Lc.exe aceita vários arquivos de entrada e produz um único arquivo .licenses.|  
|**/h**[**elp**]|Exibe sintaxe de comando e opções para a ferramenta.|  
|**/i:** *module*|Especifica os módulos que contêm os componentes listados no arquivo **/complist**. Para especificar mais de um módulo, use vários sinalizadores **/i**.|  
|**/nologo**|Suprime a exibição do banner de inicialização da Microsoft.|  
|**/outdir:** *path*|Especifica o diretório no qual o arquivo .licenses de saída deve ser colocado.|  
|**/target:** *targetPE*|Especifica o executável para o qual o arquivo .licenses está sendo gerado.|  
|**/v**|Especifica o modo detalhado; exibe informações de andamento da compilação.|  
|**@** do *arquivo*|Especifica o arquivo de resposta (.rsp).|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
  
## <a name="example"></a>Exemplo  
  
1. Se estiver usando um controle licenciado `MyCompany.Samples.LicControl1` contido em `Samples.DLL` em um aplicativo chamado `HostApp.exe`*,* você poderá criar `HostAppLic.txt` que contém o seguinte.  
  
    ```text
    MyCompany.Samples.LicControl1, Samples.DLL  
    ```  
  
2. Crie o arquivo .licenses chamado `HostApp.exe.licenses` usando o comando a seguir.  
  
    ```console  
    lc /target:HostApp.exe /complist:hostapplic.txt /i:Samples.DLL /outdir:c:\bindir  
    ```  
  
3. Compile `HostApp.exe` incluindo o arquivo .licenses como um recurso. Se estivesse compilando um aplicativo do C#, você usaria o comando a seguir para compilar o aplicativo.  
  
    ```console
    csc /res:HostApp.exe.licenses /out:HostApp.exe *.cs  
    ```  
  
 O comando a seguir compila `myApp.licenses` com base nas listas de componentes licenciados especificados por `hostapplic.txt`, `hostapplic2.txt` e `hostapplic3.txt`. O argumento `modulesList` especifica os módulos que contêm os componentes licenciados.  
  
```console  
lc /target:myApp /complist:hostapplic.txt /complist:hostapplic2.txt /complist: hostapplic3.txt /i:modulesList  
```  
  
## <a name="response-file-example"></a>Exemplo de arquivo de resposta  
 A listagem a seguir mostra um exemplo de um arquivo de resposta, `response.rsp`. Para saber mais sobre arquivos de resposta, confira [Arquivos de resposta](/visualstudio/msbuild/msbuild-response-files).  
  
```text  
/target:hostapp.exe  
/complist:hostapplic.txt
/i:WFCPrj.dll
/outdir:"C:\My Folder"  
```  
  
 A linha de comando a seguir usa o arquivo `response.rsp`.  
  
```console  
lc @response.rsp  
```  
  
## <a name="see-also"></a>Confira também

- [Ferramentas](index.md)
- [Al.exe (vinculador de assembly)](al-exe-assembly-linker.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
