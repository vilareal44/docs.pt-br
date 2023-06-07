---
title: 'Como: Descarregar um domínio do aplicativo'
description: Leia como descarregar um domínio de aplicativo no .NET usando o método AppDomain. Unload para desligar o domínio do aplicativo especificado normalmente.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Unload method
- application domains, unloading
- unloading application domains
ms.assetid: f356116d-e415-4f7c-a332-6e6a60227192
ms.openlocfilehash: b64a9553f63aa4a8deb57f23a97fa464edd64fee
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104669"
---
# <a name="how-to-unload-an-application-domain"></a>Como: Descarregar um domínio do aplicativo
Quando terminar de usar um domínio do aplicativo, descarregue-o usando o método <xref:System.AppDomain.Unload%2A?displayProperty=nameWithType>. O método **Unload** desliga normalmente o domínio de aplicativo especificado. Durante o processo de descarga, nenhum thread novo pode acessar o domínio do aplicativo e todas as estruturas de dados específicas do domínio do aplicativo são liberadas.  
  
 Os assemblies carregados no domínio do aplicativo são removidos e não ficam mais disponíveis. Se um assembly no domínio do aplicativo forem independentes de domínio, os dados para o assembly permanecerão na memória até que todo o processo seja desligado. Não há outro mecanismo para descarregar um assembly com neutralidade de domínio a não ser desligar o processo inteiro. Há situações em que a solicitação para descarregar um domínio do aplicativo não funciona e resulta em uma <xref:System.CannotUnloadAppDomainException>.  
  
 O exemplo a seguir cria um novo domínio do aplicativo chamado `MyDomain`, imprime algumas informações no console e descarrega o domínio do aplicativo. Observe que o código tenta imprimir o nome amigável do domínio do aplicativo descarregado no console. Essa ação gera uma exceção que é manipulada pelas instruções try/catch no fim do programa.  
  
## <a name="example"></a>Exemplo  
 [!code-cpp[System.AppDomain.Load#3](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.appdomain.load/cpp/source3.cpp#3)]
 [!code-csharp[System.AppDomain.Load#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.load/cs/source3.cs#3)]
 [!code-vb[System.AppDomain.Load#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.load/vb/source3.vb#3)]  
  
## <a name="see-also"></a>Confira também

- [Programação com domínios do aplicativo](application-domains.md#programming-with-application-domains)
- [Como: Criar um domínio do aplicativo](how-to-create-an-application-domain.md)
- [Usando domínios do aplicativo](use.md)
