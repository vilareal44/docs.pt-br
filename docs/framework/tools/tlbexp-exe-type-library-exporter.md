---
title: Tlbexp.exe (Exportador de Biblioteca de Tipos)
description: Examine Tlbexp.exe, o exportador da biblioteca de tipos. Essa ferramenta gera uma biblioteca de tipos que descreve os tipos definidos em um assembly Common Language Runtime (CLR).
ms.date: 03/30/2017
helpviewer_keywords:
- exporting type library [.NET Framework]
- exporter tool [.NET Framework]
- Tlbexp.exe
- Type Library Exporter
- type libraries [.NET Framework], exporting
ms.assetid: a487d61b-d166-467b-a7ca-d8b52fbff42d
ms.openlocfilehash: 3cfaa83590fefe31c437d2ff607fb579aec1da61
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517029"
---
# <a name="tlbexpexe-type-library-exporter"></a>Tlbexp.exe (Exportador de Biblioteca de Tipos)
O Exportador da Biblioteca de Tipos gera uma biblioteca de tipos que descreve os tipos definidos em um assembly do Common Language Runtime.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
tlbexp assemblyName [options]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|*assemblyName*|O assembly para o qual uma biblioteca de tipos deve ser exportada.|  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/asmpath:** *directory*|Especifica o local de pesquisa dos assemblies. Se usar essa opção, você deverá especificar explicitamente os locais de pesquisa dos assemblies referenciados, incluindo o diretório atual.<br /><br /> Quando você usa a opção **asmpath**, o Exportador da Biblioteca de Tipos não procurará um assembly no GAC (cache de assembly global).|  
|**/help**|Exibe sintaxe de comando e opções para a ferramenta.|  
|**/names:** *filename*|Especifica a capitalização de nomes em uma biblioteca de tipos. O argumento *filename* é um arquivo de texto. Cada linha no arquivo especifica a capitalização de um nome na biblioteca de tipos.|  
|**/nologo**|Suprime a exibição do banner de inicialização da Microsoft.|  
|**/oldnames**|Força Tlbexp.exe a exportar nomes de tipo decorados caso haja um conflito no nome do tipo. Esse era o comportamento padrão em versões anteriores ao .NET Framework versão 2.0.|  
|**/out:** *file*|Especifica o nome do arquivo da biblioteca de tipos a ser gerado. Se você omitir essa opção, Tlbexp.exe gerará uma biblioteca de tipos com o mesmo nome do assembly (o nome do assembly real, que talvez não seja necessariamente igual ao arquivo que contém o assembly) e uma extensão .tlb.|  
|**/silence:** `warningnumber`|Suprime a exibição do aviso especificado. Essa opção não pode ser usada com **/silent**.|  
|**/Silent**|Suprime a exibição de mensagens de sucesso. Essa opção não pode ser usada com **/silence**.|  
|**/tlbreference:** *typelibraryname*|Força Tlbexp.exe a resolver explicitamente referências da biblioteca de tipos sem consultar o Registro. Por exemplo, se o assembly B fizer referência ao assembly A, será possível usar essa opção para fornecer uma referência à biblioteca de tipos explícita, em vez de depender da biblioteca de tipos especificada no Registro. Tlbexp.exe realiza uma verificação de versão para garantir que a versão da biblioteca de tipos corresponda à versão do assembly; do contrário, ele gerará um erro.<br /><br /> Observe que a opção **tlbreference** ainda consulta o Registro nos casos em que o atributo <xref:System.Runtime.InteropServices.ComImportAttribute> é aplicado a uma interface implementada por outro tipo.|  
|**/tlbrefpath:** *path*|Caminho totalmente qualificado para uma biblioteca de tipos referenciada.|  
|**/win32**|Durante a compilação em um computador 64 bits, esta opção especifica que Tlbexp.exe gera uma biblioteca de tipos 32 bits.|  
|**/win64**|Durante a compilação em um computador 32 bits, esta opção especifica que Tlbexp.exe gera uma biblioteca de tipos 64 bits.|  
|**/verbose**|Especifica o modo detalhado; exibe uma lista de todos os assemblies referenciados para os quais uma biblioteca de tipos precisa ser gerada.|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
  
> [!NOTE]
> As opções de linha de comando para Tlbexp.exe não diferenciam maiúsculas de minúsculas e podem ser fornecidas em qualquer ordem. Você só precisa especificar o suficiente da opção para identificá-la com exclusividade. Por exemplo, **/n** equivale a **/nologo** e **/o:** *outfile.tlb* equivale a **/out:** *outfile.tlb*.  
  
## <a name="remarks"></a>Comentários  
 Tlbexp.exe gera uma biblioteca de tipos que contém definições dos tipos definidos no assembly. Aplicativos como o Visual Basic 6.0 podem usar a biblioteca de tipos gerada na associação com os tipos do .NET definidos no assembly.  
  
