---
title: Migrar do .NET Framework 1.1
description: Saiba mais sobre as etapas necessárias para executar um aplicativo compilado usando o .NET Framework 1,1 no Windows 7 ou posterior.
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework 4.5, migrating from 1.1
- .NET Framework 1.1, migrating to .NET Framework 4.5
ms.assetid: 7ead0cb3-3b19-414a-8417-a1c1fa198d9e
ms.openlocfilehash: f2b0e21ff5dbeab3395335f52799629859fb2d90
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475262"
---
# <a name="migrate-from-the-net-framework-11"></a>Migrar do .NET Framework 1,1

O Windows 7 e versões posteriores do sistema operacional Windows não dão suporte ao .NET Framework 1,1. Como resultado, os aplicativos que se destinam ao .NET Framework 1,1 não serão executados sem modificações no Windows 7 ou versões posteriores do sistema operacional. Este tópico discute as etapas necessárias para executar um aplicativo que tem como alvo o .NET Framework 1,1 no Windows 7 e versões posteriores do sistema operacional Windows. Para obter mais informações sobre o .NET Framework 1,1 e o Windows 8, consulte [executar .NET Framework aplicativos 1,1 no Windows 8 e versões posteriores](../install/run-net-framework-1-1-apps.md).

## <a name="retarget-or-recompile"></a>Redirecionar ou recompilar

Há duas maneiras de obter um aplicativo que foi compilado usando o .NET Framework 1,1 para ser executado no Windows 7 ou em um sistema operacional Windows posterior:

- Redirecione o aplicativo para que ele seja executado sob .NET Framework 4 e versões posteriores. O redirecionamento requer que você adicione um [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) elemento ao arquivo de configuração do aplicativo que permita que ele seja executado no .NET Framework 4 e versões posteriores. Esse arquivo de configuração utiliza a seguinte forma:

    ```xml
    <configuration>
       <startup>
          <supportedRuntime version="v4.0"/>
       </startup>
    </configuration>
    ```

- Recompile o aplicativo com um compilador que tem como alvo o .NET Framework 4 ou uma versão posterior. Se tiver usado originalmente o Visual Studio 2003 para desenvolver e compilar sua solução, você poderá abrir a solução no Visual Studio 2010 (e possivelmente versões posteriores também) e usar a caixa de diálogo **Compatibilidade do Projeto** para converter a solução e os arquivos do projeto dos formatos usados pelo Visual Studio 2003 para o formato MSBuild (Microsoft Build Engine).

Independentemente de preferir recompilar ou redirecionar seu aplicativo, você deve determinar se o aplicativo é afetado por alguma alterações introduzida em versões posteriores do .NET Framework. Essas alterações são de dois tipos:

- Alterações significativas ocorridas entre o .NET Framework 1.1 e as versões posteriores do .NET Framework.

- Tipos e membros do tipo que foram marcados como preteridos ou obsoletos entre o .NET Framework 1.1 e versões posteriores do .NET Framework.

Se redirecionar seu aplicativo ou recompilá-lo, você deverá examinar as alterações significativas e os tipos e membros obsoletos de cada versão do .NET Framework liberada após o .NET Framework 1.1.

## <a name="breaking-changes"></a>Alterações de quebra

Quando ocorre uma alteração significativa, dependendo da alteração específica, a solução desse problema pode estar disponível tanto para aplicativos redestinados como para recompilados. Em alguns casos, você pode adicionar um elemento filho ao [\<runtime>](../configure-apps/file-schema/startup/supportedruntime-element.md) elemento do arquivo de configuração do aplicativo para restaurar o comportamento anterior. Por exemplo, o arquivo de configuração a seguir restaura a classificação da cadeia de caracteres e o comportamento de comparação usados no .NET Framework 1.1 e pode ser usado com um aplicativo redirecionado ou recompilado.

```xml
<configuration>
   <runtime>
      <CompatSortNLSVersion enabled="4096"/>
   </runtime>
</configuration>
```

No entanto, em alguns casos, você talvez tenha que modificar o código-fonte e recompilar seu aplicativo.

Para avaliar o impacto de alterações significativas possíveis em seu aplicativo, você deve examinar a seguinte lista de alterações:

- [Alterações significativas no .NET Framework 2.0](https://docs.microsoft.com/previous-versions/aa570326(v=msdn.10)) documenta alterações no .NET Framework 2.0 SP1 que podem afetar um aplicativo destinado ao .NET Framework 1.1.

- [Alterações no .NET Framework 3.5 SP1](https://docs.microsoft.com/previous-versions/dotnet/articles/dd310284(v=msdn.10)) documenta alterações entre o NET Framework 3.5 e o .NET Framework 3.5 SP1.

- [Problemas de migração do .NET Framework 4](net-framework-4-migration-issues.md) documenta alterações entre o .NET Framework 3.5 SP1 e o .NET Framework 4.

## <a name="obsolete-types-and-members"></a>Tipos e membros obsoletos

O impacto de tipos e membros substituídos é um pouco diferente para aplicativos redirecionados e aplicativos recompilados. O uso de tipos e de membros obsoletos não afetará um aplicativo que foi escolhido novamente como destino, a menos que o tipo ou membro obsoleto tenha sido removido fisicamente do assembly. A recompilação de um aplicativo que usa tipos ou associados obsoletos geralmente resulta em um aviso de compilador, em vez de um erro do compilador. No entanto, em alguns casos, ele produz um erro do compilador, e o código que usa o tipo ou membro obsoleto não é compilado com êxito. Neste caso, você deve reescrever o código-fonte que chama o tipo ou membro obsoleto antes de recompilar seu aplicativo. Para saber mais sobre os tipos e membros obsoletos, veja [O que está obsoleto na Biblioteca de Classes](../whats-new/whats-obsolete.md).

Para avaliar o impacto dos tipos e membros que foram substituídos desde o lançamento do .NET Framework 2.0 SP1, confira [O que está obsoleto na biblioteca de classes](../whats-new/whats-obsolete.md). Examine as listas de tipos e de membros obsoletos do .NET Framework 2.0 SP1, .NET Framework 3.5 e .NET Framework 4.
