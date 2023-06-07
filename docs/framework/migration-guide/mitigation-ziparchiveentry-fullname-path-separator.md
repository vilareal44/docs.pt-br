---
title: 'Mitigação: separador de caminho ZipArchiveEntry.FullName'
description: Saiba como o separador de caminho para a propriedade ZipArchiveEntry. FullName foi alterado começando com aplicativos direcionados .NET Framework 4.6.1.
ms.date: 03/30/2017
helpviewer_keywords:
- application compatibility
- ',NET Framework application compatibility'
- .NET Framework 4.6.1
- .NET Framework 4.6.1 retargeting changes
- retargeting changes
ms.assetid: 8d575722-4fb6-49a2-8a06-f72d62dc3766
ms.openlocfilehash: 8cd6362038ce0548681f3d3b44724f3ef9ff62cb
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475288"
---
# <a name="mitigation-ziparchiveentryfullname-path-separator"></a>Mitigação: separador de caminho ZipArchiveEntry.FullName

A partir de aplicativos direcionados ao .NET Framework 4.6.1, o separador de caminho usado na <xref:System.IO.Compression.ZipArchiveEntry.FullName%2A?displayProperty=nameWithType> propriedade foi alterado da barra invertida (" \\ ") usada em versões anteriores do .NET Framework para uma barra ("/"). Os objetos <xref:System.IO.Compression.ZipArchiveEntry?displayProperty=nameWithType> são criados chamando uma das sobrecargas do método <xref:System.IO.Compression.ZipFile.CreateFromDirectory%2A?displayProperty=nameWithType>.  
  
## <a name="impact"></a>Impacto  
 A alteração traz a implementação do .NET em conformidade com a seção 4.4.17.1 da [Especificação de formato de arquivo .ZIP](https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT) e permite que arquivamentos .ZIP sejam descompactados em sistemas não Windows.  
  
 A descompactação de um arquivo zip criado por um aplicativo direcionado a uma versão anterior do .NET Framework em sistemas operacionais não Windows, como o MacOS, falha ao preservar a estrutura do diretório. Por exemplo, no MacOS, ele cria um conjunto de arquivos cujo nome de arquivo concatena o caminho do diretório, qualquer barra invertida (" \\ ") caracteres e o nome do arquivo. Consequentemente, a estrutura do diretório de arquivos descompactados não é preservada.  
  
 O impacto dessa alteração em. Os arquivos ZIP que são descompactados no sistema operacional Windows por APIs no <xref:System.IO> namespace .NET Framework devem ser mínimos, uma vez que essas APIs podem lidar diretamente com uma barra ("/") ou uma barra invertida (" \\ ") como o caractere separador de caminho.  
  
## <a name="mitigation"></a>Atenuação  
 Se esse comportamento for indesejável, você poderá recusar adicionando um parâmetro de configuração à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo. Veja a seguir a seção `<runtime>` e a opção de recusa.  
  
```xml  
<runtime>  
   <AppContextSwitchOverrides value="Switch.System.IO.Compression.ZipFile.UseBackslash=true" />  
</runtime>  
```  
  
 Além disso, os aplicativos destinados a versões anteriores do .NET Framework, mas que estão em execução no .NET Framework 4.6.1 e versões posteriores podem aceitar esse comportamento, adicionando uma configuração à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo. Veja a seguir a seção `<runtime>` e a opção de aceitação.  
  
```xml  
<runtime>  
   <AppContextSwitchOverrides value="Switch.System.IO.Compression.ZipFile.UseBackslash=false" />  
</runtime>  
```  
  
## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
