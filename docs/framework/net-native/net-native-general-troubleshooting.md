---
title: Solução de problemas gerais do .NET Nativo
ms.date: 03/30/2017
ms.assetid: ee8c5e17-35ea-48a1-8767-83298caac1e8
ms.openlocfilehash: 2bea81e380fed6c456898e9883658ef874c8dd97
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128233"
---
# <a name="net-native-general-troubleshooting"></a>Solução de problemas gerais do .NET Nativo

Este tópico descreve como solucionar problemas potenciais que você pode encontrar ao desenvolver aplicativos com o .NET Native.

- **Problema:** a janela de saída do build não é atualizada corretamente.

  **Resolução:** a janela de saída do build não será atualizada até que o build seja concluído. Os tempos de compilação podem levar vários minutos, por isso pode ocorrer um atraso para exibir as atualizações.

- **Problema:** o tempo de build de varejo do aplicativo para ARM aumentou.

  **Resolução:** Quando você implanta um aplicativo em seu dispositivo ARM, a infraestrutura de .NET Native é invocada. Essa compilação executa um grande número de otimizações, garantindo que semânticas não estáticas como reflexão continuem a funcionar. Além disso, a parte do .NET Framework que o aplicativo usa está vinculada estaticamente para otimizar o desempenho e também precisa ser compilada em código nativo. É por isso que a compilação leva mais tempo.

  No entanto, o tempo de compilação ainda está dentro de um minuto da compilação padrão para a maioria dos aplicativos em uma máquina de desenvolvimento padrão.  Normalmente, apenas gerar imagens nativas para o .NET Framework em uma máquina de desenvolvimento padrão leva vários minutos.  Mesmo com todas as otimizações para melhorar o código gerado e incluindo o .NET Framework, os tempos de compilação do aplicativo são normalmente de um ou dois minutos.

  Continuamos a trabalhar para aprimorar o desempenho da compilação investigando a multi-threaded compilação e outras otimizações.

- **Problema:** Você não sabe se seu aplicativo foi compilado usando .NET Native.

  **Resolução:** Se o compilador de .NET Native for invocado, você perceberá tempos de compilação mais longos e o Gerenciador de tarefas mostrará vários processos de componentes de .NET Native, como ILC. exe e nutc_driver. exe.

  Depois de criar com êxito seu projeto com .net Native, você encontrará a saída em obj \\ *config* \  *Arch* \\ *ProjectName*. ilc\out.  O conteúdo final do pacote nativo pode ser encontrado em \\ *bin* \\ *config*\AppX. O conteúdo final do pacote nativo estará sob \Bin \\ *Arch* \\ *config*\AppX se você tiver implantado o aplicativo.

- **Problema:** seu aplicativo compilado com o .NET Native está lançando exceções de tempo de execução (normalmente exceções [MissingMetadataException](missingmetadataexception-class-net-native.md) ou [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md)) que ele não lançava quando era compilado sem o .NET Native.

  **Resolução:** as exceções são geradas porque o .NET Native não forneceu os metadados nem o código de implementação que normalmente fica disponível por meio de reflexão. (Para obter mais informações, consulte [.net Native e compilação](net-native-and-compilation.md).) Para eliminar a exceção, você precisa adicionar uma entrada ao arquivo de [diretivas de tempo de execução (RD. xml)](runtime-directives-rd-xml-configuration-file-reference.md) para que a cadeia de ferramentas de .net Native possa disponibilizar os metadados ou o código de implementação no tempo de execução. Há duas soluções de problemas disponíveis que criarão a entrada necessária para adicionar ao seu arquivo de diretivas de runtime:

  - A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/type.html) para tipos.

  - A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/method.html) para métodos.

  Para obter mais informações, consulte [Reflexão e .NET Native](reflection-and-net-native.md).

## <a name="see-also"></a>Confira também

- [Migrando seu aplicativo da Windows Store para .NET Nativo](migrating-your-windows-store-app-to-net-native.md)
