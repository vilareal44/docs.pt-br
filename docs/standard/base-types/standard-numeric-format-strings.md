---
title: Cadeias de caracteres de formato numérico padrão
description: Neste artigo, aprenda a usar cadeias de caracteres de formato numérico padrão para formatar tipos numéricos comuns em representações de texto no .NET.
ms.date: 06/10/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- numeric format strings [.NET Framework]
- formatting [.NET Framework], numbers
- standard format strings, numeric
- format strings
- numbers [.NET Framework], formatting
- format specifiers, numeric
- standard numeric format strings
- formatting numbers [.NET Framework]
- format specifiers, standard numeric format strings
ms.openlocfilehash: 605438a5f0e4b5bd9ade96c9db0416ee8611f311
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447115"
---
# <a name="standard-numeric-format-strings"></a>Cadeias de caracteres de formato numérico padrão

As cadeias de caracteres de formato numérico padrão são usadas para formatar tipos numéricos comuns. Uma cadeia de caracteres de formato numérico padrão assume o formato `Axx`, em que:

- `A` é um caractere alfabético único chamado *especificador de formato*. Qualquer cadeia de caracteres de formato numérico que contém mais de um caractere alfabético, incluindo espaços em branco, é interpretada como uma cadeia de caracteres de formato numérico personalizado. Para obter mais informações, consulte [Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md).

- `xx` é um inteiro opcional chamado *especificador de precisão*. O especificador de precisão varia de 0 a 99 e afeta o número de dígitos no resultado. Observe que o especificador de precisão controla o número de dígitos na representação da cadeia de caracteres de um número. Ele não arredonda o número em si. Para executar uma operação de arredondamento, use o método <xref:System.Math.Ceiling%2A?displayProperty=nameWithType>, <xref:System.Math.Floor%2A?displayProperty=nameWithType> ou <xref:System.Math.Round%2A?displayProperty=nameWithType>.

  Quando o *especificador de precisão* controla o número de dígitos fracionários na cadeia de caracteres de resultado, ela reflete um número que será arredondado para um resultado representável mais próximo do resultado infinitamente preciso. Se houver dois resultados representáveis igualmente próximos:
  - **No .NET Framework e .NET Core até o .NET Core 2.0**, o runtime selecionará o resultado com o dígito menos significativo maior (ou seja, usando <xref:System.MidpointRounding.AwayFromZero?displayProperty=nameWithType>).
  - **No .NET Core 2.1 e versões posteriores**, o runtime selecionará o resultado com um dígito até menos significativo (ou seja, usando <xref:System.MidpointRounding.ToEven?displayProperty=nameWithType>).

  > [!NOTE]
  > O especificador de precisão determina o número de dígitos na cadeia de caracteres de resultado. Para acrescentar espaços à direita ou à esquerda em uma cadeia de caracteres de resultado, use o recurso [formatação de composição](composite-formatting.md) e defina um *componente de alinhamento* no item de formato.

As cadeias de caractere de formato numérico padrão têm suporte de:

- Algumas sobrecargas do método `ToString` de todos os tipos numéricos. Por exemplo, você pode fornecer uma cadeia de caracteres de formato numérico para os métodos <xref:System.Int32.ToString%28System.String%29?displayProperty=nameWithType> e <xref:System.Int32.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType>.

- O [recurso de formatação composta](composite-formatting.md) do .NET, o qual é usado por alguns métodos `Write` e `WriteLine` das classes <xref:System.Console> e <xref:System.IO.StreamWriter>, pelo método <xref:System.String.Format%2A?displayProperty=nameWithType> e pelo método <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>. O recurso de formato de composição permite a inclusão da representação da cadeia de caracteres de vários itens de dados em uma única cadeia de caracteres, para especificar a largura do campo e alinhar os números em um campo. Para obter mais informações, veja [Formatação de composição](composite-formatting.md).

- [Cadeias de caracteres interpoladas](../../csharp/language-reference/tokens/interpolated.md) em C# e Visual Basic, que fornecem uma sintaxe simplificada quando comparada a cadeias de caracteres de formato composto.

