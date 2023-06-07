---
title: Elemento <assert>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/assert
- http://schemas.microsoft.com/.NetConfiguration/v2.0#assert
helpviewer_keywords:
- <assert> element
- assert element
ms.assetid: ef4c3229-b151-4d85-8091-e6456af9b935
ms.openlocfilehash: f3c1a1670139a8262dea449bfff99c7c1c19f088
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74088942"
---
# <a name="assert-element"></a>Elemento \<assert>
Especifica se uma caixa de mensagem deve ser exibida ao chamar o método <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>; também especifica o nome do arquivo no qual as mensagens serão gravadas.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<assert>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<assert assertuienabled="true|false" logfilename="file name"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`assertuienabled`|Atributo opcional.<br /><br /> Especifica se uma caixa de mensagem deve ser exibida quando o método **debug. Assert** for avaliado como **false**.|  
|`logfilename`|Atributo opcional.<br /><br /> Especifica o nome do arquivo no qual gravar a mensagem se **debug. Assert** for avaliado como **false**.|  
  
## <a name="assertuienabled-attribute"></a>Atributo AssertUiEnabled  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`true`|Exibe a caixa de mensagem. Este é o padrão.|  
|`false`|Não exibe a caixa de mensagem.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`system.diagnostics`|Especifica os ouvintes de rastreamento que coletam, armazenam e roteiam mensagens e o nível em que uma opção de rastreamento é definida.|  
  
## <a name="remarks"></a>Comentários  
 Ambos os atributos no **\<assert>** elemento são opcionais. Você pode desabilitar caixas de mensagens sem especificar um arquivo no qual gravar as mensagens ou pode especificar um arquivo para gravar mensagens enquanto deixa as caixas de mensagens habilitadas.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como desabilitar a exibição de caixas de mensagem quando você chama **debug. Assert** e grava as mensagens em `c:\log.txt` .  
  
```xml  
<configuration>  
   <system.diagnostics>  
      <assert assertuienabled="false" logfilename="c:\log.txt"/>  
   </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.Debug>
- [Esquema de configurações de rastreamento e depuração](index.md)
