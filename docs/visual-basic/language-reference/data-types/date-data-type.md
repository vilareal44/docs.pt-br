---
title: Tipo de Dados de Data
ms.date: 07/20/2015
f1_keywords:
- vb.Date
helpviewer_keywords:
- Date data type
- dates [Visual Basic], Visual Basic data types
- times [Visual Basic], Date data type
- Date literals [Visual Basic]
- data values [Visual Basic]
- times [Visual Basic], Visual Basic data types
- dates [Visual Basic], Date data type
- data types [Visual Basic], assigning
- literals [Visual Basic], Date
- '# specifier for Date literals'
ms.assetid: d9edf5b0-e85e-438b-a1cf-1f321e7c831b
ms.openlocfilehash: 46c25e14db56d4cc3c6d59ec7649b37c35676e2e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387420"
---
# <a name="date-data-type-visual-basic"></a>Tipo de dados Data (Visual Basic)

Mantém os valores do IEEE 64 bits (8 bytes) que representam datas desde 1º de Janeiro do ano de 0001 até 31 de dezembro do ano 9999 e horas da 12:00:00 AM (meia-noite) até 11:59:59.9999999 PM. Cada incremento representa 100 nanossegundos de tempo decorrido desde o início de 1º de Janeiro do ano 1 no calendário gregoriano. O valor máximo representa 100 nanossegundos antes do início de 1º de Janeiro do ano 10000.

## <a name="remarks"></a>Comentários

Use o `Date` tipo de dados para conter valores de data, valores de tempo ou valores de data e hora.

O valor padrão de `Date` é 0:00:00 (meia-noite) em 1º de janeiro de 0001.

Você pode obter a data e a hora atuais da <xref:Microsoft.VisualBasic.DateAndTime> classe.

## <a name="format-requirements"></a>Requisitos de formato

Você deve colocar um `Date` literal dentro de sinais numéricos ( `# #` ). Você deve especificar o valor de data no formato M/d/AAAA, por exemplo `#5/31/1993#` , ou aaaa-mm-dd, por exemplo `#1993-5-31#` . Você pode usar barras ao especificar o ano primeiro.  Esse requisito é independente da sua localidade e das configurações de data e formato de hora do computador.

O motivo para essa restrição é que o significado do seu código nunca deve ser alterado dependendo da localidade em que seu aplicativo está sendo executado. Suponha que você codifique um `Date` literal de `#3/4/1998#` e pretenda que ele signifique 4 de março de 1998. Em uma localidade que usa mm/dd/aaaa, o 3/4/1998 é compilado como pretendido. Mas suponha que você implante seu aplicativo em muitos países/regiões. Em uma localidade que usa dd/mm/aaaa, seu literal embutido em código seria compilado em 3 de abril de 1998. Em uma localidade que usa aaaa/mm/dd, o literal seria inválido (abril de 1998, 0003) e causa um erro do compilador.

## <a name="workarounds"></a>Soluções Alternativas

Para converter um `Date` literal para o formato de sua localidade, ou para um formato personalizado, forneça o literal para a <xref:Microsoft.VisualBasic.Strings.Format%2A> função, especificando um formato de data predefinido ou definido pelo usuário. O exemplo a seguir demonstra isso.

```vb
MsgBox("The formatted date is " & Format(#5/31/1993#, "dddd, d MMM yyyy"))
```

Como alternativa, você pode usar um dos construtores sobrecarregados da <xref:System.DateTime> estrutura para montar um valor de data e hora. O exemplo a seguir cria um valor para representar 31 de maio de 1993 às 12:14 na tarde.

```vb
Dim dateInMay As New System.DateTime(1993, 5, 31, 12, 14, 0)
```

## <a name="hour-format"></a>Formato de hora

Você pode especificar o valor de hora em formato de 12 horas ou 24 horas, por exemplo, `#1:15:30 PM#` ou `#13:15:30#` . No entanto, se você não especificar os minutos ou os segundos, deverá especificar AM ou PM.

## <a name="date-and-time-defaults"></a>Padrões de data e hora

Se você não incluir uma data em um literal de data/hora, Visual Basic definirá a parte de data do valor como 1º de janeiro de 0001. Se você não incluir uma hora em um literal de data/hora, Visual Basic definirá a parte de hora do valor como o início do dia, ou seja, meia-noite (0:00:00).

## <a name="type-conversions"></a>Conversões de tipo

Se você converter um `Date` valor para o `String` tipo, o Visual Basic renderiza a data de acordo com o formato de data abreviada especificado pela localidade de tempo de execução e renderiza a hora de acordo com o formato de hora (12 horas ou 24 horas) especificado pela localidade de tempo de execução.

## <a name="programming-tips"></a>Dicas de programação

- **Considerações sobre interoperabilidade.** Se você estiver fazendo a interface com componentes não escritos para o .NET Framework, por exemplo, automação ou objetos COM, tenha em mente que os tipos de data/hora em outros ambientes não são compatíveis com o tipo de Visual Basic `Date` . Se você estiver passando um argumento de data/hora para esse componente, declare- `Double` o como em vez de `Date` em seu novo código de Visual Basic e use os métodos de conversão <xref:System.DateTime.FromOADate%2A?displayProperty=nameWithType> e <xref:System.DateTime.ToOADate%2A?displayProperty=nameWithType> .

- **Digite os caracteres.** `Date`Não tem caractere de tipo literal ou caractere de tipo de identificador. No entanto, o compilador trata literais delimitados entre sinais numéricos ( `# #` ) como `Date` .

- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.DateTime?displayProperty=nameWithType>.

## <a name="example"></a>Exemplo

Uma variável ou constante do `Date` tipo de dados contém a data e a hora. O exemplo a seguir ilustra isto.

```vb
Dim someDateAndTime As Date = #8/13/2002 12:14 PM#
```

## <a name="see-also"></a>Confira também

- <xref:System.DateTime?displayProperty=nameWithType>
- [Tipos de dados](index.md)
- [Cadeias de caracteres de formato de data e hora padrão](../../../standard/base-types/standard-date-and-time-format-strings.md)
- [Cadeias de caracteres de formato de data e hora personalizadas](../../../standard/base-types/custom-date-and-time-format-strings.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
