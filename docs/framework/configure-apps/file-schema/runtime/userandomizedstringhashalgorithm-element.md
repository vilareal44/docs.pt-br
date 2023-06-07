---
title: Elemento <UseRandomizedStringHashAlgorithm>
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UseRandomizedStringHashAlgorithm element
- <UseRandomizedStringHashAlgorithm> element
ms.assetid: c08125d6-56cc-4b23-b482-813ff85dc630
ms.openlocfilehash: a9afa0db516a542b74d08a4c3754a3244abbbea7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153771"
---
# <a name="userandomizedstringhashalgorithm-element"></a>Elemento \<UseRandomizedStringHashAlgorithm>
Determina se o Common Language Runtime calcula códigos de hash para cadeias de caracteres por aplicativo.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<UseRandomizedStringHashAlgorithm>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<UseRandomizedStringHashAlgorithm
   enabled=0|1 />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se os códigos de hash para cadeias de caracteres são calculados em uma base de domínio por aplicativo.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`0`|O Common Language Runtime não computa códigos hash para cadeias de caracteres por aplicativo; um único algoritmo é usado para calcular códigos de hash de cadeia de caracteres. Este é o padrão.|  
|`1`|O Common Language Runtime computa códigos de hash para cadeias de caracteres por aplicativo. Cadeias de caracteres idênticas em domínios de aplicativo diferentes e em processos diferentes terão códigos de hash diferentes.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre opções de inicialização do runtime.|  
  
## <a name="remarks"></a>Comentários  
 Por padrão, a <xref:System.StringComparer> classe e o <xref:System.String.GetHashCode%2A?displayProperty=nameWithType> método usam um único algoritmo de hash que produz um código hash consistente entre domínios de aplicativo. Isso é equivalente a definir o `enabled` atributo do `<UseRandomizedStringHashAlgorithm>` elemento como `0` . Esse é o algoritmo de hash usado no .NET Framework 4.  
  
 A <xref:System.StringComparer> classe e o <xref:System.String.GetHashCode%2A?displayProperty=nameWithType> método também podem usar um algoritmo de hash diferente que computa códigos de hash em uma base de domínio por aplicativo. Como resultado, os códigos de hash para cadeias de caracteres equivalentes serão diferentes entre domínios de aplicativo. Esse é um recurso opcional; para aproveitar isso, você deve definir o `enabled` atributo do `<UseRandomizedStringHashAlgorithm>` elemento como `1` .  
  
 A pesquisa de cadeia de caracteres em uma tabela de hash normalmente é uma operação O (1). No entanto, quando ocorre um grande número de colisões, a pesquisa pode se tornar uma operação O (n<sup>2</sup>). Você pode usar o `<UseRandomizedStringHashAlgorithm>` elemento de configuração para gerar um algoritmo de hash aleatório por domínio de aplicativo, que, por sua vez, limita o número de colisões em potencial, especialmente quando as chaves das quais os códigos de hash são calculados se baseiam na entrada de dados pelos usuários.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir define uma `DisplayString` classe que inclui uma constante de cadeia de caracteres privada, `s` , cujo valor é "esta é uma cadeia de caracteres". Também inclui um método `ShowStringHashCode` que exibe o valor de cadeia de caracteres e seu código de hash com o nome do domínio do aplicativo no qual o método está sendo executado.  
  
 [!code-csharp[System.String.GetHashCode#2](../../../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.String.GetHashCode/CS/perdomain.cs#2)]
 [!code-vb[System.String.GetHashCode#2](../../../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.String.GetHashCode/VB/perdomain.vb#2)]  
  
 Quando você executa o exemplo sem fornecer um arquivo de configuração, ele exibe uma saída semelhante à seguinte. Observe que os códigos hash para a cadeia de caracteres são idênticos nos dois domínios de aplicativo.  
  
```console
String 'This is a string.' in domain 'PerDomain.exe': 941BCEAC  
String 'This is a string.' in domain 'NewDomain': 941BCEAC  
```  
  
 Entretanto, se você adicionar o seguinte arquivo de configuração ao diretório de exemplo e, então, executar o exemplo, os códigos hash da mesma cadeia de caracteres diferirão de acordo com o domínio de aplicativo.  
  
```xml  
<?xml version ="1.0"?>  
<configuration>  
   <runtime>  
      <UseRandomizedStringHashAlgorithm enabled="1" />  
   </runtime>  
</configuration>  
```  
  
 Quando o arquivo de configuração estiver presente, o exemplo exibe a saída a seguir:  
  
```console
String 'This is a string.' in domain 'PerDomain.exe': 5435776D  
String 'This is a string.' in domain 'NewDomain': 75CC8236  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType>
- <xref:System.String.GetHashCode%2A?displayProperty=nameWithType>
- <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType>
