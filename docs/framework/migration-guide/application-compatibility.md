---
title: Tempo de execução e redirecionamento de alterações-.NET Framework
description: Saiba mais sobre a compatibilidade de aplicativos no .NET Framework e como ele é afetado pelo tempo de execução e redirecionamento de alterações ao migrar para outra versão.
ms.date: 10/29/2019
helpviewer_keywords:
- application compatibility
- .NET Framework application compatibility
- .NET Framework changes
ms.assetid: c4ba3ff2-fe59-4c5d-9e0b-86bba3cd865c
ms.openlocfilehash: 26f36dd34c6c5ecae8fc5ab373ff60d9e56f8245
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475483"
---
# <a name="application-compatibility-in-the-net-framework"></a>Compatibilidade de aplicativos no .NET Framework

A compatibilidade é uma meta importante de cada versão do .NET. A compatibilidade garante que cada versão seja aditiva, para que as versões anteriores continuem a funcionar. Por outro lado, as alterações na funcionalidade anterior (por exemplo, para melhorar o desempenho, solucionar problemas de segurança ou corrigir bugs) podem causar problemas de compatibilidade no código existente ou em aplicativos existentes que são executados em uma versão posterior.

Cada aplicativo tem como alvo uma versão específica do .NET Framework:

- Definição de uma estrutura de destino no Visual Studio.
- Especificação da estrutura de destino em um arquivo de projeto.
- Aplicação de um <xref:System.Runtime.Versioning.TargetFrameworkAttribute> ao código-fonte.

Ao migrar de uma versão do .NET Framework para outra, há dois tipos de alterações a serem consideradas:

- [Alterações em runtime](#runtime-changes)
- [Alterações de redirecionamento](#retargeting-changes)

## <a name="runtime-changes"></a>Alterações em runtime

Problemas de tempo de execução são aqueles que surgem quando um novo tempo de execução é colocado em um computador e o comportamento de um aplicativo é alterado. Ao executar em uma versão mais recente do que a que foi direcionada, o .NET Framework usa o comportamento *peculiar* para imitar a versão mais antiga de destino. O aplicativo é executado na versão mais recente, mas age como se ele fosse executado na versão mais antiga. Muitos dos problemas de compatibilidade entre versões do .NET Framework são atenuados com esse modelo de peculiaridade. Por exemplo, se um binário tiver sido compilado para .NET Framework 4,0, mas for executado em um computador com .NET Framework 4,5 ou posterior, ele será executado no modo de compatibilidade .NET Framework 4,0. Isso significa que muitas das alterações na versão posterior não afetam o binário.

A versão do .NET Framework que um aplicativo se destina é determinada pela versão de destino do assembly de entrada para o domínio do aplicativo no qual o código é executado. Todos os assemblies adicionais carregados nesse domínio de aplicativo são direcionados a essa versão. Por exemplo, no caso de um executável, a versão que o executável tem como destino é o modo de compatibilidade em que todos os assemblies são executados no domínio de aplicativo.

Para ver uma lista de alterações de tempo de execução que se aplicam ao seu ambiente, selecione a versão .NET Framework que você está direcionando atualmente e, em seguida, a versão para a qual você deseja migrar:

[!INCLUDE[versionselector](../../../includes/migration-guide/runtime/versionselector.md)]

## <a name="retargeting-changes"></a>Alterações de redirecionamento

As alterações de redirecionamento são aquelas que surgem quando um assembly é recompilado para ter como destino uma versão mais recente. Direcionar uma versão mais recente significa que o assembly incorpora os novos recursos, bem como possíveis problemas de compatibilidade para recursos antigos.

Para ver uma lista de alterações de redirecionamento que se aplicam ao seu ambiente, selecione a versão .NET Framework que você está direcionando atualmente e, em seguida, a versão para a qual você deseja migrar:

[!INCLUDE[versionselector](../../../includes/migration-guide/retargeting/versionselector.md)]

## <a name="impact-classification"></a>Classificação de impacto

Nos tópicos que descrevem o tempo de execução e redirecionando as alterações, por exemplo, [redirecionando as alterações para migrar do 4.7.2 para o 4,8](retargeting/4.7.2-4.8.md), os itens individuais são classificados pelo impacto esperado da seguinte maneira:

**Primária**\
Uma alteração significativa que afeta um grande número de aplicativos ou que exige a modificação significativa do código.

**Secundária**\
Uma alteração que afeta um pequeno número de aplicativos ou que exige modificação secundária do código.

**Caso de borda**\
Uma alteração que afeta aplicativos em cenários muito específicos que não são comuns.

**Fique**\
Uma alteração que não tem efeito visível para o desenvolvedor ou o usuário do aplicativo. O aplicativo não deve exigir modificação por conta dessa alteração.

## <a name="see-also"></a>Consulte também

- [Versões e dependências](versions-and-dependencies.md)
- [Novidades](../whats-new/index.md)
- [O que é obsoleto](../whats-new/whats-obsolete.md)
