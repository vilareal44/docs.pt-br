---
title: Usando o tipo dynamic – Guia de Programação em C#
description: Saiba como usar o tipo dinâmico. O tipo dinâmico é um tipo estático, mas objetos dinâmicos ignoram a verificação de tipo estático.
ms.date: 07/20/2015
helpviewer_keywords:
- dynamic [C#], about dynamic type
- dynamic type [C#]
ms.assetid: 3828989d-c967-4a51-b948-857ebc8fdf26
ms.openlocfilehash: 9904f0452feca388704067b1fd5432f74d0df86b
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381574"
---
# <a name="using-type-dynamic-c-programming-guide"></a>Usando o tipo dynamic (Guia de Programação em C#)

O C# 4 apresenta um novo tipo, `dynamic`. O tipo é um tipo estático, mas um objeto do tipo `dynamic` ignora a verificação de tipo estático. Na maioria dos casos, ele funciona como se tivesse o tipo `object`. Em tempo de compilação, supõem-se que um elemento que tem o tipo `dynamic` dá suporte a qualquer operação. Portanto, você não precisa se preocupar se o objeto obtém seu valor de uma API COM, de uma linguagem dinâmica como o IronPython, do HTML DOM (Modelo de Objeto do Documento), a reflexão ou de algum outro lugar no programa. No entanto, se o código não for válido, os erros serão capturados em tempo de execução.

Por exemplo, se método de instância `exampleMethod1` no código a seguir tiver apenas um parâmetro, o compilador reconhecerá que a primeira chamada para o método, `ec.exampleMethod1(10, 4)`, não é válido porque ele contém dois argumentos. Essa chamada causa um erro do compilador. A segunda chamada para o método, `dynamic_ec.exampleMethod1(10, 4)`, não é verificada pelo compilador porque o tipo de `dynamic_ec` é `dynamic`. Portanto, nenhum erro de compilador é relatado. No entanto, o erro não escapa o aviso indefinidamente. Ele é detectado em tempo de execução e causa uma exceção de tempo de execução.

[!code-csharp[CsProgGuideTypes#50](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#50)]

[!code-csharp[CsProgGuideTypes#56](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#56)]

A função do compilador nesses exemplos é reunir informações sobre o que cada instrução está propondo fazer para o objeto ou expressão que tem o tipo `dynamic`. Em tempo de execução, as informações armazenadas são examinadas e qualquer instrução inválida causa uma exceção de tempo de execução.

O resultado de operações mais dinâmicas é `dynamic`. Por exemplo, se você passar o ponteiro do mouse sobre o uso de `testSum` no exemplo a seguir, o IntelliSense exibirá o tipo **(variável local) testSum dinâmico**.

[!code-csharp[CsProgGuideTypes#51](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#51)]

As operações em que o resultado não é `dynamic` incluem:

* Conversões de `dynamic` em outro tipo.
* Chamadas de construtor que incluem argumentos do tipo `dynamic`.

Por exemplo, o tipo de `testInstance` na seguinte declaração é `ExampleClass` e não `dynamic`:

[!code-csharp[CsProgGuideTypes#52](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#52)]

Exemplos de conversão são mostrados na seção a seguir, "Conversões".

## <a name="conversions"></a>Conversões

As conversões entre objetos dinâmicos e outros tipos são fáceis. Isso permite que o desenvolvedor mude entre o comportamento dinâmico e o não dinâmico.

Qualquer objeto pode ser convertido no tipo dinâmico implicitamente, conforme mostrado nos exemplos a seguir.

[!code-csharp[CsProgGuideTypes#53](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#53)]

Por outro lado, uma conversão implícita pode ser aplicada dinamicamente a qualquer expressão do tipo `dynamic`.

[!code-csharp[CsProgGuideTypes#54](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#54)]

## <a name="overload-resolution-with-arguments-of-type-dynamic"></a>Resolução de sobrecarga com argumentos de tipo dynamic

A resolução de sobrecarga ocorre em tempo de execução em vez de em tempo de compilação se um ou mais dos argumentos em uma chamada de método tem o tipo `dynamic` ou se o receptor da chamada do método é do tipo `dynamic`. No exemplo a seguir, se o único método `exampleMethod2` acessível for definido para obter um argumento de cadeia de caracteres, enviar `d1` como o argumento não causará um erro de compilador, mas causará uma exceção de tempo de execução. A resolução de sobrecarga falha em tempo de execução porque o tipo de tempo de execução de `d1` é `int` e `exampleMethod2` requer uma cadeia de caracteres.

[!code-csharp[CsProgGuideTypes#55](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#55)]

## <a name="dynamic-language-runtime"></a>runtime de linguagem dinâmico

O DLR (Dynamic Language Runtime) é uma API que foi introduzida no .NET Framework 4. Ele fornece a infraestrutura que dá suporte ao tipo `dynamic` em C# e também à implementação das linguagens de programação dinâmicas como IronPython e IronRuby. Para obter mais informações sobre o DLR, consulte [Visão geral do Dynamic Language Runtime](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md).

## <a name="com-interop"></a>interoperabilidade COM

O C# 4 inclui vários recursos que aprimoram a experiência de interoperar com APIs COM, como as APIs de Automação do Office. Entre os aperfeiçoamentos estão o uso do tipo `dynamic` e de [argumentos nomeados e opcionais](../classes-and-structs/named-and-optional-arguments.md).

Muitos métodos COM permitem variação nos tipos de argumento e tipo de retorno, especificando os tipos como `object`. Isso exigiu a conversão explícita dos valores para coordenar com variáveis fortemente tipadas no C#. Se você compilar usando a opção [-link (opções do compilador C#)](../../language-reference/compiler-options/link-compiler-option.md) , a introdução do `dynamic` tipo permitirá tratar as ocorrências de `object` em assinaturas com como se fossem do tipo `dynamic` e, portanto, evitar grande parte da conversão. Por exemplo, as seguintes instruções de contrastam como acessar uma célula em uma planilha do Microsoft Office Excel com o tipo `dynamic` e sem o tipo `dynamic`.

[!code-csharp[csOfficeWalkthrough#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#12)]

[!code-csharp[csOfficeWalkthrough#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#13)]

## <a name="related-topics"></a>Tópicos relacionados

|Título|Descrição|
|-----------|-----------------|
|[dinâmico](../../language-reference/builtin-types/reference-types.md)|Descreve o uso da palavra-chave `dynamic`.|
|[Visão geral do tempo de execução de linguagem dinâmica](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)|Fornece uma visão geral do DLR, que é um ambiente de tempo de execução que adiciona um conjunto de serviços para as linguagens dinâmicas para o CLR (Common Language Runtime).|
|[Walkthrough: Criando e usando objetos dinâmicos](walkthrough-creating-and-using-dynamic-objects.md)|Fornece instruções passo a passo para criar um objeto dinâmico personalizado e para criar um projeto que acessa uma biblioteca `IronPython`.|
|[Como acessar objetos de interoperabilidade do Office usando recursos do C#](../interop/how-to-access-office-onterop-objects.md)|Demonstra como criar um projeto que usa argumentos nomeados e opcionais, o tipo `dynamic` e outros aprimoramentos que simplificam o acesso aos objetos de API do Office.|
