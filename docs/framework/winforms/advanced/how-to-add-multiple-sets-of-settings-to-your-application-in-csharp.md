---
title: Como adicionar vários conjuntos de configurações a um aplicativo em C#
description: Saiba como adicionar vários conjuntos de configurações de Windows Forms ao seu aplicativo em C# usando o Visual Studio.
ms.date: 03/30/2017
helpviewer_keywords:
- application settings [Windows Forms], multiple sets
- application settings [Windows Forms], C#
ms.assetid: 45007ac6-cf07-4be7-bc38-3f0ef962faf9
ms.openlocfilehash: 579374ff101758b3224bc122c46b891280ed2609
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86173439"
---
# <a name="how-to-add-multiple-sets-of-settings-to-your-application-in-c"></a>Como: adicionar vários conjuntos de configurações ao seu aplicativo em C\#

Em alguns casos, convém ter vários conjuntos de configurações em um aplicativo. Por exemplo, se você estiver desenvolvendo um aplicativo no qual se supõe que um determinado grupo de configurações deve ser alterado com frequência, talvez seja mais inteligente separá-las em um único arquivo, de modo que esse arquivo possa ser substituído em massa sem que as demais configurações sejam afetadas. O Visual Studio lhe permite adicionar vários conjuntos de configurações ao projeto. Conjuntos de configurações adicionais podem ser acessados por meio do `Properties.Settings` objeto.

## <a name="add-an-additional-set-of-settings"></a>Adicionar um conjunto adicional de configurações

1. No Visual Studio, no menu **projeto** , escolha **Adicionar novo item**.

   A caixa de diálogo **Adicionar Novo Item** é aberta.

2. Na caixa de diálogo **Adicionar novo item** , selecione **arquivo de configurações**, insira um nome para o arquivo e clique em **Adicionar** para adicionar um novo arquivo de configurações à sua solução.

3. No **Gerenciador de Soluções**, arraste o novo arquivo de configurações para a pasta **Propriedades**. Isso faz com que as suas novas configurações fiquem disponíveis no código.

4. Adicione e use configurações nesse arquivo da mesma forma que faria em qualquer outro arquivo de configurações. Você pode acessar esse grupo de configurações por meio do `Properties.Settings` objeto.

## <a name="see-also"></a>Veja também

- [Usando configurações do aplicativo e configurações do usuário](using-application-settings-and-user-settings.md)
- [Visão geral das configurações do aplicativo](application-settings-overview.md)
