---
title: Elemento <configuration>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration
helpviewer_keywords:
- <configuration> element
- configuration element
- container tags, <configuration> element
ms.assetid: 2ec1c9dc-2e5c-4ef0-9958-81670ab88449
ms.openlocfilehash: 0e09ec49024b769c516fd97085904781f64b4486
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "69921247"
---
# <a name="configuration-element"></a>Elemento \<configuration>

O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.

**\<configuration>**

## <a name="syntax"></a>Sintaxe

```xml
<configuration>
  <!-- Configuration settings -->
</configuration>
```

## <a name="attributes"></a>Atributos

Nenhum

## <a name="parent-element"></a>Elemento pai

Nenhum

## <a name="child-elements"></a>Elementos filho

|     | Descrição |
| --- | ----------- |
| [**\<assemblyBinding>**](assemblybinding-element-for-configuration.md) | Especifica a diretiva de ligação de assembly no nível de configuração.|
| [**\<startup>** Esquema de configurações](./startup/index.md) | Todos os elementos no esquema de configurações de inicialização. |
| [**\<runtime>** Esquema de configurações](./runtime/index.md) | Todos os elementos no esquema de configurações de tempo de execução. |
| [**\<system.runtime.remoting>** Esquema de configurações](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/z415cf9a(v=vs.100)) | Todos os elementos no esquema de configurações de comunicação remota. |
| [**\<system.Net>** Esquema de configurações](./network/index.md) | Todos os elementos no esquema de configurações de rede. |
| [**\<cryptographySettings>** Esquema de configurações](./cryptography/index.md) | Todos os elementos no esquema de configurações de criptografia. |
| [**\<configuration>** Esquema de seções](configuration-sections-schema.md) | Todos os elementos na seção de configuração esquema de configurações. |
| [Esquema de configurações de rastreamento e depuração](./trace-debug/index.md) | Todos os elementos no esquema de configurações de rastreamento e depuração. |
| [Esquema de definições de configuração do ASP.NET](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/b5ysx397(v=vs.100)) | Todos os elementos no esquema de configuração do ASP.NET, que incluem elementos para configurar sites e aplicativos do ASP.NET. Usado em arquivos *Web. config* . |
| [**\<webServices>** Esquema de configurações](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cctwteet(v=vs.100)) | Todos os elementos no esquema de configurações de serviços Web. |
| [Esquema de configurações Web](./web/index.md) | Todos os elementos do esquema de configurações da Web, que incluem elementos para configurar como o ASP.NET, funciona com um aplicativo host, como o IIS. Usado em arquivos *ASPNET. config* . |

## <a name="remarks"></a>Comentários

Cada arquivo de configuração deve conter exatamente um **\<configuration>** elemento.

## <a name="see-also"></a>Confira também

- [Esquema do arquivo de configuração para o .NET Framework](index.md)
