---
title: Elemento <system.codedom>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.codedom
- http://schemas.microsoft.com/.NetConfiguration/v2.0#system.codedom
helpviewer_keywords:
- compiler configuration elements, <system.codedom> element
- system.codedom element
- <system.codedom> element
ms.assetid: 672a68f7-e69f-4479-ac30-e980085ec4fe
ms.openlocfilehash: 40a3c84e1deed4d215383670176623a6a79ac41d
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79155382"
---
# <a name="systemcodedom-element"></a>Elemento \<system.codedom>
Especifica as definições de configuração do compilador para provedores de linguagem disponíveis.  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;**\<system.codedom>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.codedom>  
  <compilers> ... </compilers>  
</system.codedom>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<compilers>](compilers-element.md)|Contêiner para elementos de configuração do compilador; contém zero ou mais [\<compiler>](compiler-element.md) elementos.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<configuration>](../configuration-element.md)|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
  
## <a name="remarks"></a>Comentários  
  
## <a name="net-framework-version-20"></a>.NET Framework versão 2,0  
 O [\<system.codedom>](system-codedom-element.md) elemento contém definições de configuração de compilador para provedores de idiomas instalados em um computador além dos provedores padrão que são instalados com o .NET Framework, como o <xref:Microsoft.CSharp.CSharpCodeProvider> e o <xref:Microsoft.VisualBasic.VBCodeProvider> . O [\<compilers>](compilers-element.md) elemento contém zero ou mais [\<compiler>](compiler-element.md) elementos. Cada [\<compiler>](compiler-element.md) elemento Especifica os atributos de configuração do compilador para um provedor de idioma específico.  
  
 Os desenvolvedores e fornecedores de compilador podem adicionar definições de configuração ao arquivo de configuração da máquina (Machine. config) para uma nova <xref:System.CodeDom.Compiler.CodeDomProvider> implementação. Use o <xref:System.CodeDom.Compiler.CodeDomProvider.GetAllCompilerInfo%2A?displayProperty=nameWithType> método para enumerar programaticamente os provedores de idioma padrão e os provedores de idiomas identificados pelas definições de configuração do compilador em um computador.  
  
> [!NOTE]
> No .NET Framework versões 1,0 e 1,1, os provedores de idioma padrão fornecidos pelo .NET Framework são identificados no [\<compilers>](compilers-element.md) elemento. No .NET Framework versão 2,0, os provedores de idioma padrão não são identificados no [\<compilers>](compilers-element.md) elemento, mas podem ser enumerados usando o <xref:System.CodeDom.Compiler.CodeDomProvider.GetAllCompilerInfo%2A> método.  
  
## <a name="net-framework-versions-10-and-11"></a>.NET Framework versões 1,0 e 1,1  
 O [\<system.codedom>](system-codedom-element.md) elemento contém os parâmetros de configuração do compilador para provedores de idiomas em um computador. O [\<compilers>](compilers-element.md) elemento contém zero ou mais [\<compiler>](compiler-element.md) elementos. Cada [\<compiler>](compiler-element.md) elemento Especifica os atributos de configuração do compilador para um provedor de idioma específico.  
  
 O .NET Framework define as configurações do compilador iniciais no arquivo de configuração do computador (Machine.config). Os desenvolvedores e fornecedores do compilador podem adicionar parâmetros de configuração em uma nova implementação do <xref:System.CodeDom.Compiler.CodeDomProvider>. Use o método <xref:System.CodeDom.Compiler.CodeDomProvider.GetAllCompilerInfo%2A?displayProperty=nameWithType> para enumerar programaticamente as definições de configuração do compilador e do provedor de linguagem em um computador.  
  
## <a name="configuration-file"></a>Arquivo de configuração  
 Esse elemento pode ser usado no arquivo de configuração da máquina e no arquivo de configuração do aplicativo.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir ilustra uma configuração de compilador típica.  
  
```xml  
<configuration>  
  <system.codedom>  
    <compilers>  
      <!-- zero or more compiler elements -->  
      <compiler
        language="c#;cs;csharp"  
        extension=".cs"  
        type="Microsoft.CSharp.CSharpCodeProvider, System,
          Version=1.0.5000.0, Culture=neutral,
          PublicKeyToken=b77a5c561934e089"  
        compilerOptions=""  
        warningLevel="1" />  
    </compilers>  
  </system.codedom>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.CodeDom.Compiler.CompilerInfo>
- <xref:System.CodeDom.Compiler.CodeDomProvider>
- [Esquema de arquivos de configuração](../index.md)
- [Esquema de configurações de compilador e de provedor de linguagem](index.md)
- [\<compiler>Elementos](compiler-element.md)
