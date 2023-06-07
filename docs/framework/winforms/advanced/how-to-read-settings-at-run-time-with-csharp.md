---
title: Como ler configurações em tempo de execução com C#
description: Saiba como ler as configurações no escopo do aplicativo e no escopo do usuário em tempo de execução com C# por meio do objeto de propriedades.
ms.date: 03/30/2017
helpviewer_keywords:
- application settings [Windows Forms], reading
- application settings [Windows Forms], run time
- application settings [Windows Forms], C#
ms.assetid: dbe8bf09-5e1c-49da-9192-154033d7240b
ms.openlocfilehash: d784544e018bb693c501e35ea36fcae1946ef5eb
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903344"
---
# <a name="how-to-read-settings-at-run-time-with-c"></a>Como: ler configurações em tempo de execução com C\#

Você pode ler as configurações no escopo do aplicativo e no escopo do usuário em tempo de execução por meio do objeto de propriedades. O objeto Properties expõe todas as configurações padrão do projeto por meio do membro Properties. Settings. default no namespace padrão do projeto em que estão definidas.  
  
## <a name="to-read-settings-at-run-time-with-c"></a>Para ler as configurações em tempo de execução com C\#
  
Acesse a configuração apropriada por meio do membro Propriedades. Settings. default. O exemplo a seguir mostra como atribuir uma configuração chamada `myColor` a uma propriedade BackColor. Ele requer que você tenha criado anteriormente um arquivo de configurações que contém uma configuração nomeada `myColor` do tipo `System.Drawing.Color` . Para obter informações sobre como criar um arquivo de configurações, consulte [como: criar uma nova configuração em tempo de design](how-to-create-a-new-setting-at-design-time.md).  
  
```csharp
this.BackColor = Properties.Settings.Default.myColor;  
```  
  
## <a name="see-also"></a>Veja também

- [Usando configurações do aplicativo e configurações do usuário](using-application-settings-and-user-settings.md)
- [Como gravar configurações do usuário em tempo de execução com C#](how-to-write-user-settings-at-run-time-with-csharp.md)
- [Visão geral das configurações do aplicativo](application-settings-overview.md)
