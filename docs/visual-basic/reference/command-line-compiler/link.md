---
title: -link
ms.date: 03/10/2018
helpviewer_keywords:
- l compiler option [Visual Basic]
- EmbedInteropTypes
- embed interop types [COM Interop]
- -link compiler option [Visual Basic]
- /link compiler option [Visual Basic]
- link compiler option [Visual Basic]
- -l compiler option [Visual Basic]
- /l compiler option [Visual Basic]
ms.assetid: 1885f24a-86f5-486c-a064-9fb7e455ccec
ms.openlocfilehash: 6082806591d398aa8b16b44e769a3f8078ce62d9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387732"
---
# <a name="-link-visual-basic"></a>-link (Visual Basic)
Faz com que o compilador disponibilize as informações de tipo COM nos assemblies especificados para o projeto sendo compilado no momento.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-link:fileList  
```

ou  

```console
-l:fileList  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`fileList`|Obrigatórios. Lista delimitada por vírgulas de nomes de arquivo do assembly. Se o nome do arquivo contém um espaço, coloque o nome entre aspas.|  
  
## <a name="remarks"></a>Comentários  
 A opção `-link` permite que você implante um aplicativo que inseriu informações de tipo. O aplicativo pode usar tipos em um assembly de runtime que implementa as informações de tipo inseridas sem a necessidade de uma referência ao assembly de runtime. Se forem publicadas várias versões do assembly de runtime, o aplicativo que contém as informações de tipo inseridas poderá trabalhar com as várias versões sem precisar ser recompilado. Para obter um exemplo, consulte [Instruções passo a passo: Inserindo tipos de assemblies gerenciado](../../../standard/assembly/embed-types-visual-studio.md).  
  
 Usar a opção `-link` é especialmente útil quando você está trabalhando com a interoperabilidade COM. Você pode inserir tipos COM para que seu aplicativo não precise mais de um PIA (assembly de interoperabilidade primário) no computador de destino. A opção `-link` instrui o compilador a inserir as informações de tipo de COM do assembly de interoperabilidade referenciado no código compilado resultante. O tipo COM é identificado pelo valor CLSID (GUID). Como resultado, o aplicativo pode ser executado em um computador de destino que tem os mesmos tipos COM instalados com os mesmos valores CLSID. Os aplicativos que automatizam o Microsoft Office são um bom exemplo. Como aplicativos como o Office normalmente mantêm o mesmo valor CLSID entre diferentes versões, seu aplicativo pode usar os tipos COM referenciados contanto que o .NET Framework 4 ou posterior esteja instalado no computador de destino e seu aplicativo use métodos, propriedades ou eventos que estão incluídos nos tipos COM referenciados.  
  
 A opção `-link` incorpora apenas interfaces, estruturas e delegados. Não há suporte para a inserção de classes COM.  
  
> [!NOTE]
> Quando você cria uma instância de um tipo COM inserido no seu código, você deve criar a instância usando a interface apropriada. Tentar criar uma instância de um tipo COM inserido usando o CoClass causa um erro.  
  
 Para definir a opção `-link` em Visual Studio, adicione uma referência de assembly e defina a propriedade `Embed Interop Types` como **true**. O valor padrão da propriedade `Embed Interop Types` é **false**.  
  
 Se você vincular a um assembly COM (Assembly A) que em si faz referência a outro assembly COM (Assembly B), também precisará vincular ao Assembly B se uma das seguintes opções for verdadeira:  
  
- Um tipo do Assembly A herda de um tipo ou implementa uma interface do Assembly B.  
  
- Um campo, propriedade, evento ou método que tem um tipo de retorno ou de parâmetro do Assembly B é invocado.  
  
 Use [-LIBPATH](libpath.md) para especificar o diretório no qual uma ou mais das suas referências de assembly estão localizadas.  
  
 Assim como a opção [-Reference](reference.md) do compilador, a `-link` opção do compilador usa o arquivo de resposta Vbc. rsp, que faz referência a assemblies de .NET Framework usados com frequência. Use a opção [-noconfig](noconfig.md) do compilador se você não quiser que o compilador use o arquivo Vbc. rsp.  
  
 A forma abreviada de `-link` é `-l`.  
  
## <a name="generics-and-embedded-types"></a>Tipos genéricos e inseridos  
 As seções a seguir descrevem as limitações no uso de tipos genéricos em aplicativos que inserem tipos de interoperabilidade.  
  
### <a name="generic-interfaces"></a>Interfaces genéricas  
 As interfaces genéricas que são inseridas de um assembly de interoperabilidade não podem ser usadas. Isso é mostrado no exemplo a seguir.  
  
 [!code-vb[VbLinkCompiler#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/module1.vb#1)]  
  
### <a name="types-that-have-generic-parameters"></a>Tipos que têm parâmetros genéricos  
 Os tipos que têm um parâmetro genérico cujo tipo é inserido de um assembly de interoperabilidade não poderão ser usados se o tipo for de um assembly externo. Essa restrição não se aplica a interfaces. Por exemplo, considere a interface <xref:Microsoft.Office.Interop.Excel.Range> que é definida no assembly <xref:Microsoft.Office.Interop.Excel>. Se uma biblioteca insere tipos de interoperabilidade do assembly <xref:Microsoft.Office.Interop.Excel> e expõe um método que retorna um tipo genérico que tem um parâmetro cujo tipo é a interface <xref:Microsoft.Office.Interop.Excel.Range>, esse método deve retornar uma interface genérica, como mostrado no exemplo de código a seguir.  
  
 [!code-vb[VbLinkCompiler#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/utility.vb#2)]  
[!code-vb[VbLinkCompiler#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/utility.vb#3)]  
[!code-vb[VbLinkCompiler#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/utility.vb#4)]  
  
 No exemplo a seguir, o código do cliente pode chamar o método que retorna a interface genérica <xref:System.Collections.IList> sem erros.  
  
 [!code-vb[VbLinkCompiler#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vblinkcompiler/vb/module1.vb#5)]  
  
## <a name="example"></a>Exemplo  
 A linha de comando a seguir compila o arquivo `OfficeApp.vb` de origem e os assemblies de referência de `COMData1.dll` e `COMData2.dll` para produzir `OfficeApp.exe` .  
  
```console  
vbc -link:COMData1.dll,COMData2.dll -out:OfficeApp.exe OfficeApp.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [Instruções passo a passo: Inserindo tipos de assemblies gerenciados](../../../standard/assembly/embed-types-visual-studio.md)
- [-referência (Visual Basic)](reference.md)
- [-noconfig](noconfig.md)
- [-libpath](libpath.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Introdução à Interoperabilidade COM](../../programming-guide/com-interop/introduction-to-com-interop.md)
