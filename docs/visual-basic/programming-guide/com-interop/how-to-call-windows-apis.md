---
title: 'Como: Chamar APIs do Windows'
ms.date: 07/20/2015
helpviewer_keywords:
- API calls [Visual Basic]
- Windows API, calling
- API calls [Visual Basic], platform invoke
- calls [Visual Basic], stored procedures
ms.assetid: 27d75f0a-54ab-4ee1-b91d-43513a19b12d
ms.openlocfilehash: 2c3bb599b79575180eb2b0ec89453f01901f94c0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396837"
---
# <a name="how-to-call-windows-apis-visual-basic"></a>Como chamar APIs do Windows (Visual Basic)
Este exemplo define e chama a `MessageBox` função em user32. dll e, em seguida, passa uma cadeia de caracteres para ela.  
  
## <a name="example"></a>Exemplo  
 [!code-vb[VbVbalrInterop#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#1)]  
  
## <a name="compile-the-code"></a>Compilar o código  
 Este exemplo requer:  
  
- Uma referência ao <xref:System> namespace.  
  
## <a name="robust-programming"></a>Programação robusta  
 As seguintes condições podem causar uma exceção:  
  
- O método não é estático, é abstrato ou foi definido anteriormente. O tipo de pai é uma interface ou o comprimento de *Name* ou *DllName* é zero. (<xref:System.ArgumentException>)  
  
- O *nome* ou *DllName* é `Nothing` . (<xref:System.ArgumentNullException>)  
  
- O tipo recipiente foi criado anteriormente usando `CreateType`. (<xref:System.InvalidOperationException>)  
  
## <a name="see-also"></a>Confira também

- [Um olhar detalhado sobre invocação de plataforma](../../../framework/interop/consuming-unmanaged-dll-functions.md#a-closer-look-at-platform-invoke)
- [Exemplos de invocação de plataforma](../../../framework/interop/platform-invoke-examples.md)
- [Consumindo funções de DLL não gerenciadas](../../../framework/interop/consuming-unmanaged-dll-functions.md)
- [Definindo um método com a emissão de reflexão](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/w63y4d4f(v=vs.100))
- [Passo a passo: Fazer chamadas de APIs do Windows](walkthrough-calling-windows-apis.md)
- [Interoperabilidade COM](index.md)
