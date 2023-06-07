---
title: Depurar árvores de expressão no Visual Studio
ms.date: 07/20/2015
ms.assetid: 492cc28f-b7a2-4c47-b582-b3c437b8a5d5
ms.openlocfilehash: 3811958353d1d55ce74da41c6a45dbe730cc9790
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84375428"
---
# <a name="debugging-expression-trees-in-visual-studio-visual-basic"></a>Depurando árvores de expressão no Visual Studio (Visual Basic)
Ao depurar seus aplicativos, você pode analisar a estrutura e o conteúdo das árvores de expressão. Para obter uma visão geral da estrutura de árvore de expressão, você pode usar a propriedade `DebugView`, que representa as árvores de expressão [usando uma sintaxe especial](debugview-syntax.md). (Observe que `DebugView` está disponível apenas no modo de depuração.)  

![Captura de tela da DebugView da árvore de expressão.](media/debugging-expression-trees-in-visual-studio/debugview-visual-basic.png)

Uma vez que `DebugView` é uma cadeia de caracteres, você pode usar o [Visualizador de Texto interno](https://docs.microsoft.com/visualstudio/debugger/view-strings-visualizer#open-a-string-visualizer) para exibi-lo em várias linhas, selecionando **Visualizador de Texto** do ícone de lupa ao lado do rótulo `DebugView`.

 ![Captura de tela do Visualizador de texto aplicado aos resultados de DebugView.](media/debugging-expression-trees-in-visual-studio/string-visualizer-vb.png)

Como alternativa, você pode instalar e usar [um visualizador personalizado](https://docs.microsoft.com/visualstudio/debugger/create-custom-visualizers-of-data) para árvores de expressão, como:

- As [expressões legíveis](https://github.com/agileobjects/ReadableExpressions) ([licença MIT](https://github.com/agileobjects/ReadableExpressions/blob/master/LICENSE.md), disponível no [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=vs-publisher-1232914.ReadableExpressionsVisualizers)), renderiza a árvore de expressão como código passíveis C#, com várias opções de renderização:

  ![Captura de tela do Visualizador de expressões legíveis.](media/debugging-expression-trees-in-visual-studio/readable-expressions-visualizer.png)

- O [Visualizador de árvore de expressões](https://github.com/zspitz/ExpressionTreeVisualizer/blob/master/README.md) ([licença MIT](https://github.com/zspitz/ExpressionTreeVisualizer/blob/master/LICENSE)) fornece uma exibição de árvore da árvore de expressão e seus nós individuais; e pode renderizar a árvore de expressão usando a sintaxe Visual Basic:

  ![Captura de tela do Visualizador de árvore de expressão.](media/debugging-expression-trees-in-visual-studio/expression-tree-visualizer-vb.png)

### <a name="to-open-a-visualizer-for-an-expression-tree"></a>Para abrir um visualizador para uma árvore de expressão  
  
1. Clique no ícone de lupa que aparece ao lado da árvore de expressão em **DataTips**, uma janela **Inspeção**, a janela **Autos** ou a janela **Locais**.  
  
    É exibida uma lista de visualizadores disponíveis:

    ![Captura de tela do usuário abrindo os visualizadores do Visual Studio.](media/debugging-expression-trees-in-visual-studio/expression-tree-visualizers-vb.png)

2. Clique no visualizador que você deseja usar.  

## <a name="see-also"></a>Confira também

- [Árvores de expressão (Visual Basic)](index.md)
- [Depurando no Visual Studio](/visualstudio/debugger/debugger-feature-tour)
- [Criar visualizadores personalizados](/visualstudio/debugger/create-custom-visualizers-of-data)
- [`DebugView`sintaxe](debugview-syntax.md)
