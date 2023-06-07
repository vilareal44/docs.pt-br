---
title: Elemento <providerOption>
ms.date: 03/30/2017
f1_keywords:
- provideroption
helpviewer_keywords:
- <provideroption> element
- providerOptions
- provideroption element
ms.assetid: 014f2e0b-c0b5-4fc4-92d3-73f02978b2a1
ms.openlocfilehash: c8ba5b9a0680f5e5102c13eb5bb0c1904a168c07
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79155395"
---
# <a name="provideroption-element"></a>Elemento \<providerOption>
Especifica os atributos de versão do compilador para um provedor de idioma.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.codedom>**](system-codedom-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<compilers>**](compilers-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<compiler>**](compiler-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<providerOption>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<providerOption  
  name="option-name"  
  value="option-value"  
/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`name`|Atributo obrigatório.<br /><br /> Especifica o nome da opção; por exemplo, "CompilerVersion".|  
|`value`|Atributo obrigatório.<br /><br /> Especifica o valor para a opção; por exemplo, "v 3.5".|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<configuration>Elementos](../configuration-element.md)|O elemento raiz em todos os arquivos de configuração que é usado pelo common language runtime e aplicativos do .NET Framework.|  
|[\<system.codedom>Elementos](system-codedom-element.md)|Especifica as definições de configuração do compilador para provedores de linguagem disponíveis.|  
|[\<compilers>Elementos](compilers-element.md)|Contêiner para elementos de configuração do compilador; contém zero ou mais `<compiler>` elementos.|  
|[\<compiler>Elementos](compiler-element.md)|Especifica os atributos de configuração do compilador para um provedor de linguagem.|  
  
## <a name="remarks"></a>Comentários  
 Nos provedores de código do .NET Framework versão 3,5, Modelo de Objeto do Documento de Código (CodeDOM) podem dar suporte a opções específicas do provedor usando o `<providerOption>` elemento.  
  
 O .NET Framework 3,5 inclui assemblies .NET Framework 2,0 atualizados e fornece novos assemblies da versão 3,5 que contêm novos tipos. Os provedores de código do Microsoft C# e Visual Basic estão contidos em assemblies do .NET Framework 2,0, mas foram atualizados para dar suporte a compiladores da versão 3,5. Por padrão, os provedores de código atualizados geram código para compiladores da versão 2,0. Você pode usar o `<providerOption>` elemento para alterar a versão do compilador de destino para 3,5. Para fazer isso, especifique "CompilerVersion" para o `name` atributo e "v 3.5" para o `value` atributo. Você deve preceder o número de versão com um "v" minúsculo.  
  
 Você pode tornar a especificação de versão global adicionando o `<providerOption>` elemento ao .NET Framework 2,0 Machine. config ou ao arquivo Web. config raiz. Se você atualizar a versão do compilador padrão para 3,5 no arquivo Machine. config, poderá alterá-lo de volta para 2,0 em uma base por aplicativo usando o `<providerOption>` elemento no arquivo de configuração do aplicativo.  
  
 Os implementadores do provedor de código do CodeDOM podem processar opções personalizadas fornecendo um construtor que usa um `providerOptions` parâmetro do tipo <xref:System.Collections.Generic.IDictionary%602> .  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra como especificar que a versão 3,5 do provedor de código C# deve ser usada.  
  
```xml  
<configuration>  
  <system.codedom>  
    <compilers>  
      <!-- zero or more compiler elements -->  
      <compiler  
        language="c#;cs;csharp"  
        extension=".cs"  
        type="Microsoft.CSharp.CSharpCodeProvider, System,
          Version=2.0.3600.0, Culture=neutral,
          PublicKeyToken=b77a5c561934e089"  
        compilerOptions="/optimize"  
        warningLevel="1" >  
          <providerOption  
            name="CompilerVersion"  
            value="v3.5" />  
      </compiler>  
    </compilers>  
  </system.codedom>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.CodeDom.Compiler.CompilerInfo>
- <xref:System.CodeDom.Compiler.CodeDomProvider>
- [Esquema de arquivos de configuração](../index.md)
- [\<compilers>Elementos](compilers-element.md)
- [Especificando nomes de tipo totalmente qualificados](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md)
- [Elemento do compilador para compiladores para compilação (esquema de configurações do ASP.NET)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/a15ebt6c(v=vs.100))
