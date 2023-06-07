---
title: Objeto My.Request
ms.date: 07/20/2015
f1_keywords:
- My.MyWebExtension.Request
- My.Request
helpviewer_keywords:
- My.Request object
ms.assetid: 93d5f0e2-6b60-4a2c-8652-d90216f6ad10
ms.openlocfilehash: 38f510e2a3958761b902f37760069aa8d595ea8e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84372421"
---
# <a name="myrequest-object"></a>Objeto My.Request
Obtém o objeto <xref:System.Web.HttpRequest> para a página solicitada.  
  
## <a name="remarks"></a>Comentários  
 O objeto `My.Request` contém informações sobre a solicitação HTTP atual.  
  
 O objeto `My.Request` está disponível somente para aplicativos do ASP.NET.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir obtém a coleção de cabeçalho do `My.Request` objeto e usa o `My.Response` objeto para escrevê-lo na página ASP.net.  
  
 [!code-aspx-vb[VbVbalrMyWeb#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWeb/VB/Default.aspx#1)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Web.HttpRequest>
- [Objeto My.Response](my-response-object.md)
