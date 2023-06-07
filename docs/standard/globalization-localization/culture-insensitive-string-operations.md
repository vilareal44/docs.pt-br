---
title: Operações de cadeia de caracteres que não levam em conta a cultura
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- culture, culture-insensitive string operations
- case-sensitive comparisons
- globalization [.NET Framework], culture-insensitive string operations
- strings [.NET Framework], culture-insensitive string operations
- localization [.NET Framework], culture-insensitive string operations
- world-ready applications, culture-insensitive string operations
- culture-sensitive string operations
- culture-insensitive string operations
ms.assetid: e6e2bb94-a95d-44e2-b68c-cfdd1db77784
ms.openlocfilehash: 0db77f3e11542bb920df1e45312a72decc2a34fd
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84279783"
---
# <a name="culture-insensitive-string-operations"></a>Operações de cadeia de caracteres sem detecção de cultura

As operações de cadeia de caracteres sensíveis à cultura podem representar uma vantagem se você estiver criando aplicativos projetados para exibir resultados a usuários de acordo com a cultura. Por padrão, os métodos sensíveis à cultura obtêm a cultura a ser usada da propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A> para o thread atual.

Observe que as operações de cadeia de caracteres sensíveis à cultura nem sempre apresentam o comportamento desejado. Usar operações sensíveis à cultura quando os resultados devem ser independentes da cultura pode fazer o código falhar em culturas com mapeamentos de casos e regras de classificação personalizados. Para obter um exemplo, consulte a seção ["comparações de cadeias de caracteres que usam a cultura atual"](../base-types/best-practices-strings.md#string-comparisons-that-use-the-current-culture) no artigo [práticas recomendadas para o uso de Strings](../base-types/best-practices-strings.md) .

A forma como o aplicativo usa os resultados determina se as operações de cadeia de caracteres devem ser sensíveis à cultura ou insensíveis à cultura. Operações de cadeias de caracteres que exibem resultados para o usuário devem normalmente ser sensíveis à cultura. Por exemplo, se um aplicativo exibe uma lista classificada das cadeias de caracteres encontradas em uma caixa de listagem, o aplicativo deve executar uma classificação sensível à cultura.

Os resultados das operações de cadeia de caracteres usados internamente devem, de forma geral, ser insensíveis à cultura. Geralmente, se o aplicativo trabalha com nomes de arquivos, formatos de persistência ou informações simbólicas que não são exibidas para o usuário, os resultados das operações de cadeia de caracteres não devem variar de acordo com a cultura. Por exemplo, se um aplicativo compara uma cadeia de caracteres para determinar se ela é uma marca XML reconhecida, a comparação não deve ser sensível à cultura. Além disso, se uma decisão de segurança é baseada no resultado de uma operação de comparação de cadeias de caracteres ou de modificação de maiúsculas/minúsculas, a operação deve ser sensível à cultura para garantir que o resultado não seja afetado pelo valor de <xref:System.Globalization.CultureInfo.CurrentCulture%2A>.

Independentemente de estar ou não desenvolvendo um aplicativo que inclua código para tratar problemas de localização e globalização, você deve estar ciente dos métodos .NET Framework que, por padrão, recuperam resultados sensíveis à cultura. A finalidade deste tópico é ilustrar a maneira correta para seus aplicativos usarem esses métodos a fim de obter resultados insensíveis à cultura.

## <a name="see-also"></a>Veja também

- [Globalização e localização](index.md)
