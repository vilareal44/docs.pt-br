---
title: Cadeias de caracteres de formato TimeSpan padrão
description: Examine as cadeias de caracteres de formato TimeSpan padrão, que usam um único especificador de formato para definir a representação de texto de um valor TimeSpan no .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- format specifiers, standard time interval
- format strings
- standard time interval format strings
- standard format strings, time intervals
- format specifiers, time intervals
- time intervals [.NET Framework], formatting
- time [.NET Framework], formatting
- formatting [.NET Framework], time
- standard TimeSpan format strings
- formatting [.NET Framework], time intervals
ms.assetid: 9f6c95eb-63ae-4dcc-9c32-f81985c75794
ms.openlocfilehash: 31e4158d42d794e830d9acfe666729846c43a1ee
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768112"
---
# <a name="standard-timespan-format-strings"></a>Cadeias de caracteres de formato TimeSpan padrão

 Uma cadeia de caracteres de formato padrão <xref:System.TimeSpan> usa um único especificador de formato para definir a representação de texto de um valor <xref:System.TimeSpan> que resulta de uma operação de formatação. Qualquer sequência de formato que contenha mais de um caractere, incluindo espaço em branco, é interpretada como uma sequência de formato <xref:System.TimeSpan> personalizada. Para obter mais informações, consulte [cadeias de caracteres de formato de TimeSpan personalizado](custom-timespan-format-strings.md) .  
  
 As representações de sequência de valores <xref:System.TimeSpan> são produzidas por chamadas para as sobrecargas do método <xref:System.TimeSpan.ToString%2A?displayProperty=nameWithType>, bem como por métodos que oferecem suporte à formatação composta, como <xref:System.String.Format%2A?displayProperty=nameWithType>. Para obter mais informações, consulte [Tipos de formatação](formatting-types.md) e [Formatação de composição](composite-formatting.md). O exemplo a seguir ilustra o uso de sequências de formato padrão em operações de formatação.  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/formatexample1.cs#2)]
 [!code-vb[Conceptual.TimeSpan.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/formatexample1.vb#2)]  
  
 As sequências de formato <xref:System.TimeSpan> padrão são usadas também pelos métodos <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> e <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> para definir o formato necessário de sequências de caracteres de entrada para análise de operações. (A análise converte a representação de cadeia de caracteres de um valor para esse valor.) O exemplo a seguir ilustra o uso de cadeias de caracteres de formato padrão em operações de análise.  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/parseexample1.cs#3)]
 [!code-vb[Conceptual.TimeSpan.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/parseexample1.vb#3)]  
  
A tabela a seguir lista os especificadores de formato de intervalo de tempo padrão.  
  
|Especificador de formato|Name|Description|Exemplos|  
|----------------------|----------|-----------------|--------------|  
|"c"|Formato de constante (invariável)|Esse especificador não é sensível à cultura. Ele assume o formato `[-][d'.']hh':'mm':'ss['.'fffffff]`.<br /><br /> (As sequências de formato "t" e "T" produzem os mesmos resultados).<br /><br /> Mais informações: [o especificador de formato de constante ("c")](#the-constant-c-format-specifier).|`TimeSpan.Zero` -> 00:00:00<br /><br /> `New TimeSpan(0, 0, 30, 0)` -> 00:30:00<br /><br /> `New TimeSpan(3, 17, 25, 30, 500)` -> 3.17:25:30.5000000|  
|"g"|Formato curto geral|Esse especificador gera apenas o que é necessário. Ele é sensível à cultura e assume o formato `[-][d':']h':'mm':'ss[.FFFFFFF]`.<br /><br /> Mais informações: [o especificador de formato curto geral ("g")](#the-general-short-g-format-specifier).|`New TimeSpan(1, 3, 16, 50, 500)` -> 1:3:16:50.5 (en-US)<br /><br /> `New TimeSpan(1, 3, 16, 50, 500)` -> 1:3:16:50,5 (fr-FR)<br /><br /> `New TimeSpan(1, 3, 16, 50, 599)` -> 1:3:16:50.599 (en-US)<br /><br /> `New TimeSpan(1, 3, 16, 50, 599)` -> 1:3:16:50,599 (fr-FR)|  
|"G"|Formato longo geral|Esse especificador sempre gera dias e sete dígitos de fração. Ele é sensível à cultura e assume o formato `[-]d':'hh':'mm':'ss.fffffff`.<br /><br /> Mais informações: [o especificador de formato longo geral ("G")](#the-general-long-g-format-specifier).|`New TimeSpan(18, 30, 0)` -> 0:18:30:00.0000000 (en-US)<br /><br /> `New TimeSpan(18, 30, 0)` -> 0:18:30:00,0000000 (fr-FR)|  

## <a name="the-constant-c-format-specifier"></a>O especificador de formato de constante ("c")  
 O especificador de formato "c" retorna a representação de sequência de um valor <xref:System.TimeSpan> da seguinte forma:  
  
 [-][*d*.]*hh*:*mm*:*ss*[.*fffffff*]  
  
 Os elementos entre colchetes ([ e ]) são opcionais. O ponto (.) e os dois pontos (:) são símbolos literais. A tabela a seguir descreve os elementos restantes.  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|*-*|Um sinal negativo opcional, que indica um intervalo de tempo negativo.|  
|*d*|O número opcional de dias, sem zeros à esquerda.|  
|*hh*|O número de horas, que varia de "00" a "23".|  
|*mm*|O número de minutos, que varia de "00" a "59".|  
|*II*|O número de segundos, que varia de "00" a "59".|  
|*fffffff*|A parte de fração opcional de um segundo.  Seu valor pode variar de "0000001" (um pulso ou um décimo milionésimo de segundo) até "9999999" (9.999.999 dez milionésimos de segundo ou um segundo menos um pulso).|  
  
 Ao contrário dos especificadores de formato de "g" e "G", o especificador de formato "c" não é sensível à cultura. Ele produz a representação de cadeia de caracteres de um valor <xref:System.TimeSpan> que é invariável e é comum a todas as versões anteriores do .NET Framework, antes do .NET Framework 4. "c"é a cadeia de caracteres de formato <xref:System.TimeSpan> padrão, o método <xref:System.TimeSpan.ToString?displayProperty=nameWithType> formata um valor de intervalo de tempo usando a sequência de formato "c".  
  
> [!NOTE]
> <xref:System.TimeSpan> também oferece suporte às sequências de formato padrão "t" e "T", que são idênticas em comportamento à sequência de formato padrão "c".  
  
 O exemplo a seguir instancia dois objetos <xref:System.TimeSpan>, usa-os para executar operações aritméticas e exibe o resultado. Em cada caso, ele usa formatação composta para exibir o valor <xref:System.TimeSpan> usando o especificador de formato "c".  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardc1.cs#1)]
 [!code-vb[Conceptual.TimeSpan.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardc1.vb#1)]  

## <a name="the-general-short-g-format-specifier"></a>O especificador de formato curto geral ("g")  
 O especificador de formato <xref:System.TimeSpan> de "g" retorna a representação de sequência de um valor <xref:System.TimeSpan> em um formato compacto, incluindo apenas os elementos que são necessários. Ele tem o seguinte formato:  
  
 [-][*d*:]*h*:*mm*:*ss*[.*FFFFFFF*]  
  
 Os elementos entre colchetes ([ e ]) são opcionais. Os dois pontos (:) são um símbolo literal. A tabela a seguir descreve os elementos restantes.  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|*-*|Um sinal negativo opcional, que indica um intervalo de tempo negativo.|  
|*d*|O número opcional de dias, sem zeros à esquerda.|  
|*h*|O número de horas, que varia de "0" a "23", sem zeros à esquerda.|  
|*mm*|O número de minutos, que varia de "00" a "59".|  
|*II*|O número de minutos, que varia de "00" a "59".|  
|*.*|O separador de fração de segundo. É equivalente à propriedade <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> da cultura especificada sem substituições pelo usuário.|  
|*FFFFFFF*|As frações de segundo. Uma vez que é exibido o mínimo de dígitos possível.|  
  
 Como o especificador de formato "G", o especificador de formato "g" é localizado. Seu separador de fração de segundo se baseia na cultura atual ou de uma propriedade <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> da cultura especificada.  
  
 O exemplo a seguir instancia dois objetos <xref:System.TimeSpan>, usa-os para executar operações aritméticas e exibe o resultado. Em cada caso, ele usa formatação composta para exibir o valor <xref:System.TimeSpan> usando o especificador de formato "g". Além disso, ele formata o valor <xref:System.TimeSpan> usando as convenções de formatação da cultura atual do sistema (que, nesse caso, é Inglês - Estados Unidos ou en-US) e da cultura francês - França (fr-FR).  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardshort1.cs#4)]
 [!code-vb[Conceptual.TimeSpan.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardshort1.vb#4)]  

## <a name="the-general-long-g-format-specifier"></a>O especificador de formato longo geral ("G")  
 O especificador de formato <xref:System.TimeSpan> de "G" retorna a representação de sequência de um valor <xref:System.TimeSpan> em um formato longo que sempre inclui dias e a fração de segundo. A sequência que resulta do especificador de formato padrão "G" tem o seguinte formato:  
  
 [-] *d*:*hh*:*mm*:*SS*. *fffffff*  
  
 Os elementos entre colchetes ([ e ]) são opcionais. Os dois pontos (:) são um símbolo literal. A tabela a seguir descreve os elementos restantes.  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|*-*|Um sinal negativo opcional, que indica um intervalo de tempo negativo.|  
|*d*|O número de dias, sem zeros à esquerda.|  
|*hh*|O número de horas, que varia de "00" a "23".|  
|*mm*|O número de minutos, que varia de "00" a "59".|  
|*II*|O número de segundos, que varia de "00" a "59".|  
|*.*|O separador de fração de segundo. É equivalente à propriedade <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> da cultura especificada sem substituições pelo usuário.|  
|*fffffff*|As frações de segundo.|  
  
 Como o especificador de formato "G", o especificador de formato "g" é localizado. Seu separador de fração de segundo se baseia na cultura atual ou de uma propriedade <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> da cultura especificada.  
  
 O exemplo a seguir instancia dois objetos <xref:System.TimeSpan>, usa-os para executar operações aritméticas e exibe o resultado. Em cada caso, ele usa formatação composta para exibir o valor <xref:System.TimeSpan> usando o especificador de formato "G". Além disso, ele formata o valor <xref:System.TimeSpan> usando as convenções de formatação da cultura atual do sistema (que, nesse caso, é Inglês - Estados Unidos ou en-US) e da cultura francês - França (fr-FR).  
  
 [!code-csharp[Conceptual.TimeSpan.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.standard/cs/standardlong1.cs#5)]
 [!code-vb[Conceptual.TimeSpan.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.standard/vb/standardlong1.vb#5)]
  
## <a name="see-also"></a>Veja também

- [Formatar tipos](formatting-types.md)
- [Cadeias de caracteres de formato de TimeSpan personalizado](custom-timespan-format-strings.md)
- [Analisando cadeias de caracteres](parsing-strings.md)
