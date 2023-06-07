---
title: Processando o arquivo XML-guia de programação C#
description: Saiba mais sobre o processamento do arquivo XML em programação em C#. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- XML processing [C#]
- XML [C#], processing
ms.assetid: 60c71193-9dac-4cd3-98c5-100bd0edcc42
ms.openlocfilehash: 6f8a278ed842cd9c4176f3efff423ee048f7e9b9
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381535"
---
# <a name="process-the-xml-file-c-programming-guide"></a>Processar o arquivo XML (guia de programação C#)

O compilador gera uma cadeia de caracteres de ID para cada construção em seu código que está marcada para gerar documentação. (Para obter informações sobre como marcar seu código, consulte [marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md).) A cadeia de caracteres de ID identifica exclusivamente a construção. Programas que processam o arquivo XML podem usar a cadeia de caracteres de ID para identificar os metadados .NET correspondentes ou o item de reflexão ao qual a documentação se aplica.

## <a name="id-strings"></a>Cadeias de caracteres de ID

O arquivo XML não é uma representação hierárquica do seu código. É uma lista simples que tem uma ID gerada para cada elemento.

O compilador observa as seguintes regras quando gera as cadeias de identificação:

- Não há espaços em branco na cadeia de caracteres.

- A primeira parte da cadeia de caracteres identifica o tipo de membro usando um único caractere seguido por dois-pontos. São usados os seguintes tipos de membro:

    |Caractere|Tipo de membro|Observações|
    |---------------|-----------------|-|
    |N|namespace|Não é possível adicionar comentários de documentação a um namespace, mas será possível fazer referências cref a eles se houver suporte.|
    |T|type|Um tipo pode ser uma classe, interface, struct, enum ou delegate.|
    |F|field|
    |P|propriedade|Inclui indexadores ou outras propriedades indexadas.|
    |M|method|Inclui métodos especiais, como construtores e operadores.|
    |E|event|
    |!|cadeia de caracteres de erro|O restante da cadeia de caracteres fornece informações sobre o erro. O compilador C# gera informações de erro para links que não podem ser resolvidos.|

- A segunda parte da cadeia de caracteres é o nome totalmente qualificado do item, iniciando na raiz do namespace. O nome do item, seus tipos delimitadores e o namespace são separados por pontos. Se o nome do próprio item tiver pontos, eles serão substituídos pelo sustenido ('#'). Supõe-se que nenhum item tenha um sinal de hash diretamente em seu nome. Por exemplo, o nome totalmente qualificado do construtor de cadeia de caracteres é "System. String. #ctor".

- Para propriedades e métodos, a lista de parâmetros entre parênteses segue. Se não houver parâmetros, nenhum parêntese estará presente. Os parâmetros são separados por vírgulas. A codificação de cada parâmetro segue diretamente como ele é codificado em uma assinatura do .NET:

  - Tipos base. Tipos regulares (ELEMENT_TYPE_CLASS ou ELEMENT_TYPE_VALUETYPE) são representados como o nome totalmente qualificado do tipo.

  - Os tipos intrínsecos (por exemplo, ELEMENT_TYPE_I4, ELEMENT_TYPE_OBJECT, ELEMENT_TYPE_STRING, ELEMENT_TYPE_TYPEDBYREF e ELEMENT_TYPE_VOID) são representados como o nome totalmente qualificado do tipo completo correspondente. Por exemplo, System.Int32 ou System.TypedReference.

  - ELEMENT_TYPE_PTR é representado como um '\*' após o tipo modificado.

  - ELEMENT_TYPE_BYREF é representado como um "\@" após o tipo modificado.

  - ELEMENT_TYPE_PINNED é representado como um '^' após o tipo modificado. O compilador C# nunca gera isso.

  - ELEMENT_TYPE_CMOD_REQ é representado como um '&#124;' e o nome totalmente qualificado da classe do modificador, após o tipo modificado. O compilador C# nunca gera isso.

  - ELEMENT_TYPE_CMOD_OPT é representado como um '!' e o nome totalmente qualificado da classe do modificador, após o tipo modificado.

  - ELEMENT_TYPE_SZARRAY é representado como "[]" após o tipo de elemento da matriz.

  - ELEMENT_TYPE_GENERICARRAY é representado como "[?]" após o tipo de elemento da matriz. O compilador C# nunca gera isso.

  - ELEMENT_TYPE_ARRAY é representado como [*lowerbound*: `size` ,*lowerbound*: `size` ] em que o número de vírgulas é a Rank-1, e os limites inferiores e o tamanho de cada dimensão, se conhecido, são representados em decimal. Se um limite inferior ou tamanho não for especificado, ele será omitido. Se o limite e o tamanho inferiores de uma determinada dimensão forem omitidos, o ':' será omitido também. Por exemplo, uma matriz bidimensional com 1 como limites inferiores e tamanhos não especificados é [1:,1:].

  - ELEMENT_TYPE_FNPTR é representado como "=FUNC:`type`(*assinatura*)", em que `type` é o tipo de retorno e *assinatura* são os argumentos do método. Se não houver nenhum argumento, os parênteses serão omitidos. O compilador C# nunca gera isso.

  Os seguintes componentes de assinatura não são representados porque não são usados para diferenciar métodos sobrecarregados:

  - convenção de chamada

  - tipo de retorno

  - ELEMENT_TYPE_SENTINEL

- Somente para operadores de conversão ( `op_Implicit` e `op_Explicit` ), o valor de retorno do método é codificado como um ' ~ ' seguido pelo tipo de retorno.

- Para tipos genéricos, o nome do tipo é seguido por um caractere de acento grave e, em seguida, por um número que indica o número de parâmetros de tipo genérico. Por exemplo:

     ``<member name="T:SampleClass`2">`` é a marcação de um tipo definido como `public class SampleClass<T, U>`.

     Para métodos que usam tipos genéricos como parâmetros, os parâmetros de tipo genérico são especificados como números precedidos com backtiques (por exemplo \` , 0, \` 1). Cada número representa uma notação de matriz com base em zero para os parâmetros genéricos do tipo.

## <a name="examples"></a>Exemplos

Os exemplos a seguir mostram como as cadeias de caracteres de ID para uma classe e seus membros são gerados:

[!code-csharp[csProgGuidePointers#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuidePointers/CS/Pointers.cs#21)]

## <a name="see-also"></a>Veja também

- [Guia de programação em C#](../index.md)
- [-Doc (opções do compilador C#)](../../language-reference/compiler-options/doc-compiler-option.md)
- [Comentários da documentação XML](./index.md)