> [!IMPORTANT]
> Não é possível usar Tlbexp.exe para exportar arquivos de metadados do Windows (.winmd). Não há suporte para a exportação de assemblies do Windows Runtime.  
  
 Todo o assembly é convertido de uma só vez. Não é possível usar Tlbexp.exe para gerar informações de tipo para um subconjunto dos tipos definidos em um assembly.  
  
 Não é possível usar Tlbexp.exe para produzir uma biblioteca de tipos com base em um assembly importado usando o [Importador da Biblioteca de Tipos (Tlbimp.exe)](tlbimp-exe-type-library-importer.md). Em vez disso, você deve consultar a biblioteca de tipos original importada com Tlbimp.exe. É possível exportar uma biblioteca de tipos de um assembly que faz referência a assemblies que foram importados usando-se Tlbimp.exe. Consulte a seção de exemplos abaixo.  
  
 Tlbexp.exe coloca bibliotecas de tipos geradas no diretório de trabalho atual ou no diretório especificado para o arquivo de saída. Um único assembly pode fazer várias bibliotecas de tipos serem geradas.  
  
 Tlbexp.exe gera uma biblioteca de tipos, mas não a registra. Isso difere da [Ferramenta de Registro do Assembly (Regasm.exe)](regasm-exe-assembly-registration-tool.md), que gera e registra uma biblioteca de tipos. Para gerar e registrar uma biblioteca de tipos com COM, use Regasm.exe.  
  
 Se você não especificar `/win32` ou a opção `/win64`, Tlbexp.exe gerará uma biblioteca de tipos 32 ou 64 bits que corresponde ao tipo do computador no qual você está executando a compilação (computador 32 bits ou 64 bits). Para fins de compilação cruzada, é possível usar a opção `/win64` em um computador 32 bits para gerar uma biblioteca de tipos 64 bits e usar a opção `/win32` em um computador 64 bits para gerar uma biblioteca de tipos 32 bits. Em bibliotecas de tipos 32 bits, o valor <xref:System.Runtime.InteropServices.ComTypes.SYSKIND> é definido como <xref:System.Runtime.InteropServices.ComTypes.SYSKIND.SYS_WIN32>. Em bibliotecas de tipos 64 bits, o valor <xref:System.Runtime.InteropServices.ComTypes.SYSKIND> é definido como <xref:System.Runtime.InteropServices.ComTypes.SYSKIND.SYS_WIN64>. Todas as transformações de tipo de dados (por exemplo, tipos de dados do tamanho de ponteiro como, por exemplo, `IntPtr` e `UIntPtr`) são convertidas adequadamente.  
  
 Se você usar o atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> para especificar um valor <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType> de `VT_UNKOWN` ou `VT_DISPATCH`, Tlbexp.exe ignorará qualquer uso subsequente do campo <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType>. Por exemplo, dadas as seguintes assinaturas:  
  
```csharp
[return:MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VarEnum.VT_UNKNOWN, SafeArrayUserDefinedSubType=typeof(ConsoleKeyInfo))] public Array StructUnkSafe(){return null;}  
[return:MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VarEnum.VT_DISPATCH, SafeArrayUserDefinedSubType=typeof(ConsoleKeyInfo))] public Array StructDispSafe(){return null;}  
```  
  
 a seguinte biblioteca de tipos é gerada:  
  
```cpp
[id(0x60020004)]  
HRESULT StructUnkSafe([out, retval] SAFEARRAY(IUnknown*)* pRetVal);  
[id(0x60020005)]  
HRESULT StructDispSafe([out, retval] SAFEARRAY(IDispatch*)* pRetVal);  
```  
  
 Tlbexp.exe ignora o campo <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType>.  
  
 Como as bibliotecas de tipos não podem acomodar todas as informações encontradas em assemblies, Tlbexp.exe pode descartar alguns dados durante o processo de exportação. Para ver uma explicação do processo de transformação e a identificação da origem de cada informação emitida para uma biblioteca de tipos, consulte [Resumo da Conversão do Assembly em Biblioteca de Tipos](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100)).  
  
 O Exportador da Biblioteca de Tipos exporta métodos com parâmetros <xref:System.TypedReference> como `VARIANT`, mesmo que o objeto <xref:System.TypedReference> não tenha significado no código não gerenciado. Quando você exportar métodos com parâmetros <xref:System.TypedReference>, o Exportador da Biblioteca de Tipos não gerará um aviso ou um erro, e o código não gerenciado que usa a biblioteca de tipos resultante não será executado corretamente.  
  
 O Exportador da Biblioteca de Tipos é compatível com o Microsoft Windows 2000 e posteriores.  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir gera uma biblioteca de tipos com o mesmo nome do assembly encontrado em `myTest.dll`.  
  
```console  
tlbexp myTest.dll  
```  
  
 O comando a seguir gera uma biblioteca de tipos com o nome `clipper.tlb`.  
  
```console  
tlbexp myTest.dll /out:clipper.tlb  
```  
  
 O exemplo a seguir ilustra o uso de Tlbexp.exe para exportar uma biblioteca de tipos de um assembly que faz referência a assemblies que foram importados usando-se Tlbimp.exe.  
  
 Primeiro use Tlbimp.exe para importar a biblioteca de tipos `myLib.tlb` e salve-a como `myLib.dll`.  
  
```console  
tlbimp myLib.tlb /out:myLib.dll  
```  
  
 O comando a seguir usa o compilador do C# para compilar o `Sample.dll,` que faz referência a `myLib.dll` criado no exemplo anterior.  
  
```console  
CSC Sample.cs /reference:myLib.dll /out:Sample.dll  
```  
  
 O comando a seguir gera uma biblioteca de tipos para `Sample.dll` que faz referência a `myLib.dll`.  
  
```console  
tlbexp Sample.dll  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.TypeLibExporterFlags>
- [Ferramentas](index.md)
- [Regasm.exe (ferramenta de registro de assembly)](regasm-exe-assembly-registration-tool.md)
- [Resumo da conversão de assemblies em bibliotecas de tipos](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100))
- [Tlbimp.exe (tipo de importador de biblioteca de tipos)](tlbimp-exe-type-library-importer.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
