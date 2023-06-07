---
title: Tipos de formatação no .NET
description: Saiba como formatar tipos no .NET. Entenda como usar ou substituir o método ToString. Saiba mais sobre a formatação sensível à cultura, composta e personalizada.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data formatting [.NET Framework]
- dates [.NET Framework], formatting
- date formatting [.NET Framework]
- number formatting [.NET Framework]
- ToString method
- custom cultural settings [.NET Framework]
- numbers [.NET Framework], formatting
- formatting strings [.NET Framework]
- time [.NET Framework], formatting
- currency [.NET Framework], formatting
- types [.NET Framework], formatting
- format specifiers [.NET Framework]
- times [.NET Framework], formatting
- culture [.NET Framework], formatting
- formatting [.NET Framework], types supported
- base types [.NET Framework], formatting
- custom formatting [.NET Framework]
- strings [.NET Framework], formatting
ms.assetid: 0d1364da-5b30-4d42-8e6b-03378343343f
ms.openlocfilehash: 5d280b53d15bc674f325a726d69915d763aec34f
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447076"
---
# <a name="format-types-in-net"></a>Tipos de formato no .NET

Formatação é o processo de conversão de uma instância de classe, estrutura ou valor de enumeração em sua representação de cadeia de caracteres, de forma que a cadeia de caracteres resultante possa ser exibida aos usuários ou desserializada para restaurar o tipo de dados original. Essa conversão pode apresentar uma série de desafios:

