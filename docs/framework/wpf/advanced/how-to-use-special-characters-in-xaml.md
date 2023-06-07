---
title: Como usar caracteres especiais em XAML
description: Saiba mais sobre a sintaxe para codificar caracteres especiais no formato de arquivo UTF-8 Unicode no Visual Studio para uso em arquivos XAML no Windows Presentation Foundation.
ms.date: 03/30/2017
helpviewer_keywords:
- Unicode UTF-8 file format
- UTF-8 file format
- characters [WPF], special
- typography [WPF], special characters
- special characters [WPF]
ms.assetid: a57776d1-f353-4794-afa0-bfa3c712ed1c
ms.openlocfilehash: ac2388fd96aa26ddd99408ac9f847ce517958568
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168359"
---
# <a name="how-to-use-special-characters-in-xaml"></a>Como usar caracteres especiais em XAML
Os arquivos de marcação criados no Visual Studio são salvos automaticamente no formato de arquivo UTF-8 Unicode, o que significa que a maioria dos caracteres especiais, como as marcas de acentuação, são codificados corretamente. No entanto, há um conjunto de caracteres especiais comumente usados que são tratados de forma diferente. Esses caracteres especiais seguem o padrão XML de World Wide Web Consortium (W3C) para codificação.  
  
 A tabela a seguir mostra a sintaxe para codificar esse conjunto de caracteres especiais:  
  
|Caractere|Sintaxe|Descrição|  
|---------------|------------|-----------------|  
|<|`&lt;`|Menor que símbolo.|  
|>|`&gt;`|Sinal de maior que.|  
|&|`&amp;`|Símbolo de e comercial.|  
|"|`&quot;`|Símbolo de aspas duplas.|  
  
> [!NOTE]
> Se você criar um arquivo de marcação usando um editor de texto, como o bloco de notas do Windows, deverá salvar o arquivo no formato de arquivo Unicode UTF-8 para preservar os caracteres especiais codificados.  
  
 O exemplo a seguir mostra como você pode usar caracteres especiais no texto ao criar marcação.  
  
## <a name="example"></a>Exemplo  
 [!code-xaml[SpecialCharsSnippets#SpecialCharsSnippet1](~/samples/snippets/csharp/VS_Snippets_Wpf/SpecialCharsSnippets/CS/Window1.xaml#specialcharssnippet1)]
