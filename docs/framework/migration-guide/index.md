---
title: Guia de migração para o .NET Framework 4.8, 4.7, 4.6 e 4.5
description: Um guia para migrar para versões mais recentes do .NET Framework que inclui recursos para novos recursos e compatibilidade de aplicativos.
ms.date: 04/18/2019
helpviewer_keywords:
- .NET Framework, migrating applications to
- migration, .NET Framework
ms.assetid: 02d55147-9b3a-4557-a45f-fa936fadae3b
ms.openlocfilehash: c597f6e7b67e190ffe61a425506a1a34f254942c
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558498"
---
# <a name="migrate-to-net-framework-48-47-46-and-45"></a>Migrar para o .NET Framework 4,8, 4,7, 4,6 e 4,5

Se você criou seu aplicativo usando uma versão anterior do .NET Framework, você geralmente pode atualizá-lo para .NET Framework 4,5 e suas versões de ponto (4.5.1 e 4.5.2), .NET Framework 4,6 e suas versões pontuais (4.6.1 e 4.6.2), .NET Framework 4,7 e suas versões pontuais (4.7.1 e 4.7.2) ou .NET Framework 4,8 facilmente. Abra o projeto no Visual Studio. Se o seu projeto tiver sido criado em uma versão anterior do Visual Studio, a caixa de diálogo **Compatibilidade do Projeto** abrirá automaticamente. Para saber mais sobre como atualizar um projeto no Visual Studio, consulte [Portar, migrar e atualizar projetos do Visual Studio](/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects) e [Direcionamento e compatibilidade da plataforma Visual Studio 2019](/visualstudio/releases/2019/compatibility).

 No entanto, algumas alterações no .NET Framework exigem alterações no código. Convém também aproveitar a nova funcionalidade no .NET Framework 4.5 e suas versões de correção, no .NET Framework 4.6 e suas versões de correção, no .NET Framework 4.7 e suas versões de correção ou no .NET Framework 4.8. Fazer esses tipos de alterações em seu aplicativo para uma nova versão do .NET Framework normalmente é chamado de *migração*. Se seu aplicativo não precisar ser migrado, você poderá executá-lo no .NET Framework 4,5 ou em uma versão posterior sem recompilá-lo.

## <a name="migration-resources"></a>Recursos de migração

Examine os documentos a seguir antes de migrar seu aplicativo de versões anteriores do .NET Framework para a versão 4,5, 4.5.1, 4.5.2, 4,6, 4.6.1, 4.6.2, 4,7, 4.7.1, 4.7.2 ou 4,8:

- Confira [Versões e dependências](versions-and-dependencies.md) para compreender a versão do CLR subjacente a cada versão do .NET Framework e examinar diretrizes para segmentação de seus aplicativos com êxito.

- Examine a [compatibilidade de aplicativos](application-compatibility.md) para saber mais sobre o tempo de execução e redirecionar as alterações que podem afetar seu aplicativo e como tratá-las.

- Confira [O que está obsoleto na Biblioteca de Classes](../whats-new/whats-obsolete.md) para determinar todos os tipos ou membros no seu código que ficaram obsoletos e as alternativas recomendadas.

- Confira [Novidades](../whats-new/index.md) para obter descrições dos novos recursos que você talvez queira adicionar ao seu aplicativo.

## <a name="see-also"></a>Confira também

- [Compatibilidade de aplicativos](application-compatibility.md)
- [Migrar do .NET Framework 1.1](migrating-from-the-net-framework-1-1.md)
- [Compatibilidade entre versões](version-compatibility.md)
- [Versões e dependências](versions-and-dependencies.md)
- [Como configurar um aplicativo para dar suporte a .NET Framework 4 ou versões posteriores](how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
- [Novidades](../whats-new/index.md)
- [O que está obsoleto na biblioteca de classes](../whats-new/whats-obsolete.md)
- [.NET Framework a política de suporte oficial](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)
- [Problemas de migração do .NET Framework 4](net-framework-4-migration-issues.md)
