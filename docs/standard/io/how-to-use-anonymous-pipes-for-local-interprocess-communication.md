---
title: 'Como: Usar pipes anônimos para comunicação entre processos locais'
description: Saiba como usar pipes anônimos para comunicação entre processos locais em um computador local no .NET. Os pipes anônimos exigem menos sobrecarga do que os pipes nomeados.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- anonymous pipes [.NET Framework]
- parent-child communication [.NET Framework]
- pipes [.NET Framework]
- one-way communication [.NET Framework]
- local computer communication [.NET Framework], pipes
ms.assetid: e7773c77-c646-4a01-8a96-a003d59fc4c9
ms.openlocfilehash: 090a25aea4f280fc2ad00cf7777a501c475dfc66
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594797"
---
# <a name="how-to-use-anonymous-pipes-for-local-interprocess-communication"></a>Como: Usar pipes anônimos para comunicação entre processos locais
Os pipes anônimos fornecem comunicação entre processos em um computador local. Eles oferecem menos funcionalidades que pipes nomeados, mas também exigem menos sobrecarga. Você pode usar pipes anônimos para facilitar a comunicação entre processos em um computador local. Você não pode usar pipes anônimos para comunicações através de uma rede.  
  
 Para implementar pipes anônimos, use as classes <xref:System.IO.Pipes.AnonymousPipeServerStream> e <xref:System.IO.Pipes.AnonymousPipeClientStream>.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra uma maneira de enviar uma cadeia de caracteres de um processo pai para um processo filho usando pipes anônimos. Este exemplo cria um objeto <xref:System.IO.Pipes.AnonymousPipeServerStream> em um processo pai com um valor <xref:System.IO.Pipes.PipeDirection> de <xref:System.IO.Pipes.PipeDirection.Out>. Em seguida, o processo pai cria um processo filho usando um identificador de cliente para criar um objeto <xref:System.IO.Pipes.AnonymousPipeClientStream>. O processo filho tem um valor <xref:System.IO.Pipes.PipeDirection> de <xref:System.IO.Pipes.PipeDirection.In>.  
  
 Em seguida, o processo pai envia uma cadeia de caracteres fornecida pelo usuário ao processo filho. A cadeia de caracteres é exibida no console do processo filho.  
  
 O exemplo a seguir mostra o processo do servidor.  
  
 [!code-cpp[System.IO.Pipes.AnonymousPipeServerStream_Sample#01](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeServerStream_Sample/cpp/program.cpp#01)]
 [!code-csharp[System.IO.Pipes.AnonymousPipeServerStream_Sample#01](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeServerStream_Sample/cs/Program.cs#01)]
 [!code-vb[System.IO.Pipes.AnonymousPipeServerStream_Sample#01](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeServerStream_Sample/vb/program.vb#01)]  

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra o processo do cliente. O processo do servidor inicia o processo do cliente e oferece a ele um identificador de cliente. O executável que resulta do código do cliente deve ser chamado de `pipeClient.exe` e copiado para o mesmo diretório que o executável do servidor antes que o processo do servidor seja executado.  
  
 [!code-cpp[System.IO.Pipes.AnonymousPipeClientStream_Sample#01](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeClientStream_Sample/cpp/program.cpp#01)]
 [!code-csharp[System.IO.Pipes.AnonymousPipeClientStream_Sample#01](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeClientStream_Sample/cs/Program.cs#01)]
 [!code-vb[System.IO.Pipes.AnonymousPipeClientStream_Sample#01](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.Pipes.AnonymousPipeClientStream_Sample/vb/program.vb#01)]  
  
## <a name="see-also"></a>Consulte também

- [Pipes](pipe-operations.md)
- [Como: Usar pipes nomeados para comunicação entre processos na rede](how-to-use-named-pipes-for-network-interprocess-communication.md)
