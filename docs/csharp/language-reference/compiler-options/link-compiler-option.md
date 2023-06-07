---
title: -link (opções do compilador C#)
ms.date: 07/20/2015
helpviewer_keywords:
- /l compiler option [C#]
- /link compiler option [C#]
- -l compiler option [C#]
- EmbedInteropTypes
- l compiler option [C#]
- embed interop types [COM Interop]
- -link compiler option [C#]
- link compiler option [C#]
ms.assetid: 00da70c6-9ea1-43c2-86f2-aa7f26c03475
ms.openlocfilehash: d5684298bbd736cae2d9c13381431036806aab17
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144455"
---
# <a name="-link-c-compiler-options"></a>-link (opções do compilador C#)
Faz com que o compilador disponibilize as informações de tipo COM nos assemblies especificados para o projeto sendo compilado no momento.

## <a name="syntax"></a>Sintaxe

```console
-link:fileList
// -or-
-l:fileList
```

## <a name="arguments"></a>Argumentos
 `fileList`  
 Obrigatórios. Lista delimitada por vírgulas de nomes de arquivo do assembly. Se o nome do arquivo contém um espaço, coloque o nome entre aspas.

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

 Como a opção do compilador [-reference](./reference-compiler-option.md), a opção do compilador `-link` usa o arquivo de resposta Csc.rsp, que faz referência a assemblies do .NET Framework usados com frequência. Use a opção do compilador [-noconfig](./noconfig-compiler-option.md) se não quiser que o compilador use o arquivo Csc.rsp.

 A forma abreviada de `-link` é `-l`.

## <a name="generics-and-embedded-types"></a>Tipos genéricos e inseridos
 As seções a seguir descrevem as limitações no uso de tipos genéricos em aplicativos que inserem tipos de interoperabilidade.

### <a name="generic-interfaces"></a>Interfaces genéricas
 As interfaces genéricas que são inseridas de um assembly de interoperabilidade não podem ser usadas. Isso é mostrado no exemplo a seguir.

 [!code-csharp[VbLinkCompilerCS#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/program.cs#1)]

### <a name="types-that-have-generic-parameters"></a>Tipos que têm parâmetros genéricos
 Os tipos que têm um parâmetro genérico cujo tipo é inserido de um assembly de interoperabilidade não poderão ser usados se o tipo for de um assembly externo. Essa restrição não se aplica a interfaces. Por exemplo, considere a interface <xref:Microsoft.Office.Interop.Excel.Range> que é definida no assembly <xref:Microsoft.Office.Interop.Excel>. Se uma biblioteca insere tipos de interoperabilidade do assembly <xref:Microsoft.Office.Interop.Excel> e expõe um método que retorna um tipo genérico que tem um parâmetro cujo tipo é a interface <xref:Microsoft.Office.Interop.Excel.Range>, esse método deve retornar uma interface genérica, como mostrado no exemplo de código a seguir.

[!code-csharp[VbLinkCompilerCS#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/utility.cs)]

 No exemplo a seguir, o código do cliente pode chamar o método que retorna a interface genérica <xref:System.Collections.IList> sem erros.

 [!code-csharp[VbLinkCompilerCS#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/program.cs#5)]

## <a name="example"></a>Exemplo
 O código a seguir compila o arquivo de origem `OfficeApp.cs` e faz referência aos assemblies de `COMData1.dll` e `COMData2.dll` para produzir `OfficeApp.exe`.

```csharp
csc -link:COMData1.dll,COMData2.dll -out:OfficeApp.exe OfficeApp.cs
```

## <a name="see-also"></a>Consulte também

- [Opções do compilador C#](./index.md)
- [Instruções passo a passo: Inserindo tipos de assemblies gerenciados](../../../standard/assembly/embed-types-visual-studio.md)
- [-Reference (opções do compilador C#)](./reference-compiler-option.md)
- [-noconfig (opções do compilador C#)](./noconfig-compiler-option.md)
- [Compilando pela linha de comando com csc.exe](./command-line-building-with-csc-exe.md)
- [Visão geral sobre interoperabilidade](../../programming-guide/interop/interoperability-overview.md)
