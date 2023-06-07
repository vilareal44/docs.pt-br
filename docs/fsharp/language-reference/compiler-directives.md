---
title: Diretivas de compilador
description: 'Saiba mais sobre as diretivas de pré-processador de linguagem F #, diretivas de compilação condicional, diretivas de linha e diretivas de compilador.'
ms.date: 12/10/2018
f1_keywords:
- '#endif_FS'
ms.openlocfilehash: aee307eb7bccc8d91b5162f3f43db3b806b761d0
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855368"
---
# <a name="compiler-directives"></a>Diretivas de compilador

Este tópico descreve as diretivas do processador e as diretivas do compilador.

## <a name="preprocessor-directives"></a>Diretivas de pré-processador

Uma diretiva de pré-processador é prefixada com o símbolo # e aparece em uma linha por si só. Ele é interpretado pelo pré-processador, que é executado antes do próprio compilador.

A tabela a seguir lista as diretivas de pré-processador que estão disponíveis em F #.

|Diretiva|Descrição|
|---------|-----------|
|`#if`*símbolo* de|Dá suporte à compilação condicional. O código na seção após o `#if` será incluído se o *símbolo* for definido. O símbolo também pode ser negado com `!` .|
|`#else`|Dá suporte à compilação condicional. Marca uma seção de código a ser incluída se o símbolo usado com o anterior `#if` não estiver definido.|
|`#endif`|Dá suporte à compilação condicional. Marca o final de uma seção condicional de código.|
|`#`descritos *int*,<br/>`#`descritos *int* *cadeia de caracteres*int,<br/>`#`descritos *int* *textual-cadeia de caracteres*|Indica a linha de código-fonte original e o nome do arquivo para depuração. Esse recurso é fornecido para ferramentas que geram código-fonte F #.|
|`#nowarn`*WarningCode*|Desabilita um aviso do compilador ou avisos. Para desabilitar um aviso, localize o número da saída do compilador e inclua-o entre aspas. Omita o prefixo "FS". Para desabilitar vários números de aviso na mesma linha, inclua cada número entre aspas e separe cada cadeia de caracteres por um espaço. Por exemplo:

`#nowarn "9" "40"`

O efeito de desabilitar um aviso se aplica ao arquivo inteiro, incluindo partes do arquivo que precede a diretiva. |

## <a name="conditional-compilation-directives"></a>Diretivas de compilação condicional

O código que é desativado por uma dessas diretivas aparece esmaecido no editor de Visual Studio Code.

> [!NOTE]
> O comportamento das diretivas de compilação condicional não é o mesmo que em outros idiomas. Por exemplo, você não pode usar expressões boolianas que envolvam símbolos e `true` `false` não têm nenhum significado especial. Os símbolos que você usa na `if` diretiva devem ser definidos pela linha de comando ou nas configurações do projeto; não há nenhuma `define` diretiva de pré-processador.

O código a seguir ilustra o uso das `#if` `#else` diretivas, e `#endif` . Neste exemplo, o código contém duas versões da definição de `function1` . Quando `VERSION1` é definido usando a [opção-define do compilador](https://msdn.microsoft.com/library/434394ae-0d4a-459c-a684-bffede519a04), o código entre a `#if` diretiva e a `#else` diretiva é ativado. Caso contrário, o código entre `#else` e `#endif` é ativado.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7301.fs)]

Não há nenhuma `#define` diretiva de pré-processador em F #. Você deve usar a opção do compilador ou as configurações do projeto para definir os símbolos usados pela `#if` diretiva.

As diretivas de compilação condicional podem ser aninhadas. O recuo não é significativo para diretivas de pré-processador.

Você também pode negar um símbolo com `!` . Neste exemplo, o valor de uma cadeia de caracteres é algo somente quando _não_ está Depurando:

```fsharp
#if !DEBUG
let str = "Not debugging!"
#else
let str = "Debugging!"
#endif
```

## <a name="line-directives"></a>Diretivas de linha

Ao compilar, o compilador relata erros no código F # referenciando números de linha nos quais cada erro ocorre. Esses números de linha começam em 1 para a primeira linha em um arquivo. No entanto, se você estiver gerando código-fonte F # de outra ferramenta, os números de linha no código gerado geralmente não serão de interesse, pois os erros no código F # gerado provavelmente surgirão de outra fonte. A `#line` diretiva fornece uma maneira para os autores de ferramentas que geram código-fonte F # para passar informações sobre os números de linha e os arquivos de origem originais para o código F # gerado.

Quando você usa a `#line` diretiva, os nomes de arquivo devem ser colocados entre aspas. A menos que o token textual ( `@` ) apareça na frente da cadeia de caracteres, você deve escapar dos caracteres de barra invertida usando dois caracteres de barra invertida em vez de um para usá-los no caminho. A seguir estão os tokens de linha válidos. Nesses exemplos, suponha que o arquivo original `Script1` resulte em um arquivo de código F # gerado automaticamente quando ele é executado por meio de uma ferramenta e que o código no local dessas diretivas é gerado de alguns tokens na linha 25 no arquivo `Script1` .

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7303.fs)]

Esses tokens indicam que o código F # gerado nesse local é derivado de algumas construções em ou próximo à linha `25` `Script1` .

## <a name="compiler-directives"></a>Diretivas de compilador

As diretivas de compilador assemelham-se a diretivas de pré-processador, porque elas são prefixadas com um sinal #, mas em vez de serem interpretadas pelo pré-processador, elas são deixadas para o compilador interpretar e agir.

A tabela a seguir lista a diretiva de compilador que está disponível em F #.

|Diretiva|Descrição|
|---------|-----------|
|`#light`["on" &#124; "off"]|Habilita ou desabilita a sintaxe leve, para compatibilidade com outras versões do ML. Por padrão, a sintaxe leve está habilitada. A sintaxe detalhada está sempre habilitada. Portanto, você pode usar a sintaxe leve e a sintaxe detalhada. A diretiva `#light` por si só é equivalente a `#light "on"` . Se você especificar `#light "off"` , deverá usar a sintaxe detalhada para todas as construções de linguagem. A sintaxe na documentação para F # é apresentada com a suposição de que você está usando a sintaxe leve. Para obter mais informações, consulte [sintaxe detalhada](verbose-syntax.md).|

Para diretivas do interpretador (fsi.exe), consulte [programação interativa com F #](../tutorials/fsharp-interactive/index.md).

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Opções do compilador](compiler-options.md)
