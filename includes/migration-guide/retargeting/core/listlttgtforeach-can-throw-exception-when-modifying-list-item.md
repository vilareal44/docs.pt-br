---
ms.openlocfilehash: 3300a74b81fc9eeba670ee0ceb2c2615fd3925bd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617110"
---
### <a name="listlttgtforeach-can-throw-exception-when-modifying-list-item"></a>List&lt;T&gt;.ForEach agora gera exceção durante a modificação de item de lista

#### <a name="details"></a>Detalhes

Começando no .NET Framework 4.5, um enumerador <xref:System.Collections.Generic.List%601.ForEach(System.Action{%600})> gerará uma exceção <xref:System.InvalidOperationException?displayProperty=fullName> se um elemento na coleção de chamada for modificado. Anteriormente, isso não geraria uma exceção, mas podia levar a condições de corrida.

#### <a name="suggestion"></a>Sugestão

De modo ideal, o código deve ser corrigido para não modificar listas durante a enumeração de seus elementos, pois essa nunca é uma operação segura. Porém, para reverter para o comportamento anterior, um aplicativo pode ser direcionado ao .NET Framework 4.0.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.5         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Collections.Generic.List%601.ForEach(System.Action{%600})?displayProperty=nameWithType>
