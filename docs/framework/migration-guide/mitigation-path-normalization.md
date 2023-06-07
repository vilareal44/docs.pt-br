---
title: 'Mitigação: normalização do caminho'
description: Saiba como a normalização de caminho no .NET Framework foi alterada começando com aplicativos direcionados .NET Framework 4.6.2.
ms.date: 03/30/2017
ms.assetid: 158d47b1-ba6d-4fa6-8963-a012666bdc31
ms.openlocfilehash: 89dcc46d9f266ffd3635dc0cc02b634720356eda
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475210"
---
# <a name="mitigation-path-normalization"></a>Mitigação: normalização do caminho
A partir de aplicativos direcionados .NET Framework 4.6.2, a normalização de caminho no .NET Framework foi alterada.  
  
## <a name="what-is-path-normalization"></a>O que é normalização do caminho?  
 Normalizar um caminho envolve modificar a cadeia de caracteres que identifica um caminho ou arquivo para que ele esteja em conformidade com um caminho válido no sistema operacional de destino. Normalmente, a normalização envolve:  
  
- Padronização de separadores de diretório e componente.  
  
- Aplicação do diretório atual a um caminho relativo.  
  
- Avaliação do diretório relativo (`.`) ou do diretório pai (`..`) em um caminho.  
  
- Remoção de determinados caracteres.  
  
## <a name="the-changes"></a>As alterações  
 Começando com os aplicativos direcionados ao .NET Framework 4.6.2, a normalização do caminho foi alterada nos seguintes aspectos:  
  
- O runtime atende à função [GetFullPathName](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea) do sistema operacional para normalizar caminhos.  
  
- A normalização não envolve mais a remoção do fim dos segmentos do diretório (como um espaço no fim do nome de um diretório).  
  
- Suporte à sintaxe do caminho do dispositivo em confiança total, incluindo `\\.\` e, para APIs de E/S de arquivo em mscorlib.dll, `\\?\`.  
  
- O runtime não valida caminhos de sintaxe do dispositivo.  
  
- Há suporte ao uso da sintaxe de dispositivo para acessar fluxos de dados alternados.  
  
## <a name="impact"></a>Impacto  

Para os aplicativos direcionados ao .NET Framework 4.6.2 ou posterior, essas alterações estão ativadas por padrão. Elas devem melhorar o desempenho e ao mesmo tempo permitir que os métodos acessem caminhos anteriormente inacessíveis.  
  
Os aplicativos direcionados ao .NET Framework 4.6.1 e versões anteriores, mas que são executados no .NET Framework 4.6.2 ou posteriores, não são afetados por essa alteração.  
  
## <a name="mitigation"></a>Atenuação  
 Os aplicativos que visam o .NET Framework 4.6.2 ou posterior podem recusar essa alteração e usar a normalização herdada adicionando o seguinte à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo:  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=true" />
</runtime>  
```  
  
Aplicativos direcionados para o .NET Framework 4.6.1 ou anterior, mas que estão em execução no .NET Framework 4.6.2 ou posterior podem habilitar as alterações na normalização de caminho adicionando a seguinte linha à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo Application. Configuration:  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=false" />
</runtime>  
```  
  
## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