- A maneira que os valores são armazenados internamente não necessariamente reflete a maneira que os usuários desejam exibi-los. Por exemplo, um número de telefone pode ser armazenado no formato 8009999999, que não é amigável. Em vez disso, ele deve ser exibido como 800-999-9999. Consulte a seção [cadeias de caracteres de formato personalizado](#custom-format-strings) para obter um exemplo que formata um número dessa maneira.

- Às vezes, a conversão de um objeto para sua representação de cadeia de caracteres não é intuitiva. Por exemplo, não é claro o modo como a representação de cadeia de caracteres de um objeto de Temperatura ou um objeto de Pessoa deve aparecer. Para obter um exemplo que formata um objeto de temperatura de várias maneiras, consulte a seção [formatos de cadeia de caracteres de formato padrão](#standard-format-strings) .

- Muitas vezes, os valores exigem formatação que leva em conta a cultura. Por exemplo, em um aplicativo que usa números para refletir valores monetários, as cadeias de caracteres numéricas devem incluir o símbolo de moeda da cultura atual, o separador de grupo (que, na maioria das culturas, é o separador de milhar) e o símbolo decimal. Para obter um exemplo, consulte a seção [formatação sensível à cultura com provedores de formato](#culture-sensitive-formatting-with-format-providers) .

- Um aplicativo pode ter que exibir o mesmo valor de maneiras diferentes. Por exemplo, um aplicativo pode representar um membro de enumeração ao exibindo uma representação de cadeia de caracteres de seu nome ou exibindo seu valor subjacente. Para obter um exemplo que formata um membro da enumeração <xref:System.DayOfWeek> de maneiras diferentes, veja a seção [Cadeias de caracteres de formato padrão](#standard-format-strings).

> [!NOTE]
> A formatação converte o valor de um tipo em uma representação de cadeia de caracteres. A análise é o inverso da formatação. Uma operação de análise cria uma instância de um tipo de dados com base em na representação de sua cadeia de caracteres. Para obter informações sobre como converter cadeias de caracteres para outros tipos de dados, consulte [analisando cadeias de caracteres](parsing-strings.md).

O .NET dá suporte à formatação avançada, que permite aos desenvolvedores atender a esses requisitos.

## <a name="formatting-in-net"></a>Formatação em .NET

O mecanismo básico de formatação é a implementação padrão do método <xref:System.Object.ToString%2A?displayProperty=nameWithType>, que é abordado na seção [Formatação padrão usando o método ToString](#default-formatting-using-the-tostring-method), mais adiante neste tópico. No entanto, o .NET oferece várias maneiras de modificar e estender o suporte à formatação padrão. Entre elas estão as seguintes:

- Substituindo o <xref:System.Object.ToString%2A?displayProperty=nameWithType> método para definir uma representação de cadeia de caracteres personalizada do valor de um objeto. Para obter mais informações, consulte a seção [substituir o método ToString](#override-the-tostring-method) mais adiante neste tópico.

- Definição de especificadores de formato que habilitam a representação de cadeia de caracteres do valor de um objeto para assumir vários formulários. Por exemplo, o especificador de formato "X" na instrução a seguir converte um inteiro na representação de cadeia de caracteres de um valor hexadecimal.

     [!code-csharp[Conceptual.Formatting.Overview#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/specifier1.cs#3)]
     [!code-vb[Conceptual.Formatting.Overview#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/specifier1.vb#3)]

     Para obter mais informações sobre especificadores de formato, veja a seção [Cadeias de caracteres de formato e método ToString](#the-tostring-method-and-format-strings).

- O uso de provedores de formato para aproveitar as convenções de formatação de uma cultura específica. Por exemplo, a instrução a seguir exibe um valor de moeda usando as convenções de formatação da cultura en-US.

     [!code-csharp[Conceptual.Formatting.Overview#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/specifier1.cs#10)]
     [!code-vb[Conceptual.Formatting.Overview#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/specifier1.vb#10)]

     Para obter mais informações sobre formatação com provedores de formato, consulte a seção [provedores de formato](#culture-sensitive-formatting-with-format-providers) .

- A implementação da interface <xref:System.IFormattable> para dar suporte tanto à conversão de cadeia de caracteres com a classe <xref:System.Convert> quanto à formatação de composição. Para obter mais informações, consulte a seção [Interface IFormattable](#the-iformattable-interface).

- O uso da formatação de composição para inserir a representação de cadeia de caracteres de um valor em uma cadeia de caracteres maior. Para obter mais informações, consulte a seção [formatação composta](#composite-formatting) .

- Implementando <xref:System.ICustomFormatter> e <xref:System.IFormatProvider> para fornecer uma solução completa de formatação personalizada. Para obter mais informações, consulte a seção [formatação personalizada com ICustomFormatter](#custom-formatting-with-icustomformatter) .

As seções a seguir examinam esses métodos para converter um objeto em sua representação de cadeia de caracteres.

## <a name="default-formatting-using-the-tostring-method"></a>Formatação padrão usando o método ToString

Cada tipo é derivado de <xref:System.Object?displayProperty=nameWithType> herda automaticamente um método `ToString` sem parâmetros, que retorna o nome do tipo por padrão. O exemplo a seguir ilustra o método `ToString` padrão. Ele define uma classe chamada `Automobile` que não tem nenhuma implementação. Quando a classe é instanciada e seu método `ToString` é chamado, ele exibe seu nome de tipo. Observe que o método `ToString` não é chamado explicitamente no exemplo. O método <xref:System.Console.WriteLine%28System.Object%29?displayProperty=nameWithType> chama implicitamente o método `ToString` do objeto é passado para ele como um argumento.

[!code-csharp[Conceptual.Formatting.Overview#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/default1.cs#1)]
[!code-vb[Conceptual.Formatting.Overview#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/default1.vb#1)]

> [!WARNING]
> A partir do Windows 8.1, a Windows Runtime inclui uma <xref:Windows.Foundation.IStringable> interface com um único método, [isastringable. ToString](xref:Windows.Foundation.IStringable.ToString%2A), que fornece suporte de formatação padrão. No entanto, recomendamos que tipos gerenciados não implementem a interface `IStringable`. Para obter mais informações, veja a seção "Windows Runtime e a Interface `IStringable`" na página de referência <xref:System.Object.ToString%2A?displayProperty=nameWithType>.

Já que todos os tipos, com a exceção das interfaces, são derivados de <xref:System.Object>, essa funcionalidade é fornecida automaticamente para suas estruturas ou classes personalizadas. No entanto, a funcionalidade oferecida pelo método `ToString` padrão é limitada: embora ele identifique o tipo, não fornece nenhuma informação sobre uma instância do tipo. Para fornecer uma representação de cadeia de caracteres de um objeto que fornece informações sobre o objeto, você deve substituir o método `ToString`.

> [!NOTE]
> As estruturas herdam de <xref:System.ValueType>, que por sua vez é derivado de <xref:System.Object>. Embora <xref:System.ValueType> substitua <xref:System.Object.ToString%2A?displayProperty=nameWithType>, sua implementação é idêntica.

## <a name="override-the-tostring-method"></a>Substituir o método ToString

A exibição do nome de um tipo é geralmente de uso limitado e não permite que os consumidores dos seus tipos diferenciem uma instância da outra. No entanto, você pode substituir o `ToString` método para fornecer uma representação mais útil do valor de um objeto. O exemplo a seguir define um objeto `Temperature` e substitui seu método `ToString` para exibir a temperatura em graus Celsius.

[!code-csharp[Conceptual.Formatting.Overview#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/overrides1.cs#2)]
[!code-vb[Conceptual.Formatting.Overview#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/overrides1.vb#2)]

No .NET, o `ToString` método de cada tipo de valor primitivo foi substituído para exibir o valor do objeto em vez de seu nome. A tabela a seguir mostra a substituição de cada tipo primitivo. Observe que a maioria dos métodos substituídos chama outra sobrecarga do método `ToString` e passa-a ao especificador de formato "G", que define o formato geral de seu tipo, além de um objeto <xref:System.IFormatProvider> que representa a cultura atual.

|Type|Substituição de ToString|
|----------|-----------------------|
|<xref:System.Boolean>|Retorna <xref:System.Boolean.TrueString?displayProperty=nameWithType> ou <xref:System.Boolean.FalseString?displayProperty=nameWithType>.|
|<xref:System.Byte>|Chama `Byte.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.Byte> para a cultura atual.|
|<xref:System.Char>|Retorna o caractere como uma cadeia de caracteres.|
|<xref:System.DateTime>|Chama `DateTime.ToString("G", DatetimeFormatInfo.CurrentInfo)` para formatar o valor de data e hora para a cultura atual.|
|<xref:System.Decimal>|Chama `Decimal.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.Decimal> para a cultura atual.|
|<xref:System.Double>|Chama `Double.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.Double> para a cultura atual.|
|<xref:System.Int16>|Chama `Int16.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.Int16> para a cultura atual.|
|<xref:System.Int32>|Chama `Int32.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.Int32> para a cultura atual.|
|<xref:System.Int64>|Chama `Int64.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.Int64> para a cultura atual.|
|<xref:System.SByte>|Chama `SByte.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.SByte> para a cultura atual.|
|<xref:System.Single>|Chama `Single.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.Single> para a cultura atual.|
|<xref:System.UInt16>|Chama `UInt16.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.UInt16> para a cultura atual.|
|<xref:System.UInt32>|Chama `UInt32.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.UInt32> para a cultura atual.|
|<xref:System.UInt64>|Chama `UInt64.ToString("G", NumberFormatInfo.CurrentInfo)` para formatar o valor <xref:System.UInt64> para a cultura atual.|

## <a name="the-tostring-method-and-format-strings"></a>As cadeias de caractere de formato e do método ToString

Contar com o método padrão `ToString` ou substituir `ToString` é apropriado quando um objeto tem uma única representação de cadeia de caracteres. No entanto, o valor de um objeto normalmente tem várias representações. Por exemplo, uma temperatura pode ser expressa em graus Fahrenheit, graus Celsius ou kelvins. De forma similar, o valor de inteiro 10 pode ser representado de várias maneiras, incluindo 10, 10,0, 1.0e01 ou US $10,00.

Para permitir que um único valor tenha várias representações de cadeia de caracteres, o .NET usa cadeias de caracteres de formato. Uma cadeia de caracteres de formato é uma cadeia de caracteres que contém um ou mais especificadores de formato predefinidos, que são caracteres únicos ou grupos de caracteres que definem como o método `ToString` deve formatar sua saída. A cadeia de caracteres de formato é passada como um parâmetro para o método `ToString` do objeto e determina como a representação de cadeia de caracteres do valor do objeto deve ser exibida.

Todos os tipos numéricos, de data e hora e tipos de enumeração no .NET dão suporte a um conjunto predefinido de especificadores de formato. Você também pode usar cadeias de caracteres de formato para definir várias representações de cadeia de caracteres de seus tipos de dados definidos pelo aplicativo.

### <a name="standard-format-strings"></a>Cadeias de caracteres de formato padrão

Uma cadeia de caracteres de formato padrão contém um único especificador de formato, que é um caractere alfabético que define a representação de cadeia de caracteres do objeto ao qual ela é aplicada, junto com um especificador de precisão opcional que afeta a quantos dígitos serão exibidos na cadeia de caracteres de resultado. Se o especificador de precisão for omitido ou não houver suporte a ele, um especificador de formato padrão será equivalente a uma cadeia de caracteres de formato padrão.

O .NET define um conjunto de especificadores de formato padrão para todos os tipos numéricos, todos os tipos de data e hora e todos os tipos de enumeração. Por exemplo, cada uma dessas categorias dá suporte a um especificador de formato padrão "G", que define uma representação de cadeia de caracteres geral de um valor desse mesmo tipo.

Cadeias de caracteres de formato padrão para tipos de enumeração controlam diretamente a representação de cadeia de caracteres de um valor. As cadeias de caracteres de formato passadas para o método de um valor de enumeração `ToString` determinam se o valor é exibido usando seu nome de cadeia de caracteres (os especificadores de formato "G" e "F"), seu valor integral subjacente (o especificador de formato "D") ou seu valor hexadecimal (o especificador de formato "X"). O exemplo a seguir ilustra o uso de cadeias de caracteres de formato padrão para formatar um valor de enumeração <xref:System.DayOfWeek>.

[!code-csharp[Conceptual.Formatting.Overview#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/standard1.cs#4)]
[!code-vb[Conceptual.Formatting.Overview#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/standard1.vb#4)]

Para obter informações sobre cadeias de caracteres de formato de enumeração, consulte [Strings de formato de enumeração](enumeration-format-strings.md).

Cadeias de caracteres de formato padrão para tipos numéricos geralmente definem uma cadeia de caracteres de resultado cuja aparência exata é controlada por um ou mais valores de propriedade. Por exemplo, o especificador de formato "C" formata um número como um valor de moeda. Quando você chama o `ToString` método com o especificador de formato "C" como o único parâmetro, os seguintes valores de Propriedade do objeto da cultura atual <xref:System.Globalization.NumberFormatInfo> são usados para definir a representação da cadeia de caracteres do valor numérico:

- A <xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A> propriedade, que especifica o símbolo de moeda da cultura atual.

- A propriedade <xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A> ou <xref:System.Globalization.NumberFormatInfo.CurrencyPositivePattern%2A>, que retorna um inteiro que determina o seguinte:

  - O posicionamento do símbolo da moeda.

  - Se valores negativos são indicados por um sinal de negativo à esquerda, um sinal de negativo à direita ou parênteses.

  - Se um espaço é ou não exibido entre o valor numérico e o símbolo da moeda.

- A propriedade <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A>, que define o número de dígitos fracionários na cadeia de caracteres de resultado.

- O propriedade <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A>, que define o símbolo do separador decimal na cadeia de caracteres de resultado.

- A propriedade <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A>, que define o símbolo de separador de grupo.

- A propriedade <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSizes%2A>, que define o número de dígitos em cada grupo à esquerda da vírgula decimal.

- A propriedade <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>, que determinará o sinal de negativo usado na cadeia de caracteres de resultado se parênteses não forem usados para indicar valores negativos.

Além disso, cadeias de caracteres de formato numérico podem incluir um especificador de precisão. O significado desse especificador depende da cadeia de caracteres de formato com o qual ele é usado, mas ele normalmente indica o número total de dígitos ou o número de dígitos fracionários que devem aparecer na cadeia de caracteres de resultado. Por exemplo, o exemplo a seguir usa a cadeia de caracteres numérica padrão "X4" e um especificador de precisão para criar um valor de cadeia de caracteres com quatro dígitos hexadecimais.

[!code-csharp[Conceptual.Formatting.Overview#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/precisionspecifier1.cs#6)]
[!code-vb[Conceptual.Formatting.Overview#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/precisionspecifier1.vb#6)]

Para obter mais informações sobre cadeias de caracteres de formatação numérica padrão, consulte [cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md).

Cadeias de caracteres de formato padrão para valores de data e hora são aliases para cadeias de caracteres de formato personalizado armazenadas por uma propriedade <xref:System.Globalization.DateTimeFormatInfo> particular. Por exemplo, chamar o `ToString` método de um valor de data e hora com o especificador de formato "D" exibe a data e a hora usando a cadeia de caracteres de formato personalizado armazenada na propriedade da cultura atual <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> . (Para obter mais informações sobre cadeias de caracteres de formato personalizado, consulte a [próxima seção](#custom-format-strings).) O exemplo a seguir ilustra essa relação.

[!code-csharp[Conceptual.Formatting.Overview#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/alias1.cs#5)]
[!code-vb[Conceptual.Formatting.Overview#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/alias1.vb#5)]

Para obter mais informações sobre cadeias de caracteres de formato de data e hora padrão, consulte [cadeias de caracteres de formato padrão de data e hora](standard-date-and-time-format-strings.md).

Você também pode usar cadeias de formato padrão para definir a representação de cadeia de caracteres de um objeto definido pelo aplicativo que é produzido pelo método do objeto `ToString(String)` . Você pode definir os especificadores de formato padrão específicos que dão suporte a seu objeto e você pode determinar se eles diferenciam ou não maiúsculas de minúsculas. A implementação do `ToString(String)` método deve dar suporte ao seguinte:

- Um especificador de formato "G" que representa um formato comum ou habitual do objeto. A sobrecarga sem parâmetros do método `ToString` de seu objeto deve chamar sua sobrecarga `ToString(String)` e passar cadeia de caracteres de formato padrão "G".

- Suporte para um especificador de formato que é igual a uma referência nula (`Nothing` em Visual Basic). Um especificador de formato igual a uma referência nula deve ser considerado equivalente ao especificador de formato "G".

Por exemplo, um `Temperature` classe interna pode armazenar a temperatura em graus Celsius e usar especificadores de formato para representar o valor do objeto `Temperature` em graus Fahrenheit, graus Celsius e kelvins. O exemplo a seguir ilustra esse cenário.

[!code-csharp[Conceptual.Formatting.Overview#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/appstandard1.cs#7)]
[!code-vb[Conceptual.Formatting.Overview#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/appstandard1.vb#7)]

### <a name="custom-format-strings"></a>Cadeias de formato personalizadas

Além de cadeias de caracteres de formato padrão, o .NET define cadeias de caracteres de formato personalizado para valores numéricos e valores de data e hora. Uma cadeia de caracteres de formato personalizado consiste em um ou mais especificadores de formato personalizado que definem a representação de cadeia de caracteres de um valor. Por exemplo, a cadeia de caracteres de formato de data e hora personalizada "aaaa/mm/dd hh:mm:ss.ffff t zzz" converte uma data em sua representação de cadeia de caracteres no formato "2008/11/15 07:45:00.0000 P -08:00" para a cultura en-US. Da mesma forma, a cadeia de caracteres de formato personalizado "0000" converte o valor inteiro 12 em "0012". Para obter uma lista completa de cadeias de caracteres de formato personalizado, veja [Cadeias de caracteres de formato de data e hora personalizado](custom-date-and-time-format-strings.md) e [Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md).

Se uma cadeia de caracteres de formato consiste em um único especificador de formato personalizado, o especificador de formato deve ser precedido pelo símbolo de porcentagem (%) para evitar confusão com um especificador de formato padrão. O exemplo a seguir usa o especificador de formato personalizado "M" para exibir um número de um ou dois dígitos do mês de uma data específica.

[!code-csharp[Conceptual.Formatting.Overview#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/singlecustom1.cs#8)]
[!code-vb[Conceptual.Formatting.Overview#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/singlecustom1.vb#8)]

Muitas cadeias de caracteres de formato padrão para valores de data e hora são aliases para cadeias de caracteres de formato personalizado que são definidas pelas propriedades do objeto <xref:System.Globalization.DateTimeFormatInfo>. Cadeias de caracteres de formato personalizado também oferecem flexibilidade considerável no fornecimento de formação definida pelo aplicativo para valores numéricos ou valores de data e hora. Você pode definir suas próprias cadeias de caracteres de resultado personalizadas para valores numéricos e valores de data e hora combinando vários especificadores de formato personalizado em uma única cadeia de caracteres de formato personalizado. O exemplo a seguir define uma cadeia de caracteres de formato personalizado que exibe o dia da semana entre parênteses após o nome do mês, o dia e o ano.

[!code-csharp[Conceptual.Formatting.Overview#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/custom1.cs#9)]
[!code-vb[Conceptual.Formatting.Overview#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/custom1.vb#9)]

O exemplo a seguir define uma cadeia de caracteres de formato personalizado que exibe um valor <xref:System.Int64> como um número de telefone de sete dígitos padrão dos EUA, junto com seu código de área.

[!code-csharp[Conceptual.Formatting.Overview#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/telnumber1.cs#21)]
[!code-vb[Conceptual.Formatting.Overview#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/telnumber1.vb#21)]

Embora as cadeias de caracteres de formato padrão geralmente tratem da maioria das necessidades de formatação para os tipos definidos pelo aplicativo, você também pode definir especificadores de formato personalizado para formatar seus tipos.

### <a name="format-strings-and-net-types"></a>Cadeias de caracteres de formato e tipos do .NET

Todos os tipos numéricos (ou seja, os tipos <xref:System.Byte>, <xref:System.Decimal>, <xref:System.Double>, <xref:System.Int16>, <xref:System.Int32>, <xref:System.Int64>, <xref:System.SByte>, <xref:System.Single>, <xref:System.UInt16>, <xref:System.UInt32>, <xref:System.UInt64> e <xref:System.Numerics.BigInteger>), bem como o <xref:System.DateTime>, <xref:System.DateTimeOffset>, <xref:System.TimeSpan>, <xref:System.Guid>, e todos os tipos de enumeração, suportam a formatação com cadeias de caracteres de formato. Para obter informações sobre as cadeias de caracteres de formato específicas às quais cada tipo dá suporte, veja os seguintes tópicos:

|Título|Definição|
|-----------|----------------|
|[Cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md)|Descreve cadeias de caracteres de formato padrão que criam representações de cadeia de caracteres de valores numéricos frequentemente usadas.|
|[Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md)|Descreve cadeias de caracteres de formato personalizado que criam formatos específicos de aplicativo para valores numéricos.|
|[Cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md)|Descreve cadeias de caracteres de formato padrão que criam representações de cadeia de caracteres de valores <xref:System.DateTime> e <xref:System.DateTimeOffset> frequentemente usadas.|
|[Cadeias de caracteres de formato de data e hora personalizadas](custom-date-and-time-format-strings.md)|Descreve cadeias de caracteres de formato personalizado que criam formatos específicos de aplicativo para valores <xref:System.DateTime> e <xref:System.DateTimeOffset>.|
|[Cadeias de caracteres de formato standard TimeSpan](standard-timespan-format-strings.md)|Descreve cadeias de caracteres de formato padrão que criam representações de intervalos de tempo frequentemente usadas.|
|[Cadeias de caracteres de formato de TimeSpan personalizado](custom-timespan-format-strings.md)|Descreve cadeias de caracteres de formato personalizado que criam formatos específicos de aplicativo para intervalos de tempo.|
|[Cadeias de caracteres de formato de enumeração](enumeration-format-strings.md)|Descreve cadeias de caracteres de formato padrão que são usadas para criar representações de cadeia de caracteres de valores de enumeração.|
|<xref:System.Guid.ToString%28System.String%29?displayProperty=nameWithType>|Descreve cadeias de caracteres de formato padrão para valores <xref:System.Guid>.|

## <a name="culture-sensitive-formatting-with-format-providers"></a>Formatação sensível à cultura com provedores de formato

Embora os especificadores de formato permitam personalizar a formatação de objetos, a produção de uma representação de cadeia de caracteres de objetos significativa geralmente requer informações de formatação adicionais. Por exemplo, formatar um número como um valor de moeda usando a cadeia de caracteres de formato padrão "C" ou então uma cadeia de caracteres de formato personalizado como "$ #,#.00" requer que, no mínimo, informações sobre o símbolo correto da moeda, o separador de grupo e o separador decimal estejam disponíveis para inclusão na cadeia de caracteres formatada. No .NET, essas informações de formatação adicionais são disponibilizadas por meio da interface <xref:System.IFormatProvider>, que é fornecida como um parâmetro para um ou mais sobrecargas do método `ToString` de tipos numéricos e tipos de data e hora. Implementações <xref:System.IFormatProvider> são usadas em .NET para dar suporte à formatação específica à cultura. O exemplo a seguir ilustra como a representação de cadeia de caracteres de um objeto é alterado quando ele é formatado com três objetos <xref:System.IFormatProvider> que representam diferentes culturas.

[!code-csharp[Conceptual.Formatting.Overview#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformatprovider1.cs#11)]
[!code-vb[Conceptual.Formatting.Overview#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformatprovider1.vb#11)]

A interface <xref:System.IFormatProvider> inclui um método, <xref:System.IFormatProvider.GetFormat%28System.Type%29>, que tem um único parâmetro que especifica o tipo de objeto que fornece informações de formatação. Se o método puder fornecer um objeto desse tipo, ele retornará esse objeto. Caso contrário, ele retornará uma referência nula (`Nothing` em Visual Basic).

<xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> é um método de retorno de chamada. Quando você chama uma sobrecarga do método `ToString` que inclui um parâmetro <xref:System.IFormatProvider>, ela chama o método <xref:System.IFormatProvider.GetFormat%2A> daquele objeto <xref:System.IFormatProvider>. O método <xref:System.IFormatProvider.GetFormat%2A> é responsável por retornar um objeto que fornece as informações de formatação necessárias, conforme especificadas pelo seu parâmetro `formatType` para o método `ToString`.

Um número de métodos de conversão de cadeia de caracteres ou formatação inclui um parâmetro de tipo <xref:System.IFormatProvider>, mas em muitos casos o valor do parâmetro é ignorado quando o método é chamado. A tabela a seguir lista alguns dos métodos de formatação que usam o parâmetro e o tipo do objeto <xref:System.Type> que passam para o método <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType>.

|Método|Tipo de parâmetro `formatType`|
|------------|------------------------------------|
|`ToString` método de tipos numéricos|<xref:System.Globalization.NumberFormatInfo?displayProperty=nameWithType>|
|`ToString` método de tipos de data e hora|<xref:System.Globalization.DateTimeFormatInfo?displayProperty=nameWithType>|
|<xref:System.String.Format%2A?displayProperty=nameWithType>|<xref:System.ICustomFormatter?displayProperty=nameWithType>|
|<xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>|<xref:System.ICustomFormatter?displayProperty=nameWithType>|

> [!NOTE]
> Os métodos `ToString` dos tipos numéricos e dos tipos de data e hora estão sobrecarregados, e somente algumas das sobrecargas incluem um parâmetro <xref:System.IFormatProvider>. Se um método não tiver um parâmetro do tipo <xref:System.IFormatProvider>, o objeto retornado pela propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> será passado. Por exemplo, uma chamada para o método <xref:System.Int32.ToString?displayProperty=nameWithType> padrão acaba resultando em uma chamada de método como esta: `Int32.ToString("G", System.Globalization.CultureInfo.CurrentCulture)`.

O .NET fornece três classes que implementam <xref:System.IFormatProvider>:

- <xref:System.Globalization.DateTimeFormatInfo>, uma classe que fornece informações de formatação para valores de data e hora para uma cultura específica. Sua implementação de <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> retorna uma instância de si mesma.

- <xref:System.Globalization.NumberFormatInfo>, uma classe que fornece informações de formatação numérica para uma cultura específica. Sua implementação de <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> retorna uma instância de si mesma.

- <xref:System.Globalization.CultureInfo>. Sua implementação de <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> pode retornar um objeto <xref:System.Globalization.DateTimeFormatInfo> para fornecer informações de formatação numérica ou um objeto <xref:System.Globalization.NumberFormatInfo> para fornecer informações de formatação para valores de data e hora.

Você também pode implementar seu próprio provedor de formato para substituir qualquer uma dessas classes. No entanto, o <xref:System.IFormatProvider.GetFormat%2A> método de implementação deve retornar um objeto do tipo listado na tabela anterior se ele tiver que fornecer informações de formatação para o `ToString` método.

### <a name="culture-sensitive-formatting-of-numeric-values"></a>Formatação de valores numéricos que leva em conta a cultura

Por padrão, a formatação de valores numéricos leva em conta a cultura. Se você não especificar uma cultura quando chamar um método de formatação, as convenções de formatação da cultura do thread atual serão usadas. Isso é ilustrado no exemplo a seguir, que altera a cultura do thread atual quatro vezes e, em seguida, chama o método <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType>. Em cada caso, a cadeia de caracteres de resultado reflete as convenções de formatação da cultura atual. Isso ocorre porque os métodos `ToString` e `ToString(String)` encapsulam chamadas para o método `ToString(String, IFormatProvider)` de cada tipo numérico.

[!code-csharp[Conceptual.Formatting.Overview#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific3.cs#19)]
[!code-vb[Conceptual.Formatting.Overview#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific3.vb#19)]

Você também pode formatar um valor numérico para uma cultura específica chamando uma sobrecarga `ToString` que tenha um parâmetro de `provider` e passando-o a um dos dois elementos a seguir:

- Um objeto <xref:System.Globalization.CultureInfo> que representa a cultura cujas convenções de formatação devem ser usadas. Seu método <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> retorna o valor da propriedade <xref:System.Globalization.CultureInfo.NumberFormat%2A?displayProperty=nameWithType>, que é o objeto <xref:System.Globalization.NumberFormatInfo> que fornece informações de formatação específicas da cultura para valores numéricos.

- Um objeto <xref:System.Globalization.NumberFormatInfo> que define as convenções de formatação específicas da cultura a serem usadas. Seu método <xref:System.Globalization.NumberFormatInfo.GetFormat%2A> retorna uma instância de si mesmo.

O exemplo a seguir usa objetos <xref:System.Globalization.NumberFormatInfo> que representam as culturas inglês (Estados Unidos) e inglês (Grã-Bretanha), além das culturas neutras francês e russo, para formatar um número de ponto flutuante.

[!code-csharp[Conceptual.Formatting.Overview#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific4.cs#20)]
[!code-vb[Conceptual.Formatting.Overview#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific4.vb#20)]

### <a name="culture-sensitive-formatting-of-date-and-time-values"></a>Formatação de valores de data e hora que leva em conta a cultura

Por padrão, a formatação de valores de data e hora leva em conta a cultura. Se você não especificar uma cultura quando chamar um método de formatação, as convenções de formatação da cultura do thread atual serão usadas. Isso é ilustrado no exemplo a seguir, que altera a cultura do thread atual quatro vezes e, em seguida, chama o método <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType>. Em cada caso, a cadeia de caracteres de resultado reflete as convenções de formatação da cultura atual. Isso ocorre porque os métodos <xref:System.DateTime.ToString?displayProperty=nameWithType>, <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType>, <xref:System.DateTimeOffset.ToString?displayProperty=nameWithType> e <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> encapsulam chamadas para os métodos <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> e <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType>.

[!code-csharp[Conceptual.Formatting.Overview#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific1.cs#17)]
[!code-vb[Conceptual.Formatting.Overview#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific1.vb#17)]

Você também pode formatar um valor de data e hora para uma cultura específica chamando uma sobrecarga <xref:System.DateTime.ToString%2A?displayProperty=nameWithType> ou <xref:System.DateTimeOffset.ToString%2A?displayProperty=nameWithType> que tenha um parâmetro `provider` e passando-o a um dos dois elementos a seguir:

- Um objeto <xref:System.Globalization.CultureInfo> que representa a cultura cujas convenções de formatação devem ser usadas. Seu método <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> retorna o valor da propriedade <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType>, que é o objeto <xref:System.Globalization.DateTimeFormatInfo> que fornece informações de formatação específicas da cultura para valores de data e hora.

- Um objeto <xref:System.Globalization.DateTimeFormatInfo> que define as convenções de formatação específicas da cultura a serem usadas. Seu método <xref:System.Globalization.DateTimeFormatInfo.GetFormat%2A> retorna uma instância de si mesmo.

O exemplo a seguir usa objetos <xref:System.Globalization.DateTimeFormatInfo> que representam as culturas inglês (Estados Unidos) e inglês (Grã-Bretanha), além das culturas neutras francês e russo, para formatar uma data.

[!code-csharp[Conceptual.Formatting.Overview#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/culturespecific2.cs#18)]
[!code-vb[Conceptual.Formatting.Overview#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/culturespecific2.vb#18)]

## <a name="the-iformattable-interface"></a>A interface IFormattable

Normalmente, os tipos que sobrecarregam o método `ToString` com uma cadeia de caracteres de formato e um parâmetro <xref:System.IFormatProvider> também implementam a interface <xref:System.IFormattable>. Essa interface tem um único membro, <xref:System.IFormattable.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType>, que inclui como parâmetros uma cadeia de caracteres de formato e um provedor de formato.

Implementar a interface <xref:System.IFormattable> para a classe definida pelo aplicativo oferece duas vantagens:

- Suporte para conversão de cadeia de caracteres pela classe <xref:System.Convert>. As chamadas para os métodos <xref:System.Convert.ToString%28System.Object%29?displayProperty=nameWithType> e <xref:System.Convert.ToString%28System.Object%2CSystem.IFormatProvider%29?displayProperty=nameWithType> chamam sua implementação <xref:System.IFormattable> automaticamente.

- Suporte à formatação composição. Se um item de formato que inclui uma cadeia de caracteres de formato for usado para formatar seu tipo personalizado, o Common Language Runtime chamará automaticamente a implementação <xref:System.IFormattable> e passará a ele a cadeia de caracteres de formato. Para obter mais informações sobre a formatação composta com métodos como <xref:System.String.Format%2A?displayProperty=nameWithType> ou <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> , consulte a seção de [formatação composta](#composite-formatting) .

O exemplo a seguir define uma classe `Temperature` que implementa a interface <xref:System.IFormattable>. Ela dá suporte aos especificadores de formato "C" ou "G" para exibir a temperatura em graus Celsius, o especificador de formato "F" para exibir a temperatura em Fahrenheit e o especificador de formato "K" para exibir a temperatura em Kelvin.

[!code-csharp[Conceptual.Formatting.Overview#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformattable.cs#12)]
[!code-vb[Conceptual.Formatting.Overview#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformattable.vb#12)]

O exemplo a seguir instancia um objeto `Temperature`. Depois, ele chama o método <xref:System.Convert.ToString%2A> e usa várias cadeias de caracteres de formato de composição para obter diferentes representações de cadeia de caracteres de um objeto `Temperature`. Cada uma dessas chamadas de método, por sua vez, chama a implementação <xref:System.IFormattable> da classe `Temperature`.

[!code-csharp[Conceptual.Formatting.Overview#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/iformattable.cs#13)]
[!code-vb[Conceptual.Formatting.Overview#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/iformattable.vb#13)]

## <a name="composite-formatting"></a>Formatação de composição

Alguns métodos, tais como <xref:System.String.Format%2A?displayProperty=nameWithType> e <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>, dão suporte à formatação de composição. Uma cadeia de caracteres de formato de composição é um tipo de modelo que retorna uma única cadeia de caracteres que incorpora a representação de cadeia de caracteres de zero, um ou mais objetos. Cada objeto é representado na cadeia de caracteres de formato de composição por um item de formato indexado. O índice do item de formato corresponde à posição do objeto que ele representa na lista de parâmetros do método. Os índices são baseados em zero. Por exemplo, na seguinte chamada para o método <xref:System.String.Format%2A?displayProperty=nameWithType>, o primeiro item de formato, `{0:D}`, é substituído pela representação de cadeia de caracteres de `thatDate`; o segundo item de formato, `{1}`, é substituído pela representação de cadeia de caracteres de `item1` e, por fim, o terceiro item de formato, `{2:C2}`, é substituído pela representação de cadeia de caracteres de `item1.Value`.

[!code-csharp[Conceptual.Formatting.Overview#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/composite1.cs#14)]
[!code-vb[Conceptual.Formatting.Overview#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/composite1.vb#14)]

Além de substituir um item de formato pela representação de cadeia de caracteres de seu objeto correspondente, os itens de formato também permitem que você controle o seguinte:

- O modo específico em que um objeto é representado como uma cadeia de caracteres, se o objeto implementa a interface <xref:System.IFormattable> e se dá suporte a cadeias de caracteres de formato. Você pode fazer isso seguindo o índice do item de formato com um `:` (dois-pontos) seguido por uma cadeia de caracteres de formato válido. O exemplo anterior já fez isso ao formatar um valor de data com a cadeia de caracteres de formato (por exemplo, `{0:d}`) "d" (padrão de data abreviada) e formatando um valor numérico com a cadeia de caracteres de formato "C2" (por exemplo, `{2:C2}`) para representar o número como um valor de moeda com dois dígitos decimais fracionários.

- A largura do campo que contém a representação de cadeia de caracteres do objeto e o alinhamento da representação de cadeia de caracteres nesse campo. Você pode fazer isso seguindo o índice do item de formato com uma `,` (vírgula) seguida da largura do campo. A cadeia de caracteres será alinhada à direita no campo se a largura do campo for um valor positivo ou à esquerda se esse valor for negativo. O exemplo a seguir alinha os valores de data à esquerda em um campo de 20 caracteres e alinha valores decimais com um dígito fracionário à direita em um campo de 11 caracteres.

     [!code-csharp[Conceptual.Formatting.Overview#22](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/composite2.cs#22)]
     [!code-vb[Conceptual.Formatting.Overview#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/composite2.vb#22)]

     Observe que, se o componente de cadeia de caracteres de alinhamento e o componente de cadeia de caracteres de formato estiverem presentes, o primeiro precederá o último (por exemplo, `{0,-20:g}`.

Para obter mais informações sobre formatação composta, consulte [formatação composta](composite-formatting.md).

## <a name="custom-formatting-with-icustomformatter"></a>Formatação personalizada com ICustomFormatter

Dois métodos de formatação de composição <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> e <xref:System.Text.StringBuilder.AppendFormat%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>, incluem um parâmetro de provedor de formato que dá suporte à formatação personalizada. Quando qualquer um desses métodos de formatação é chamado, ele passa um <xref:System.Type> objeto que representa uma <xref:System.ICustomFormatter> interface para o método do provedor de formato <xref:System.IFormatProvider.GetFormat%2A> . O método <xref:System.IFormatProvider.GetFormat%2A>, em seguida, será responsável por retornar a implementação <xref:System.ICustomFormatter> que oferece formatação personalizada.

A interface <xref:System.ICustomFormatter> tem um único método, <xref:System.ICustomFormatter.Format%28System.String%2CSystem.Object%2CSystem.IFormatProvider%29>, que é chamado automaticamente por um método de formatação de composição, uma vez para cada item de formato em uma cadeia de caracteres de formato de composição. O método <xref:System.ICustomFormatter.Format%28System.String%2CSystem.Object%2CSystem.IFormatProvider%29> tem três parâmetros: uma cadeia de caracteres de formato, que representa o argumento `formatString` em um item de formato, um objeto a ser formatado e um objeto <xref:System.IFormatProvider> que oferece serviços de formatação. Normalmente, a classe que implementa <xref:System.ICustomFormatter> também implementa <xref:System.IFormatProvider>, portanto este último parâmetro é uma referência para a própria classe de formatação personalizada. O método retorna uma representação de cadeia de caracteres formatada personalizada do objeto a ser formatado. Se o método não for capaz de formatar o objeto, ele deverá retornar uma referência nula (`Nothing` em Visual Basic).

O exemplo a seguir fornece uma implementação <xref:System.ICustomFormatter> chamada `ByteByByteFormatter` que exibe valores inteiros como uma sequência de valores hexadecimais de dois dígitos seguidos por um espaço.

[!code-csharp[Conceptual.Formatting.Overview#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/icustomformatter1.cs#15)]
[!code-vb[Conceptual.Formatting.Overview#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/icustomformatter1.vb#15)]

O exemplo a seguir usa a classe `ByteByByteFormatter` para formatar valores inteiros. Observe que o método <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> é chamado mais de uma vez na segunda chamada de método <xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> e que o provedor padrão <xref:System.Globalization.NumberFormatInfo> é usado na terceira chamada do método porque o método .`ByteByByteFormatter.Format` não reconhece a cadeia de caracteres de formato "N0" e retorna uma referência nula (`Nothing` no Visual Basic).

[!code-csharp[Conceptual.Formatting.Overview#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.formatting.overview/cs/icustomformatter1.cs#16)]
[!code-vb[Conceptual.Formatting.Overview#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.formatting.overview/vb/icustomformatter1.vb#16)]

## <a name="related-topics"></a>Tópicos relacionados

|Título|Definição|
|-----------|----------------|
|[Cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md)|Descreve cadeias de caracteres de formato padrão que criam representações de cadeia de caracteres de valores numéricos frequentemente usadas.|
|[Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md)|Descreve cadeias de caracteres de formato personalizado que criam formatos específicos de aplicativo para valores numéricos.|
|[Cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md)|Descreve cadeias de caracteres de formato padrão que criam representações de cadeia de caracteres de valores <xref:System.DateTime> frequentemente usadas.|
|[Cadeias de caracteres de formato de data e hora personalizadas](custom-date-and-time-format-strings.md)|Descreve cadeias de caracteres de formato personalizado que criam formatos específicos de aplicativo para valores <xref:System.DateTime>.|
|[Cadeias de caracteres de formato standard TimeSpan](standard-timespan-format-strings.md)|Descreve cadeias de caracteres de formato padrão que criam representações de intervalos de tempo frequentemente usadas.|
|[Cadeias de caracteres de formato de TimeSpan personalizado](custom-timespan-format-strings.md)|Descreve cadeias de caracteres de formato personalizado que criam formatos específicos de aplicativo para intervalos de tempo.|
|[Cadeias de caracteres de formato de enumeração](enumeration-format-strings.md)|Descreve cadeias de caracteres de formato padrão que são usadas para criar representações de cadeia de caracteres de valores de enumeração.|
|[Formatação composta](composite-formatting.md)|Descreve como inserir um ou mais valores formatados em uma cadeia de caracteres. A cadeia de caracteres pode posteriormente ser exibida no console ou gravada em um fluxo.|
|[Analisando cadeias de caracteres](parsing-strings.md)|Descreve como inicializar objetos para os valores descritos pelas representações de cadeia de caracteres desses objetos. A análise é a operação inversa da formatação.|

## <a name="reference"></a>Referência

- <xref:System.IFormattable?displayProperty=nameWithType>
- <xref:System.IFormatProvider?displayProperty=nameWithType>
- <xref:System.ICustomFormatter?displayProperty=nameWithType>
