---
title: Introdução à char codificação acter no .net
description: Saiba mais sobre char codificação e decodificação de acter no .net.
ms.date: 03/09/2020
no-loc:
- Rune
- char
- string
dev_langs:
- csharp
helpviewer_keywords:
- encoding, understanding
ms.openlocfilehash: a5d838176bf4437a295ebe6c2cea8b1fe0eeeb61
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656287"
---
# <a name="character-encoding-in-net"></a>Codificação de caracteres no .NET

Este artigo fornece uma introdução aos char sistemas de codificação acter que são usados pelo .net. O artigo explica como os <xref:System.String> tipos,, <xref:System.Char> <xref:System.Text.Rune> e <xref:System.Globalization.StringInfo> funcionam com Unicode, UTF-16 e UTF-8.

O termo * char acter* é usado aqui no sentido geral do *que um leitor percebe como um único elemento de exibição*. Exemplos comuns são a letra "a", o símbolo "@" e o Emoji " 🐂 ". Às vezes, o que parece um char acter é, na verdade, composto por vários elementos de exibição independentes, como explica a seção sobre [clusters grafemas](#grapheme-clusters) .

## <a name="the-no-locstring-and-no-locchar-types"></a>Os string char tipos e

Uma instância da [string](xref:System.String) classe representa algum texto. Um `string` é logicamente uma sequência de valores de 16 bits, cada um deles é uma instância da [char](xref:System.Char) estrutura. O [ string . Propriedade Length](xref:System.String.Length) retorna o número de `char` instâncias na `string` instância.

A função de exemplo a seguir imprime os valores em notação hexadecimal de todas as `char` instâncias em um `string` :

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/PrintStringChars. cs" ID = "SnippetPrintChars":::

Passe string "Olá" para essa função e obtenha a seguinte saída:

```csharp
PrintChars("Hello");
```

```output
"Hello".Length = 5
s[0] = 'H' ('\u0048')
s[1] = 'e' ('\u0065')
s[2] = 'l' ('\u006c')
s[3] = 'l' ('\u006c')
s[4] = 'o' ('\u006f')
```

Cada char acter é representado por um único `char` valor. Esse padrão é verdadeiro para a maioria dos idiomas do mundo. Por exemplo, aqui está a saída de dois char Acters chineses que parece *nǐ hǎo* e Mean *Hello*:

```csharp
PrintChars("你好");
```

```output
"你好".Length = 2
s[0] = '你' ('\u4f60')
s[1] = '好' ('\u597d')
```

No entanto, para alguns idiomas e para alguns símbolos e Emoji, são necessárias duas `char` instâncias para representar um único char acter. Por exemplo, compare o char acters e as `char` instâncias na palavra que significa *Osage* na linguagem Osage:

```csharp
PrintChars("𐓏𐓘𐓻𐓘𐓻𐓟 𐒻𐓟");
```

```output
"𐓏𐓘𐓻𐓘𐓻𐓟 𐒻𐓟".Length = 17
s[0] = '�' ('\ud801')
s[1] = '�' ('\udccf')
s[2] = '�' ('\ud801')
s[3] = '�' ('\udcd8')
s[4] = '�' ('\ud801')
s[5] = '�' ('\udcfb')
s[6] = '�' ('\ud801')
s[7] = '�' ('\udcd8')
s[8] = '�' ('\ud801')
s[9] = '�' ('\udcfb')
s[10] = '�' ('\ud801')
s[11] = '�' ('\udcdf')
s[12] = ' ' ('\u0020')
s[13] = '�' ('\ud801')
s[14] = '�' ('\udcbb')
s[15] = '�' ('\ud801')
s[16] = '�' ('\udcdf')
```

No exemplo anterior, cada char acter, exceto o espaço, é representado por duas `char` instâncias.

Um único Emoji Unicode também é representado por dois `char` s, como visto no exemplo a seguir, mostrando um Emoji Ox:

```
"🐂".Length = 2
s[0] = '�' ('\ud83d')
s[1] = '�' ('\udc02')
```

Esses exemplos mostram que o valor de `string.Length` , que indica o número de `char` instâncias, não indica necessariamente o número de char acters exibidas. Uma única `char` instância por si só não representa necessariamente um char acter.

Os `char` pares que mapeiam para um único char acter são chamados de *pares substitutos*. Para entender como eles funcionam, você precisa entender a codificação Unicode e UTF-16.

## <a name="unicode-code-points"></a>Pontos de código Unicode

O Unicode é um padrão de codificação internacional para uso em várias plataformas e em vários idiomas e scripts.

O padrão Unicode define mais de 1,1 milhões [pontos de código](https://www.unicode.org/glossary/#code_point). Um ponto de código é um valor inteiro que pode variar de 0 a `U+10FFFF` (decimal 1.114.111). Alguns pontos de código são atribuídos a letras, símbolos ou Emoji. Outras são atribuídas a ações que controlam como Text ou char acters são exibidas, como avançar para uma nova linha. Muitos pontos de código ainda não foram atribuídos.

Aqui estão alguns exemplos de atribuições de ponto de código, com links para char TS Unicode em que aparecem:

|Decimal|Hex       |Exemplo|Descrição|
|------:|----------|-------|-----------|
|10     | `U+000A` |N/D| [ALIMENTAÇÃO DE LINHA](https://www.unicode.org/charts/PDF/U0000.pdf) |
|65     | `U+0061` | um | [LETRA LATINA MINÚSCULA A](https://www.unicode.org/charts/PDF/U0000.pdf) |
|562    | `U+0232` | Ȳ | [LETRA LATINA MAIÚSCULA Y COM MACRON](https://www.unicode.org/charts/PDF/U0180.pdf) |
|68.675 | `U+10C43`| 𐱃 | [ANTIGA LETRA TURCO ORKHON EM](https://www.unicode.org/charts/PDF/U10C00.pdf) |
|127.801| `U+1F339`| 🌹 | [Emoji rosa](https://www.unicode.org/charts/PDF/U1F300.pdf) |

Os pontos de código são normalmente referenciados usando a sintaxe `U+xxxx` , em que `xxxx` é o valor inteiro codificado em hexadecimal.

Dentro de todo o intervalo de pontos de código, há dois subintervalos:

* O **BMP (plano multilíngüe básico)** no intervalo `U+0000..U+FFFF` . Esse intervalo de 16 bits fornece 65.536 pontos de código, o suficiente para abranger a maioria dos sistemas de escrita do mundo.
* **Pontos de código suplementares** no intervalo `U+10000..U+10FFFF` . Esse intervalo de 21 bits fornece mais de um milhão de pontos de código adicionais que podem ser usados para linguagens menos conhecidas e outras finalidades, como emojis.

O diagrama a seguir ilustra a relação entre o BMP e os pontos de código suplementares.

:::image type="content" source="media/:::no-Loc (Char)::: acter-Encoding-Introduction/BMP-and-Supplementary. svg "alt-text =" BMP e pontos de código suplementares ":::

## <a name="utf-16-code-units"></a>Unidades de código UTF-16

o formato de transformação Unicode de 16 bits ([UTF-16](https://www.unicode.org/faq/utf_bom.html#UTF16)) é um char sistema de codificação acter que usa *unidades de código* de 16 bits para representar pontos de código Unicode. O .NET usa UTF-16 para codificar o texto em um `string` . Uma `char` instância representa uma unidade de código de 16 bits.

Uma única unidade de código de 16 bits pode representar qualquer ponto de código no intervalo de 16 bits do plano multilíngüe básico. Mas, para um ponto de código no intervalo suplementar, `char` são necessárias duas instâncias.

## <a name="surrogate-pairs"></a>Pares substitutos

A conversão de valores de 2 16 bits em um único valor de 21 bits é facilitada por um intervalo especial denominado *pontos de código substitutos*, de `U+D800` até `U+DFFF` (decimal 55.296 a 57.343), inclusive.

O diagrama a seguir ilustra a relação entre o BMP e os pontos de código substitutos.

:::image type="content" source="media/:::no-Loc (Char)::: acter-Encoding-Introduction/BMP-and-Surrogate. svg "alt-text =" BMP e pontos de código substitutos ":::

Quando um ponto de código *substituto alto* ( `U+D800..U+DBFF` ) é imediatamente seguido por um ponto de código *substituto baixo* ( `U+DC00..U+DFFF` ), o par é interpretado como um ponto de código suplementar usando a seguinte fórmula:

```
code point = 0x10000 +
  ((high surrogate code point - 0xD800) * 0x0400) +
  (low surrogate code point - 0xDC00)
```

Esta é a mesma fórmula usando a notação decimal:

```
code point = 65,536 +
  ((high surrogate code point - 55,296) * 1,024) +
  (low surrogate code point - 56,320)
```

Um ponto de código substituto *alto* não tem um valor de número mais alto que um ponto de código substituto *baixo* . O ponto de código substituto alto é chamado de "alta" porque é usado para calcular os 11 bits de ordem superior do intervalo completo de pontos de código de 21 bits. O ponto de código substituto baixo é usado para calcular os 10 bits de ordem inferior.

Por exemplo, o ponto de código real que corresponde ao par substituto `0xD83C` e `0xDF39`  é calculado da seguinte maneira:

```
actual = 0x10000 + ((0xD83C - 0xD800) * 0x0400) + (0xDF39 - 0xDC00)
       = 0x10000 + (          0x003C  * 0x0400) +           0x0339
       = 0x10000 +                      0xF000  +           0x0339
       = 0x1F339
```

Aqui está o mesmo cálculo usando a notação decimal:

```
actual =  65,536 + ((55,356 - 55,296) * 1,024) + (57,145 - 56320)
       =  65,536 + (              60  * 1,024) +             825
       =  65,536 +                     61,440  +             825
       = 127,801
```

O exemplo anterior demonstra que `"\ud83c\udf39"` é a codificação UTF-16 do `U+1F339 ROSE ('🌹')` ponto de código mencionado anteriormente.

## <a name="unicode-scalar-values"></a>Valores escalares Unicode

O termo [valor escalar Unicode](https://www.unicode.org/glossary/#unicode_scalar_value) refere-se a todos os pontos de código que não sejam os pontos de código substitutos. Em outras palavras, um valor escalar é qualquer ponto de código atribuído a um char acter ou pode ser atribuído a um char acter no futuro. "Caractere" aqui refere-se a qualquer coisa que possa ser atribuída a um ponto de código, que inclui itens como ações que controlam como o texto ou char acters são exibidos.

O diagrama a seguir ilustra os pontos de código de valor escalar.

:::image type="content" source="media/:::no-Loc (Char)::: acter-Encoding-Introduction/Scalar-Values. svg "alt-text =" valores escalares ":::

### <a name="the-no-locrune-type-as-a-scalar-value"></a>O Rune tipo como um valor escalar

A partir do .NET Core 3,0, o <xref:System.Text.Rune?displayProperty=fullName> tipo representa um valor escalar Unicode. **`Rune` Não está disponível no .NET Core 2. x ou .NET Framework 4. x.**

Os `Rune` construtores validam que a instância resultante é um valor escalar Unicode válido; caso contrário, elas geram uma exceção. O exemplo a seguir mostra o código que instancia instâncias com êxito `Rune` porque a entrada representa valores escalares válidos:

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/instancie Rune s.cs" ID = "SnippetValid":::

O exemplo a seguir gera uma exceção porque o ponto de código está no intervalo substituto e não faz parte de um par substituto:

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/instancie Rune s.cs" ID = "SnippetInvalidSurrogate":::

O exemplo a seguir gera uma exceção porque o ponto de código está além do intervalo suplementar:

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/instancie Rune s.cs" ID = "SnippetInvalidHigh":::

### <a name="no-locrune-usage-example-changing-letter-case"></a>Rune exemplo de uso: alterando o caso da letra

Uma API que usa `char` e pressupõe que está trabalhando com um ponto de código que é um valor escalar não funciona corretamente se `char` for de um par substituto. Por exemplo, considere o seguinte método que chama <xref:System.Char.ToUpperInvariant%2A?displayProperty=nameWithType> em cada char um string :

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/ConvertToUpper. cs" ID = "SnippetBadExample":::

Se o `input` string contiver a letra minúscula Deseret `er` ( `𐑉` ), esse código não o converterá em letras maiúsculas ( `𐐡` ). O código chama `char.ToUpperInvariant` separadamente em cada ponto de código substituto `U+D801` e `U+DC49` . Mas `U+D801` não tem informações suficientes para identificá-lo como uma letra minúscula, portanto, `char.ToUpperInvariant` deixa-o sozinho. E ele lida `U+DC49` da mesma maneira. O resultado é que ' 𐑉 ' minúsculas em `input` string não é convertido em letras maiúsculas ' 𐑉 '.

Aqui estão duas opções para converter corretamente um string em maiúsculas:

* Chame <xref:System.String.ToUpperInvariant%2A?displayProperty=nameWithType> na entrada em string vez de iterar `char` por- `char` . O `string.ToUpperInvariant` método tem acesso a ambas as partes de cada par alternativo, portanto, ele pode manipular todos os pontos de código Unicode corretamente.
* Itere pelos valores escalares Unicode como `Rune` instâncias em vez de `char` instâncias, conforme mostrado no exemplo a seguir. Como uma `Rune` instância é um valor escalar Unicode válido, ela pode ser passada para APIs que esperam operar em um valor escalar. Por exemplo, chamar, <xref:System.Text.Rune.ToUpperInvariant%2A?displayProperty=nameWithType> conforme mostrado no exemplo a seguir, fornece os resultados corretos:

  ::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/ConvertToUpper. cs" ID = "SnippetGoodExample":::

### <a name="other-no-locrune-apis"></a>Outras Rune APIs

O `Rune` tipo expõe as analogias de muitas das `char` APIs. Por exemplo, os métodos a seguir espelham APIs estáticas no `char` tipo:

* <xref:System.Text.Rune.IsLetter%2A?displayProperty=nameWithType>
* <xref:System.Text.Rune.IsWhiteSpace%2A?displayProperty=nameWithType>
* <xref:System.Text.Rune.IsLetterOrDigit%2A?displayProperty=nameWithType>
* <xref:System.Text.Rune.GetUnicodeCategory%2A?displayProperty=nameWithType>

Para obter o valor escalar bruto de uma `Rune` instância, use a <xref:System.Text.Rune.Value%2A?displayProperty=nameWithType> propriedade.

Para converter uma `Rune` instância de volta em uma sequência de `char` s, use <xref:System.Text.Rune.ToString%2A?displayProperty=nameWithType> ou o <xref:System.Text.Rune.EncodeToUtf16%2A?displayProperty=nameWithType> método.

Como qualquer valor escalar Unicode é representável por um único `char` ou por um par alternativo, qualquer `Rune` instância pode ser representada por no máximo 2 `char` instâncias. Use <xref:System.Text.Rune.Utf16SequenceLength%2A?displayProperty=nameWithType> para ver quantas `char` instâncias são necessárias para representar uma `Rune` instância.

Para obter mais informações sobre o `Rune` tipo .net, consulte a [ `Rune` referência da API](xref:System.Text.Rune).

## <a name="grapheme-clusters"></a>Clusters grafemas

O que parece um char acter pode resultar de uma combinação de vários pontos de código, portanto, um termo mais descritivo que é geralmente usado no lugar de " char acter" é o [cluster grafemas](https://www.unicode.org/glossary/#grapheme_cluster). O termo equivalente no .NET é o [elemento de texto](xref:System.Globalization.StringInfo.GetTextElementEnumerator%2A).

Considere as `string` instâncias "a", "á", "á" e " `👩🏽‍🚒` ". Se seu sistema operacional os tratar como especificado pelo padrão Unicode, cada `string` uma dessas instâncias aparecerá como um único elemento de texto ou cluster grafemas. Mas os dois últimos são representados por mais de um ponto de código de valor escalar.

* O string "a" é representado por um valor escalar e contém uma `char` instância.

  * `U+0061 LATIN SMALL LETTER A`

* O string "á" é representado por um valor escalar e contém uma `char` instância.

  * `U+00E1 LATIN SMALL LETTER A WITH ACUTE`

* O string "á" tem a mesma aparência de "á", mas é representado por dois valores escalares e contém duas `char` instâncias.

  * `U+0061 LATIN SMALL LETTER A`
  * `U+0301 COMBINING ACUTE ACCENT`

* Por fim, o string " `👩🏽‍🚒` " é representado por quatro valores escalares e contém sete `char` instâncias.

  * `U+1F469 WOMAN` (intervalo suplementar, requer um par alternativo)
  * `U+1F3FD EMOJI MODIFIER FITZPATRICK TYPE-4` (intervalo suplementar, requer um par alternativo)
  * `U+200D ZERO WIDTH JOINER`
  * `U+1F692 FIRE ENGINE` (intervalo suplementar, requer um par alternativo)

Em alguns dos exemplos anteriores, como o modificador de acentuação de acento ou o modificador de Tom de pele, o ponto de código não é exibido como um elemento autônomo na tela. Em vez disso, ele serve para modificar a aparência de um elemento de texto que veio antes dele. Esses exemplos mostram que pode ser necessário usar vários valores escalares para criar o que achamos como um único " char acter", ou "cluster grafemas".

Para enumerar os clusters grafemas de um `string` , use a <xref:System.Globalization.StringInfo> classe, conforme mostrado no exemplo a seguir. Se você estiver familiarizado com o Swift, o `StringInfo` tipo .net será conceitualmente semelhante ao [ `character` tipo de Swift](https://developer.apple.com/documentation/swift/character).

### <a name="example-count-no-locchar-no-locrune-and-text-element-instances"></a>Exemplo: contagem char , Rune e instâncias de elemento de texto

Em APIs do .NET, um cluster grafemas é chamado de *elemento de texto*. O método a seguir demonstra as diferenças entre o `char` , o `Rune` e as instâncias de elemento de texto em um `string` :

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/CountTextElements. cs" ID = "SnippetCountMethod":::

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/CountTextElements. cs" ID = "SnippetCallCountMethod":::

Se você executar esse código no .NET Framework ou no .NET Core 3,1 ou anterior, a contagem de elementos de texto para o emoji será mostrada `4` . Isso ocorre devido a um bug na `StringInfo` classe que é corrigido no .NET 5.

### <a name="example-splitting-no-locstring-instances"></a>Exemplo: dividindo string instâncias

Ao dividir `string` instâncias, evite dividir pares substitutos e clusters grafemas. Considere o seguinte exemplo de código incorreto, que pretende inserir quebras de linha a cada 10 char acters em um string :

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/InsertNewlines. cs" ID = "SnippetBadExample":::

Como esse código enumera `char` instâncias, um par substituto que ocorre em cima de 10 `char` limites será dividido e uma nova linha injetada entre eles. Essa inserção apresenta dados corrompidos, pois os pontos de código substitutos são significativos apenas como pares.

O potencial de corrupção de dados não será eliminado se você enumerar `Rune` instâncias (valores escalares) em vez de `char` instâncias. Um conjunto de `Rune` instâncias pode criar um cluster grafemas que se espalha por 10 `char` limites. Se o conjunto de clusters grafemas for dividido, ele não poderá ser interpretado corretamente.

Uma abordagem melhor é quebrar o string ao contar os clusters grafemas, ou elementos de texto, como no exemplo a seguir:

::: código Language = "CSharp" origem = "Snippets/ char acter-Encoding-Introduction/Csharp/InsertNewlines. cs" ID = "SnippetGoodExample":::

Como observado anteriormente, no entanto, em implementações do .NET que não seja o .NET 5, a `StringInfo` classe pode lidar com alguns clusters do grafemas incorretamente.

## <a name="utf-8-and-utf-32"></a>UTF-8 e UTF-32

As seções anteriores se concentram em UTF-16 porque é isso que o .NET usa para codificar `string` instâncias. Há outros sistemas de codificação para Unicode- [UTF-8](https://www.unicode.org/faq/utf_bom.html#UTF8) e [UTF-32](https://www.unicode.org/faq/utf_bom.html#UTF32). Essas codificações usam unidades de código de 8 bits e unidades de código de 32 bits, respectivamente.

Como o UTF-16, o UTF-8 requer várias unidades de código para representar alguns valores escalares Unicode. O UTF-32 pode representar qualquer valor escalar em uma única unidade de código de 32 bits.

Aqui estão alguns exemplos que mostram como o mesmo ponto de código Unicode é representado em cada um desses três sistemas de codificação Unicode:

```
Scalar: U+0061 LATIN SMALL LETTER A ('a')
UTF-8 : [ 61 ]           (1x  8-bit code unit  = 8 bits total)
UTF-16: [ 0061 ]         (1x 16-bit code unit  = 16 bits total)
UTF-32: [ 00000061 ]     (1x 32-bit code unit  = 32 bits total)

Scalar: U+0429 CYRILLIC CAPITAL LETTER SHCHA ('Щ')
UTF-8 : [ D0 A9 ]        (2x  8-bit code units = 16 bits total)
UTF-16: [ 0429 ]         (1x 16-bit code unit  = 16 bits total)
UTF-32: [ 00000429 ]     (1x 32-bit code unit  = 32 bits total)

Scalar: U+A992 JAVANESE LETTER GA ('ꦒ')
UTF-8 : [ EA A6 92 ]     (3x  8-bit code units = 24 bits total)
UTF-16: [ A992 ]         (1x 16-bit code unit  = 16 bits total)
UTF-32: [ 0000A992 ]     (1x 32-bit code unit  = 32 bits total)

Scalar: U+104CC OSAGE CAPITAL LETTER TSHA ('𐓌')
UTF-8 : [ F0 90 93 8C ]  (4x  8-bit code units = 32 bits total)
UTF-16: [ D801 DCCC ]    (2x 16-bit code units = 32 bits total)
UTF-32: [ 000104CC ]     (1x 32-bit code unit  = 32 bits total)
```

Conforme observado anteriormente, uma única unidade de código UTF-16 de um [par substituto](#surrogate-pairs) não faz sentido por si só. Da mesma forma, uma única unidade de código UTF-8 não faz sentido por si só, se estiver em uma sequência de dois, três ou quatro usadas para calcular um valor escalar.

### <a name="endianness"></a>Endianness

No .NET, as unidades de código UTF-16 de um string são armazenadas em memória contígua como uma sequência de inteiros de 16 bits ( `char` instâncias). Os bits de unidades de código individuais são dispostos de acordo com a [endian](https://en.wikipedia.org/wiki/Endianness) da arquitetura atual.

Em uma arquitetura little-endian, a string consistir dos pontos de código UTF-16 `[ D801 DCCC ]` seria disposta na memória como os bytes `[ 0x01, 0xD8, 0xCC, 0xDC ]` . Em uma arquitetura big endian string , a mesma seria disposta na memória como os bytes `[ 0xD8, 0x01, 0xDC, 0xCC ]` .

Sistemas de computador que se comunicam entre si devem concordar com a representação de dados que cruzam a conexão. A maioria dos protocolos de rede usa UTF-8 como padrão ao transmitir texto, parcialmente para evitar problemas que podem resultar de um computador big endian se comunicando com um computador little-endian. Os string pontos de código UTF-8 `[ F0 90 93 8C ]` sempre serão representados como bytes, `[ 0xF0, 0x90, 0x93, 0x8C ]` independentemente da endian.

Para usar UTF-8 para transmissão de texto, os aplicativos .NET geralmente usam um código como o exemplo a seguir:

```csharp
string stringToWrite = GetString();
byte[] stringAsUtf8Bytes = Encoding.UTF8.GetBytes(stringToWrite);
await outputStream.WriteAsync(stringAsUtf8Bytes, 0, stringAsUtf8Bytes.Length);
```

No exemplo anterior, o método [Encoding. UTF8. GetBytes](xref:System.Text.UTF8Encoding.GetBytes%2A) DECODIFICA o UTF-16 de `string` volta em uma série de valores escalares Unicode e, em seguida, ele codifica novamente esses valores escalares em UTF-8 e coloca a sequência resultante em uma `byte` matriz. O método [Encoding. UTF8. GetString](xref:System.Text.UTF8Encoding.GetString%2A) executa a transformação oposta, convertendo uma matriz UTF-8 `byte` em um UTF-16 `string` .

> [!WARNING]
> Como o UTF-8 é comum na Internet, pode ser tentador ler bytes brutos da transmissão e tratá-los como se fossem UTF-8. No entanto, você deve validar que ele está realmente bem formado. Um cliente mal-intencionado pode enviar UTF-8 mal formado para seu serviço. Se você operar nesses dados como se eles estivessem bem formados, isso poderá causar erros ou brechas de segurança em seu aplicativo. Para validar dados UTF-8, você pode usar um método como `Encoding.UTF8.GetString` , que executará a validação durante a conversão dos dados de entrada em um `string` .

### <a name="well-formed-encoding"></a>Codificação bem formada

Uma codificação Unicode bem formada é uma string das unidades de código que podem ser decodificadas inequivocadamente e sem erro em uma sequência de valores escalares Unicode. Os dados bem formados podem ser transcodificados livremente entre UTF-8, UTF-16 e UTF-32.

A questão de uma sequência de codificação estar bem formada ou não não está relacionada à endian da arquitetura de uma máquina. Uma sequência UTF-8 mal formada é mal formada da mesma forma em computadores big endian e little-endian.

Aqui estão alguns exemplos de codificações mal formadas:

* Em UTF-8, a sequência `[ 6C C2 61 ]` está mal formada porque `C2` não pode ser seguida por `61` .

* Em UTF-16, a sequência `[ DC00 DD00 ]` (ou, em C#, a string `"\udc00\udd00"` ) está mal formada porque o substituto baixo `DC00` não pode ser seguido por outro substituto baixo `DD00` .

* Em UTF-32, a sequência `[ 0011ABCD ]` está mal formada porque `0011ABCD` está fora do intervalo de valores escalares Unicode.

No .NET, as `string` instâncias quase sempre contêm dados UTF-16 bem formados, mas isso não é garantido. Os exemplos a seguir mostram um código C# válido que cria dados UTF-16 mal formados em `string` instâncias.

* Um literal mal formado:

  ```csharp
  const string s = "\ud800";
  ```

* Um sub string que divide um par substituto:

  ```csharp
  string x = "\ud83e\udd70"; // "🥰"
  string y = x.Substring(1, 1); // "\udd70" standalone low surrogate
  ```

APIs como [`Encoding.UTF8.GetString`](xref:System.Text.UTF8Encoding.GetString%2A) nunca retornam instâncias mal formadas `string` . `Encoding.GetString` e os `Encoding.GetBytes` métodos detectam sequências mal formadas na entrada e executam char a substituição de acter ao gerar a saída. Por exemplo, se [`Encoding.ASCII.GetString(byte[])`](xref:System.Text.ASCIIEncoding.GetString%2A) o vê um byte não-ASCII na entrada (fora do intervalo U + 0000.. U + 007F), ele insere um '? ' na instância retornada `string` . [`Encoding.UTF8.GetString(byte[])`](xref:System.Text.UTF8Encoding.GetString%2A) Substitui sequências UTF-8 mal formadas por `U+FFFD REPLACEMENT CHARACTER ('�')` na `string` instância retornada. Para obter mais informações, consulte [o padrão Unicode, as](https://www.unicode.org/versions/latest/)seções 5,22 e 3,9.

As `Encoding` classes internas também podem ser configuradas para lançar uma exceção em vez de executar a char substituição de acter quando seqüências mal formadas são vistas. Essa abordagem é geralmente usada em aplicativos sensíveis à segurança em que a char substituição acter pode não ser aceitável.

```csharp
byte[] utf8Bytes = ReadFromNetwork();
UTF8Encoding encoding = new UTF8Encoding(encoderShouldEmitUTF8Identifier: false, throwOnInvalidBytes: true);
string asString = encoding.GetString(utf8Bytes); // will throw if 'utf8Bytes' is ill-formed
```

Para obter informações sobre como usar as `Encoding` classes internas, consulte [como usar char classes de codificação acter no .net](character-encoding.md).

## <a name="see-also"></a>Consulte também

- <xref:System.String>
- <xref:System.Char>
- <xref:System.Text.Rune>
- [Globalização e localização](../globalization-localization/index.md)
