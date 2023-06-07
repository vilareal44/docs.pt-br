---
title: Elemento <compiler>
ms.date: 08/14/2018
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#compiler
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.codedom/compilers/compiler
helpviewer_keywords:
- compiler configuration elements, <compiler> element
- <compiler> element
- compiler configuration attributes
- compiler element
ms.assetid: 7a151659-b803-4c27-b5ce-1c4aa0d5a823
ms.openlocfilehash: 46676f25597f85596598d6f67c98930971cb0447
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74088052"
---
# <a name="compiler-element"></a>Elemento \<compiler>

Especifica os atributos de configuração do compilador para um provedor de linguagem.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.codedom>**](system-codedom-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<compilers>**](compilers-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<compiler>**

## <a name="syntax"></a>Sintaxe

```xml
<compiler
  language="languageName[;...;...]"
  extension="fileExtension[;...;...]"
  type="typeName, assemblyName"
  warningLevel="number"
  compilerOptions="option1 option2"
/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`compilerOptions`|Atributo opcional.<br /><br /> Especifica argumentos adicionais específicos do compilador para compilação. Os valores para o `compilerOptions` atributo são normalmente listados em um tópico de opções de compilador para o compilador.|
|`extension`|Atributo obrigatório.<br /><br /> Fornece uma lista separada por ponto-e-vírgula das extensões de nome de arquivo usadas pelos arquivos de origem para o provedor de idiomas. Por exemplo, ".cs".|
|`language`|Atributo obrigatório.<br /><br /> Fornece uma lista separada por ponto-e-vírgula de nomes de idiomas com suporte pelo provedor de idiomas. Por exemplo, "c#;cs;csharp".|
|`type`|Atributo obrigatório.<br /><br /> Especifica o nome do tipo do provedor de idioma, incluindo o nome do assembly que contém a implementação do provedor. O nome do tipo deve atender aos requisitos definidos na [especificação de nomes de tipo totalmente qualificados](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md).|
|`warningLevel`|Atributo opcional.<br /><br /> Especifica o nível de aviso do compilador padrão; determina o nível no qual o provedor de idioma trata avisos de compilação como erros.|

### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|
|-------------|-----------------|
|[\<providerOption>Elementos](provideroption-element.md)|Especifica os atributos de versão do compilador para um provedor de idiomas.|

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|[\<configuration>Elementos](../configuration-element.md)|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|[\<system.codedom>Elementos](system-codedom-element.md)|Especifica as definições de configuração do compilador para provedores de linguagem disponíveis.|
|[\<compilers>Elementos](compilers-element.md)|Contêiner para elementos de configuração do compilador; contém zero ou mais `<compiler>` elementos.|

## <a name="remarks"></a>Comentários

Cada `<compiler>` elemento Especifica os atributos de configuração do compilador para um provedor de idioma específico. O provedor estende a <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType> classe para um idioma específico; o `<compiler>` elemento define as configurações do compilador e do gerador de código para o provedor de idiomas.

O .NET Framework define as configurações do compilador iniciais no arquivo de configuração do computador (Machine.config). Os desenvolvedores e fornecedores do compilador podem adicionar parâmetros de configuração em uma nova implementação do <xref:System.CodeDom.Compiler.CodeDomProvider>. Use o método <xref:System.CodeDom.Compiler.CodeDomProvider.GetAllCompilerInfo%2A?displayProperty=nameWithType> para enumerar programaticamente as definições de configuração do compilador e do provedor de linguagem em um computador.

Os elementos do compilador no aplicativo ou arquivo de configuração da Web podem complementar ou substituir as configurações no arquivo de configuração da máquina. Se mais de uma implementação de provedor estiver configurada para o mesmo nome de idioma ou a mesma extensão de arquivo, a última configuração de correspondência substituirá quaisquer provedores configurados anteriormente para esse nome de idioma ou extensão de arquivo.

## <a name="configuration-file"></a>Arquivo de configuração

Esse elemento pode ser usado no arquivo de configuração da máquina e no arquivo de configuração do aplicativo.

## <a name="example"></a>Exemplo

O exemplo a seguir ilustra um elemento de configuração de compilador típico:

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
        warningLevel="1" />
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
