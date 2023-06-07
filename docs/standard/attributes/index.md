---
title: Estendendo metadados por meio de atributos
description: Saiba como estender metadados usando atributos no .NET. Os atributos são declarações descritivas semelhantes a palavras-chave para anotar elementos de programação, como tipos e campos.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- metadata, extending
- attributes [.NET Framework], metadata
- elements [.NET Framework], attributes
- common language runtime, attributes
- annotating programming elements
- keyword-like descriptive declarations
- runtime, attributes
- extending metadata
ms.assetid: 30386922-1e00-4602-9ebf-526b271a8b87
ms.openlocfilehash: c77de970c5550bd896e83854414592d70eb9dc48
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598626"
---
# <a name="extending-metadata-using-attributes"></a>Estendendo metadados por meio de atributos
O Common Language Runtime permite adicionar declarações descritivas parecidas com palavras, chamadas atributos, para anotar elementos de programação como tipos, campos, métodos e propriedades. Quando você compila seu código para o runtime, ele é convertido em MSIL (Microsoft Intermediate Language) e colocado dentro de um arquivo PE (executável portátil) com metadados gerados pelo compilador. Os atributos permitem colocar informações descritivas extras em metadados que podem ser extraídos usando serviços de reflexão de runtime. O compilador cria atributos quando você declara instâncias de classes especiais que derivam de <xref:System.Attribute?displayProperty=nameWithType>.  
  
 O .NET Framework usa atributos por vários motivos e para solucionar vários problemas. Os atributos descrevem como serializar dados, especificar características que são usadas para impor segurança e limitar otimizações pelo compilador JIT (Just-In-Time) para que o código permaneça fácil de depurar. Os atributos também podem registrar o nome de um arquivo ou o autor do código, ou controlar a visibilidade de controles e membros durante o desenvolvimento de formulários.  
  
## <a name="related-topics"></a>Tópicos relacionados  
  
|Title|Descrição|  
|-----------|-----------------|  
|[Aplicando atributos](applying-attributes.md)|Descreve como aplicar um atributo a um elemento do código.|  
|[Escrevendo atributos personalizados](writing-custom-attributes.md)|Descreve como criar classes de atributos personalizados.|  
|[Recuperando informações armazenadas em atributos](retrieving-information-stored-in-attributes.md)|Descreve como recuperar atributos personalizados para o código que é carregado no contexto de execução.|  
|[Metadados e componentes autodescritivos](../metadata-and-self-describing-components.md)|Fornece uma visão geral dos metadados e descreve como eles são implementados em um arquivo executável PE (executável portátil) do .NET Framework.|  
|[Como: Carregar assemblies no contexto somente de reflexão](../../framework/reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)|Explica como recuperar informações de atributos personalizados no contexto somente reflexão.|  
  
## <a name="reference"></a>Referência  
 <xref:System.Attribute?displayProperty=nameWithType>
