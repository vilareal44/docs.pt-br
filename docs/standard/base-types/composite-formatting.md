---
title: Formatação de composição
description: Saiba mais sobre a formatação composta do .NET, que usa como entrada uma lista de objetos e uma cadeia de caracteres de formato composto, contendo texto fixo com espaços reservados indexados.
ms.date: 10/26/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parameter specifiers
- strings [.NET Framework], alignment
- format specifiers, composite formatting
- strings [.NET Framework], composite
- composite formatting
- objects [.NET Framework], formatting multiple objects
ms.assetid: 87b7d528-73f6-43c6-b71a-f23043039a49
ms.openlocfilehash: 36197b382c449a2570e1d5530f307c4e66b0d983
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84447258"
---
# <a name="composite-formatting"></a>Formatação de composição

O recurso de formatação de composição do .NET utiliza uma lista de objetos e uma cadeia de caracteres de formato de composição como entrada. Uma cadeia de formato de composição consiste em um texto fixo intercalado com espaços reservados indexados, chamados de itens de formato, que correspondem aos objetos na lista. A operação de formatação produz uma cadeia de caracteres de resultado que consiste no texto fixo original intercalado com a representação de cadeia de caracteres dos objetos na lista.  
  
> [!IMPORTANT]
> Em vez de usar cadeias de caracteres de formato composto, você pode usar *cadeias de caracteres interpoladas* se o idioma e a versão de idioma que você está usando são compatíveis com elas. Uma cadeia de caracteres interpolada é uma cadeia de caracteres que contém *expressões interpoladas*. Cada expressão interpolada é resolvida com o valor da expressão e incluída na cadeia de caracteres resultante quando a cadeia de caracteres é atribuída. Para saber mais, confira [Interpolação de cadeia de caracteres (Referência de C#)](../../csharp/language-reference/tokens/interpolated.md) ou [Cadeias de caracteres interpoladas (Referência do Visual Basic)](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md).

O recurso de formatação de composição tem suporte de métodos como:  
  
- <xref:System.String.Format%2A?displayProperty=nameWithType>, que retorna uma cadeia de caracteres de resultado formatada.  
  
- <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>, que acrescenta uma cadeia de caracteres de resultado formatada a um objeto <xref:System.Text.StringBuilder>.
- Algumas sobrecargas do método <xref:System.Console.WriteLine%2A?displayProperty=nameWithType>, as quais exibem uma cadeia de caracteres de resultado formatada para o console.  
  
- Algumas sobrecargas do método <xref:System.IO.TextWriter.WriteLine%2A?displayProperty=nameWithType>, as quais escrevem a cadeia de caracteres de resultado formatada em um fluxo ou arquivo. As classes derivadas de <xref:System.IO.TextWriter>, como <xref:System.IO.StreamWriter> e <xref:System.Web.UI.HtmlTextWriter>, também têm essa funcionalidade.  
  
- <xref:System.Diagnostics.Debug.WriteLine%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>, que gera uma mensagem formatada para rastrear ouvintes.  
  
- Os métodos <xref:System.Diagnostics.Trace.TraceError%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>, <xref:System.Diagnostics.Trace.TraceInformation%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> e <xref:System.Diagnostics.Trace.TraceWarning%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>, os quais produzem mensagens formatadas rastrear ouvintes.  
  
- O método <xref:System.Diagnostics.TraceSource.TraceInformation%28System.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType>, que grava um método informativo para rastrear ouvintes.  
  
## <a name="composite-format-string"></a>Cadeia de formato de composição  
 Uma cadeia de formato de composição e uma lista de objetos são usadas como argumentos dos métodos que dão suporte ao recurso de formatação de composição. Uma cadeia de formato de composição consiste em zero ou mais sequências de texto fixo intercaladas com um ou mais itens de formato. O texto fixo é qualquer cadeia de caracteres que você escolher e cada item de formato corresponde a um objeto ou a uma estrutura demarcada na lista. O recurso de formatação de composição retorna uma nova cadeia de caracteres de resultado em que cada item de formato é substituído pela representação de cadeia de caracteres do objeto correspondente na lista.  
  
 Considere o fragmento de código <xref:System.String.Format%2A> a seguir.  
  
 [!code-csharp[Formatting.Composite#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#1)]
 [!code-vb[Formatting.Composite#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#1)]  
  
 O texto fixo é "`Name =` "e"`, hours =` ". Os itens de formato são "`{0}`", cujo índice é 0, que corresponde ao objeto `name`, e "`{1:hh}`", cujo índice é 1, que corresponde ao objeto `DateTime.Now`.  
  
## <a name="format-item-syntax"></a>Sintaxe do item de formato  
 Cada item de formato assume a forma a seguir e consiste nos seguintes componentes:  
  
 `{`*índice*[ `,` *alinhamento*] [ `:` *FormatString*]`}`  
  
 As chaves correspondentes ("{" e "}") são necessárias.  
  
### <a name="index-component"></a>Componente de índice  
 O componente obrigatório *index*, também chamado de especificador de parâmetro, é um número iniciado por 0 que identifica um item correspondente na lista de objetos. Ou seja, o item de formato cujo especificador de parâmetro é 0 formata o primeiro objeto na lista, o item de formato cujo especificador de parâmetro é 1 formata o segundo objeto na lista e assim por diante. O exemplo a seguir inclui quatro especificadores de parâmetros, numerados de zero a três, para representar números primos menores que dez:  
  
 [!code-csharp[Formatting.Composite#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/index1.cs#7)]
 [!code-vb[Formatting.Composite#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/index1.vb#7)]  
  
 Vários itens de formato podem fazer referência ao mesmo elemento na lista de objetos ao especificar o mesmo especificador de parâmetro. Por exemplo, você pode formatar o mesmo valor numérico em formato hexadecimal, científico e numérico especificando uma cadeia de caracteres de formato composto, como: "0x {0:X} {0:E} {0:N} ", como mostra o exemplo a seguir.  
  
 [!code-csharp[Formatting.Composite#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/index1.cs#10)]
 [!code-vb[Formatting.Composite#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/index1.vb#10)]  
  
 Cada item de formato pode fazer referência a qualquer objeto na lista. Por exemplo, se houver três objetos, você poderá formatar o segundo, o primeiro e o terceiro objeto especificando uma cadeia de caracteres de formato composto como esta: " {1} {0} {2} ". Um objeto que não é referenciado por um item de formato é ignorado. Um <xref:System.FormatException> é gerado em tempo de execução se um especificador de parâmetro designa um item fora dos limites da lista de objetos.  
  
### <a name="alignment-component"></a>Componente de alinhamento  
 O componente opcional *alignment* é um inteiro com sinal que indica a largura preferencial do campo formatado. Se o valor de *alignment* for menor que o comprimento da cadeia de caracteres formatada, *alignment* será ignorado e o comprimento da cadeia de caracteres formatada será usado como a largura do campo. Os dados formatados no campo serão alinhados à direita se *alignment* for positivo e serão alinhados à esquerda se *alignment* for negativo. Se for necessário preenchimento, espaços em branco serão usados. A vírgula é necessária se *alignment* for especificado.  
  
 O exemplo a seguir define duas matrizes, uma contendo os nomes dos funcionários e outra contendo as horas trabalhadas por eles em um período de duas semanas. A cadeia de caracteres de formato de composição alinha à esquerda os nomes em um campo de 20 caracteres e alinha à direita as horas em um campo de 5 caracteres. Observe que a cadeia de caracteres de formato padrão "N1" também é usada para formatar horas com um dígito fracionário.  
  
 [!code-csharp[Formatting.Composite#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/alignment1.cs#8)]
 [!code-vb[Formatting.Composite#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/alignment1.vb#8)]  
  
### <a name="format-string-component"></a>Componente da cadeia de caracteres de formato  
 O componente opcional *formatString* é uma cadeia de caracteres de formato que é apropriada para o tipo de objeto que está sendo formatado. Especifique uma cadeia de caracteres de formato numérico padrão ou personalizado se o objeto correspondente é um valor numérico, uma cadeia de caracteres de formato de data e hora padrão ou personalizado, se o objeto é um objeto <xref:System.DateTime> ou uma [cadeia de caracteres de formato de enumeração](enumeration-format-strings.md) se o objeto correspondente é um valor de enumeração. Se *formatString* não for especificado, o especificador de formato geral ("G") para um tipo numérico, de data e hora ou enumeração será usado. Os dois-pontos são necessários quando *formatString* é especificado.  
  
 A tabela a seguir lista tipos ou categorias de tipos na biblioteca de classes do .NET Framework que dão suporte a um conjunto predefinido de cadeias de caracteres de formato e fornece links para tópicos que relacionam as cadeias de caracteres de formato com suporte. Observe que a formatação de cadeias de caracteres é um mecanismo extensível que possibilita definir novas cadeias de caracteres de formato para todos os tipos existentes, bem como definir um conjunto de cadeias de caracteres de formato com suporte por um tipo definido por aplicativo. Para saber mais, veja os tópicos de interface <xref:System.IFormattable> e <xref:System.ICustomFormatter>.  
  
|Tipo ou categoria de tipo|Consulte|  
|---------------------------|---------|  
|Tipos de data e hora (<xref:System.DateTime>, <xref:System.DateTimeOffset>)|[Cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md)<br /><br /> [Cadeias de caracteres de formato de data e hora personalizadas](custom-date-and-time-format-strings.md)|  
|Tipos de enumeração (todos os tipos derivados de <xref:System.Enum?displayProperty=nameWithType>)|[Cadeias de caracteres de formato de enumeração](enumeration-format-strings.md)|  
|Tipos numéricos (<xref:System.Numerics.BigInteger>, <xref:System.Byte>, <xref:System.Decimal>, <xref:System.Double>, <xref:System.Int16>, <xref:System.Int32>, <xref:System.Int64>, <xref:System.SByte>, <xref:System.Single>, <xref:System.UInt16>, <xref:System.UInt32>, <xref:System.UInt64>)|[Cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md)<br /><br /> [Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md)|  
|<xref:System.Guid>|<xref:System.Guid.ToString%28System.String%29?displayProperty=nameWithType>|  
|<xref:System.TimeSpan>|[Cadeias de caracteres de formato standard TimeSpan](standard-timespan-format-strings.md)<br /><br /> [Cadeias de caracteres de formato de TimeSpan personalizado](custom-timespan-format-strings.md)|  
  
### <a name="escaping-braces"></a>Chaves de escape  
 As chaves de abertura e fechamento são interpretadas como o início e o fim de um item de formato. Consequentemente, você deve usar uma sequência de escape para exibir uma chave de abertura ou fechamento literal. Especifique duas chaves de abertura ("{{") no texto fixo para exibir uma chave de abertura ("{"), ou duas chaves de fechamento ("}}") para exibir uma chave de fechamento ("}"). As chaves em um item de formato são interpretadas sequencialmente na ordem em que são encontradas. Não há suporte para interpretação de chaves aninhadas.  
  
 A forma como as chaves de escape são interpretadas pode levar a resultados inesperados. Por exemplo, considere o item de formato "{{{0:D}}}", que deve exibir uma chave de abertura, um valor numérico formatado como número decimal e uma chave de fechamento. No entanto, o item de formato na verdade é interpretado da seguinte forma:  
  
1. As duas primeiras chaves de abertura ("{{") são escapadas e produzem uma chave de abertura.  
  
2. Os três caracteres seguintes ("{0:") são interpretados como o início de um item de formato.  
  
3. O caractere seguinte ("D") seria interpretado como o especificador de formato numérico Decimal padrão, mas as duas chaves de escape seguintes ("}} ") produzem uma única chave. Como a cadeia de caracteres resultante ("D}") não é um especificador de formato numérico padrão, ela é interpretada como uma cadeia de formato personalizado que visa exibir a cadeia de caracteres literal "D}".  
  
4. A última chave ("}") é interpretada como o final do item de formato.  
  
5. O resultado final que é exibido é a cadeia de caracteres literal, "{D}". O valor numérico que deveria ser formatado não é exibido.  
  
 Uma maneira de escrever seu código para evitar a interpretação incorreta de chaves de escape e itens de formato é formatar as chaves e os itens de formato separadamente. Ou seja, na primeira operação de formato exibir uma chave de abertura literal, na operação seguinte exibir o resultado do item de formato e, na operação final, exibir uma chave de fechamento literal. O exemplo a seguir ilustra esta abordagem.  
  
 [!code-csharp[Formatting.Composite#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Escaping1.cs#2)]
 [!code-vb[Formatting.Composite#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Escaping1.vb#2)]  
  
### <a name="processing-order"></a>Ordem de processamento  
 Se a chamada ao método de formatação composto inclui um argumento <xref:System.IFormatProvider> cujo valor não é `null`, o runtime chama seu método <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> para solicitar uma implementação de <xref:System.ICustomFormatter>. Se o método conseguir retornar uma implementação de <xref:System.ICustomFormatter>, ela será armazenada em cache durante a chamada do método de formatação composta.
  
 Cada valor na lista de parâmetros que corresponde a um item de formato é convertido em uma cadeia de caracteres da seguinte maneira:  
  
1. Se o valor a ser formatado for `null`, uma cadeia de caracteres vazia <xref:System.String.Empty?displayProperty=nameWithType> será retornada.  
  
2. Se uma implementação de <xref:System.ICustomFormatter> estiver disponível, o runtime chamará seu método <xref:System.ICustomFormatter.Format%2A>. Ele passará ao método o valor *formatString* do item de formato, se houver ou `null` se não houver, junto com a implementação de <xref:System.IFormatProvider>. Se a chamada ao método <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> retorna `null`, a execução continua para a próxima etapa; caso contrário, o resultado da chamada <xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> é retornado.
  
3. Se o valor implementa a interface <xref:System.IFormattable>, o método <xref:System.IFormattable.ToString%28System.String%2CSystem.IFormatProvider%29> da interface é chamado. O método receberá o valor *formatString* se houver um no item de formato, ou `null` se não houver. O argumento <xref:System.IFormatProvider> é determinado da seguinte forma:  
  
    - Para um valor numérico, se um método de formatação composto com um argumento <xref:System.IFormatProvider> não nulo é chamado, o runtime solicita um objeto <xref:System.Globalization.NumberFormatInfo> do seu método <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType>. Se ele não conseguir fornecer um, se o valor do argumento for `null` ou se o método de formatação composta não tiver um parâmetro <xref:System.IFormatProvider>, o objeto <xref:System.Globalization.NumberFormatInfo> para a cultura do thread atual será usado.  
  
    - Para um valor de data e hora, se um método de formatação composto com um argumento <xref:System.IFormatProvider> não nulo é chamado, o runtime solicita um objeto <xref:System.Globalization.DateTimeFormatInfo> do seu método <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType>. Se ele não conseguir fornecer um, se o valor do argumento for `null` ou se o método de formatação composta não tiver um parâmetro <xref:System.IFormatProvider>, o objeto <xref:System.Globalization.DateTimeFormatInfo> para a cultura do thread atual será usado.  
  
    - Para objetos de outros tipos, se um método de formatação de composição for chamado com um argumento <xref:System.IFormatProvider>, seu valor é passado diretamente para a implementação <xref:System.IFormattable.ToString%2A?displayProperty=nameWithType>. Caso contrário, `null` é passado para a implementação <xref:System.IFormattable.ToString%2A?displayProperty=nameWithType>.  
  
4. O método sem parâmetros `ToString` do tipo, o qual substitui <xref:System.Object.ToString?displayProperty=nameWithType> ou herda o comportamento da sua classe base, é chamado. Nesse caso, a cadeia de caracteres de formato especificada pelo componente *formatString* no item de formato, se houver, será ignorada.  
  
 O alinhamento é aplicado após as etapas anteriores terem sido executadas.  
  
## <a name="code-examples"></a>Exemplos de código  
 O exemplo a seguir mostra uma cadeia de caracteres criada usando formatação de composição e outra criada usando o método `ToString` de um objeto. Os dois tipos de formatação produzem resultados equivalentes.  
  
 [!code-csharp[Formatting.Composite#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#3)]
 [!code-vb[Formatting.Composite#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#3)]  
  
 Supondo que o dia atual seja uma quinta-feira de maio, o valor de ambas as cadeias de caracteres no exemplo anterior é `Thursday May` na cultura do inglês dos EUA.  
  
 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> expõe a mesma funcionalidade que <xref:System.String.Format%2A?displayProperty=nameWithType>. A única diferença entre os dois métodos é que <xref:System.String.Format%2A?displayProperty=nameWithType> retorna o resultado como uma cadeia de caracteres, enquanto que <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> grava o resultado no fluxo de saída associado ao objeto <xref:System.Console>. O exemplo a seguir usa o método <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> para formatar o valor de `MyInt` como um valor de moeda.  
  
 [!code-csharp[Formatting.Composite#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#4)]
 [!code-vb[Formatting.Composite#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#4)]  
  
 O exemplo a seguir demonstra a formatação de vários objetos, incluindo a formatação de um objeto de duas maneiras diferentes.  
  
 [!code-csharp[Formatting.Composite#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#5)]
 [!code-vb[Formatting.Composite#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#5)]  
  
 O exemplo a seguir demonstra o uso de alinhamento na formatação. Os argumentos formatados são colocados entre caracteres de barra vertical (&#124;) para realçar o alinhamento resultante.  
  
 [!code-csharp[Formatting.Composite#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#6)]
 [!code-vb[Formatting.Composite#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#6)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Console.WriteLine%2A>
- <xref:System.String.Format%2A?displayProperty=nameWithType>
- [Interpolação de cadeia de caracteres (C#)](../../csharp/language-reference/tokens/interpolated.md)
- [Interpolação de cadeia de caracteres (Visual Basic)](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md)
- [Formatar tipos](formatting-types.md)
- [Cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md)
- [Cadeias de caracteres de formato numérico personalizado](custom-numeric-format-strings.md)
- [Cadeias de caracteres de formato de data e hora padrão](standard-date-and-time-format-strings.md)
- [Cadeias de caracteres de formato de data e hora personalizadas](custom-date-and-time-format-strings.md)
- [Cadeias de caracteres de formato standard TimeSpan](standard-timespan-format-strings.md)
- [Cadeias de caracteres de formato de TimeSpan personalizado](custom-timespan-format-strings.md)
- [Cadeias de caracteres de formato de enumeração](enumeration-format-strings.md)
