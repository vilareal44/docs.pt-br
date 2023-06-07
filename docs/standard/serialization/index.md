---
title: Serialização-.NET
description: Este artigo fornece informações sobre as tecnologias de serialização do .NET, incluindo serialização binária, serialização de XML e SOAP e serialização JSON.
ms.date: 09/02/2019
helpviewer_keywords:
- JSON serialization
- XML serialization, defined
- binary serialization
- serializing objects
- serialization
- objects, serializing
ms.assetid: 4d1111c0-9447-4231-a997-96a2b74b3453
ms.openlocfilehash: b3d76c14dc9180a5f19781122d1a42bcae603e76
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "83377241"
---
# <a name="serialization-in-net"></a>Serialização no .NET

A serialização é o processo de conversão do estado de um objeto em um formulário que possa ser persistido ou transportado. O complemento de serialização é desserialização, que converte um fluxo em um objeto. Juntos, esses processos permitem que os dados sejam armazenados e transferidos.  
  
O .NET apresenta as seguintes tecnologias de serialização:  
  
- A [serialização binária](binary-serialization.md) preserva a fidelidade do tipo, que é útil para preservar o estado de um objeto entre diferentes invocações de um aplicativo. Por exemplo, você pode compartilhar um objeto entre diferentes aplicativos serializando-o para a área de transferência. Você pode serializar um objeto para um fluxo, um disco, a memória, pela rede, e assim por diante. O acesso remoto usa a serialização para passar objetos “por valor” de um computador ou domínio de aplicativo para outro.  
  
- A [serialização de XML e SOAP](xml-and-soap-serialization.md) serializa apenas campos e propriedades públicas e não preserva a fidelidade do tipo. Isso é útil quando você deseja fornecer ou consumir dados sem restringir o aplicativo que usa os dados. Como o XML é um padrão aberto, é uma opção atrativa para compartilhar dados pela Web. SOAP é, da mesma forma, um padrão aberto, uma opção atrativa.  
  
- A [serialização JSON](system-text-json-overview.md) serializa apenas as propriedades públicas e não preserva a fidelidade do tipo. JSON é um padrão aberto que é uma opção atraente para compartilhar dados na Web.

## <a name="reference"></a>Referência

<xref:System.Runtime.Serialization>  
Contém classes que podem ser usadas para serialização e desserialização de objetos.
  
<xref:System.Xml.Serialization>  
Contém classes que podem ser usadas para serializar objetos em documentos ou fluxos de formato XML.

<xref:System.Text.Json>  
Contém classes que podem ser usadas para serializar objetos em fluxos ou documentos de formato JSON.
