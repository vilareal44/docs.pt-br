---
title: Definir atributos do assembly
description: Você pode definir atributos de assembly para um assembly .NET, incluindo identidade de assembly, informativo, manifesto de assembly e atributos de nome forte.
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET Framework], attributes
- assembly binding, attributes
- assembly manifest, attributes
ms.assetid: 36a98a81-b5b5-4c19-912a-11f91eff7f4e
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: e3a077dcd1b62a4676a3ac6492a90e38c548e41b
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83378643"
---
# <a name="set-assembly-attributes"></a>Definir atributos do assembly

Os atributos de assembly são valores que fornecem informações sobre um assembly. Os atributos são divididos nos seguintes conjuntos de informações:

- Atributos de identidade do assembly

- Atributos informativos

- Atributos de manifesto do assembly

- Atributos de nome forte

## <a name="assembly-identity-attributes"></a>Atributos de identidade do assembly

Três atributos, com um nome forte (se aplicável), determinam a identidade de um assembly: nome, versão e cultura. Esses atributos formam o nome completo do assembly e são necessários ao fazer referência ao assembly no código. Você pode usar atributos para definir a versão e a cultura de um assembly. O compilador ou o [Assembly Linker (Al.exe)](../../framework/tools/al-exe-assembly-linker.md) define o valor do nome quando o assembly é criado, com base no arquivo que contém o manifesto do assembly.

A tabela a seguir descreve os atributos de versão e cultura.

|Atributo de identidade do assembly|Descrição|
|---------------------------------|-----------------|
|<xref:System.Reflection.AssemblyCultureAttribute>|Campo enumerado que indica a cultura compatível com o assembly. Um assembly também pode especificar a independência da cultura, indicando que ela contém os recursos para a cultura padrão. **Observação:** o runtime trata qualquer assembly que não tenha o atributo de cultura definido como nulo, como um assembly satélite. Esses assemblies estão sujeitos às regras de associação de assembly satélite. Para obter mais informações, consulte [como o tempo de execução localiza assemblies](../../framework/deployment/how-the-runtime-locates-assemblies.md).|
|<xref:System.Reflection.AssemblyFlagsAttribute>|O valor que define os atributos de assembly; por exemplo, se o assembly pode ser executado lado a lado.|
|<xref:System.Reflection.AssemblyVersionAttribute>|Valor numérico no formato *principal*.*secundário*.*compilação*.*revisão* (por exemplo, 2.4.0.0). O Common Language Runtime usa esse valor para executar operações de associação em assemblies com nome forte. **Observação:**  Se o <xref:System.Reflection.AssemblyInformationalVersionAttribute> atributo não for aplicado a um assembly, o número de versão especificado pelo <xref:System.Reflection.AssemblyVersionAttribute> atributo será usado pelas <xref:System.Windows.Forms.Application.ProductVersion%2A?displayProperty=nameWithType> Propriedades, <xref:System.Windows.Forms.Application.UserAppDataPath%2A?displayProperty=nameWithType> e <xref:System.Windows.Forms.Application.UserAppDataRegistry%2A?displayProperty=nameWithType> .|

O exemplo de código a seguir mostra como aplicar os atributos de versão e cultura a um assembly.

```cpp
// Set version number for the assembly.
[assembly:AssemblyVersionAttribute("4.3.2.1")];
// Set culture as German.
[assembly:AssemblyCultureAttribute("de")];
```

```csharp
// Set version number for the assembly.
[assembly:AssemblyVersionAttribute("4.3.2.1")]
// Set culture as German.
[assembly:AssemblyCultureAttribute("de")]
```

```vb
' Set version number for the assembly.
<Assembly:AssemblyVersionAttribute("4.3.2.1")>
' Set culture as German.
<Assembly:AssemblyCultureAttribute("de")>
```

## <a name="informational-attributes"></a>Atributos informativos

Você pode usar atributos informativos para fornecer informações adicionais corporativas ou de produto para um assembly. A tabela a seguir descreve os atributos informativos que você pode aplicar a um assembly.

