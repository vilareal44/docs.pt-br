---
title: Elemento <codeBase>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#codeBase
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/assemblyBinding/dependentAssembly/codeBase
helpviewer_keywords:
- <codeBase> element
- container tags, <codeBase> element
- codeBase element
ms.assetid: d48a3983-2297-43ff-a14d-1f29d3995822
ms.openlocfilehash: 475b7df55ed509157c1da0aeb8f979de238c72b5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70971882"
---
# <a name="codebase-element"></a>Elemento \<codeBase>

Especifica onde o Common Language Runtime pode encontrar um assembly.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<assemblyBinding>**](assemblybinding-element-for-runtime.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<dependentAssembly>**](dependentassembly-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<codeBase>**

## <a name="syntax"></a>Sintaxe

```xml
   <codeBase
        version="Assembly version"
        href="URL of assembly"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`href`|Atributo obrigatório.<br /><br /> Especifica a URL em que o tempo de execução pode encontrar a versão especificada do assembly.|
|`version`|Atributo obrigatório.<br /><br /> Especifica a versão do assembly à qual a codebase se aplica. O formato de um número de versão do assembly é *Major. Minor. Build. Revision*.|

## <a name="version-attribute"></a>Atributo de versão

|Valor|Descrição|
|-----------|-----------------|
|Os valores válidos para cada parte do número de versão são de 0 a 65535.|Não aplicável.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`buildproviders`|Define uma coleção de provedores de compilação usados para compilar arquivos de recursos personalizados. Você pode ter qualquer número de provedores de compilação.|
|`compilation`|Define todas as configurações de compilação que o ASP.NET usa.|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`System.web`|Especifica o elemento raiz para a seção de configuração ASP.NET.|

## <a name="remarks"></a>Comentários

Para que o tempo de execução use a **\<codeBase>** configuração em um arquivo de configuração de máquina ou arquivo de política do Publicador, o arquivo também deve redirecionar a versão do assembly. Os arquivos de configuração do aplicativo podem ter uma configuração de CodeBase sem redirecionar a versão do assembly. Depois de determinar qual versão do assembly usar, o tempo de execução aplica a configuração de codebase do arquivo que determina a versão. Se nenhuma codebase for indicada, o tempo de execução investigará o assembly da maneira usual.

Se o assembly tiver um nome forte, a configuração de CodeBase poderá estar em qualquer lugar na intranet local ou na Internet. Se o assembly for um assembly particular, a configuração de CodeBase deverá ser um caminho relativo ao diretório do aplicativo.

Para assemblies sem um nome forte, a versão é ignorada e o carregador usa a primeira aparência de \<codebase> dentro \<dependentAssembly> . Se houver uma entrada no arquivo de configuração de aplicativo que redireciona a associação a outro assembly, o redirecionamento terá precedência mesmo que a versão do assembly não corresponda à solicitação de associação.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como especificar onde o tempo de execução pode encontrar um assembly.

```xml
<configuration>
   <runtime>
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
         <dependentAssembly>
            <assemblyIdentity name="myAssembly"
                              publicKeyToken="32ab4ba45e0a69a1"
                              culture="neutral" />
            <codeBase version="2.0.0.0"
                      href="http://www.litwareinc.com/myAssembly.dll"/>
         </dependentAssembly>
      </assemblyBinding>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Esquema de configurações de tempo de execução](index.md)
- [Esquema do arquivo de configuração](../index.md)
- [Especificar o local de um assembly](../../../../standard/assembly/location.md)
- [Como o tempo de execução localiza assemblies](../../../deployment/how-the-runtime-locates-assemblies.md)
