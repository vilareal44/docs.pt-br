---
title: Elemento <assemblyBinding> para <configuration>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/assemblyBinding
helpviewer_keywords:
- assemblyBinding Element
- <assemblyBinding> Element
ms.assetid: 6cc55983-b894-449b-8e26-b258e53939cd
ms.openlocfilehash: 21cf5e749b0dae310c3326f8abf82c6678fc97e9
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79155473"
---
# <a name="assemblybinding-element-for-configuration"></a>Elemento \<assemblyBinding> para \<configuration>

Especifica a diretiva de ligação de assembly no nível de configuração.

[**\<configuration>**](configuration-element.md) &nbsp;&nbsp;**\<assemblyBinding>**

## <a name="syntax"></a>Sintaxe

```xml
<assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
  <!-- Configuration files to include. -->
</assemblyBinding>
```

## <a name="attribute"></a>Atributo

|           | Descrição |
| --------- | ----------- |
| **xmlns** | Atributo obrigatório.<br><br>Especifica o namespace XML necessário para a associação de assembly. Use a cadeia de caracteres "urn: schemas-microsoft-com: asm. v1" como o valor. |

## <a name="parent-element"></a>Elemento pai

|     | Descrição |
| --- | ----------- |
| [**\<configuration>**](configuration-element.md) | O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework. |

## <a name="child-element"></a>Elemento filho

|     | Descrição |
| --- | ----------- |
| [**\<linkedConfiguration>**](linkedconfiguration-element.md) | Especifica um arquivo de configuração a ser incluído. |

## <a name="remarks"></a>Comentários

O [**\<linkedConfiguration>**](linkedconfiguration-element.md) elemento simplifica o gerenciamento de assemblies de componente, permitindo que os arquivos de configuração de aplicativo incluam arquivos de configuração de assembly em locais bem conhecidos, em vez de duplicar as definições de configuração de assembly.

> [!NOTE]
> O **\<linkedConfiguration>** elemento não tem suporte para aplicativos com manifestos lado a lado do Windows.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como incluir um arquivo de configuração no disco rígido local:

```xml
<configuration>
  <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
    <linkedConfiguration href="file://c:\Program Files\Contoso\sharedConfig.xml" />
  </assemblyBinding>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração para o .NET Framework](index.md)
