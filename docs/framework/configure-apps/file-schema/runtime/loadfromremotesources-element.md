---
title: Elemento <loadFromRemoteSources>
ms.date: 05/24/2018
helpviewer_keywords:
- loadFromRemoteSources element
- <loadFromRemoteSources> element
ms.assetid: 006d1280-2ac3-4db6-a984-a3d4e275046a
ms.openlocfilehash: a0dcffe378cdd09de0fbd8f0a6ef0635c033fd9c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79154056"
---
# <a name="loadfromremotesources-element"></a>Elemento \<loadFromRemoteSources>
Especifica se os assemblies carregados de fontes remotas devem receber confiança total no .NET Framework 4 e posterior.
  
> [!NOTE]
> Se você foi direcionado para este artigo devido a uma mensagem de erro na lista de erros do projeto do Visual Studio ou um erro de compilação, consulte [como: usar um assembly da Web no Visual Studio](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ee890038(v=vs.100)).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<loadFromRemoteSources>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<loadFromRemoteSources
   enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se um assembly que é carregado de uma fonte remota deve receber confiança total.|  
  
## <a name="enabled-attribute"></a>atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`false`|Não conceda confiança total a aplicativos de fontes remotas. Este é o padrão.|  
|`true`|Conceda confiança total a aplicativos de fontes remotas.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre opções de inicialização do runtime.|  
  
## <a name="remarks"></a>Comentários

No .NET Framework 3,5 e versões anteriores, se você carregar um assembly de um local remoto, o código no assembly será executado em confiança parcial com um conjunto de concessão que depende da zona da qual ele está carregado. Por exemplo, se você carregar um assembly de um site, ele será carregado na zona da Internet e terá o conjunto de permissões da Internet. Em outras palavras, ele é executado em uma área restrita da Internet.

A partir do .NET Framework 4, a política de CAS (segurança de acesso ao código) é desabilitada e os assemblies são carregados em confiança total. Normalmente, isso concederia confiança total a assemblies carregados com o <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> método que anteriormente estava na área restrita. Para evitar isso, a capacidade de executar código em assemblies carregados de uma fonte remota é desabilitada por padrão. Por padrão, se você tentar carregar um assembly remoto, um <xref:System.IO.FileLoadException> com uma mensagem de exceção semelhante à seguinte será lançada:

```text
System.IO.FileNotFoundException: Could not load file or assembly 'file:assem.dll' or one of its dependencies. Operation is not supported.
(Exception from HRESULT: 0x80131515)
File name: 'file:assem.dll' --->
System.NotSupportedException: An attempt was made to load an assembly from a network location which would have caused the assembly
to be sandboxed in previous versions of the .NET Framework. This release of the .NET Framework does not enable CAS policy by default,
so this load may be dangerous. If this load is not intended to sandbox the assembly, please enable the loadFromRemoteSources switch.
```

Para carregar o assembly e executar seu código, você deve:

- Crie explicitamente uma área restrita para o assembly (consulte [como: executar código parcialmente confiável em uma área restrita](../../../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)).

- Execute o código do assembly em confiança total. Você faz isso Configurando o `<loadFromRemoteSources>` elemento. Ele permite que você especifique que os assemblies que são executados em confiança parcial em versões anteriores do .NET Framework agora são executados com confiança total no .NET Framework 4 e versões posteriores.

> [!IMPORTANT]
> Se o assembly não deve ser executado com confiança total, não defina este elemento de configuração. Em vez disso, crie uma área restrita <xref:System.AppDomain> na qual carregar o assembly.

O `enabled` atributo para o `<loadFromRemoteSources>` elemento é efetivo somente quando a segurança de acesso ao código (CAS) está desabilitada. Por padrão, a política de CAS é desabilitada no .NET Framework 4 e em versões posteriores. Se você definir `enabled` como `true` , os assemblies remotos recebem confiança total.

Se `enabled` não estiver definido como `true` , um <xref:System.IO.FileLoadException> será lançado sob uma das seguintes condições:

- O comportamento de área restrita do domínio atual é diferente do seu comportamento no .NET Framework 3,5. Isso requer que a política de CAS seja desabilitada e o domínio atual não seja colocado em área restrita.

- O assembly que está sendo carregado não é da `MyComputer` zona.

Definindo o `<loadFromRemoteSources>` elemento para `true` impedir que essa exceção seja gerada. Ele permite que você especifique que você não depende do Common Language Runtime para colocar os assemblies carregados em área restrita para segurança e que eles podem ter permissão para serem executados em confiança total.

## <a name="notes"></a>Observações

- No .NET Framework 4,5 e versões posteriores, os assemblies em compartilhamentos de rede local são executados em confiança total por padrão; Você não precisa habilitar o `<loadFromRemoteSources>` elemento.

- Se um aplicativo tiver sido copiado da Web, ele será sinalizado pelo Windows como sendo um aplicativo Web, mesmo que ele resida no computador local. Você pode alterar essa designação alterando suas propriedades de arquivo ou pode usar o `<loadFromRemoteSources>` elemento para conceder confiança total ao assembly. Como alternativa, você pode usar o <xref:System.Reflection.Assembly.UnsafeLoadFrom%2A> método para carregar um assembly local que o sistema operacional tenha sinalizado como tendo sido carregado da Web.

- Você pode obter um <xref:System.IO.FileLoadException> em um aplicativo que está sendo executado em um aplicativo do Windows Virtual PC. Isso pode acontecer quando você tenta carregar um arquivo de pastas vinculadas no computador host. Ele também pode ocorrer quando você tenta carregar um arquivo de uma pasta vinculada por [serviços de área de trabalho remota](/windows/win32/termserv/terminal-services-portal) (serviços de terminal). Para evitar a exceção, defina `enabled` como `true` .

## <a name="configuration-file"></a>Arquivo de configuração

Esse elemento é normalmente usado no arquivo de configuração do aplicativo, mas pode ser usado em outros arquivos de configuração, dependendo do contexto. Para obter mais informações, consulte o artigo [usos mais implícitos da política de CAS: loadFromRemoteSources](https://docs.microsoft.com/archive/blogs/shawnfa/more-implicit-uses-of-cas-policy-loadfromremotesources) no blog de segurança do .net.  

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como conceder confiança total a assemblies carregados de fontes remotas.

```xml
<configuration>  
   <runtime>  
      <loadFromRemoteSources enabled="true"/>  
   </runtime>  
</configuration>  
```

## <a name="see-also"></a>Confira também

- [Mais usos implícitos da política de CAS: loadFromRemoteSources](https://docs.microsoft.com/archive/blogs/shawnfa/more-implicit-uses-of-cas-policy-loadfromremotesources)
- [Como executar código parcialmente confiável em uma área restrita](../../../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)
- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>
