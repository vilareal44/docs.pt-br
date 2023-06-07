---
title: Cadeias de caracteres de formato numérico personalizado
description: Saiba como criar uma cadeia de caracteres de formato numérico personalizado para formatar dados numéricos no .NET. Uma cadeia de caracteres de formato numérico personalizado tem um ou mais especificadores numéricos personalizados.
ms.date: 06/25/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- numeric format strings [.NET Framework]
- formatting [.NET Framework], numbers
- format strings
- custom numeric format strings
- numbers [.NET Framework], formatting
- format specifiers, numeric
- formatting numbers [.NET Framework]
- format specifiers, custom numeric format strings
ms.assetid: 6f74fd32-6c6b-48ed-8241-3c2b86dea5f4
ms.openlocfilehash: bd96766c7483a3de1a3c70d1efbe1aa91ea45fbc
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447232"
---
# <a name="custom-numeric-format-strings"></a>Cadeias de caracteres de formato numérico personalizado

Você pode criar uma cadeia de caracteres de formato numérico personalizado, que consiste em um ou mais especificadores numéricos personalizados, para definir a formatação de dados numéricos. Uma cadeia de caracteres de formato numérico personalizado é qualquer cadeia de caracteres que é não uma [cadeia de caracteres de formato numérico padrão](standard-numeric-format-strings.md).

As cadeias de caracteres de formato numérico personalizado têm suporte de algumas sobrecargas do método `ToString` de todos os tipos numéricos. Por exemplo, você pode fornecer uma cadeia de caracteres de formato numérico para os métodos <xref:System.Int32.ToString%28System.String%29> e <xref:System.Int32.ToString%28System.String%2CSystem.IFormatProvider%29> do tipo <xref:System.Int32>. Cadeias de caracteres de formato numérico personalizado também têm suporte no [recurso de formatação composta](composite-formatting.md) do .NET Framework, o qual é usado por alguns métodos `Write` e `WriteLine` das classes <xref:System.Console> e <xref:System.IO.StreamWriter>, o método <xref:System.String.Format%2A?displayProperty=nameWithType> e o método <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>. O recurso de [Interpolação de cadeia de caracteres](../../csharp/language-reference/tokens/interpolated.md) também é compatível com cadeias de caracteres de formato numérico personalizado.

