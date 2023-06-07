---
title: Elemento <gcAllowVeryLargeObjects>
ms.date: 03/30/2017
helpviewer_keywords:
- gcAllowVeryLargeObjects element
- <gcAllowVeryLargeObjects> element
ms.assetid: 5c7ea24a-39ac-4e5f-83b7-b9f9a1b556ab
ms.openlocfilehash: 8b2f39a0867228474afdee788474fda11f14ca82
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79154121"
---
# <a name="gcallowverylargeobjects-element"></a>Elemento \<gcAllowVeryLargeObjects>
Em plataformas de 64 bits, habilita matrizes com mais de 2 gigabytes (GB) de tamanho total.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<gcAllowVeryLargeObjects>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<gcAllowVeryLargeObjects
   enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se as matrizes maiores que 2 GB no tamanho total estão habilitadas em plataformas de 64 bits.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`false`|Matrizes maiores que 2 GB no tamanho total não estão habilitadas. Este é o padrão.|  
|`true`|Matrizes maiores que 2 GB no tamanho total são habilitadas em plataformas de 64 bits.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre opções de inicialização do runtime.|  
  
## <a name="remarks"></a>Comentários  
 O uso desse elemento em seu arquivo de configuração de aplicativo permite que matrizes com mais de 2 GB de tamanho, mas não alteram outros limites no tamanho do objeto ou tamanho da matriz:  
  
- O número máximo de elementos em uma matriz é <xref:System.UInt32.MaxValue?displayProperty=nameWithType> .  
  
- O índice máximo em qualquer dimensão única é 2.147.483.591 (0x7FFFFFC7) para matrizes de bytes e matrizes de estruturas de byte único e 2.146.435.071 (0X7FEFFFFF) para outros tipos.  
  
- O tamanho máximo para cadeias de caracteres e outros objetos que não são da matriz não são alterados.  
  
> [!CAUTION]
> Antes de habilitar esse recurso, verifique se o seu aplicativo não inclui um código não seguro que assuma que todas as matrizes tenham menos de 2 GB de tamanho. Por exemplo, o código não seguro que usa matrizes como buffers pode ser suscetível a estouros de buffer se ele for escrito na suposição de que as matrizes não excederão 2 GB.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como habilitar esse recurso para um aplicativo.  
  
```xml  
<configuration>  
  <runtime>  
    <gcAllowVeryLargeObjects enabled="true" />  
  </runtime>  
</configuration>  
```  
  
## <a name="supported-in"></a>Com suporte em

.NET Framework 4,5 e versões posteriores

## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
