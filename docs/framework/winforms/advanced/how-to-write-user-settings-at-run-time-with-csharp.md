---
title: Como gravar configurações do usuário em tempo de execução com C#
description: Saiba como escrever configurações em tempo de execução com C#, mantendo as alterações nas configurações entre sessões de aplicativo chamando o método Save.
ms.date: 03/30/2017
helpviewer_keywords:
- application settings [Windows Forms], run time
- application settings [Windows Forms], writing user settings
- application settings [Windows Forms], C#
ms.assetid: 9d061c7d-b33b-470f-a36d-edccb1d6f9a3
ms.openlocfilehash: 6154ff1b92d6c2d4c788299e59eb8f73e6b69c4b
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904319"
---
# <a name="how-to-write-user-settings-at-run-time-with-c"></a>Como: gravar configurações de usuário em tempo de execução com C\#

As configurações com escopo de aplicativo são somente leitura e só podem ser alteradas em tempo de design ou alterando o arquivo. config entre sessões de aplicativo. As configurações que têm escopo de usuário, no entanto, podem ser gravadas em tempo de execução, assim como você alteraria qualquer valor de propriedade. O novo valor persiste pela duração da sessão do aplicativo. Você pode manter as alterações nas configurações entre sessões do aplicativo chamando o método Save.  
  
## <a name="how-to-write-and-persist-user-settings-at-run-time-with-c"></a>Como: gravar e persistir configurações de usuário em tempo de execução com C\#
  
1. Acesse a configuração e atribua um novo valor, conforme mostrado neste exemplo:  
  
   ```csharp
   Properties.Settings.Default.myColor = Color.AliceBlue;  
   ```  
  
2. Se você quiser manter as alterações nas configurações entre as sessões do aplicativo, chame o método Save, conforme mostrado neste exemplo:  
  
    ```csharp
    Properties.Settings.Default.Save();  
    ```  
  
As configurações de usuário são salvas em um arquivo dentro de uma subpasta da pasta de dados de aplicativo oculto local do usuário.  
  
## <a name="see-also"></a>Veja também

- [Usando configurações do aplicativo e configurações do usuário](using-application-settings-and-user-settings.md)
- [Como ler configurações em tempo de execução com C#](how-to-read-settings-at-run-time-with-csharp.md)
- [Visão geral das configurações do aplicativo](application-settings-overview.md)