> [!TIP]
> Baixe o **Utilitário de Formatação**, um aplicativo do Windows Forms do .NET Core que permite aplicar cadeias de caracteres de formato a valores numéricos ou de data e hora e exibir a cadeia de caracteres de resultado. O código-fonte está disponível para o [C#](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-cs) e o [Visual Basic](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-vb).

<a name="table"></a>A tabela a seguir descreve os especificadores de formato numérico padrão e exibe a saída de exemplo produzida por cada especificador de formato. Consulte a seção [Notas](#NotesStandardFormatting) para obter informações adicionais sobre como usar cadeias de caracteres de formato numérico padrão e a seção [Exemplo](#example) para obter uma ilustração abrangente de seu uso.

|Especificador de formato|Nome|Descrição|Exemplos|
|----------------------|----------|-----------------|--------------|
|"C" ou "c"|Moeda|Resultado: um valor de moeda.<br /><br /> Compatível com: todos os tipos numéricos.<br /><br /> Especificador de precisão: número de casas decimais.<br /><br /> Especificador de precisão padrão: definido por <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A?displayProperty=nameWithType>.<br /><br /> Para saber mais: [especificador de formato de moeda ("C")](#CFormatString).|123,456 ("C", en-US)-> \\ $123.46<br /><br /> 123.456 ("C", fr-FR) -> 123,46 €<br /><br /> 123.456 ("C", ja-JP) -> ¥123<br /><br /> -123,456 ("C3", en-US)-> ( \\ $123.456)<br /><br /> -123.456 ("C3", fr-FR) -> -123,456 €<br /><br /> -123.456 ("C3", ja-JP) -> -¥123.456|
|"D" ou "d"|Decimal|Resultado: dígitos inteiros com sinal negativo opcional.<br /><br /> Compatível com: somente tipos integrais.<br /><br /> Especificador de precisão: número mínimo de dígitos.<br /><br /> Especificador de precisão padrão: número mínimo de dígitos necessários.<br /><br /> Para saber mais: [especificador de formato decimal ("D")](#DFormatString).|1234 ("D") -> 1234<br /><br /> -1234 ("D6") -> -001234|
|"E" ou "e"|Exponencial (científica)|Resultado: notação Exponencial.<br /><br /> Compatível com: todos os tipos numéricos.<br /><br /> Especificador de precisão: número de casas decimais.<br /><br /> Especificador de precisão padrão: 6.<br /><br /> Para saber mais: [especificador de formato exponencial ("E")](#EFormatString).|1052.0329112756 ("E", en-US) -> 1.052033E+003<br /><br /> 1052.0329112756 ("e", fr-FR) -> 1,052033e+003<br /><br /> -1052.0329112756 ("e2", en-US) -> -1.05e+003<br /><br /> -1052.0329112756 ("E2", fr-FR) -> -1,05E+003|
|"F" ou "f"|Ponto fixo|Resultado: dígitos integrais e decimais com sinal negativo opcional.<br /><br /> Compatível com: todos os tipos numéricos.<br /><br /> Especificador de precisão: número de casas decimais.<br /><br /> Especificador de precisão padrão: definido por <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=nameWithType>.<br /><br /> Para saber mais: [especificador de formato de ponto fixo ("F")](#FFormatString).|1234.567 ("F", en-US) -> 1234.57<br /><br /> 1234.567 ("F", de-DE) -> 1234,57<br /><br /> 1234 ("F1", en-US) -> 1234.0<br /><br /> 1234 ("F1", de-DE) -> 1234,0<br /><br /> -1234.56 ("F4", en-US) -> -1234.5600<br /><br /> -1234.56 ("F4", de-DE) -> -1234,5600|
|"G" ou "g"|Geral|Resultado: a mais compacta entre notação de ponto fixo ou científica.<br /><br /> Compatível com: todos os tipos numéricos.<br /><br /> Especificador de precisão: número de dígitos significativos.<br /><br /> Especificador de precisão padrão: depende do tipo numérico.<br /><br /> Para saber mais: [especificador de formato geral ("G")](#GFormatString).|-123.456 ("G", en-US) -> -123.456<br /><br /> -123.456 ("G", sv-SE) -> -123,456<br /><br /> 123.4546 ("G4", en-US) -> 123.5<br /><br /> 123.4546 ("G4", sv-SE) -> 123,5<br /><br /> -1.234567890e-25 ("G", en-US) -> -1.23456789E-25<br /><br /> -1.234567890e-25 ("G", sv-SE) -> -1,23456789E-25|
|"N" ou "n"|Número|Resultado: dígitos integrais e decimais, separadores de grupo e um separador decimal com sinal negativo opcional.<br /><br /> Compatível com: todos os tipos numéricos.<br /><br /> Especificador de precisão: número de casas decimais desejadas.<br /><br /> Especificador de precisão padrão: definido por <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=nameWithType>.<br /><br /> Para saber mais: [especificador de formato numérico ("N")](#NFormatString).|1234.567 ("N", en-US) -> 1,234.57<br /><br /> 1234.567 ("N", ru-RU) -> 1 234,57<br /><br /> 1234 ("N1", en-US) -> 1,234.0<br /><br /> 1234 ("N1", ru-RU) -> 1 234,0<br /><br /> -1234.56 ("N3", en-US) -> -1,234.560<br /><br /> -1234.56 ("N3", ru-RU) -> -1 234,560|
|"P" ou "p"|Porcentagem|Resultado: número multiplicado por 100 e exibido com um sinal de porcentagem.<br /><br /> Compatível com: todos os tipos numéricos.<br /><br /> Especificador de precisão: número de casas decimais desejadas.<br /><br /> especificador de precisão padrão: definido por <xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A?displayProperty=nameWithType>.<br /><br /> Para saber mais: [especificador de formato de porcentagem ("P")](#PFormatString).|1 ("P", en-US) -> 100.00 %<br /><br /> 1 ("P", fr-FR) -> 100,00 %<br /><br /> -0.39678 ("P1", en-US) -> -39.7 %<br /><br /> -0.39678 ("P1", fr-FR) -> -39,7 %|
|"R" ou "r"|Ida e volta|Resultado: uma cadeia de caracteres que pode ir e voltar para um número idêntico.<br /><br /> Compatível com: <xref:System.Single>, <xref:System.Double> e <xref:System.Numerics.BigInteger>.<br /><br /> Observação: recomendado apenas para o tipo <xref:System.Numerics.BigInteger>. Para os tipos <xref:System.Double>, use "G17"; para os tipos <xref:System.Single>, use "G9". <br> Especificador de precisão: ignorado.<br /><br /> Para saber mais: [especificador de formato de ida e volta ("R")](#RFormatString).|123456789.12345678 ("R") -> 123456789.12345678<br /><br /> -1234567890.12345678 ("R") -> -1234567890.1234567|
|"X" ou "x"|Hexadecimal|Resultado: uma cadeia de caracteres hexadecimal.<br /><br /> Compatível com: somente tipos integrais.<br /><br /> Especificador de precisão: número de dígitos na cadeia de caracteres de resultado.<br /><br /> Para saber mais: [especificador de formato hexadecimal ("X")](#XFormatString).|255 ("X") -> FF<br /><br /> -1 ("x") -> ff<br /><br /> 255 ("x4") -> 00ff<br /><br /> -1 ("X4") -> 00FF|
|Qualquer outro caractere único|Especificador desconhecido|Resultado: gera uma <xref:System.FormatException> em tempo de execução.||

<a name="Using"></a>

## <a name="using-standard-numeric-format-strings"></a>Usando Cadeias de Caracteres de Formato Numérico Padrão

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

Uma cadeia de caracteres de formato numérico padrão pode ser usada para definir a formatação de um valor numérico em uma de duas formas:

- Ela pode ser passada para uma sobrecarga do método `ToString` que tem um parâmetro `format`. O exemplo a seguir formata um valor numérico como uma cadeia de caracteres de moeda na cultura atual (nesse caso, a cultura en-US).

  [!code-cpp[Formatting.Numeric.Standard#10](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#10)]
  [!code-csharp-interactive[Formatting.Numeric.Standard#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#10)]
  [!code-vb[Formatting.Numeric.Standard#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#10)]

- Ele pode ser fornecido como o argumento `formatString` em um item de formato usado com métodos como <xref:System.String.Format%2A?displayProperty=nameWithType>, <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> e <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>. Para obter mais informações, veja [Formatação de composição](composite-formatting.md). O exemplo a seguir usa um item de formato para inserir um valor de moeda em uma cadeia de caracteres.

  [!code-cpp[Formatting.Numeric.Standard#11](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#11)]
  [!code-csharp-interactive[Formatting.Numeric.Standard#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#11)]
  [!code-vb[Formatting.Numeric.Standard#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#11)]

  Opcionalmente, você pode fornecer um argumento `alignment` para especificar a largura do campo numérico e se o valor é alinhado à direita ou à esquerda. O exemplo a seguir alinha à esquerda um valor de moeda em um campo de 28 caracteres e alinha à direita um valor de moeda em um campo de 14 caracteres.

  [!code-cpp[Formatting.Numeric.Standard#12](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#12)]
  [!code-csharp-interactive[Formatting.Numeric.Standard#12](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#12)]
  [!code-vb[Formatting.Numeric.Standard#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#12)]

- Ele pode ser fornecido como o argumento `formatString` em um item de expressão interpolada de uma cadeia de caracteres interpolada. Para obter mais informações, consulte o tópico [Interpolação de cadeia de caracteres](../../csharp/language-reference/tokens/interpolated.md) na referência do C# ou o tópico [Cadeias de caracteres interpoladas](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md) na referência do Visual Basic.

As seções a seguir fornecem informações detalhadas sobre cada uma das cadeias de caracteres de formato numérico padrão.

<a name="CFormatString"></a>

## <a name="the-currency-c-format-specifier"></a>O Especificador de Formato da Moeda ("C")

O especificador de formato "C" (ou moeda) converte um número em uma cadeia de caracteres que representa um valor de moeda. O especificador de precisão indica o número desejado de casas decimais na cadeia de caracteres resultante. Se o especificador de precisão for omitido, a precisão padrão será definida pela propriedade <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A?displayProperty=nameWithType>.

Se o valor a ser formatado tem mais do que o número especificado ou padrão de casas decimais, o valor fracionário é arredondado na cadeia de caracteres de resultado. Se o valor à direita do número de casas decimais especificadas for 5 ou mais, o último dígito da cadeia de caracteres do resultado será arredondado para cima.

A cadeia de caracteres de resultado é afetada pelas informações de formatação do objeto <xref:System.Globalization.NumberFormatInfo> atual. A tabela a seguir lista as propriedades <xref:System.Globalization.NumberFormatInfo> que controlam a formatação da cadeia de caracteres retornada.

|Propriedade NumberFormatInfo|Descrição|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.CurrencyPositivePattern%2A>|Define o posicionamento do símbolo de moeda para valores positivos.|
|<xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A>|Define o posicionamento do símbolo da moeda para valores negativos e especifica se o sinal de negativo é representado por parênteses ou pela propriedade <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>.|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Define o sinal de negativo usado se <xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A> indicar que parênteses não são usados.|
|<xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A>|Define o símbolo de moeda.|
|<xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A>|Define o número padrão de dígitos decimais em um valor de moeda. Esse valor pode ser substituído usando-se o especificador de precisão.|
|<xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A>|Define a cadeia de caracteres que separa dígitos decimais e integrais.|
|<xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A>|Define a cadeia de caracteres que separa grupos de números integrais.|
|<xref:System.Globalization.NumberFormatInfo.CurrencyGroupSizes%2A>|Define o número de dígitos inteiros que aparecem em um grupo.|

O exemplo a seguir formata um <xref:System.Double> valor com o especificador de formato de moeda:

[!code-cpp[Formatting.Numeric.Standard#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#1)]
[!code-csharp[Formatting.Numeric.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#1)]
[!code-vb[Formatting.Numeric.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#1)]

[Voltar à tabela](#table)

<a name="DFormatString"></a>

## <a name="the-decimal-d-format-specifier"></a>O Especificador de Formato Decimal ("D")

O especificador de formato “D” (ou decimal) converte um número em uma cadeia de caracteres de dígitos decimais (0-9), prefixados por um sinal de negativo se o número for negativo. Esse formato é compatível apenas com tipos integrais.

O especificador de precisão indica o número mínimo de dígitos desejados na cadeia de caracteres resultante. Se necessário, o número é preenchido com zeros à esquerda para produzir o número de dígitos fornecido pelo especificador de precisão. Quando nenhum especificador de precisão é especificado, o padrão é o valor mínimo necessário para representar o inteiro sem zeros à esquerda.

A cadeia de caracteres de resultado é afetada pelas informações de formatação do objeto <xref:System.Globalization.NumberFormatInfo> atual. Conforme mostrado na tabela a seguir, uma única propriedade afeta a formatação da cadeia de caracteres de resultado.

|Propriedade NumberFormatInfo|Descrição|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Define a cadeia de caracteres que indica que um número é negativo.|

O exemplo a seguir formata um valor <xref:System.Int32> com o especificador de formato decimal.

[!code-cpp[Formatting.Numeric.Standard#2](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#2)]
[!code-csharp-interactive[Formatting.Numeric.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#2)]
[!code-vb[Formatting.Numeric.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#2)]

[Voltar à tabela](#table)

<a name="EFormatString"></a>

## <a name="the-exponential-e-format-specifier"></a>O Especificador de Formato Exponencial ("E")

O especificador do formato do exponencial ("E") converte um número em uma cadeia de caracteres no formato "-d.ddd…E+ddd" ou "-d.ddd…e+ddd", em que cada 'd' indica um dígito (0-9). A cadeia de caracteres é iniciada com um sinal de negativo se o número é negativo. Exatamente um dígito sempre precede o ponto decimal.

O especificador de precisão indica o número desejado de dígitos após o ponto decimal. Quando o especificador de precisão é omitido, um padrão de seis dígitos após o ponto decimal é usado.

A caixa do especificador de formato indica se o expoente é prefixado com um "E" ou um "e". O expoente sempre consistem em um sinal de positivo ou negativo e um mínimo de três dígitos. O expoente é preenchido com zeros para atender a esse mínimo, se necessário.

A cadeia de caracteres de resultado é afetada pelas informações de formatação do objeto <xref:System.Globalization.NumberFormatInfo> atual. A tabela a seguir lista as propriedades <xref:System.Globalization.NumberFormatInfo> que controlam a formatação da cadeia de caracteres retornada.

|Propriedade NumberFormatInfo|Descrição|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Define a cadeia de caracteres que indica que o número é negativo para o coeficiente e o expoente.|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Define a cadeia de caracteres que separa o dígito integral dos dígitos decimais no coeficiente.|
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|Define a cadeia de caracteres que indica que um expoente é positivo.|

O exemplo a seguir formata um <xref:System.Double> valor com o especificador de formato exponencial:

[!code-cpp[Formatting.Numeric.Standard#3](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#3)]
[!code-csharp[Formatting.Numeric.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#3)]
[!code-vb[Formatting.Numeric.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#3)]

[Voltar à tabela](#table)

<a name="FFormatString"></a>

## <a name="the-fixed-point-f-format-specifier"></a>O Especificador de Formato de Ponto Fixo ("F")

O especificador de formato de ponto fixo ("F") converte um número em uma cadeia de caracteres no formato "-ddd.ddd…", em que cada "d" indica um dígito (0-9). A cadeia de caracteres é iniciada com um sinal de negativo se o número é negativo.

O especificador de precisão indica o número de casas decimais desejadas. Quando o especificador de precisão é omitido, a propriedade <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=nameWithType> atual fornece a precisão numérica.

A cadeia de caracteres de resultado é afetada pelas informações de formatação do objeto <xref:System.Globalization.NumberFormatInfo> atual. A tabela a seguir lista as propriedades do objeto <xref:System.Globalization.NumberFormatInfo> que controlam a formatação da cadeia de caracteres de resultado.

|Propriedade NumberFormatInfo|Descrição|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Define a cadeia de caracteres que indica que um número é negativo.|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Define a cadeia de caracteres que separa dígitos integrais de dígitos decimais.|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A>|Define o número padrão de dígitos decimais. Esse valor pode ser substituído usando-se o especificador de precisão.|

O exemplo a seguir formata um <xref:System.Double> e um <xref:System.Int32> valor com o especificador de formato de ponto fixo:

[!code-cpp[Formatting.Numeric.Standard#4](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#4)]
[!code-csharp[Formatting.Numeric.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#4)]
[!code-vb[Formatting.Numeric.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#4)]

[Voltar à tabela](#table)

<a name="GFormatString"></a>

## <a name="the-general-g-format-specifier"></a>O Especificador de Formato Geral ("G")

O especificador de formato geral ("G") converte um número para a notação mais compacta entre ponto fixo ou científica, dependendo do tipo do número e se um especificador de precisão está presente. O especificador de precisão define o número máximo de dígitos significativos que podem aparecer na cadeia de caracteres de resultado. Quando o especificador de precisão é omitido ou zero, o tipo do número determina a precisão padrão, conforme indicado na tabela a seguir.

|Tipo numérico|Precisão padrão|
|------------------|-----------------------|
|<xref:System.Byte> ou <xref:System.SByte>|3 dígitos|
|<xref:System.Int16> ou <xref:System.UInt16>|5 dígitos|
|<xref:System.Int32> ou <xref:System.UInt32>|10 dígitos|
|<xref:System.Int64>|19 dígitos|
|<xref:System.UInt64>|20 dígitos|
|<xref:System.Numerics.BigInteger>|Ilimitado (igual a ["R"](#RFormatString))|
|<xref:System.Single>|7 dígitos|
|<xref:System.Double>|15 dígitos|
|<xref:System.Decimal>|29 dígitos|

A notação de ponto fixo é usada se o expoente que resultaria ao expressar o número na notação científica é maior do que -5 e menor que o especificador de precisão; caso contrário, a notação científica é usada. Se necessário, o resultado contém um ponto decimal, e os zeros à direita após o ponto decimal são omitidos. Se o especificador de precisão estiver presente e o número de dígitos significativos no resultado exceder a precisão especificada, o excesso de dígitos à direita será removido por arredondamento.

No entanto, se o número for <xref:System.Decimal> e o especificador de precisão for omitido, a notação de ponto fixo será usada sempre e os zeros à direita serão preservados.

Se a notação científica for usada, o expoente no resultado será prefixado com "E" se o especificador de formato for "G" ou "e" se o especificador de formato for "g". O expoente contém um mínimo de dois dígitos. Isso difere do formato de notação científica que é gerado pelo especificador de formato exponencial, o qual inclui um mínimo de três dígitos no expoente.

Observe que, quando é usado com um valor <xref:System.Double>, o especificador de formato "G17" garante que o valor de <xref:System.Double> original faça a viagem de ida e volta com êxito. Isso ocorre porque <xref:System.Double> é um número de ponto flutuante de precisão dupla compatível com IEEE 754 2008 (`binary64`) que oferece até 17 dígitos significativos de precisão. Recomendamos seu uso em vez do [especificador de formato "R"](#RFormatString), pois em alguns casos "R" não realiza a viagem de ida e volta dos valores de ponto flutuante de precisão dupla. O exemplo a seguir ilustra esse caso.

[!code-csharp-interactive[Round-tripping a Double](../../../samples/snippets/standard/base-types/format-strings/csharp/g17.cs#GeneralFormatSpecifier)]
[!code-vb[Round-tripping a Double](../../../samples/snippets/standard/base-types/format-strings/vb/g17.vb)]

Quando usado com um valor <xref:System.Single>, o especificador de formato "G9" garante que o valor <xref:System.Single> original faça a viagem de ida e volta com êxito. Isso ocorre porque <xref:System.Single> é um número de ponto flutuante com precisão única compatível com IEEE 754-2008 (`binary32`) que fornece até nove dígitos significativos de precisão. Por motivos de desempenho, recomendamos seu uso em vez do [especificador de formato "R"](#RFormatString).

A cadeia de caracteres de resultado é afetada pelas informações de formatação do objeto <xref:System.Globalization.NumberFormatInfo> atual. A tabela a seguir lista as propriedades <xref:System.Globalization.NumberFormatInfo> que controlam a formatação da cadeia de caracteres de resultado.

|Propriedade NumberFormatInfo|Descrição|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Define a cadeia de caracteres que indica que um número é negativo.|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Define a cadeia de caracteres que separa dígitos integrais de dígitos decimais.|
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|Define a cadeia de caracteres que indica que um expoente é positivo.|

O exemplo a seguir formata valores de ponto flutuante asvariados com o especificador de formato geral:

[!code-cpp[Formatting.Numeric.Standard#5](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#5)]
[!code-csharp[Formatting.Numeric.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#5)]
[!code-vb[Formatting.Numeric.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#5)]

[Voltar à tabela](#table)

<a name="NFormatString"></a>

## <a name="the-numeric-n-format-specifier"></a>O Especificador de Formato Numérico ("N")

O especificador de formato numérico ("N") converte um número em uma cadeia de caracteres no formato "-d,ddd,ddd.ddd…", em que "-" indica um símbolo de número negativo, se necessário, "d" indica um dígito (0-9), "," indica um separador de grupos de número e "." indica um símbolo de ponto decimal. O especificador de precisão indica o número desejado de dígitos após o ponto decimal. Se o especificador de precisão for omitido, o número de casas decimais será definido pela propriedade <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=nameWithType> atual.

A cadeia de caracteres de resultado é afetada pelas informações de formatação do objeto <xref:System.Globalization.NumberFormatInfo> atual. A tabela a seguir lista as propriedades <xref:System.Globalization.NumberFormatInfo> que controlam a formatação da cadeia de caracteres de resultado.

|Propriedade NumberFormatInfo|Descrição|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Define a cadeia de caracteres que indica que um número é negativo.|
|<xref:System.Globalization.NumberFormatInfo.NumberNegativePattern%2A>|Define o formato dos valores negativos e especifica se o sinal de negativo é representado por parênteses ou a propriedade <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>.|
|<xref:System.Globalization.NumberFormatInfo.NumberGroupSizes%2A>|Define o número total de dígitos integrais que aparecem entre os separadores de grupo.|
|<xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A>|Define a cadeia de caracteres que separa grupos de números integrais.|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Define a cadeia de caracteres que separa dígitos decimais e integrais.|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A>|Define o número padrão de dígitos decimais. Esse valor pode ser substituído usando um especificador de precisão.|

O exemplo a seguir formata valores de ponto flutuante classificados com o especificador de formato de número:

[!code-cpp[Formatting.Numeric.Standard#6](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#6)]
[!code-csharp[Formatting.Numeric.Standard#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#6)]
[!code-vb[Formatting.Numeric.Standard#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#6)]

[Voltar à tabela](#table)

<a name="PFormatString"></a>

## <a name="the-percent-p-format-specifier"></a>O Especificador de Formato de Porcentagem ("P")

O especificador de formato de porcentagem (“P”) multiplica um número por 100 e o converte em uma cadeia de caracteres que representa uma porcentagem. O especificador de precisão indica o número de casas decimais desejadas. Se o especificador de precisão for omitido, a precisão numérica padrão fornecida pela propriedade <xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A> atual será usada.

A tabela a seguir lista as propriedades <xref:System.Globalization.NumberFormatInfo> que controlam a formatação da cadeia de caracteres retornada.

|Propriedade NumberFormatInfo|Descrição|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.PercentPositivePattern%2A>|Define o posicionamento do símbolo de porcentagem para valores positivos.|
|<xref:System.Globalization.NumberFormatInfo.PercentNegativePattern%2A>|Define o posicionamento do símbolo de porcentagem e o símbolo negativo para valores negativos.|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Define a cadeia de caracteres que indica que um número é negativo.|
|<xref:System.Globalization.NumberFormatInfo.PercentSymbol%2A>|Define o símbolo de porcentagem.|
|<xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A>|Define o número padrão de dígitos decimais em um valor percentual. Esse valor pode ser substituído usando-se o especificador de precisão.|
|<xref:System.Globalization.NumberFormatInfo.PercentDecimalSeparator%2A>|Define a cadeia de caracteres que separa dígitos decimais e integrais.|
|<xref:System.Globalization.NumberFormatInfo.PercentGroupSeparator%2A>|Define a cadeia de caracteres que separa grupos de números integrais.|
|<xref:System.Globalization.NumberFormatInfo.PercentGroupSizes%2A>|Define o número de dígitos inteiros que aparecem em um grupo.|

O exemplo a seguir formata valores de ponto flutuante com o especificador de formato de porcentagem:

[!code-cpp[Formatting.Numeric.Standard#7](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#7)]
[!code-csharp[Formatting.Numeric.Standard#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#7)]
[!code-vb[Formatting.Numeric.Standard#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#7)]

[Voltar à tabela](#table)

<a name="RFormatString"></a>

## <a name="the-round-trip-r-format-specifier"></a>O Especificador de Formato da Viagem de Ida e Volta ("R")

O especificador de formato de ida e volta ("R") tenta garantir que um valor numérico convertido em uma cadeia de caracteres seja analisado com o mesmo valor numérico. Esse formato é compatível apenas com os tipos <xref:System.Single>, <xref:System.Double> e <xref:System.Numerics.BigInteger>.

Para valores <xref:System.Double>, o especificador de formato "R" em alguns casos falha em realizar a viagem de ida e volta com êxito do valor original. Para os valores <xref:System.Double> e <xref:System.Single>, ele também oferece desempenho relativamente baixo. Em vez disso, recomendamos que você use o especificador de formato ["G17"](#GFormatString) para os valores <xref:System.Double> e o especificador de formato ["G9"](#GFormatString) para realizar a viagem de ida e volta dos valores <xref:System.Single>.

Quando um valor <xref:System.Numerics.BigInteger> é formatado usando esse especificador, sua representação de cadeia de caracteres contém todos os dígitos significativos no valor de <xref:System.Numerics.BigInteger>.

Embora você possa incluir um especificador de precisão, ele será ignorado. Idas e voltas têm precedência sobre a precisão quando esse especificador é usado.
A cadeia de caracteres de resultado é afetada pelas informações de formatação do objeto <xref:System.Globalization.NumberFormatInfo> atual. A tabela a seguir lista as propriedades <xref:System.Globalization.NumberFormatInfo> que controlam a formatação da cadeia de caracteres de resultado.

|Propriedade NumberFormatInfo|Descrição|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|Define a cadeia de caracteres que indica que um número é negativo.|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|Define a cadeia de caracteres que separa dígitos integrais de dígitos decimais.|
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|Define a cadeia de caracteres que indica que um expoente é positivo.|

O exemplo a seguir formata um valor <xref:System.Numerics.BigInteger> com o especificador de formato de ida e volta.

[!code-cpp[R format specifier with a BigInteger](../../../samples/snippets/standard/base-types/format-strings/biginteger-r.cpp)]
[!code-csharp[R format specifier with a BigInteger](../../../samples/snippets/standard/base-types/format-strings/biginteger-r.cs)]
[!code-vb[R format specifier with a BigInteger](../../../samples/snippets/standard/base-types/format-strings/biginteger-r.vb)]

> [!IMPORTANT]
> Em alguns casos, os valores <xref:System.Double> formatados com a cadeia de caracteres de formato numérico padrão "R" não realizam a viagem de ida e volta se forem compilados usando as opções `/platform:x64` ou `/platform:anycpu` e executados em sistemas de 64 bits. Para saber mais, consulte o seguinte parágrafo.

Para solucionar o problema de valores <xref:System.Double> formatados com a cadeia de caracteres no formato numérico padrão "R" que não conseguem realizar a viagem de ida e volta se forem compilados usando as opções `/platform:x64` ou `/platform:anycpu` e executados em sistemas de 64 bits, formate os valores <xref:System.Double> usando a cadeia de caracteres de formato numérico padrão "G17". O exemplo a seguir usa a cadeia de caracteres de formato "R" com um <xref:System.Double> valor que não faz a viagem de ida e volta com êxito e também usa a cadeia de caracteres de formato "G17" para fazer uma viagem de ida e volta com êxito ao valor original:

[!code-csharp[System.Double.ToString#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Double.ToString/cs/roundtripex1.cs#RoundTrip)]
[!code-vb[System.Double.ToString#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Double.ToString/vb/roundtripex1.vb#5)]

[Voltar à tabela](#table)

<a name="XFormatString"></a>

## <a name="the-hexadecimal-x-format-specifier"></a>O Especificador de Formato Hexadecimal ("X")

O especificador de formato hexadecimal ("X") converte um número em uma cadeia de caracteres de dígitos hexadecimais. A caixa do especificador de formato indica se caracteres maiúsculos ou minúsculos devem ser usados para os dígitos hexadecimais maiores que 9. Por exemplo, use "X" para produzir "ABCDEF" e "x" para produzir "abcdef". Esse formato é compatível apenas com tipos integrais.

O especificador de precisão indica o número mínimo de dígitos desejados na cadeia de caracteres resultante. Se necessário, o número é preenchido com zeros à esquerda para produzir o número de dígitos fornecido pelo especificador de precisão.

A cadeia de caracteres do resultado não é afetada pelas informações de formatação do objeto <xref:System.Globalization.NumberFormatInfo> atual.

O exemplo a seguir formata valores <xref:System.Int32> com o especificador de formato hexadecimal.

[!code-cpp[Formatting.Numeric.Standard#9](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#9)]
[!code-csharp-interactive[Formatting.Numeric.Standard#9](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#9)]
[!code-vb[Formatting.Numeric.Standard#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#9)]

[Voltar à tabela](#table)

<a name="NotesStandardFormatting"></a>

## <a name="notes"></a>Observações

### <a name="control-panel-settings"></a>Configurações do Painel de Controle

As configurações no item **Opções Regionais e de Idioma** do Painel de Controle influenciam a cadeia de caracteres de resultado produzida por uma operação de formatação. Essas configurações são usadas para inicializar o objeto <xref:System.Globalization.NumberFormatInfo> associado à cultura do thread atual, a qual fornece os valores usados para determinar a formatação. Computadores que usam configurações diferentes geram cadeias de caracteres de resultado diferentes.

Além disso, se o construtor <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29> for usado para criar uma instância de um novo objeto <xref:System.Globalization.CultureInfo> que representa a mesma cultura que a cultura atual do sistema, quaisquer personalizações estabelecidas pelo item **Opções Regionais e de Idioma** no Painel de Controle serão aplicadas ao novo objeto <xref:System.Globalization.CultureInfo>. Você pode usar o construtor <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> para criar um objeto <xref:System.Globalization.CultureInfo> que não reflita as personalizações de um sistema.

### <a name="numberformatinfo-properties"></a>Propriedades de NumberFormatInfo

A formatação é influenciada pelas propriedades do objeto <xref:System.Globalization.NumberFormatInfo> atual, o qual é fornecido implicitamente pela cultura do thread atual ou explicitamente pelo parâmetro <xref:System.IFormatProvider> do método que invoca a formatação. Especifique um objeto <xref:System.Globalization.NumberFormatInfo> ou <xref:System.Globalization.CultureInfo> para esse parâmetro.

> [!NOTE]
> Para obter informações sobre como personalizar os padrões ou cadeias de caracteres usados na formatação de valores numéricos, consulte o tópico de classe <xref:System.Globalization.NumberFormatInfo>.

### <a name="integral-and-floating-point-numeric-types"></a>Tipos numéricos integrais e de ponto flutuante

Algumas descrições de especificadores de formato numérico padrão referem-se a tipos inteiros ou de ponto flutuante. Os tipos numéricos integrais são <xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.Int32>, <xref:System.Int64>, <xref:System.UInt16>, <xref:System.UInt32>, <xref:System.UInt64> e <xref:System.Numerics.BigInteger>. Os tipos numéricos de ponto flutuante são <xref:System.Decimal>, <xref:System.Single> e <xref:System.Double>.

### <a name="floating-point-infinities-and-nan"></a>Infinitos de ponto flutuante e NaN

Independentemente da cadeia de caracteres de formato, se o valor de um tipo de ponto flutuante <xref:System.Single> ou <xref:System.Double> é infinito positivo, infinito negativo ou um não é um número (NaN), a cadeia de caracteres formatada é o valor das respectivas propriedades <xref:System.Globalization.NumberFormatInfo.PositiveInfinitySymbol%2A>, <xref:System.Globalization.NumberFormatInfo.NegativeInfinitySymbol%2A> ou <xref:System.Globalization.NumberFormatInfo.NaNSymbol%2A> especificadas pelo objeto <xref:System.Globalization.NumberFormatInfo> aplicável no momento.

## <a name="example"></a>Exemplo

[!INCLUDE[interactive-note](~/includes/csharp-interactive-partial-note.md)]

O exemplo a seguir formata um inteiro e um valor numérico de ponto flutuante usando a cultura en-US e todos os especificadores de formato numérico padrão. Este exemplo usa dois tipos numéricos específicos, (<xref:System.Double> e <xref:System.Int32>), mas poderia produzir resultados semelhantes para qualquer um dos tipos base numéricos (<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.Int32>, <xref:System.Int64>, <xref:System.UInt16>, <xref:System.UInt32>, <xref:System.UInt64>, <xref:System.Numerics.BigInteger>, <xref:System.Decimal> e <xref:System.Single>).

[!code-csharp[system.x.tostring-and-culture#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.X.ToString-and-Culture/cs/xts.cs#FinalExample)]
[!code-vb[system.x.tostring-and-culture#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.X.ToString-and-Culture/vb/xts.vb#1)]

## <a name="see-also"></a>Confira também

- <xref:System.Globalization.NumberFormatInfo>
- [Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md)
- [Formatar tipos](formatting-types.md)
- [Como: Preencher um número com zeros à esquerda](how-to-pad-a-number-with-leading-zeros.md)
- [Formatação composta](composite-formatting.md)
- [Amostra: Utilitário de Formatação do WinForms do .NET Core (C#)](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-cs)
- [Amostra: Utilitário de Formatação do WinForms do .NET Core (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/windowsforms-formatting-utility-vb)