|Atributos informativos|Descrição|
|-----------------------------|-----------------|
|<xref:System.Reflection.AssemblyCompanyAttribute>|O valor de cadeia de caracteres que especifica um nome de empresa.|
|<xref:System.Reflection.AssemblyCopyrightAttribute>|O valor de cadeia de caracteres que especifica informações sobre direitos autorais.|
|<xref:System.Reflection.AssemblyFileVersionAttribute>|O valor de cadeia de caracteres que especifica o número de versão do arquivo Win32. Isso normalmente tem como padrão a versão do assembly.|
|<xref:System.Reflection.AssemblyInformationalVersionAttribute>|O valor de cadeia de caracteres que especifica informações de versão que não são usadas pelo Common Language Runtime, como o número de versão completo de um produto. **Observação:** se esse atributo for aplicado a um assembly, a cadeia de caracteres que ele especifica poderá ser obtida no tempo de execução usando a propriedade <xref:System.Windows.Forms.Application.ProductVersion%2A?displayProperty=nameWithType>. A cadeia de caracteres também é usada na chave do Registro e no caminho fornecidos pelas propriedades <xref:System.Windows.Forms.Application.UserAppDataPath%2A?displayProperty=nameWithType> e <xref:System.Windows.Forms.Application.UserAppDataRegistry%2A?displayProperty=nameWithType>.|
|<xref:System.Reflection.AssemblyProductAttribute>|O valor de cadeia de caracteres que especifica informações do produto.|
|<xref:System.Reflection.AssemblyTrademarkAttribute>|O valor de cadeia de caracteres que especifica informações de marca registrada.|

Esses atributos podem aparecer na página Propriedades do Windows do assembly ou podem ser substituídos usando a opção de compilador **/win32res** para especificar seu próprio arquivo de recurso Win32.

## <a name="assembly-manifest-attributes"></a>Atributos de manifesto do assembly

Também é possível usar atributos de manifesto do assembly para fornecer informações no manifesto do assembly, incluindo título, descrição, o alias padrão e a configuração. A tabela a seguir descreve os atributos de manifesto do assembly.

|Atributo de manifesto do assembly|Descrição|
|---------------------------------|-----------------|
|<xref:System.Reflection.AssemblyConfigurationAttribute>|O valor de cadeia de caracteres que indica a configuração do assembly, como Retail ou Depuração. O runtime não usa esse valor.|
|<xref:System.Reflection.AssemblyDefaultAliasAttribute>|O valor de cadeia de caracteres que especifica um alias padrão a ser usado ao fazer referências de assemblies. Esse valor fornece um nome amigável quando o nome do assembly em si não o for (como um valor GUID). Esse valor também pode ser usado como uma forma abreviada do nome completo do assembly.|
|<xref:System.Reflection.AssemblyDescriptionAttribute>|O valor de cadeia de caracteres que especifica uma breve descrição que resume a natureza e o objetivo do assembly.|
|<xref:System.Reflection.AssemblyTitleAttribute>|O valor de cadeia de caracteres que especifica um nome amigável para o assembly. Por exemplo, um assembly chamado *comdlg* pode ter o título controle de caixa de diálogo comum da Microsoft.|

## <a name="strong-name-attributes"></a>Atributos de nome forte

Você pode usar atributos de nome forte para definir um nome forte para um assembly. A tabela a seguir descreve os atributos de nome forte.

|Atributo de nome forte|Descrição|
|----------------------------|-----------------|
|<xref:System.Reflection.AssemblyDelaySignAttribute>|O valor booliano que indica que a assinatura em atraso está sendo usada.|
|<xref:System.Reflection.AssemblyKeyFileAttribute>|O valor de cadeia de caracteres que indica o nome do arquivo que contém a chave pública (se usando assinatura em atraso) ou as chaves pública e privada passadas como um parâmetro ao construtor desse atributo. Observe que o nome do arquivo é relativo ao caminho do arquivo de saída (o *. exe* ou *. dll*), não o caminho do arquivo de origem.|
|<xref:System.Reflection.AssemblyKeyNameAttribute>|Indica o contêiner de chaves que contém o par de chaves passado como um parâmetro ao construtor desse atributo.|

O exemplo de código a seguir mostra os atributos a serem aplicados ao usar a assinatura de atraso para criar um assembly de nome forte com um arquivo de chave pública chamado *myKey. SNK*.

```cpp
[assembly:AssemblyKeyFileAttribute("myKey.snk")];
[assembly:AssemblyDelaySignAttribute(true)];
```

```csharp
[assembly:AssemblyKeyFileAttribute("myKey.snk")]
[assembly:AssemblyDelaySignAttribute(true)]
```

```vb
<Assembly:AssemblyKeyFileAttribute("myKey.snk")>
<Assembly:AssemblyDelaySignAttribute(True)>
```

## <a name="see-also"></a>Veja também

- [Criar assemblies](create.md)
