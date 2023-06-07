---
title: Valores de Retorno para a Função CStr
ms.date: 07/20/2015
helpviewer_keywords:
- times [Visual Basic], CStr Function return values
- type conversion [Visual Basic]
- dates [Visual Basic], CStr Function return values
- CStr function
- strings [Visual Basic], return value
- Date data type [Visual Basic], converting
- dates [Visual Basic]
- String data type [Visual Basic], converting
ms.assetid: 3aa744e7-1419-45d5-85e3-e5abc2953673
ms.openlocfilehash: 6039ba3f6bd1c364db818807604ee0851bc23d50
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406368"
---
# <a name="return-values-for-the-cstr-function-visual-basic"></a>Retornar valores para a função CStr (Visual Basic)
A tabela a seguir descreve os valores de retorno para `CStr` para tipos de dados diferentes do `expression` .  
  
|Se o `expression` tipo for|Retornos de `CStr`|  
|-----------------------------|--------------------|  
|[Tipo de dados booliano](../data-types/boolean-data-type.md)|Uma cadeia de caracteres que contém "true" ou "false".|  
|[Tipo de Dados de Data](../data-types/date-data-type.md)|Uma cadeia de caracteres que contém um `Date` valor (data e hora) no formato de data abreviada do seu sistema.|  
|[Tipos de dados numéricos](../../programming-guide/language-features/data-types/numeric-data-types.md)|Uma cadeia de caracteres que representa o número.|  
  
## <a name="cstr-and-date"></a>CStr e Date  
 O `Date` tipo sempre contém informações de data e hora. Para fins de conversão de tipo, Visual Basic considera 1/1/0001 (1º de Janeiro do ano 1) como um *valor neutro* para a data e 00:00:00 (meia-noite) para ser um valor neutro para o tempo. `CStr`Não inclui valores neutros na cadeia de caracteres resultante. Por exemplo, se você converter `#January 1, 0001 9:30:00#` em uma cadeia de caracteres, o resultado será "9:30:00 AM"; as informações de data são suprimidas. No entanto, as informações de data ainda estão presentes no `Date` valor original e podem ser recuperadas com funções como <xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A> .  
  
> [!NOTE]
> A `CStr` função executa sua conversão com base nas configurações de cultura atuais para o aplicativo. Para obter a representação de cadeia de caracteres de um número em uma cultura específica, use o método do número `ToString(IFormatProvider)` . Por exemplo, use <xref:System.Double.ToString%2A?displayProperty=nameWithType> ao converter um valor do tipo `Double` para um `String` .  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A>
- [Funções de conversão do tipo](type-conversion-functions.md)
- [Tipo de dados booliano](../data-types/boolean-data-type.md)
- [Tipo de Dados de Data](../data-types/date-data-type.md)
