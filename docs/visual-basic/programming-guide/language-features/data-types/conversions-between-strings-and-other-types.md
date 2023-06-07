---
title: Conversões entre cadeias de caracteres e outros tipos
ms.date: 07/20/2015
helpviewer_keywords:
- data type conversion [Visual Basic], string
- conversions [Visual Basic], type
- string conversion [Visual Basic]
- conversions [Visual Basic], data type
- type conversion [Visual Basic], string
- regional options
ms.assetid: c3a99596-f09a-44a5-81dd-1b89a094f1df
ms.openlocfilehash: ae8f7c2159191536013fafd8bfd10fb9a93fb785
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84394215"
---
# <a name="conversions-between-strings-and-other-types-visual-basic"></a>Conversões entre cadeias de caracteres e outros tipos (Visual Basic)
Você pode converter um valor numérico, `Boolean` ou de data/hora em um `String` . Você também pode converter na direção inversa — de um valor de cadeia de caracteres para Numeric, `Boolean` ou `Date` – desde que o conteúdo da cadeia de caracteres possa ser interpretado como um valor válido do tipo de dados de destino. Se não puderem, ocorrerá um erro em tempo de execução.  
  
 As conversões para todas essas atribuições, em qualquer direção, são conversões redutoras. Você deve usar as palavras-chave de conversão de tipo ( `CBool` , `CByte` , `CDate` ,,,, `CDbl` `CDec` `CInt` `CLng` , `CSByte` , `CShort` ,,,,, `CSng` `CStr` `CUInt` `CULng` `CUShort` e `CType` ). As <xref:Microsoft.VisualBasic.Strings.Format%2A> <xref:Microsoft.VisualBasic.Conversion.Val%2A> funções e oferecem controle adicional sobre conversões entre cadeias de caracteres e números.  
  
 Se você tiver definido uma classe ou estrutura, poderá definir operadores de conversão de tipo entre `String` e o tipo de sua classe ou estrutura. Para obter mais informações, consulte [como: definir um operador de conversão](../procedures/how-to-define-a-conversion-operator.md).  
  
## <a name="conversion-of-numbers-to-strings"></a>Conversão de números em cadeias de caracteres  
 Você pode usar a `Format` função para converter um número em uma cadeia de caracteres formatada, que pode incluir não apenas os dígitos apropriados, mas também os símbolos de formatação, como um símbolo de moeda (como `$` ), separadores de milhares ou *símbolos de agrupamento de dígitos* (como `,` ) e um separador decimal (como `.` ). `Format`o usa automaticamente os símbolos apropriados de acordo com as configurações de **Opções regionais** especificadas no **painel de controle**do Windows.  
  
 Observe que o operador concatenation ( `&` ) pode converter um número em uma cadeia de caracteres implicitamente, como mostra o exemplo a seguir.  
  
```vb  
' The following statement converts count to a String value.  
Str = "The total count is " & count  
```  
  
## <a name="conversion-of-strings-to-numbers"></a>Conversão de cadeias de caracteres em números  
 Você pode usar a `Val` função para converter explicitamente os dígitos em uma cadeia de caracteres em um número. `Val`lê a cadeia de caracteres até encontrar um caractere que não seja um dígito, espaço, tabulação, alimentação de linha ou ponto. As sequências "&O" e "&H" alteram a base do sistema numérico e encerram a verificação. Até que a leitura seja interrompida, `Val` o converte todos os caracteres apropriados em um valor numérico. Por exemplo, a instrução a seguir retorna o valor `141.825` .  
  
 `Val("   14   1.825 miles")`  
  
 Quando Visual Basic converte uma cadeia de caracteres em um valor numérico, ele usa as configurações de **Opções regionais** especificadas no **painel de controle** do Windows para interpretar o separador de milhar, o separador decimal e o símbolo de moeda. Isso significa que uma conversão pode ter sucesso em uma configuração, mas não em outra. Por exemplo, `"$14.20"` é aceitável na localidade inglês (Estados Unidos), mas não em qualquer localidade em francês.  
  
## <a name="see-also"></a>Confira também

- [Conversões de tipo no Visual Basic](type-conversions.md)
- [Conversões de Widening e Narrowing](widening-and-narrowing-conversions.md)
- [Conversões implícitas e explícitas](implicit-and-explicit-conversions.md)
- [Como converter um objeto em outro tipo no Visual Basic](how-to-convert-an-object-to-another-type.md)
- [Conversões de matriz](array-conversions.md)
- [Tipos de dados](../../../language-reference/data-types/index.md)
- [Funções de conversão do tipo](../../../language-reference/functions/type-conversion-functions.md)
- [Desenvolver aplicativos localizados e globalizados](/visualstudio/ide/globalizing-and-localizing-applications)