> [!TIP]
> Baixe o **Utilitário de Formatação**, um aplicativo do Windows Forms do .NET Core que permite aplicar cadeias de caracteres de formato a valores numéricos ou de data e hora e exibir a cadeia de caracteres de resultado. O código-fonte está disponível para o [C#](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-cs) e o [Visual Basic](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-vb).

<a name="table"></a>A tabela a seguir descreve os especificadores de formato numérico personalizado e exibe a saída de exemplo produzida por cada especificador de formato. Consulte a seção [Notas](#NotesCustomFormatting) para obter informações adicionais sobre como usar cadeias de caracteres de formato numérico personalizado e a seção [Exemplo](#example) para obter uma ilustração abrangente de seu uso.

|Especificador de formato|Nome|Descrição|Exemplos|
|----------------------|----------|-----------------|--------------|
|"0"|Espaço reservado de zero|Substitui o zero pelo dígito correspondente, se houver um presente. Caso contrário, o zero aparecerá na cadeia de caracteres de resultado.<br /><br /> Mais informações: [Especificador de formato personalizado "0"](#Specifier0).|1234.5678 ("00000") -> 01235<br /><br /> 0.45678 (en-US "0,00") -> 0.46<br /><br /> 0.45678 ("0.00", fr-FR) -> 0,46|
|"#"|Espaço reservado de dígito|Substitui o símbolo "#" pelo dígito correspondente, se houver um presente. Caso contrário, nenhum dígito aparecerá na cadeia de caracteres de resultado.<br /><br /> Observe que nenhum dígito aparece na cadeia de caracteres de resultado se o dígito na cadeia de entrada correspondente for um 0 não significativo. Por exemplo, 0003 ("####") -> 3.<br /><br /> Mais informações: [o especificador personalizado "#"](#SpecifierD).|1234.5678 ("#####") -> 1235<br /><br /> 0.45678 ("#.##", en-US) -> .46<br /><br /> 0.45678 ("#.##", fr-FR) -> ,46|
|"."|Ponto decimal|Determina a posição do separador decimal na cadeia de caracteres de resultado.<br /><br /> Mais informações: [o "." Especificador personalizado](#SpecifierPt).|0.45678 (en-US "0,00") -> 0.46<br /><br /> 0.45678 ("0.00", fr-FR) -> 0,46|
|","|Separador de grupo e escala numérica|Funciona tanto como um separador de grupo quanto como um especificador de escala numérica. Como separador de grupo, insere um caractere separador de grupo localizado entre cada grupo. Como especificador de escala numérica, divide um número por 1000 para cada vírgula especificada.<br /><br /> Mais informações: [Especificador de formato personalizado ","](#SpecifierTh).|Especificador de separador de grupo:<br /><br /> 2147483647 ("##,#", en-US) -> 2,147,483,647<br /><br /> 2147483647 ("##,#", es-ES) -> 2.147.483.647<br /><br /> Especificador de escala:<br /><br /> 2147483647 ("#,#,,", en-US) -> 2,147<br /><br /> 2147483647 ("#,#,,", es-ES) -> 2.147|
|"%"|Espaço reservado percentual|Multiplica um número por 100 e insere um símbolo percentual localizado na cadeia de caracteres de resultado.<br /><br /> Mais informações: [Especificador de formato personalizado "%"](#SpecifierPct).|0.3697 ("%#0.00", en-US) -> %36.97<br /><br /> 0.3697 ("%#0.00", el-GR) -> %36,97<br /><br /> 0.3697 ("##.0 %", en-US) -> 37.0 %<br /><br /> 0.3697 ("##.0 %", el-GR) -> 37,0 %|
|"‰"|Espaço reservado por milhar|Multiplica um número por 1000 e insere um símbolo de por milhar localizado na cadeia de caracteres de resultado.<br /><br /> Mais informações: [Especificador de formato personalizado "‰"](#SpecifierPerMille).|0.03697 ("#0.00‰", en-US) -> 36.97‰<br /><br /> 0.03697 ("#0.00‰", ru-RU) -> 36,97‰|
|"E0"<br /><br /> "E+0"<br /><br /> "E-0"<br /><br /> "e0"<br /><br /> "e+0"<br /><br /> "e-0"|Notação exponencial|Se seguida por pelo menos um 0 (zero), formata o resultado usando notação exponencial. A caixa de “E” ou “e” indica a caixa do símbolo do expoente na cadeia de caracteres de resultado. O número de zero depois do caractere “E” ou “e” determina o número mínimo de dígitos do expoente. Um sinal de positivo (+) indica que um caractere de sinal sempre precede o expoente. Um sinal de negativo (-) indica que um caractere de sinal precede apenas expoentes negativos.<br /><br /> Mais informações: [Especificadores de formato personalizado "E" e "e"](#SpecifierExponent).|987654 ("#0.0e0") -> 98.8e4<br /><br /> 1503.92311 ("0.0##e+00") -> 1.504e+03<br /><br /> 1.8901385E-16 ("0.0e+00") -> 1.9e-16|
|"\\"|Caractere de escape|Faz com que o próximo caractere seja interpretado como um literal em vez de como um especificador de formato personalizado.<br /><br /> Mais informações: [Caractere de escape "\\"](#SpecifierEscape).|987654 ("\\###00\\#") -> #987654#|
|'*cadeia de caracteres*'<br /><br /> "*String*"|Delimitador de cadeia de caracteres literal|Indica que os caracteres delimitados devem ser copiados para a cadeia de caracteres de resultado sem sofrerem alterações.<br/><br/>Para saber mais: [Literais de cadeia de caracteres](#character-literals).|68 ("# ' graus'") -> 68 graus<br /><br /> 68 ("# ' graus'") -> 68 graus|
|;|Separador de seção|Define seções com cadeias de caracteres de formato separadas para números positivos, negativos e zero.<br /><br /> Mais informações: [Separador de seção ";"](#SectionSeparator).|12.345 ("#0.0#;(#0.0#);-\0-") -> 12.35<br /><br /> 0 ("#0.0#;(#0.0#);-\0-") -> -0-<br /><br /> -12.345 ("#0.0#;(#0.0#);-\0-") -> (12.35)<br /><br /> 12.345 ("#0.0#;(#0.0#)") -> 12.35<br /><br /> 0 ("#0.0#;(#0.0#)") -> 0.0<br /><br /> -12.345 ("#0.0#;(#0.0#)") -> (12.35)|
|Outros|Todos os outros caracteres|O caractere é copiado, inalterado, para a cadeia de caracteres de resultado.<br/><br/>Para saber mais: [Literais de cadeia de caracteres](#character-literals).|68 ("# °") -> 68 °|

As seções a seguir fornecem informações detalhadas sobre cada um dos especificadores de formato numérico personalizado.

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-partial-note.md)]

<a name="Specifier0"></a>

## <a name="the-0-custom-specifier"></a>Especificador personalizado "0"

O especificador de formato personalizado “0 " funciona como um símbolo de espaço reservado de zero. Se o valor que está sendo formatado tiver um dígito na posição em que o zero aparece na cadeia de caracteres de formato, esse dígito será copiado para a cadeia de caracteres de resultado; caso contrário, um zero aparecerá na cadeia de caracteres de resultado. A posição do zero mais à esquerda antes do ponto decimal e o zero mais à direita após o ponto decimal determina o intervalo de dígitos que estão sempre presentes na cadeia de caracteres de resultado.

O especificador "00" faz com que o valor seja arredondado para o dígito mais próximo anterior ao decimal, no qual o arredondamento para cima sempre é usado. Por exemplo, a formatação de 34,5 com "00" resultaria no valor 35.

O exemplo a seguir mostra vários valores formatados com cadeias de caracteres de formato personalizado que incluem espaços reservados de zero.

[!code-cpp[Formatting.Numeric.Custom#1](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#1)]
[!code-csharp[Formatting.Numeric.Custom#1](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#1)]
[!code-vb[Formatting.Numeric.Custom#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#1)]

[Voltar à tabela](#table)

<a name="SpecifierD"></a>

## <a name="the--custom-specifier"></a>Especificador personalizado "#"

O especificador de formato personalizado “# " funciona como um símbolo de espaço reservado de dígito. Se o valor que está sendo formatado tem um dígito na posição onde o símbolo "#" aparece na cadeia de caracteres de formato, esse dígito é copiado para a cadeia de caracteres de resultado. Caso contrário, nada é armazenado nessa posição na cadeia de caracteres de resultado.

Observe que esse especificador nunca exibe um zero que não seja um dígito significativo, mesmo se o zero é o único dígito na cadeia de caracteres. Ele exibirá zero somente se ele for um dígito significativo no número que está sendo exibido.

A cadeia de caracteres de formato "##" faz com que o valor seja arredondado para o dígito mais próximo anterior ao decimal, no qual o arredondamento para cima sempre é usado. Por exemplo, a formatação de 34,5 com "##" resultaria no valor 35.

O exemplo a seguir mostra vários valores formatados com cadeias de caracteres de formato personalizado que incluem espaços reservados de dígito.

[!code-cpp[Formatting.Numeric.Custom#2](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#2)]
[!code-csharp[Formatting.Numeric.Custom#2](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#2)]
[!code-vb[Formatting.Numeric.Custom#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#2)]

Para retornar uma cadeia de caracteres de resultado na qual dígitos ausentes ou zeros à esquerda são substituídos por espaços, use o [recurso de formatação composta](composite-formatting.md) e especifique uma largura de campo, como ilustra o exemplo a seguir.

[!code-cpp[Formatting.Numeric.Custom#12](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/SpaceOrDigit1.cpp#12)]
[!code-csharp[Formatting.Numeric.Custom#12](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/SpaceOrDigit1.cs#12)]
[!code-vb[Formatting.Numeric.Custom#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/SpaceOrDigit1.vb#12)]

[Voltar à tabela](#table)

<a name="SpecifierPt"></a>

## <a name="the--custom-specifier"></a>Especificador personalizado "."

O especificador de formato personalizado "." insere um separador decimal localizado na cadeia de caracteres resultante. O primeiro caractere de ponto na cadeia de caracteres de formato determina o local do separador decimal no valor formatado; quaisquer caracteres de ponto adicionais são ignorados.

O caractere que é usado como separador decimal na cadeia de caracteres de resultado é determinado pela propriedade <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> do objeto <xref:System.Globalization.NumberFormatInfo> que controla a formatação.

O exemplo a seguir usa o especificador de formato "." para definir a posição do ponto decimal em várias cadeias de caracteres de resultados.

[!code-cpp[Formatting.Numeric.Custom#3](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#3)]
[!code-csharp[Formatting.Numeric.Custom#3](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#3)]
[!code-vb[Formatting.Numeric.Custom#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#3)]

[Voltar à tabela](#table)

<a name="SpecifierTh"></a>

## <a name="the--custom-specifier"></a>Especificador personalizado ","

O caractere "," funciona tanto como um separador de grupo quanto como um especificador de escala do número.

- Separador de grupo: se uma ou mais vírgulas forem especificadas entre dois espaços reservados de dígito (0 ou #) que formatam os dígitos integrais de um número, um caractere separador de grupo será inserido entre cada grupo de números na parte integral da saída.

  As propriedades <xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A> e <xref:System.Globalization.NumberFormatInfo.NumberGroupSizes%2A> do objeto <xref:System.Globalization.NumberFormatInfo> atual determinam o caractere usado como separador de número de grupo e o tamanho de cada grupo de números. Por exemplo, se a cadeia de caracteres "#,#" e a cultura invariável forem usadas para formatar o número 1000, a saída será "1.000".

- Especificador de escala numérica: se uma ou mais vírgulas forem especificadas imediatamente à esquerda do ponto decimal explícito ou implícito, o número a ser formatado será dividido por 1000 para cada vírgula. Por exemplo, se a cadeia de caracteres "0,," for usada para formatar o número 100 milhões, a saída será "100".

Você pode usar os especificadores de separador de grupo e escala numérica na mesma cadeia de caracteres de formato. Por exemplo, se a cadeia de caracteres "#, 0,," e a cultura invariante forem usadas para formatar o número um bilhão, a saída será "1.000".

O exemplo a seguir ilustra o uso da vírgula como um separador de grupo.

[!code-cpp[Formatting.Numeric.Custom#4](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#4)]
[!code-csharp[Formatting.Numeric.Custom#4](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#4)]
[!code-vb[Formatting.Numeric.Custom#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#4)]

O exemplo a seguir ilustra o uso da vírgula como um especificador de escala numérica.

[!code-cpp[Formatting.Numeric.Custom#5](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#5)]
[!code-csharp[Formatting.Numeric.Custom#5](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#5)]
[!code-vb[Formatting.Numeric.Custom#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#5)]

[Voltar à tabela](#table)

<a name="SpecifierPct"></a>

## <a name="the--custom-specifier"></a>Especificador personalizado "%"

Um sinal de porcentagem (%) em uma cadeia de caracteres de formato faz com que um número seja multiplicado por 100 antes de ser formatado. O símbolo de porcentagem localizado é inserido no próprio número no local em que o % aparece na cadeia de caracteres de formato. O caractere de porcentagem usado é definido pela propriedade <xref:System.Globalization.NumberFormatInfo.PercentSymbol%2A> do objeto <xref:System.Globalization.NumberFormatInfo> atual.

O exemplo a seguir define várias cadeias de caracteres de formato personalizado que incluem o especificador personalizado "%".

[!code-cpp[Formatting.Numeric.Custom#6](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#6)]
[!code-csharp[Formatting.Numeric.Custom#6](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#6)]
[!code-vb[Formatting.Numeric.Custom#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#6)]

[Voltar à tabela](#table)

<a name="SpecifierPerMille"></a>

## <a name="the--custom-specifier"></a>Especificador personalizado "‰"

Um caractere de por mil sinal (‰ ou \u2030) em uma cadeia de caracteres de formato faz com que um número seja multiplicado por 1000 antes de ser formatado. O símbolo de por mil apropriado é inserido na cadeia de caracteres retornada no local em que o símbolo ‰ aparece na cadeia de caracteres de formato. O caractere de por mil usado é definido pela propriedade <xref:System.Globalization.NumberFormatInfo.PerMilleSymbol%2A?displayProperty=nameWithType> do objeto que fornece informações de formatação específicas da cultura.

O exemplo a seguir define uma cadeia de caracteres de formato personalizado que inclui o especificador personalizado "‰".

[!code-cpp[Formatting.Numeric.Custom#9](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#9)]
[!code-csharp[Formatting.Numeric.Custom#9](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#9)]
[!code-vb[Formatting.Numeric.Custom#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#9)]

[Voltar à tabela](#table)

<a name="SpecifierExponent"></a>

## <a name="the-e-and-e-custom-specifiers"></a>Especificadores personalizados "E" e "e"

Se qualquer uma das cadeias de caracteres "E", " E + ", " E - ", "e", " e + " ou " e - " estiver presente na cadeia de caracteres de formato e for seguida imediatamente por pelo menos um caractere zero, então o número será formatado usando notação científica com um "E" ou "e" inserido entre o número e o expoente. O número de zeros após o indicador de notação científica determina o número mínimo de dígitos que serão enviados para o expoente. Os formatos "E+" e "e+" indicam que um caractere de positivo ou negativo sempre deve preceder o expoente. Os formatos "E", " E-", "e" ou "e-" indicam que um caractere de sinal só deve preceder os expoentes negativos.

O exemplo a seguir formata vários valores numéricos usando os especificadores para notação científica.

[!code-cpp[Formatting.Numeric.Custom#7](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#7)]
[!code-csharp[Formatting.Numeric.Custom#7](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#7)]
[!code-vb[Formatting.Numeric.Custom#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#7)]

[Voltar à tabela](#table)

<a name="SpecifierEscape"></a>

## <a name="the--escape-character"></a>Caractere de escape "\\"

Os símbolos “#”, “0 ", “. ”, “,”, “% e "‰" em uma cadeia de caracteres de formato são interpretados como especificadores de formato e não como caracteres literais. Dependendo da sua posição em uma cadeia de caracteres de formato personalizado, o "E" em maiúscula e minúscula, bem como os símbolos + e -, também podem ser interpretados como especificadores de formato.

Para impedir que um caractere seja interpretado como um especificador de formato, você pode precedê-lo com uma barra invertida, que é o caractere de escape. O caractere de escape significa que o próximo caractere é um literal de caractere que deve ser incluído inalterado na cadeia de caracteres de resultado.

Para incluir uma barra invertida em uma cadeia de caracteres de resultado, você deve escapá-la com outra barra invertida (`\\`).

> [!NOTE]
> Alguns compiladores, como os compiladores C++ e C#, também podem interpretar um único caractere de barra invertida como um caractere de escape. Para garantir que uma cadeia de caracteres seja interpretada corretamente quando formatada, você poderá usar o caractere literal de cadeia de caracteres textual (o caractere @) antes da cadeia de caracteres em C# ou adicionar outro caractere de barra invertida antes de cada barra invertida em C# e em C++. O exemplo de C# a seguir ilustra ambas as abordagens.

O exemplo a seguir usa o caractere de escape para impedir que a operação de formatação interprete os caracteres "#", "0" e "\\" como caracteres de escape ou especificadores de formato. Os exemplos em C# usam uma barra invertida adicional para garantir que uma barra invertida seja interpretada como um caractere literal.

[!code-cpp[Formatting.Numeric.Custom#11](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/escape1.cpp#11)]
[!code-csharp-interactive[Formatting.Numeric.Custom#11](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/escape1.cs#11)]
[!code-vb[Formatting.Numeric.Custom#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/escape1.vb#11)]

[Voltar à tabela](#table)

<a name="SectionSeparator"></a>

## <a name="the--section-separator"></a>Separador de seção ";"

O ponto-e-vírgula (;) é um especificador de formato condicional que aplica formatação diferente a um número dependendo se o valor é positivo, negativo ou zero. Para produzir esse comportamento, uma cadeia de caracteres de formato personalizado pode conter até três seções separadas por ponto-e-vírgula. Essas seções são descritas na tabela a seguir.

|Número de seções|Descrição|
|------------------------|-----------------|
|Uma seção|A cadeia de caracteres de formato aplica-se a todos os valores.|
|Duas seções|A primeira seção aplica-se a valores positivos e zeros e a segunda seção aplica-se a valores negativos.<br /><br /> Se o número a ser formatado for negativo, mas se tornar zero após o arredondamento de acordo com o formato na segunda seção, então o zero resultante será formatado de acordo com a primeira seção.|
|Três seções|A primeira seção aplica-se a valores positivos, a segunda seção aplica-se a valores negativos e a terceira seção aplica-se a zeros.<br /><br /> A segunda seção pode ser deixada em branco (ao não ter nada entre os pontos-e-vírgulas), caso em que a primeira seção se aplica a todos os valores diferentes de zero.<br /><br /> Se o número a ser formatado for não zero, mas se tornar zero após o arredondamento de acordo com o formato na primeira ou segunda seção, então o zero resultante será formatado de acordo com a terceira seção.|

Separadores de seção ignoram qualquer formatação preexistente associada a um número quando o valor final é formatado. Por exemplo, valores negativos sempre são exibidos sem um sinal de negativ o quando os separadores de seção são usados. Se desejar que o valor final formatado contenha um sinal de negativo, você deverá incluir explicitamente o sinal de negativo como parte de especificador de formato personalizado.

O exemplo a seguir utiliza o especificador de formato ";" para formatar números positivos, negativos e zero de maneira diferente.

[!code-cpp[Formatting.Numeric.Custom#8](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#8)]
[!code-csharp-interactive[Formatting.Numeric.Custom#8](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#8)]
[!code-vb[Formatting.Numeric.Custom#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#8)]

[Voltar à tabela](#table)

## <a name="character-literals"></a>Literais de caracteres

Os especificadores de formato que aparecem em uma cadeia de caracteres de formato numérica personalizada sempre são interpretados como caracteres de formatação e nunca como caracteres literais. Isso inclui os seguintes caracteres:

- [0](#Specifier0)
- [\#](#SpecifierD)
- [%](#SpecifierPct)
- [‰](#SpecifierPerMille)
- '
- [\\](#SpecifierEscape)
- [.](#SpecifierPt)
- [,](#SpecifierTh)
- [E ou e](#SpecifierExponent), de acordo com a posição na cadeia de caracteres de formato.

Todos os outros caracteres sempre são interpretados como literais de caracteres e, em uma operação de formatação, são incluídos na cadeia de caracteres de resultado inalterada.  Em uma operação de análise, eles devem corresponder exatamente aos caracteres na cadeia de entrada; a comparação diferencia maiúsculas de minúsculas.

O exemplo a seguir ilustra um uso comum de unidades de caractere literal (neste caso, milhares):

[!code-csharp-interactive[literal characters](~/samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/literal2.cs#1)]
[!code-vb[literal characters](~/samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/literal2.vb#1)]

Há duas maneiras de indicar que os caracteres devem ser interpretados como caracteres literais e não como caracteres de formatação, para que possam ser incluídos em uma cadeia de caracteres de resultado ou analisados com êxito em uma cadeia de caracteres de entrada:

- Pelo escape de um caractere de formatação. Para saber mais, confira [O caractere de escape "\\"](#SpecifierEscape).

- Colocando toda a cadeia de caracteres literal entre aspas ou apóstrofos.

O exemplo a seguir usa as duas abordagem para incluir caracteres reservados em uma cadeia de caracteres de formato numérica personalizada.

[!code-csharp-interactive[including reserved characters](~/samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/literal1.cs#1)]
[!code-vb[including reserved characters](~/samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/literal1.vb#1)]

<a name="NotesCustomFormatting"></a>

## <a name="notes"></a>Observações

### <a name="floating-point-infinities-and-nan"></a>Infinitos de ponto flutuante e NaN

Independentemente da cadeia de caracteres de formato, se o valor de um tipo de ponto flutuante <xref:System.Single> ou <xref:System.Double> é infinito positivo, infinito negativo ou um não é um número (NaN), a cadeia de caracteres formatada é o valor das respectivas propriedades <xref:System.Globalization.NumberFormatInfo.PositiveInfinitySymbol%2A>, <xref:System.Globalization.NumberFormatInfo.NegativeInfinitySymbol%2A> ou <xref:System.Globalization.NumberFormatInfo.NaNSymbol%2A> especificadas pelo objeto <xref:System.Globalization.NumberFormatInfo> aplicável no momento.

### <a name="control-panel-settings"></a>Configurações do Painel de Controle

As configurações no item **Opções Regionais e de Idioma** do Painel de Controle influenciam a cadeia de caracteres de resultado produzida por uma operação de formatação. Essas configurações são usadas para inicializar o objeto <xref:System.Globalization.NumberFormatInfo> associado à cultura de thread atual, a qual fornece valores usados para controlar a formatação. Computadores que usam configurações diferentes geram cadeias de caracteres de resultado diferentes.

Além disso, se o constructo <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29> for usado para criar uma instância de um novo objeto <xref:System.Globalization.CultureInfo> que representa a mesma cultura que a cultura atual do sistema, quaisquer personalizações estabelecidas pelo item **Opções Regionais e de Idioma** no Painel de Controle serão aplicadas ao novo objeto <xref:System.Globalization.CultureInfo>. Você pode usar o construtor <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> para criar um objeto <xref:System.Globalization.CultureInfo> que não reflita as personalizações de um sistema.

### <a name="rounding-and-fixed-point-format-strings"></a>Arredondamento e cadeias de caracteres de formato de ponto fixo

Para cadeias de caracteres de formato de ponto fixo (isto é, cadeias de caracteres de formato que não contêm caracteres de formato de notação científica), os números são arredondados para tantas casas decimais quanto houver espaços reservados de dígitos à direita do ponto decimal. Se a cadeia de caracteres de formato não contiver um ponto decimal, o número será arredondado para o inteiro mais próximo. Se o número tiver mais dígitos do que espaços reservados de dígitos à esquerda do ponto decimal, os dígitos extras serão copiados para a cadeia de caracteres de resultado imediatamente antes do primeiro espaço reservado de dígito.

[Voltar à tabela](#table)

<a name="example"></a>

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra duas cadeias de caracteres de formato numérico personalizado. Em ambos os casos, o espaço reservado de dígito (`#`) exibe os dados numéricos, e todos os outros caracteres são copiados para a cadeia de caracteres de resultado.

[!code-cpp[Formatting.Numeric.Custom#10](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/example1.cpp#10)]
[!code-csharp-interactive[Formatting.Numeric.Custom#10](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/example1.cs#10)]
[!code-vb[Formatting.Numeric.Custom#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/example1.vb#10)]

[Voltar à tabela](#table)

## <a name="see-also"></a>Confira também

- <xref:System.Globalization.NumberFormatInfo?displayProperty=nameWithType>
- [Formatar tipos](formatting-types.md)
- [Cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md)
- [Como: Preencher um número com zeros à esquerda](how-to-pad-a-number-with-leading-zeros.md)
- [Amostra: Utilitário de Formatação do WinForms do .NET Core (C#)](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-cs)
- [Amostra: Utilitário de Formatação do WinForms do .NET Core (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-vb)
