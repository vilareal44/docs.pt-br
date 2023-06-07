---
title: SecAnnotate.exe (ferramenta Anotador de Segurança do .NET)
description: Use SecAnnotate.exe, a ferramenta de anotação de segurança do .NET. Identifique as partes SecurityCritical e SecuritySafeCritical de um ou mais assemblies.
ms.date: 03/30/2017
helpviewer_keywords:
- SecAnnotate.exe
- Security Annotator tool
ms.assetid: 8104d208-7813-4a1d-8a75-58f9a7bcb8c9
ms.openlocfilehash: 440ad39f1afb54ad517bc73f05d1e60748b7b520
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517211"
---
# <a name="secannotateexe-net-security-annotator-tool"></a>SecAnnotate.exe (ferramenta Anotador de Segurança do .NET)
A ferramenta Anotador de Segurança do .NET (SecAnnotate.exe) é um aplicativo de linha de comando que identifica as partes `SecurityCritical` e `SecuritySafeCritical` de um ou mais assemblies.  
  
 Uma extensão do Visual Studio, [Anotador de Segurança](https://marketplace.visualstudio.com/items?itemName=sheldonb.SecurityAnnotator), fornece uma interface gráfica do usuário para SecAnnotate.exe e permite executar a ferramenta no Visual Studio.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte, em que *parameters* são descritos na seção a seguir e *assemblies* consistem em um ou mais nomes de assembly separados por espaços em branco:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
SecAnnotate.exe [parameters] [assemblies]  
```  
  
## <a name="parameters"></a>parâmetros  
  
|Opção|Descrição|  
|------------|-----------------|  
|`/a`<br /><br /> ou<br /><br /> `/showstatistics`|Mostra estatísticas sobre o uso de transparência em assemblies que estão sendo analisados.|  
|`/d:`*do diretório*<br /><br /> ou<br /><br /> `/referencedir:`*do diretório*|Especifica um diretório para procura assemblies dependentes durante a anotação.|  
|`/i`<br /><br /> ou<br /><br /> `/includesignatures`|Inclui informações de assinatura estendidas no arquivo de relatório da anotação.|  
|`/n`<br /><br /> ou<br /><br /> `/nogac`|Suprime a procura de assemblies referenciados no cache de assembly global.|  
|`/o:` *output.xml*<br /><br /> ou<br /><br /> `/out:` *output.xml*|Especifica o arquivo de anotação de saída.|  
|`/p:` *maxpasses*<br /><br /> ou<br /><br /> `/maximumpasses:` *maxpasses*|Especifica o número máximo de passagens de anotação que serão feitas em assemblies antes de parar a geração de novas anotações.|  
|`/q`<br /><br /> ou<br /><br /> `/quiet`|Especifica o modo silencioso, no qual o anotador não produz mensagens de status; ele produz apenas informações de erro.|  
|`/r:`*assembly* do<br /><br /> ou<br /><br /> `/referenceassembly:`*assembly* do|Inclui o assembly especificado durante a resolução de assemblies dependentes durante a anotação. Os assemblies de referência recebem prioridade sobre assemblies encontrados no caminho de referência.|  
|`/s:`*RuleName*<br /><br /> ou<br /><br /> `/suppressrule:`*RuleName*|Suprime a execução da regra de transparência especificada em assemblies de entrada.|  
|`/t`<br /><br /> ou<br /><br /> `/forcetransparent`|Força a ferramenta Anotador a tratar todos os assemblies sem anotações de transparência como se fossem totalmente transparentes.|  
|`/t`:*assembly*<br /><br /> ou<br /><br /> `/forcetransparent`:*assembly*|Force o assembly indicado para ser transparente, independentemente das anotações no nível de assembly atuais.|  
|||  
|`/v`<br /><br /> ou<br /><br /> `/verify`|Verifica apenas se as anotações de um assembly estão corretas; não tenta fazer várias passagens para encontrar todas as anotações obrigatórias se o assembly não verificar.|  
|`/x`<br /><br /> ou<br /><br /> `/verbose`|Especifica a saída detalhada durante a anotação.|  
|`/y:`*do diretório*<br /><br /> ou<br /><br /> `/symbolpath:`*do diretório*|Inclui o diretório especificado durante a procura de arquivos de símbolo durante a anotação.|  
  
## <a name="remarks"></a>Comentários  
 Os parâmetros e os assemblies também podem ser fornecidos em um arquivo de resposta especificado na linha de comando e prefixado com uma arroba (@). Cada linha no arquivo de resposta deve conter um único parâmetro ou nome de assembly.  
  
 Para obter mais informações sobre o Anotador de Segurança do .NET, consulte a entrada [Using SecAnnotate to Analyze Your Assemblies for Transparency Violations](https://docs.microsoft.com/archive/blogs/shawnfa/using-secannotate-to-analyze-your-assemblies-for-transparency-violations-an-example) (Usando o SecAnnotate para analisar os assemblies quanto a violações de transparência) no .NET Security Blog.  
  
## <a name="examples"></a>Exemplos
