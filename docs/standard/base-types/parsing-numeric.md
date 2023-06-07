---
title: Análise de cadeias de caracteres numéricas no .NET
description: Saiba mais sobre a análise de cadeias de caracteres numéricas no .NET. Saiba como analisar com provedores de formato, valores de enumeração NumberStyles e dígitos Unicode.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parsing strings, numeric strings
- numeric strings
- enumerations [.NET Framework], parsing strings
- base types, parsing strings
ms.assetid: e39324ee-72e5-42d4-a80d-bf3ee7fc6c59
ms.openlocfilehash: b184bad10b816c1eae798302337b5c901732ad7f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589533"
---
# <a name="parsing-numeric-strings-in-net"></a>Análise de cadeias de caracteres numéricas no .NET
Todos os tipos numéricos têm dois métodos de análise estáticos, `Parse` e `TryParse`, que podem ser usados para converter a representação de cadeia de caracteres de um número em um tipo numérico. Esses métodos permitem analisar cadeias de caracteres que foram produzidas usando as cadeias de caracteres de formato documentadas em [Cadeias de caracteres de formato numérico padrão](standard-numeric-format-strings.md) e [Cadeias de caracteres de formato numérico personalizadas](custom-numeric-format-strings.md). Por padrão, os métodos `Parse` e `TryParse` conseguem converter cadeias de caracteres que contêm dígitos decimais integrais somente em valores inteiros. Podem converter cadeias de caracteres que contêm dígitos decimais integrais e dígitos fracionários, separadores de grupo e um separador decimal em valores de ponto flutuante. O método `Parse` lança uma exceção se a operação falhar, enquanto o método `TryParse` retorna `false`.  
  
## <a name="parsing-and-format-providers"></a>Provedores de análise e formato  
 Normalmente, as representações de cadeia de caracteres de valores numéricos diferem por cultura. Elementos de separadores de cadeias de caracteres numéricas, como símbolos de moeda, separadores de grupo (ou milhares) e separadores decimais, variam por cultura. Os métodos de análise usam, implícita ou explicitamente, um provedor de formato que reconhece essas variações específicas da cultura. Se nenhum provedor de formato for especificado em uma chamada ao método `Parse` ou `TryParse`, será usado o provedor de formato associado à cultura do thread atual (o objeto <xref:System.Globalization.NumberFormatInfo> retornado pela propriedade <xref:System.Globalization.NumberFormatInfo.CurrentInfo%2A?displayProperty=nameWithType>).  
  
 Um provedor de formato é representado por uma implementação <xref:System.IFormatProvider>. Essa interface tem um único membro, o método <xref:System.IFormatProvider.GetFormat%2A>, cujo único parâmetro é um objeto <xref:System.Type> que representa o tipo a ser formatado. Esse método retorna um objeto que fornece informações de formatação. O .NET dá suporte às duas implementações <xref:System.IFormatProvider> a seguir para analisar cadeias de caracteres numéricas:  
  
- Um objeto <xref:System.Globalization.CultureInfo> cujo método <xref:System.Globalization.CultureInfo.GetFormat%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Globalization.NumberFormatInfo> que fornece informações de formatação específicas da cultura.  
  
