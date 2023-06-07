---
title: Compilador de XSLT (xsltc.exe)
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 672a5ac8-8305-4d28-ba10-11089c2c0924
ms.openlocfilehash: 18f351546699487316858198b705e970de4856c1
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84282604"
---
# <a name="xslt-compiler-xsltcexe"></a>Compilador de XSLT (xsltc.exe)
O compilador XSLT (xsltc.exe) compila folhas de estilos XSLT e gera um assembly. A folha de estilos compilada pode ser passada diretamente para o método <xref:System.Xml.Xsl.XslCompiledTransform.Load%28System.Type%29?displayProperty=nameWithType>. Você não pode gerar assemblies assinados com xsltc.exe.  
  
 A ferramenta xsltc.exe está incluída no Visual Studio. para obter mais informações, consulte os [Downloads do Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
xsltc [options] [/class:<name>] <sourceFile> [[/class:<name>] <sourceFile>...]  
```  
  
## <a name="argument"></a>Argumento  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|`sourceFile`|Especifica o nome da folha de estilos. A folha de estilos deve ser um arquivo local ou estar localizada na intranet.|  
  
## <a name="options"></a>Opções  
  
|Opção|Description|  
|------------|-----------------|  
|`/c[lass]:` `name`|Especifica o nome da classe para a folha de estilos a seguir. O nome da classe pode ser totalmente qualificado.<br /><br /> O nome da classe utiliza como padrão o nome da folha de estilos. Por exemplo, se a folha de estilos customers.xsl for compilada, o nome da classe padrão será customers.|  
|`/debug[`+&#124;-`]`|Especifica se informações de depuração devem ser geradas.<br /><br /> Especificar `+` ou `/debug` faz o compilador gerar informações de depuração e colocá-lo em um arquivo de banco de dados do programa (PDB). O nome do arquivo PDB gerado é `assemblyName`.pdb.<br /><br /> Especificar `-`, que é aplicado se você não especificar `/debug`, não cria nenhuma informação de depuração. Um assembly comercial é gerado. **Observação:** a compilação no modo de depuração pode afetar o desempenho do XSLT significativamente.|  
|`/help`|Exibe sintaxe de comando e opções para a ferramenta.|  
|`/nologo`|Suprime a notificação de direitos autorais do compilador da exibição.|  
|`/platform:` `string`|Especifica as plataformas nas quais o assembly pode ser executado. O exemplo a seguir descreve os valores válidos da plataforma:<br /><br /> O `x86` compila o assembly para ser executado pelo Common Language Runtime compatível com x86, de 32 bits<br /><br /> O `x64` compila o assembly para ser executado pelo Common Language Runtime de 64 bits em um computador que oferece suporte ao conjunto de instruções de AMD64 ou EM64T.<br /><br /> O Itanium compila seu assembly para que ele seja executado pelo Common Language Runtime de 64 bits em um computador que tem um processador Itanium.<br /><br /> O `anycpu` compila o assembly para ser executado em qualquer plataforma. Este é o padrão.|  
|`/out:` `assemblyName`|Especifica o nome do assembly que é a saída. O nome do assembly tem como padrão o nome da folha de estilos principal ou a primeira folha de estilos se várias folhas de estilos estiverem presentes.<br /><br /> Se a folha de estilos contiver scripts, os scripts serão salvos em um assembly separado. Nomes assembly de script são gerados do nome do assembly principal. Por exemplo, se você especificou CustOrders.dll para o nome do assembly, o primeiro assembly de script será chamado CustOrders_Script1.dll.|  
|`/settings:` `document+-, script+-, DTD+-,`|Especifica se deve permitir funções `document()`, script XSLT ou definição de tipo de documento (DTD) na folha de estilos.<br /><br /> O comportamento padrão desabilita o suporte para DTD, a função `document()` e script.|  
|`@` `file`|Permite que você especifique um arquivo que contém as opções do compilador.|  
|`?`|Exibe sintaxe de comando e opções para a ferramenta.|  
  
## <a name="remarks"></a>Comentários  
 As soluções XSLT podem consistir em vários módulos de folha de estilos. A ferramenta xsltc.exe gera assemblies de folhas de estilos. Os assemblies podem então ser passados no método <xref:System.Xml.Xsl.XslCompiledTransform.Load%28System.Type%29?displayProperty=nameWithType>. Isso pode ajudar a reduzir os custos de desempenho em alguns cenários de implantação de XSLT.  
  
> [!NOTE]
> Você também deve incluir o assembly compilado como uma referência em seu aplicativo.  
  
 A ferramenta xsltc.exe não valida os nomes de classe (`/class:`*name*) nem de assembly (`/out:`*assemblyName*). Erros serão lançados pelo Common Language Runtime se os nomes forem inválidos.  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir compila a folha de estilos e cria um assembly chamado booksort.dll.  
  
```console  
xsltc booksort.xsl  
```  
  
 O comando a seguir compila a folha de estilos e cria um assembly e o arquivo de PDB que são chamados booksort.dll e booksort.pdb respectivamente.  
  
```console  
xsltc booksort.xsl /debug  
```  
  
 O comando a seguir cria uma folha de estilos que contém um elemento msxsl:script e cria dois assemblies chamados calc.dll e calc_Script1.dll.  
  
```console  
xsltc /settings:script+ calc.xsl  
```  
  
 O comando a seguir permite o processamento de DTD e suporte de script e cria dois assemblies chamados myTest.dll e myTest_Script1.dll.  
  
```console  
xsltc /settings:DTD+,script+ /out:myTest calc.xsl  
```  
  
 O comando a seguir compila dois módulos de folha de estilos e cria um único assembly chamado booksort.dll.  
  
```console  
xsltc booksort.xsl output.xsl  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.Xsl.XslCompiledTransform>
- [Como: Executar uma transformação XSLT usando um assembly](how-to-perform-an-xslt-transformation-by-using-an-assembly.md)
- [Transformações XSLT](xslt-transformations.md)
