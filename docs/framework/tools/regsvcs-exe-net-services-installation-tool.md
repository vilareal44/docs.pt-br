---
title: Regsvcs.exe (Ferramenta de Instalação de Serviços .NET)
description: Use Regsvcs.exe, a ferramenta de instalação de serviços .NET. Carregue e registre um assembly, configure os serviços que você adicionou programaticamente a uma classe e muito mais.
ms.date: 03/30/2017
helpviewer_keywords:
- Regsvcs.exe
- .NET Services Installation tool
- loading assemblies
- assemblies [.NET Framework], registering
- type libraries
- registering assemblies
ms.assetid: 5220fe58-5aaf-4e8e-8bc3-b78c63025804
ms.openlocfilehash: 6d0090eda764113407e35a3bcec139f1c7cfb050
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517237"
---
# <a name="regsvcsexe-net-services-installation-tool"></a>Regsvcs.exe (Ferramenta de Instalação de Serviços .NET)
A ferramenta Instalação de Serviços .NET realiza as seguintes ações:  
  
- Carrega e registra um assembly.  
  
- Gera, registra e instala uma biblioteca de tipos em um aplicativo COM+ especificado.  
  
- Configura serviços adicionados programaticamente à classe.  
  
 Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
      regsvcs [/c | /fc | /u] [/tlb:typeLibraryFile] [/extlb]  
[/reconfig] [/componly] [/appname:applicationName]  
[/nologo] [/quiet]assemblyFile.dll
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|*assemblyFile.dll*|O arquivo do assembly de origem. O assembly deve ser assinado com um nome forte. Para obter mais informações, consulte [Assinando um assembly com um nome forte](../../standard/assembly/sign-strong-name.md).|  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/appdir:** *path*|Especifica o diretório raiz do aplicativo.|  
|**/appname:** *applicationName*|Especifica o nome do aplicativo COM+ a ser encontrado ou criado.|  
|**/c**|Cria o aplicativo de destino.|  
|**/componly**|Configura apenas componentes; ignora métodos e interfaces.|  
|**/exapp**|Especifica a ferramenta para aguardar um aplicativo existente.|  
|**/extlb**|Usa uma biblioteca de tipos existente.|  
|**/FC**|Encontra ou cria o aplicativo de destino.|  
|**/help**|Exibe sintaxe de comando e opções para a ferramenta.|  
|**/noreconfig**|Não reconfigura um aplicativo de destino existente.|  
|**/nologo**|Suprime a exibição do banner de inicialização da Microsoft.|  
|**/parname:** *name*|Especifica o nome ou a ID do aplicativo COM+ a ser encontrado ou criado.|  
|**/reconfig**|Reconfigura um aplicativo de destino existente. Esse é o padrão.|  
|**/tlb:** *typelibraryfile*|Especifica a biblioteca de tipos a ser instalada.|  
|**/u**|Desinstala o aplicativo de destino.|  
|**/quiet**|Especifica o modo silencioso; suprime o logotipo e a exibição da mensagem de êxito.|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
  
## <a name="remarks"></a>Comentários  
 O Regsvcs.exe exige um arquivo do assembly de origem especificado por *assemblyFile.dll*. Esse assembly deve ser assinado com um nome forte. Para obter mais informações sobre a assinatura de nome forte, consulte [Assinando um assembly com um nome forte](../../standard/assembly/sign-strong-name.md). Os nomes do aplicativo de destino e do arquivo da biblioteca de tipos são opcionais. O argumento *applicationName* pode ser gerado com base no arquivo do assembly de origem e será criado por Regsvcs.exe, se ainda não existir. O argumento *typelibraryfile* pode especificar um nome da biblioteca de tipos. Se você não especificar um nome da biblioteca de tipos, Regsvcs.exe usará o nome do assembly como o padrão.  
  
 Quando Regsvcs.exe registra os métodos de um componente, ele está sujeito a [demandas](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/9kc0c6st(v=vs.100)) e [demandas de link](../misc/link-demands.md) nesses métodos. Como a ferramenta é executada em um ambiente totalmente confiável, a maioria das demandas de uma permissão é bem-sucedida. No entanto, Regsvcs.exe não pode registrar componentes com métodos protegidos por uma demanda ou uma exigência de vínculo para <xref:System.Security.Permissions.StrongNameIdentityPermission> ou <xref:System.Security.Permissions.PublisherIdentityPermission>.  
  
 Você deve ter privilégios administrativos no computador local para usar Regsvcs.exe.  
  
 Se falhar durante a realização de alguma dessas ações, Regsvcs.exe exibirá mensagens de erro correspondentes.  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir adiciona todas as classes públicas contidas em `myTest.dll` a `myTargetApp` (um aplicativo COM+ existente) e produz a biblioteca de tipos `myTest.tlb`.  
  
```console  
regsvcs /appname:myTargetApp myTest.dll  
```  
  
 O comando a seguir adiciona todas as classes públicas contidas em `myTest.dll` a `myTargetApp` (um aplicativo COM+ existente) e produz a biblioteca de tipos `newTest.tlb`.  
  
```console  
regsvcs /appname:myTargetApp /tlb:newTest.tlb myTest.dll  
```  
  
## <a name="see-also"></a>Consulte também

- [Ferramentas](index.md)
- [Como assinar um assembly com um nome forte](../../standard/assembly/sign-strong-name.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