- Um objeto <xref:System.Globalization.NumberFormatInfo> cujo método <xref:System.Globalization.NumberFormatInfo.GetFormat%2A?displayProperty=nameWithType> retorna a si mesmo.  
  
 O exemplo a seguir tenta converter cada cadeia de caracteres em uma matriz em um valor <xref:System.Double>. Em primeiro lugar, tenta analisar a cadeia de caracteres usando um provedor de formato que reflete as convenções da cultura do inglês (Estados Unidos). Se essa operação lançar um <xref:System.FormatException>, tentará analisar a cadeia de caracteres usando um provedor de formato que reflita as convenções da cultura de francês (França).  
  
 [!code-csharp[Parsing.Numbers#1](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/formatproviders1.cs#1)]
 [!code-vb[Parsing.Numbers#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/formatproviders1.vb#1)]  
  
## <a name="parsing-and-numberstyles-values"></a>Análise e valores NumberStyles  
 Os elementos de estilo (como espaço em branco, separadores de grupo e separador decimal) que a operação de análise pode manipular são definidos por um valor de enumeração <xref:System.Globalization.NumberStyles>. Por padrão, as cadeias de caracteres que representam valores inteiros são analisadas usando o valor <xref:System.Globalization.NumberStyles.Integer?displayProperty=nameWithType>, que permite apenas dígitos numéricos, espaço em branco à esquerda e à direita e um sinal à esquerda. Cadeias de caracteres que representam valores de ponto flutuante são analisadas usando uma combinação dos valores <xref:System.Globalization.NumberStyles.Float?displayProperty=nameWithType> e <xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType>. Esse estilo de composição permite dígitos decimais junto com o espaço em branco à esquerda e à direita, um sinal à esquerda, um separador decimal, um separador de grupo e um expoente. Chamando uma sobrecarga do método `Parse` ou `TryParse` que inclui um parâmetro do tipo <xref:System.Globalization.NumberStyles> e definindo um ou mais sinalizadores <xref:System.Globalization.NumberStyles>, é possível controlar os elementos de estilo que podem estar presentes na cadeia de caracteres para que a operação de análise seja bem-sucedida.  
  
 Por exemplo, uma cadeia de caracteres que contém um separador de grupo não pode ser convertida em um valor <xref:System.Int32> usando o método <xref:System.Int32.Parse%28System.String%29?displayProperty=nameWithType>. No entanto, a conversão será bem-sucedida se você usar o sinalizador <xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType>, como mostra o exemplo a seguir.  
  
 [!code-csharp[Parsing.Numbers#2](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/styles1.cs#2)]
 [!code-vb[Parsing.Numbers#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/styles1.vb#2)]  
  
> [!WARNING]
> A operação de análise sempre usa as convenções de formatação de uma determinada cultura. Se você não especificar uma cultura passando um objeto <xref:System.Globalization.CultureInfo> ou <xref:System.Globalization.NumberFormatInfo>, a cultura associada ao thread atual será usada.  
  
 A tabela a seguir lista os membros da enumeração <xref:System.Globalization.NumberStyles> e descreve o efeito que eles têm sobre a operação de análise.  
  
|Valor NumberStyles|Efeito sobre a cadeia de caracteres a ser analisada|  
|------------------------|---------------------------------------|  
|<xref:System.Globalization.NumberStyles.None?displayProperty=nameWithType>|São permitidos apenas dígitos numéricos.|  
|<xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType>|O separador decimal e dígitos fracionários são permitidos. Para valores inteiros, apenas zero é permitido como um dígito fracionário. Os separadores decimais válidos são determinados pela propriedade <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A?displayProperty=nameWithType> ou <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A?displayProperty=nameWithType>.|  
|<xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType>|O caractere “e” ou “E” pode ser usado para indicar notação exponencial. Para saber mais, confira <xref:System.Globalization.NumberStyles>.|  
|<xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>|É permitido um espaço em branco à esquerda.|  
|<xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>|É permitido um espaço em branco à direita.|  
|<xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>|Um sinal positivo ou negativo pode anteceder dígitos numéricos.|  
|<xref:System.Globalization.NumberStyles.AllowTrailingSign?displayProperty=nameWithType>|Um sinal positivo ou negativo pode seguir dígitos numéricos.|  
|<xref:System.Globalization.NumberStyles.AllowParentheses?displayProperty=nameWithType>|Parênteses podem ser usados para indicar valores negativos.|  
|<xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType>|É permitido um separador de grupo. O caractere separador de grupo é determinado pela propriedade <xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A?displayProperty=nameWithType> ou <xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A?displayProperty=nameWithType>.|  
|<xref:System.Globalization.NumberStyles.AllowCurrencySymbol?displayProperty=nameWithType>|É permitido o símbolo de moeda. O símbolo de moeda é definido pela propriedade <xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A?displayProperty=nameWithType>.|  
|<xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType>|A cadeia de caracteres a ser analisada é interpretada como um número hexadecimal. Pode incluir os dígitos decimais 0-9, A-F e a-f. Este sinalizador pode ser usado apenas para analisar valores inteiros.|  
  
 Além disso, a enumeração <xref:System.Globalization.NumberStyles> oferece os estilos de composição a seguir, que incluem vários sinalizadores <xref:System.Globalization.NumberStyles>.  
  
|Valor NumberStyles de composição|Inclui membros|  
|----------------------------------|----------------------|  
|<xref:System.Globalization.NumberStyles.Integer?displayProperty=nameWithType>|Inclui os estilos <xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType> e <xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>. É o estilo padrão usado para analisar valores inteiros.|  
|<xref:System.Globalization.NumberStyles.Number?displayProperty=nameWithType>|Inclui os estilos <xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowTrailingSign?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType> e <xref:System.Globalization.NumberStyles.AllowThousands?displayProperty=nameWithType>.|  
|<xref:System.Globalization.NumberStyles.Float?displayProperty=nameWithType>|Inclui os estilos <xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowLeadingSign?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowDecimalPoint?displayProperty=nameWithType> e <xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType>.|  
|<xref:System.Globalization.NumberStyles.Currency?displayProperty=nameWithType>|Inclui todos os estilos, exceto <xref:System.Globalization.NumberStyles.AllowExponent?displayProperty=nameWithType> e <xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType>.|  
|<xref:System.Globalization.NumberStyles.Any?displayProperty=nameWithType>|Inclui todos os estilos, exceto <xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType>.|  
|<xref:System.Globalization.NumberStyles.HexNumber?displayProperty=nameWithType>|Inclui os estilos <xref:System.Globalization.NumberStyles.AllowLeadingWhite?displayProperty=nameWithType>, <xref:System.Globalization.NumberStyles.AllowTrailingWhite?displayProperty=nameWithType> e <xref:System.Globalization.NumberStyles.AllowHexSpecifier?displayProperty=nameWithType>.|  
  
## <a name="parsing-and-unicode-digits"></a>Análise e Dígitos Unicode  
 O padrão Unicode define pontos de código para dígitos em diversos sistemas de escrita. Por exemplo, os pontos de código de U+0030 a U+0039 representam os dígitos latinos básicos 0 a 9; os pontos de código de U+09E6 a U+09EF representam os dígitos bengali de 0 a 9; e os pontos de código de U+FF10 a U+FF19 representam os dígitos de largura inteira 0 a 9. No entanto, os únicos dígitos numéricos reconhecidos pelos métodos de análise são os dígitos latinos básicos 0-9 com os pontos de código de U+0030 a U+0039. Se um método de análise numérica passar uma cadeia de caracteres que contenha outros dígitos, o método lançará um <xref:System.FormatException>.  
  
 O exemplo a seguir usa o método <xref:System.Int32.Parse%2A?displayProperty=nameWithType> para analisar cadeias de caracteres que consistem em dígitos em diferentes sistemas de escrita. Como mostra a saída do exemplo, a tentativa de analisar os dígitos latinos básicos é bem-sucedida, mas a tentativa de analisar os dígitos de largura inteira, indo-arábicos e bengali.  
  
 [!code-csharp[Parsing.Numbers#3](../../../samples/snippets/csharp/VS_Snippets_CLR/parsing.numbers/cs/unicode1.cs#3)]
 [!code-vb[Parsing.Numbers#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/parsing.numbers/vb/unicode1.vb#3)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Globalization.NumberStyles>
- [Analisando cadeias de caracteres](parsing-strings.md)
- [Formatar tipos](formatting-types.md)
