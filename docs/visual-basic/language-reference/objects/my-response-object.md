---
title: Objeto My.Response
ms.date: 07/20/2015
f1_keywords:
- My.MyWebExtension.Response
- My.Response
helpviewer_keywords:
- My.Response object
ms.assetid: 626359bc-3165-40b4-bfaf-2c610e26eb5b
ms.openlocfilehash: 962108264563c5e0b2894c5c856a5f23a3c1a8b4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84372448"
---
# <a name="myresponse-object"></a>Objeto My.Response
Obtém o <xref:System.Web.HttpResponse> objeto associado ao <xref:System.Web.UI.Page> . Esse objeto permite que você envie dados de resposta HTTP para um cliente e contém informações sobre essa resposta.  
  
## <a name="remarks"></a>Comentários  
 O `My.Response` objeto contém o <xref:System.Web.HttpResponse> objeto atual associado à página.  
  
 O `My.Response` objeto só está disponível para aplicativos ASP.net.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir obtém a coleção de cabeçalho do `My.Request` objeto e usa o `My.Response` objeto para escrevê-lo na página ASP.net.  
  
 [!code-aspx-vb[VbVbalrMyWeb#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWeb/VB/Default.aspx#1)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Web.HttpResponse>
- [Objeto My.Request](my-request-object.md)
