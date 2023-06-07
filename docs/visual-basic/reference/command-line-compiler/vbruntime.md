---
title: -vbruntime
ms.date: 03/13/2018
f1_keywords:
- vbruntime
- -vbruntime
helpviewer_keywords:
- vbruntime compiler option [Visual Basic]
- -vbruntime compiler option [Visual Basic]
- /vbruntime compiler option [Visual Basic]
ms.assetid: 1aa0239e-511a-4c29-957d-fd72877b350a
ms.openlocfilehash: 31b719fb7e43cdd6ac44424b359999410dd608a5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403038"
---
# <a name="-vbruntime"></a>-vbruntime
Especifica que o compilador deve compilar sem uma referência para a biblioteca de tempo de execução Visual Basic ou com uma referência a uma biblioteca de tempo de execução específica.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-vbruntime:{ - | + | * | path }  
```  
  
## <a name="arguments"></a>Argumentos  
 \-  
 Compile sem uma referência para a biblioteca de tempo de execução Visual Basic.  
  
 \+  
 Compile com uma referência à biblioteca padrão de tempo de execução de Visual Basic.  
  
 \*  
 Compile sem uma referência à biblioteca de tempo de execução Visual Basic e insira a funcionalidade principal da biblioteca de tempo de execução Visual Basic no assembly.  
  
 `path`  
 Compile com uma referência à biblioteca (DLL) especificada.  
  
## <a name="remarks"></a>Comentários  
 A `-vbruntime` opção de compilador permite que você especifique que o compilador deve compilar sem uma referência para a biblioteca de tempo de execução Visual Basic. Se você compilar sem uma referência para a biblioteca de tempo de execução Visual Basic, erros ou avisos serão registrados em constructos de código ou linguagem que geram uma chamada para um auxiliar de tempo de execução Visual Basic. (Um *auxiliar de tempo de execução Visual Basic* é uma função definida em Microsoft. VisualBasic. dll que é chamada em tempo de execução para executar uma semântica de linguagem específica.)  
  
 A `-vbruntime+` opção produz o mesmo comportamento que ocorre se nenhuma `-vbruntime` opção for especificada. Você pode usar a `-vbruntime+` opção para substituir as `-vbruntime` opções anteriores.  
  
 A maioria dos objetos do `My` tipo não está disponível quando você usa `-vbruntime-` as `-vbruntime:path` Opções ou.  
  
## <a name="embedding-visual-basic-runtime-core-functionality"></a>Inserindo Visual Basic funcionalidade básica de tempo de execução  
 A `-vbruntime*` opção permite que você compile sem uma referência a uma biblioteca de tempo de execução. Em vez disso, a funcionalidade básica da biblioteca Visual Basic Runtime é inserida no assembly do usuário. Você pode usar essa opção se seu aplicativo for executado em plataformas que não contêm o tempo de execução de Visual Basic.  
  
 Os seguintes membros de tempo de execução são inseridos:  
  
- Classe <xref:Microsoft.VisualBasic.CompilerServices.Conversions>  
  
- Método <xref:Microsoft.VisualBasic.Strings.AscW%28System.Char%29>  
  
- Método <xref:Microsoft.VisualBasic.Strings.AscW%28System.String%29>  
  
- Método <xref:Microsoft.VisualBasic.Strings.ChrW%28System.Int32%29>  
  
- <xref:Microsoft.VisualBasic.Constants.vbBack>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbCr>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbCrLf>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbFormFeed>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbLf>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbNewLine>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbNullChar>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbNullString>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbTab>amortiza  
  
- <xref:Microsoft.VisualBasic.Constants.vbVerticalTab>amortiza  
  
- Alguns objetos do `My` tipo  
  
 Se você compilar usando a `-vbruntime*` opção e seu código fizer referência a um membro da biblioteca de tempo de execução Visual Basic que não está incorporada com a funcionalidade principal, o compilador retornará um erro que indica que o membro não está disponível.  
  
## <a name="referencing-a-specified-library"></a>Referenciando uma biblioteca especificada  
 Você pode usar o `path` argumento para compilar com uma referência a uma biblioteca de tempo de execução personalizada em vez da biblioteca de tempo de execução padrão Visual Basic.  
  
 Se o valor do `path` argumento for um caminho totalmente qualificado para uma dll, o compilador usará esse arquivo como a biblioteca de tempo de execução. Se o valor do `path` argumento não for um caminho totalmente qualificado para uma dll, o compilador Visual Basic pesquisará a DLL identificada na pasta atual primeiro. Em seguida, ele pesquisará no caminho que você especificou usando a opção de compilador [-sdkpath](sdkpath.md) . Se a `-sdkpath` opção do compilador não for usada, o compilador pesquisará a DLL identificada na pasta .NET Framework ( `%systemroot%\Microsoft.NET\Framework\versionNumber` ).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar a `-vbruntime` opção para compilar com uma referência a uma biblioteca personalizada.  
  
```console
vbc -vbruntime:C:\VBLibraries\CustomVBLibrary.dll  
```  
  
## <a name="see-also"></a>Confira também

- [Núcleo de Visual Basic – novo modo de compilação no Visual Studio 2010 SP1](https://devblogs.microsoft.com/vbteam/vb-core-new-compilation-mode-in-visual-studio-2010-sp1/)
- [Compilador de linha de comando do Visual Basic](index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [-sdkpath](sdkpath.md)
