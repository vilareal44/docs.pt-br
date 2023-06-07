---
title: Manipulando exceções de interoperabilidade COM
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- unmanaged code, exceptions
- exceptions, unmanaged code
- HRESULTs
- exceptions, COM interop
- COM interop, exceptions
ms.assetid: e6104aa8-8e5f-4069-b864-def85579c96c
ms.openlocfilehash: 9c8eb374058ddbd2ba3d866079f0f40b292b69ea
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286100"
---
# <a name="handling-com-interop-exceptions"></a>Manipulando exceções de interoperabilidade COM
Os códigos gerenciado e não gerenciado podem trabalhar juntos para tratar de exceções. Se um método lança uma exceção no código gerenciado, o common language runtime pode passar um HRESULT para um objeto COM. Se um método falhar no código não gerenciado, retornando um HRESULT de falha, o runtime lançará uma exceção que pode ser detectada pelo código gerenciado.  
  
 O runtime mapeia automaticamente o HRESULT da interoperabilidade COM para exceções mais específicas. Por exemplo, E_ACCESSDENIED se torna <xref:System.UnauthorizedAccessException>, E_OUTOFMEMORY se torna <xref:System.OutOfMemoryException> e assim por diante.  
  
 Se o HRESULT for um resultado personalizado, ou se for desconhecido para o runtime, o runtime passará um <xref:System.Runtime.InteropServices.COMException> genérico ao cliente. A propriedade **ErrorCode** do **COMException** contém o valor de HRESULT.  
  
## <a name="working-with-ierrorinfo"></a>Trabalhar com IErrorInfo  
 Quando um erro é passado do COM para o código gerenciado, o runtime preenche o objeto de exceção com informações do erro. Objetos COM que dão suporte a IErrorInfo e retornam HRESULTS fornecem essas informações para exceções de código gerenciado. Por exemplo, o runtime mapeia a Descrição do erro COM para a propriedade <xref:System.Exception.Message%2A> da exceção. Se o HRESULT não fornecer mais informações sobre o erro, o runtime preencherá muitas das propriedades da exceção com valores padrão.  
  
 Se um método falhar no código não gerenciado, uma exceção poderá ser passada para um segmento de código gerenciado. O tópico [HRESULTS e exceções](../../framework/interop/how-to-map-hresults-and-exceptions.md) contém uma tabela que mostra como HRESULTS mapeia para objetos de exceção de runtime.  

## <a name="see-also"></a>Veja também

- [Exceções](index.md)
