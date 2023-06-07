---
title: Como criar uma nova configuração em tempo de design
description: Saiba como criar uma nova configuração de Windows Forms em tempo de design usando o Settings designer no Visual Studio.
ms.date: 03/30/2017
helpviewer_keywords:
- application settings [Windows Forms], design time
- application settings [Windows Forms], creating
ms.assetid: c5d60a66-6507-462f-a81f-e3bc0a804e16
ms.openlocfilehash: ce37b42191999e29de2f2f7f7e7abfa0ec3f4d47
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325838"
---
# <a name="how-to-create-a-new-setting-at-design-time"></a>Como: criar uma nova configuração em tempo de design

Você pode criar uma nova configuração no tempo de design usando o Settings designer no Visual Studio. O designer de configurações é uma interface de estilo de grade que permite criar novas configurações e especificar propriedades para essas configurações. Você deve especificar nome, valor, tipo e escopo para as novas configurações. Quando uma configuração é criada, ela é acessível no código.

## <a name="create-a-new-setting-at-design-time-in-c"></a>Criar uma nova configuração em tempo de design em C\#

1. Abra o Visual Studio.

2. Em **Gerenciador de soluções**, expanda o nó **Propriedades** do seu projeto.

3. Clique duas vezes no arquivo. Settings no qual você deseja adicionar uma nova configuração. O nome padrão para esse arquivo é Settings. Settings.

4. No designer de configurações, defina o **nome**, o **valor**, o **tipo**e o **escopo** da sua configuração. Cada linha representa uma única configuração.

## <a name="create-a-new-setting-at-design-time-in-visual-basic"></a>Criar uma nova configuração em tempo de design no Visual Basic

1. Abra o Visual Studio.

2. Em **Gerenciador de soluções**, clique com o botão direito do mouse no nó do projeto e escolha **Propriedades**.

3. Na página **Propriedades** , selecione a guia **configurações** .

4. No designer de configurações, defina o **nome**, o **valor**, o **tipo**e o **escopo** da sua configuração. Cada linha representa uma única configuração.

## <a name="see-also"></a>Veja também

- [Usando configurações do aplicativo e configurações do usuário](using-application-settings-and-user-settings.md)
- [Visão geral das configurações do aplicativo](application-settings-overview.md)
- [Como Alterar o Valor de uma Configuração Existente em Tempo de Design](how-to-change-the-value-of-an-existing-setting-at-design-time.md)
