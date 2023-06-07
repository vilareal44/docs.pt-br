---
title: Criar um controle que aproveita os recursos de tempo de design do Visual Studio
description: Saiba como criar um designer personalizado para um controle personalizado no Windows Forms que aproveita os recursos de tempo de design.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms controls, creating
- design-time functionality [Windows Forms], Windows Forms
- DocumentDesigner class [Windows Forms]
- walkthroughs [Windows Forms], controls
ms.assetid: 6f487c59-cb38-4afa-ad2e-95edacb1d626
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 03c04578ecb01b689f58d41a46eef5793fb1182c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613904"
---
# <a name="walkthrough-create-a-control-that-takes-advantage-of-design-time-features"></a>Walkthrough: criar um controle que aproveita os recursos de tempo de design

A experiência de tempo de design para um controle personalizado pode ser aprimorada por meio da criação de um designer personalizado associado.

Este artigo ilustra como criar um designer personalizado para um controle personalizado. Você implementará um `MarqueeControl` tipo e uma classe de designer associada chamada `MarqueeControlRootDesigner` .

O `MarqueeControl` tipo implementa uma exibição semelhante a um letreiro de cinema com luzes animadas e texto intermitente.

O designer para esse controle interage com o ambiente de design para fornecer uma experiência de tempo de design personalizada. Com o designer personalizado, você pode montar uma implementação `MarqueeControl` personalizada com luzes animadas e o texto piscando em muitas combinações. Você pode usar o controle montado em um formulário como qualquer outro controle dos Windows Forms.

Quando você terminar com este guia, o controle personalizado terá uma aparência semelhante à seguinte:

![O aplicativo que mostra um letreiro que informa o texto e os botões iniciar e parar.](./media/creating-a-wf-control-design-time-features/demo-marquee-control.gif)

Para obter a listagem de códigos completa, consulte [Como criar um controle dos Windows Forms que aproveita funcionalidades de tempo de design](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/307hck25(v=vs.120)).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este passo a passos, você precisará do Visual Studio.

## <a name="create-the-project"></a>Criar o projeto

A primeira etapa é criar o projeto do aplicativo. Você usará este projeto para criar o aplicativo que hospeda o controle personalizado.

No Visual Studio, crie um novo projeto de aplicativo Windows Forms e nomeie-o **MarqueeControlTest**.

## <a name="create-the-control-library-project"></a>Criar o projeto de biblioteca de controle

1. Adicione um projeto de biblioteca de controles dos Windows Forms à solução. Nomeie o projeto **MarqueeControlLibrary**.

2. Usando o **Gerenciador de Soluções**, exclua o controle padrão do projeto, excluindo o arquivo de origem denominado "UserControl1.cs" ou "UserControl1.vb", dependendo da linguagem de sua escolha.

3. Adicione um novo <xref:System.Windows.Forms.UserControl> item ao `MarqueeControlLibrary` projeto. Dê ao novo arquivo de origem um nome de base de **MarqueeControl**.

4. Usando o **Gerenciador de Soluções**, crie uma nova pasta no projeto `MarqueeControlLibrary`.

5. Clique com o botão direito do mouse na pasta **Design** e adicione uma nova classe. Nomeie-o como **MarqueeControlRootDesigner**.

6. Você precisará usar tipos do assembly System. Design, portanto, adicione essa referência ao `MarqueeControlLibrary` projeto.

## <a name="reference-the-custom-control-project"></a>Referenciar o projeto de controle personalizado

Você usará o projeto `MarqueeControlTest` para testar o controle personalizado. O projeto de teste fará o reconhecimento do controle personalizado quando você adicionar uma referência do projeto ao assembly `MarqueeControlLibrary`.

No projeto `MarqueeControlTest`, adicione uma referência do projeto ao assembly `MarqueeControlLibrary`. Certifique-se de usar a guia **Projetos** da caixa de diálogo **Adicionar Referência**, em vez de referenciar o assembly `MarqueeControlLibrary` diretamente.

## <a name="define-a-custom-control-and-its-custom-designer"></a>Definir um controle personalizado e seu designer personalizado

Seu controle personalizado será derivado da <xref:System.Windows.Forms.UserControl> classe. Isso permite que o controle contenha outros controles e dá a seu controle muita funcionalidade padrão.

O controle personalizado terá um designer personalizado associado. Isso permite que você crie uma experiência de design exclusiva, desenvolvida especificamente para seu controle personalizado.

Você associa o controle a seu designer usando a <xref:System.ComponentModel.DesignerAttribute> classe. Como você está desenvolvendo todo o comportamento do tempo de design do seu controle personalizado, o designer personalizado implementará a <xref:System.ComponentModel.Design.IRootDesigner> interface.

### <a name="to-define-a-custom-control-and-its-custom-designer"></a>Para definir um controle personalizado e o respectivo designer personalizado

1. Abra o arquivo de origem `MarqueeControl` no **Editor de Código**. Na parte superior do arquivo, importe os seguintes namespaces:

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#220](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrol.cs#220)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#220](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrol.vb#220)]

2. Adicione o <xref:System.ComponentModel.DesignerAttribute> à `MarqueeControl` declaração de classe. Isso associa o controle personalizado ao respectivo designer.

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#240](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrol.cs#240)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#240](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrol.vb#240)]

3. Abra o arquivo de origem `MarqueeControlRootDesigner` no **Editor de Código**. Na parte superior do arquivo, importe os seguintes namespaces:

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#520](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#520)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#520](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#520)]

4. Altere a declaração de `MarqueeControlRootDesigner` para herdar da classe <xref:System.Windows.Forms.Design.DocumentDesigner>. Aplique o <xref:System.ComponentModel.ToolboxItemFilterAttribute> para especificar a interação do designer com a **caixa de ferramentas**.

   > [!NOTE]
   > A definição da `MarqueeControlRootDesigner` classe foi colocada em um namespace chamado MarqueeControlLibrary. Design. Essa declaração coloca o designer em um namespace especial reservado para tipos relacionados a design.

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#530](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#530)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#530](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#530)]

5. Defina o construtor para a classe `MarqueeControlRootDesigner`. Inserir uma <xref:System.Diagnostics.Trace.WriteLine%2A> instrução no corpo do construtor. Isso será útil para depuração.

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#540](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#540)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#540](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#540)]

## <a name="create-an-instance-of-your-custom-control"></a>Criar uma instância do seu controle personalizado

1. Adicione um novo <xref:System.Windows.Forms.UserControl> item ao `MarqueeControlTest` projeto. Dê ao novo arquivo de origem um nome de base de **DemoMarqueeControl**.

2. Abra o arquivo `DemoMarqueeControl` no **Editor de Código**. Na parte superior do arquivo, importe o namespace `MarqueeControlLibrary`:

   ```vb
   Imports MarqueeControlLibrary
   ```

   ```csharp
   using MarqueeControlLibrary;
   ```

3. Altere a declaração de `DemoMarqueeControl` para herdar da classe `MarqueeControl`.

4. Compile o projeto.

5. Abra o Form1 no Designer de Formulários do Windows.

6. Localize a guia **Componentes MarqueeControlTest** na **Caixa de Ferramentas** e abra-a. Arraste um `DemoMarqueeControl` da **Caixa de Ferramentas** para seu formulário.

7. Compile o projeto.

## <a name="set-up-the-project-for-design-time-debugging"></a>Configurar o projeto para depuração em tempo de design

Quando você estiver desenvolvendo uma experiência personalizada em tempo de design, será necessário depurar seus controles e componentes. Há uma maneira simples de configurar seu projeto para permitir a depuração em tempo de design. Para obter mais informações, consulte [Passo a passo: depurando controles dos Windows Forms personalizados em tempo de design](walkthrough-debugging-custom-windows-forms-controls-at-design-time.md).

1. Clique com o botão direito do mouse no projeto `MarqueeControlLibrary` e selecione **Propriedades**.

2. Na caixa de diálogo **páginas de propriedades do MarqueeControlLibrary** , selecione a página de **depuração** .

3. Na seção **Iniciar ação** , selecione **Iniciar programa externo**. Você estará Depurando uma instância separada do Visual Studio, então clique nas reticências ( ![ o botão de reticências (...) no botão janela Propriedades do Visual Studio ](./media/visual-studio-ellipsis-button.png) ) para procurar o IDE do Visual Studio. O nome do arquivo executável é devenv.exe e, se você tiver instalado no local padrão, seu caminho será *% ProgramFiles (x86)% \ Microsoft Visual Studio\2019 \\ \<edition>\Common7\IDE\devenv.exe*.

4. Selecione **OK** para fechar a caixa de diálogo.

5. Clique com o botão direito do mouse no projeto MarqueeControlLibrary e selecione **definir como projeto de inicialização** para habilitar essa configuração de depuração.

## <a name="checkpoint"></a>Ponto de verificação

Agora você está pronto para depurar o comportamento de tempo de design de seu controle personalizado. Depois de determinar que o ambiente de depuração está configurado corretamente, você testará a associação entre o controle personalizado e o designer personalizado.

### <a name="to-test-the-debugging-environment-and-the-designer-association"></a>Para testar o ambiente de depuração e a associação com o designer

1. Abra o arquivo de origem MarqueeControlRootDesigner no **Editor de código** e coloque um ponto de interrupção na <xref:System.Diagnostics.Trace.WriteLine%2A> instrução.

2. Pressione **F5** para iniciar a sessão de depuração.

   Uma nova instância do Visual Studio é criada.

3. Na nova instância do Visual Studio, abra a solução MarqueeControlTest. Você pode localizar facilmente a solução selecionando **Projetos Recentes** no menu **Arquivo**. O arquivo de solução MarqueeControlTest. sln será listado como o arquivo usado mais recentemente.

4. Abra o `DemoMarqueeControl` no designer.

   A instância de depuração do Visual Studio Obtém o foco e a execução para o ponto de interrupção. Pressione **F5** para continuar a sessão de depuração.

Nesse ponto, tudo está preparado para você desenvolver e depurar seu controle personalizado e o respectivo designer personalizado associado. O restante deste artigo se concentra nos detalhes da implementação de recursos do controle e do designer.

## <a name="implement-the-custom-control"></a>Implementar o controle personalizado

O `MarqueeControl` é um <xref:System.Windows.Forms.UserControl> com um pouco de personalização. Ele apresenta dois métodos: `Start` (que inicia a animação do letreiro) e `Stop` (que interrompe a animação). Já que `MarqueeControl` contém controles filho que implementam a interface `IMarqueeWidget`, `Start` e `Stop` enumeram cada controle filho e chamam os métodos `StartMarquee` e `StopMarquee`, respectivamente, em cada controle filho que implementa `IMarqueeWidget`.

A aparência dos `MarqueeBorder` controles e `MarqueeText` depende do layout; portanto, `MarqueeControl` o substitui o <xref:System.Windows.Forms.Control.OnLayout%2A> método e as chamadas <xref:System.Windows.Forms.Control.PerformLayout%2A> em controles filho desse tipo.

Esta é a extensão das personalizações de `MarqueeControl`. Os recursos de tempo de execução são implementados pelos controles `MarqueeBorder` e `MarqueeText` e os recursos de tempo de design são implementados pelas classes `MarqueeBorderDesigner` e `MarqueeControlRootDesigner`.

### <a name="to-implement-your-custom-control"></a>Para implementar seu controle personalizado

1. Abra o arquivo de origem `MarqueeControl` no **Editor de Código**. Implemente os métodos `Start` e `Stop`.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#260](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrol.cs#260)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#260](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrol.vb#260)]

2. Substituir o método <xref:System.Windows.Forms.Control.OnLayout%2A>.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#270](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrol.cs#270)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#270](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrol.vb#270)]

## <a name="create-a-child-control-for-your-custom-control"></a>Criar um controle filho para seu controle personalizado

O `MarqueeControl` hospedará os dois tipos de controle filho: o controle `MarqueeBorder` e o controle `MarqueeText`.

- `MarqueeBorder`: esse controle pinta uma borda de "luzes" em torno das próprias bordas. As luzes piscam em sequência, por isso elas parecem se mover em torno da borda. A velocidade na qual as luzes piscam é controlada por uma propriedade chamada `UpdatePeriod`. Várias outras propriedades personalizadas determinam outros aspectos da aparência do controle. Dois métodos, chamados `StartMarquee` e `StopMarquee`, controlam quando a animação começa e para.

- `MarqueeText`: esse controle pinta uma cadeia de caracteres piscando. Assim como o controle `MarqueeBorder`, a velocidade na qual o texto pisca é controlada pela propriedade `UpdatePeriod`. O controle `MarqueeText` também tem os métodos `StartMarquee` e `StopMarquee` em comum com o controle `MarqueeBorder`.

Em tempo de design, o `MarqueeControlRootDesigner` permite que esses tipos de controle sejam adicionados a um `MarqueeControl` em qualquer combinação.

Recursos comuns dos dois controles são incluídos em uma interface chamada `IMarqueeWidget`. Isso permite que o `MarqueeControl` descubra os controles filho relacionados ao letreiro e dê a eles tratamento especial.

Para implementar o recurso de animação periódica, você usará <xref:System.ComponentModel.BackgroundWorker> objetos do <xref:System.ComponentModel?displayProperty=nameWithType> namespace. Você pode usar <xref:System.Windows.Forms.Timer> objetos, mas quando muitos `IMarqueeWidget` objetos estão presentes, o único thread da interface do usuário pode não conseguir acompanhar a animação.

### <a name="to-create-a-child-control-for-your-custom-control"></a>Para criar um controle filho para o seu controle personalizado

1. Adicione um novo item de classe ao projeto `MarqueeControlLibrary`. Dê ao novo arquivo de origem o nome base "IMarqueeWidget".

2. Abra o arquivo de origem `IMarqueeWidget` no **Editor de Código** e altere a declaração de `class` para `interface`:

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/imarqueewidget.cs#2)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/imarqueewidget.vb#2)]

3. Adicione o código a seguir à interface `IMarqueeWidget` para expor dois métodos e uma propriedade que manipulam a animação do letreiro:

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/imarqueewidget.cs#3)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/imarqueewidget.vb#3)]

4. Adicione um novo item de **Controle Personalizado** ao projeto `MarqueeControlLibrary`. Dê ao novo arquivo de origem o nome base "MarqueeText".

5. Arraste um <xref:System.ComponentModel.BackgroundWorker> componente da **caixa de ferramentas** para seu `MarqueeText` controle. Esse componente permitirá que o controle `MarqueeText` se atualize de modo assíncrono.

6. Na janela **Propriedades** , defina o <xref:System.ComponentModel.BackgroundWorker> componente `WorkerReportsProgress` e <xref:System.ComponentModel.BackgroundWorker.WorkerSupportsCancellation%2A> as propriedades como **true**. Essas configurações permitem que o <xref:System.ComponentModel.BackgroundWorker> componente gere o evento periodicamente <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> e cancele as atualizações assíncronas.

   Para obter mais informações, consulte [BackgroundWorker Component](backgroundworker-component.md).

7. Abra o arquivo de origem `MarqueeText` no **Editor de Código**. Na parte superior do arquivo, importe os seguintes namespaces:

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#120](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#120)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#120](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#120)]

8. Altere a declaração de `MarqueeText` para herdar de <xref:System.Windows.Forms.Label> e para implementar a `IMarqueeWidget` interface:

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#130](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#130)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#130](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#130)]

9. Declare as variáveis de instância que correspondem às propriedades expostas e inicialize-as no construtor. O campo `isLit` determina se o texto será pintado na cor fornecida pela propriedade `LightColor`.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#140](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#140)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#140](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#140)]

10. Implemente a interface `IMarqueeWidget`.

    Os `StartMarquee` `StopMarquee` métodos e invocam os <xref:System.ComponentModel.BackgroundWorker> componentes <xref:System.ComponentModel.BackgroundWorker.RunWorkerAsync%2A> e os <xref:System.ComponentModel.BackgroundWorker.CancelAsync%2A> métodos para iniciar e parar a animação.

    Os <xref:System.ComponentModel.CategoryAttribute.Category%2A> <xref:System.ComponentModel.BrowsableAttribute.Browsable%2A> atributos e são aplicados à `UpdatePeriod` propriedade para que apareçam em uma seção personalizada da janela Propriedades chamada "Marquee".

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#150](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#150)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#150](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#150)]

11. Implemente os acessadores de propriedade. Você vai expor duas propriedades aos clientes: `LightColor` e `DarkColor` . Os <xref:System.ComponentModel.CategoryAttribute.Category%2A> <xref:System.ComponentModel.BrowsableAttribute.Browsable%2A> atributos e são aplicados a essas propriedades, de modo que as propriedades apareçam em uma seção personalizada da janela Propriedades chamada "Marquee".

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#160](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#160)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#160](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#160)]

12. Implemente os manipuladores para os <xref:System.ComponentModel.BackgroundWorker> <xref:System.ComponentModel.BackgroundWorker.DoWork> eventos e componentes <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> .

    O <xref:System.ComponentModel.BackgroundWorker.DoWork> manipulador de eventos é suspenso para o número de milissegundos especificado pelo `UpdatePeriod` , em seguida, gera o <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> evento até que o código interrompa a animação chamando <xref:System.ComponentModel.BackgroundWorker.CancelAsync%2A> .

    O <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> manipulador de eventos alterna o texto entre seu estado luminoso e escuro para dar a aparência de piscando.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#180](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#180)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#180](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#180)]

13. Substitua o <xref:System.Windows.Forms.Control.OnPaint%2A> método para habilitar a animação.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#170](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueetext.cs#170)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#170](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueetext.vb#170)]

14. Pressione **F6** para criar a solução.

## <a name="create-the-marqueeborder-child-control"></a>Criar o controle filho MarqueeBorder

O controle `MarqueeBorder` é um pouco mais sofisticado do que o controle `MarqueeText`. Ele tem mais propriedades e a animação no <xref:System.Windows.Forms.Control.OnPaint%2A> método é mais envolvida. A princípio, ele é bem semelhante ao controle `MarqueeText`.

Como o `MarqueeBorder` controle pode ter controles filho, ele precisa estar ciente de <xref:System.Windows.Forms.Control.Layout> eventos.

### <a name="to-create-the-marqueeborder-control"></a>Para criar o controle MarqueeBorder

1. Adicione um novo item de **Controle Personalizado** ao projeto `MarqueeControlLibrary`. Dê ao novo arquivo de origem o nome base "MarqueeBorder".

2. Arraste um <xref:System.ComponentModel.BackgroundWorker> componente da **caixa de ferramentas** para seu `MarqueeBorder` controle. Esse componente permitirá que o controle `MarqueeBorder` se atualize de modo assíncrono.

3. Na janela **Propriedades** , defina o <xref:System.ComponentModel.BackgroundWorker> componente `WorkerReportsProgress` e <xref:System.ComponentModel.BackgroundWorker.WorkerSupportsCancellation%2A> as propriedades como **true**. Essas configurações permitem que o <xref:System.ComponentModel.BackgroundWorker> componente gere o evento periodicamente <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> e cancele as atualizações assíncronas. Para obter mais informações, consulte [BackgroundWorker Component](backgroundworker-component.md).

4. Na janela **Propriedades** , selecione o botão **eventos** . Anexe manipuladores para os <xref:System.ComponentModel.BackgroundWorker.DoWork> <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> eventos e.

5. Abra o arquivo de origem `MarqueeBorder` no **Editor de Código**. Na parte superior do arquivo, importe os seguintes namespaces:

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#20](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#20)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#20](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#20)]

6. Altere a declaração de `MarqueeBorder` para herdar de <xref:System.Windows.Forms.Panel> e para implementar a `IMarqueeWidget` interface.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#30](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#30)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#30](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#30)]

7. Declare duas enumerações para gerenciar o estado do controle `MarqueeBorder`: `MarqueeSpinDirection`, que determina a direção na qual as luzes "giram" ao redor da borda e `MarqueeLightShape`, que determina a forma das luzes (circulares ou quadradas). Coloque essas declarações antes da declaração de classe `MarqueeBorder`.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#97](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#97)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#97](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#97)]

8. Declare as variáveis de instância que correspondem às propriedades expostas e inicialize-as no construtor.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#40](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#40)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#40](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#40)]

9. Implemente a interface `IMarqueeWidget`.

    Os `StartMarquee` `StopMarquee` métodos e invocam os <xref:System.ComponentModel.BackgroundWorker> componentes <xref:System.ComponentModel.BackgroundWorker.RunWorkerAsync%2A> e os <xref:System.ComponentModel.BackgroundWorker.CancelAsync%2A> métodos para iniciar e parar a animação.

    Já que o controle `MarqueeBorder` pode conter controles filho, o método `StartMarquee` enumera todos os controles filho e chama `StartMarquee` naqueles que implementam `IMarqueeWidget`. O método `StopMarquee` tem uma implementação semelhante.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#50](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#50)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#50](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#50)]

10. Implemente os acessadores de propriedade. O controle `MarqueeBorder` tem várias propriedades para controlar sua aparência.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#60](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#60)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#60](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#60)]

11. Implemente os manipuladores para os <xref:System.ComponentModel.BackgroundWorker> <xref:System.ComponentModel.BackgroundWorker.DoWork> eventos e componentes <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> .

    O <xref:System.ComponentModel.BackgroundWorker.DoWork> manipulador de eventos é suspenso para o número de milissegundos especificado pelo `UpdatePeriod` , em seguida, gera o <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> evento até que o código interrompa a animação chamando <xref:System.ComponentModel.BackgroundWorker.CancelAsync%2A> .

    O <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> manipulador de eventos incrementa a posição da luz "base", da qual o estado claro/escuro das outras luzes é determinado e chama o <xref:System.Windows.Forms.Control.Refresh%2A> método para fazer com que o controle seja redesenhado.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#90](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#90)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#90](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#90)]

12. Implemente os métodos auxiliares, `IsLit` e `DrawLight`.

    O método `IsLit` determina a cor de uma luz em uma posição especificada. As luzes "acesas" são desenhadas na cor fornecida pela propriedade `LightColor`, enquanto as luzes "escuras" são desenhadas na cor fornecida pela propriedade `DarkColor`.

    O método `DrawLight` desenha uma luz usando a cor, forma e posição apropriadas.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#80](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#80)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#80](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#80)]

13. Substitua os métodos <xref:System.Windows.Forms.Control.OnLayout%2A> e <xref:System.Windows.Forms.Control.OnPaint%2A>.

    O <xref:System.Windows.Forms.Control.OnPaint%2A> método desenha as luzes ao longo das bordas do `MarqueeBorder` controle.

    Como o <xref:System.Windows.Forms.Control.OnPaint%2A> método depende das dimensões do `MarqueeBorder` controle, você precisa chamá-lo sempre que o layout for alterado. Para conseguir isso, substitua <xref:System.Windows.Forms.Control.OnLayout%2A> e chame <xref:System.Windows.Forms.Control.Refresh%2A> .

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#70](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#70)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#70](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#70)]

## <a name="create-a-custom-designer-to-shadow-and-filter-properties"></a>Criar um designer personalizado para sombrear e filtrar Propriedades

A classe `MarqueeControlRootDesigner` fornece a implementação para o designer raiz. Além desse designer, que opera no `MarqueeControl` , você precisará de um designer personalizado que esteja especificamente associado ao `MarqueeBorder` controle. Este designer oferece um comportamento personalizado apropriado no contexto do designer raiz personalizado.

Especificamente, o `MarqueeBorderDesigner` fará o "sombreamento" de determinadas propriedades do controle `MarqueeBorder` e as filtrará, alterando a interação delas com o ambiente de design.

Interceptar chamadas para o acessador de propriedade de um componente é algo conhecido como "sombreamento". Isso permite que um designer controle o valor definido pelo usuário e, opcionalmente, passe esse valor para o componente que está sendo criado.

Neste exemplo, as <xref:System.Windows.Forms.Control.Visible%2A> Propriedades e <xref:System.Windows.Forms.Control.Enabled%2A> serão sombreadas pelo `MarqueeBorderDesigner` , o que impede que o usuário torne o `MarqueeBorder` controle invisível ou desabilitado durante o tempo de design.

Designers também podem adicionar e remover propriedades. Para este exemplo, a <xref:System.Windows.Forms.Control.Padding%2A> propriedade será removida em tempo de design, porque o `MarqueeBorder` controle define o preenchimento de forma programática com base no tamanho das luzes especificadas pela `LightSize` propriedade.

A classe base para o `MarqueeBorderDesigner` é <xref:System.ComponentModel.Design.ComponentDesigner> , que tem métodos que podem alterar os atributos, as propriedades e os eventos expostos por um controle em tempo de design:

- <xref:System.ComponentModel.Design.ComponentDesigner.PreFilterProperties%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PostFilterProperties%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PreFilterAttributes%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PostFilterAttributes%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PreFilterEvents%2A>

- <xref:System.ComponentModel.Design.ComponentDesigner.PostFilterEvents%2A>

Ao alterar a interface pública de um componente usando esses métodos, siga estas regras:

- Adicionar ou remover itens apenas nos métodos `PreFilter`

- Modificar itens existentes apenas nos métodos `PostFilter`

- Sempre chamar a implementação base primeiro nos métodos `PreFilter`

- Sempre chamar a implementação base por último nos métodos `PostFilter`

Aderir a essas regras garante que todos os designers no ambiente de tempo de design tenham uma exibição consistente de todos os componentes que estão sendo criados.

A <xref:System.ComponentModel.Design.ComponentDesigner> classe fornece um dicionário para gerenciar os valores das propriedades sombreadas, o que alivia você da necessidade de criar variáveis de instância específicas.

### <a name="to-create-a-custom-designer-to-shadow-and-filter-properties"></a>Para criar um designer personalizado para propriedades de sombra e de filtro

1. Clique com o botão direito do mouse na pasta **Design** e adicione uma nova classe. Dê ao arquivo de origem um nome de base de **MarqueeBorderDesigner**.

2. Abra o arquivo de origem MarqueeBorderDesigner no **Editor de código**. Na parte superior do arquivo, importe os seguintes namespaces:

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#420](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborderdesigner.cs#420)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#420](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborderdesigner.vb#420)]

3. Altere a declaração de `MarqueeBorderDesigner` para herdar de <xref:System.Windows.Forms.Design.ParentControlDesigner> .

    Porque o `MarqueeBorder` controle pode conter controles filho, `MarqueeBorderDesigner` herdados de <xref:System.Windows.Forms.Design.ParentControlDesigner> , que manipula a interação pai-filho.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#430](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborderdesigner.cs#430)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#430](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborderdesigner.vb#430)]

4. Substitua a implementação base de <xref:System.ComponentModel.Design.ComponentDesigner.PreFilterProperties%2A> .

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#450](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborderdesigner.cs#450)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#450](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborderdesigner.vb#450)]

5. Implemente <xref:System.Windows.Forms.Control.Enabled%2A> as <xref:System.Windows.Forms.Control.Visible%2A> Propriedades e. Essas implementações fazem sombreamento das propriedades do controle.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#440](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborderdesigner.cs#440)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#440](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborderdesigner.vb#440)]

## <a name="handle-component-changes"></a>Lidar com alterações de componente

A classe `MarqueeControlRootDesigner` fornece a experiência de tempo de design personalizada para suas instâncias de `MarqueeControl`. A maior parte da funcionalidade de tempo de design é herdada da <xref:System.Windows.Forms.Design.DocumentDesigner> classe. Seu código implementará duas personalizações específicas: tratamento de alterações de componentes e adição de verbos de designer.

Conforme os usuários projetam suas instâncias de `MarqueeControl`, o designer raiz controlará as alterações para o `MarqueeControl` e os respectivos controles filho. O ambiente de tempo de design oferece um serviço conveniente, <xref:System.ComponentModel.Design.IComponentChangeService> , para controlar alterações no estado do componente.

Você adquire uma referência a esse serviço consultando o ambiente com o <xref:System.ComponentModel.Design.ComponentDesigner.GetService%2A> método. Se a consulta for bem-sucedida, o designer poderá anexar um manipulador para o <xref:System.ComponentModel.Design.IComponentChangeService.ComponentChanged> evento e executar quaisquer tarefas necessárias para manter um estado consistente no tempo de design.

No caso da `MarqueeControlRootDesigner` classe, você chamará o <xref:System.Windows.Forms.Control.Refresh%2A> método em cada `IMarqueeWidget` objeto contido no `MarqueeControl` . Isso fará com que o `IMarqueeWidget` objeto seja redesenhado adequadamente quando as propriedades como suas pai <xref:System.Windows.Forms.Control.Size%2A> forem alteradas.

### <a name="to-handle-component-changes"></a>Para manipular alterações de componente

1. Abra o `MarqueeControlRootDesigner` arquivo de origem no **Editor de código** e substitua o <xref:System.Windows.Forms.Design.DocumentDesigner.Initialize%2A> método. Chame a implementação base de <xref:System.Windows.Forms.Design.DocumentDesigner.Initialize%2A> e a consulta para o <xref:System.ComponentModel.Design.IComponentChangeService> .

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#580](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#580)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#580](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#580)]

2. Implemente o <xref:System.ComponentModel.Design.IComponentChangeService.OnComponentChanged%2A> manipulador de eventos. Teste o tipo do componente de envio e, se for um `IMarqueeWidget` , chame seu <xref:System.Windows.Forms.Control.Refresh%2A> método.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#560](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#560)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#560](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#560)]

## <a name="add-designer-verbs-to-your-custom-designer"></a>Adicionar verbos de designer ao seu designer personalizado

Um verbo do designer é um comando de menu vinculado a um manipulador de eventos. Verbos do designer são adicionados ao menu de atalho de um componente em tempo de design. Para obter mais informações, consulte <xref:System.ComponentModel.Design.DesignerVerb>.

Você adicionará dois verbos do designer a seus designers: **Executar Teste** e **Parar Teste**. Esses verbos permitirão que você exiba o comportamento em tempo de execução de `MarqueeControl` em tempo de design. Esses verbos serão adicionados ao `MarqueeControlRootDesigner` .

Quando **Executar Teste** é invocado, o manipulador de eventos de verbo chamará o método `StartMarquee` no `MarqueeControl`. Quando **Parar Teste** é invocado, o manipulador de eventos de verbo chamará o método `StopMarquee` no `MarqueeControl`. A implementação dos métodos `StartMarquee` e `StopMarquee` chama esses métodos nos controles contidos que implementam `IMarqueeWidget`, de modo que quaisquer controles `IMarqueeWidget` independentes também participam do teste.

### <a name="to-add-designer-verbs-to-your-custom-designers"></a>Para adicionar verbos do designer a seus designers personalizados

1. Na classe `MarqueeControlRootDesigner`, adicione manipuladores de eventos denominados `OnVerbRunTest` e `OnVerbStopTest`.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#570](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#570)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#570](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#570)]

2. Conecte esses manipuladores de eventos a seus verbos do designer correspondentes. `MarqueeControlRootDesigner`herda um <xref:System.ComponentModel.Design.DesignerVerbCollection> de sua classe base. Você criará dois novos <xref:System.ComponentModel.Design.DesignerVerb> objetos e os adicionará a essa coleção no <xref:System.Windows.Forms.Design.DocumentDesigner.Initialize%2A> método.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#590](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueecontrolrootdesigner.cs#590)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#590](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueecontrolrootdesigner.vb#590)]

## <a name="create-a-custom-uitypeeditor"></a>Criar um UITypeEditor personalizado

Quando você cria uma experiência de tempo de design personalizada para os usuários, geralmente é desejável criar uma interação personalizada com a janela Propriedades. Você pode fazer isso criando um <xref:System.Drawing.Design.UITypeEditor> .

O controle `MarqueeBorder` expõe várias propriedades na janela Propriedades. Duas dessas propriedades, `MarqueeSpinDirection` e `MarqueeLightShape`, são representadas por enumerações. Para ilustrar o uso de um editor de tipo de interface do usuário, a `MarqueeLightShape` Propriedade terá uma <xref:System.Drawing.Design.UITypeEditor> classe associada.

### <a name="to-create-a-custom-ui-type-editor"></a>Para criar um editor de tipo de interface do usuário personalizado

1. Abra o arquivo de origem `MarqueeBorder` no **Editor de Código**.

2. Na definição da `MarqueeBorder` classe, declare uma classe chamada `LightShapeEditor` derivada de <xref:System.Drawing.Design.UITypeEditor> .

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#96](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#96)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#96](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#96)]

3. Declare uma <xref:System.Windows.Forms.Design.IWindowsFormsEditorService> variável de instância chamada `editorService` .

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#92](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#92)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#92](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#92)]

4. Substituir o método <xref:System.Drawing.Design.UITypeEditor.GetEditStyle%2A>. Essa implementação retorna <xref:System.Drawing.Design.UITypeEditorEditStyle.DropDown> , que diz ao ambiente de design como exibir o `LightShapeEditor` .

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#93](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#93)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#93](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#93)]

5. Substituir o método <xref:System.Drawing.Design.UITypeEditor.EditValue%2A>. Essa implementação consulta o ambiente de design de um <xref:System.Windows.Forms.Design.IWindowsFormsEditorService> objeto. Se for bem-sucedida, ela criará um `LightShapeSelectionControl`. O <xref:System.Windows.Forms.Design.IWindowsFormsEditorService.DropDownControl%2A> método é invocado para iniciar o `LightShapeEditor` . O valor retornado dessa invocação é enviado de volta para o ambiente de design.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#94](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/marqueeborder.cs#94)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#94](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/marqueeborder.vb#94)]

## <a name="create-a-view-control-for-your-custom-uitypeeditor"></a>Criar um controle de exibição para seu UITypeEditor personalizado

A propriedade `MarqueeLightShape` dá suporte a dois tipos de formas de luz: `Square` e `Circle`. Você criará um controle personalizado usado unicamente para exibir graficamente esses valores na janela Propriedades. Esse controle personalizado será usado pelo <xref:System.Drawing.Design.UITypeEditor> para interagir com o janela Propriedades.

### <a name="to-create-a-view-control-for-your-custom-ui-type-editor"></a>Para criar um controle de exibição para o editor de tipo de interface do usuário personalizado

1. Adicione um novo <xref:System.Windows.Forms.UserControl> item ao `MarqueeControlLibrary` projeto. Dê ao novo arquivo de origem um nome de base de **LightShapeSelectionControl**.

2. Arraste dois <xref:System.Windows.Forms.Panel> controles da **caixa de ferramentas** para o `LightShapeSelectionControl` . Nomeie-os `squarePanel` e `circlePanel`. Posicione-os lado a lado. Defina a <xref:System.Windows.Forms.Control.Size%2A> propriedade de ambos os <xref:System.Windows.Forms.Panel> controles como **(60, 60)**. Defina a <xref:System.Windows.Forms.Control.Location%2A> Propriedade do `squarePanel` controle como **(8, 10)**. Defina a <xref:System.Windows.Forms.Control.Location%2A> Propriedade do `circlePanel` controle como **(80, 10)**. Por fim, defina a <xref:System.Windows.Forms.Control.Size%2A> propriedade de `LightShapeSelectionControl` como **(150, 80)**.

3. Abra o arquivo de origem `LightShapeSelectionControl` no **Editor de Código**. Na parte superior do arquivo, importe o namespace <xref:System.Windows.Forms.Design?displayProperty=nameWithType>:

   ```vb
   Imports System.Windows.Forms.Design
   ```

   ```csharp
   using System.Windows.Forms.Design;
   ```

4. Implemente <xref:System.Windows.Forms.Control.Click> manipuladores de eventos para os `squarePanel` `circlePanel` controles e. Esses métodos invocam <xref:System.Windows.Forms.Design.IWindowsFormsEditorService.CloseDropDown%2A> para encerrar a <xref:System.Drawing.Design.UITypeEditor> sessão de edição personalizada.

    [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#390](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#390)]
    [!code-vb[System.Windows.Forms.Design.DocumentDesigner#390](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#390)]

5. Declare uma <xref:System.Windows.Forms.Design.IWindowsFormsEditorService> variável de instância chamada `editorService` .

   ```vb
   Private editorService As IWindowsFormsEditorService
   ```

   ```csharp
   private IWindowsFormsEditorService editorService;
   ```

6. Declare uma variável de instância `MarqueeLightShape` chamada `lightShapeValue`.

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#330](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#330)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#330](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#330)]

7. No `LightShapeSelectionControl` Construtor, anexe os <xref:System.Windows.Forms.Control.Click> manipuladores de eventos aos `squarePanel` eventos e dos `circlePanel` controles <xref:System.Windows.Forms.Control.Click> . Além disso, defina uma sobrecarga de construtor que atribua o valor `MarqueeLightShape` do ambiente de design ao campo `lightShapeValue`.

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#340](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#340)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#340](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#340)]

8. No <xref:System.ComponentModel.Component.Dispose%2A> método, desanexe os <xref:System.Windows.Forms.Control.Click> manipuladores de eventos.

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#350](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#350)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#350](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#350)]

9. Em **Gerenciador de Soluções**, clique no botão **Mostrar Todos os Arquivos**. Abra o arquivo LightShapeSelectionControl.Designer.cs ou LightShapeSelectionControl. designer. vb e remova a definição padrão do <xref:System.ComponentModel.Component.Dispose%2A> método.

10. Implemente a propriedade `LightShape`.

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#360](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#360)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#360](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#360)]

11. Substituir o método <xref:System.Windows.Forms.Control.OnPaint%2A>. Essa implementação desenhará um círculo e um quadrado preenchidos. Ela também realçará o valor selecionado desenhando uma borda ao redor de uma forma ou de outra.

     [!code-csharp[System.Windows.Forms.Design.DocumentDesigner#380](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/CS/lightshapeselectioncontrol.cs#380)]
     [!code-vb[System.Windows.Forms.Design.DocumentDesigner#380](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.Design.DocumentDesigner/VB/lightshapeselectioncontrol.vb#380)]

## <a name="test-your-custom-control-in-the-designer"></a>Testar seu controle personalizado no designer

Neste ponto, você pode compilar o projeto `MarqueeControlLibrary`. Teste a implementação, criando um controle que herda da classe `MarqueeControl` e usando-o em um formulário.

### <a name="to-create-a-custom-marqueecontrol-implementation"></a>Para criar uma implementação personalizada de MarqueeControl

1. Abra `DemoMarqueeControl` no Designer de Formulários do Windows. Isso cria uma instância do tipo `DemoMarqueeControl` e a exibe em uma instância do tipo `MarqueeControlRootDesigner`.

2. Na **caixa de ferramentas**, abra a guia **componentes do MarqueeControlLibrary** . Você verá os `MarqueeBorder` controles e `MarqueeText` disponíveis para seleção.

3. Arraste uma instância do controle `MarqueeBorder` para a superfície de design `DemoMarqueeControl`. Encaixe esse controle `MarqueeBorder` no controle pai.

4. Arraste uma instância do controle `MarqueeText` para a superfície de design `DemoMarqueeControl`.

5. Compile a solução.

6. Clique com o botão direito do mouse no `DemoMarqueeControl` e, do menu de atalho, selecione a opção **Executar Teste** para iniciar a animação. Clique em **Parar Teste** para interromper a animação.

7. Abra o **Form1** no modo de exibição de design.

8. Coloque dois <xref:System.Windows.Forms.Button> controles no formulário. Nomeie-os `startButton` e `stopButton` altere os <xref:System.Windows.Forms.Control.Text%2A> valores de propriedade para **Iniciar** e **parar**, respectivamente.

9. Implemente <xref:System.Windows.Forms.Control.Click> manipuladores de eventos para ambos os <xref:System.Windows.Forms.Button> controles.

10. Na **caixa de ferramentas**, abra a guia **componentes do MarqueeControlTest** . Você verá o `DemoMarqueeControl` disponível para seleção.

11. Arraste uma instância de `DemoMarqueeControl` na superfície de design **Form1**.

12. Nos <xref:System.Windows.Forms.Control.Click> manipuladores de eventos, invoque os `Start` `Stop` métodos e no `DemoMarqueeControl` .

    ```vb
    Private Sub startButton_Click(sender As Object, e As System.EventArgs)
        Me.demoMarqueeControl1.Start()
    End Sub 'startButton_Click

    Private Sub stopButton_Click(sender As Object, e As System.EventArgs)
    Me.demoMarqueeControl1.Stop()
    End Sub 'stopButton_Click
    ```

    ```csharp
    private void startButton_Click(object sender, System.EventArgs e)
    {
        this.demoMarqueeControl1.Start();
    }

    private void stopButton_Click(object sender, System.EventArgs e)
    {
        this.demoMarqueeControl1.Stop();
    }
    ```

13. Defina o projeto `MarqueeControlTest` como o projeto de inicialização e execute-o. Você verá o formulário exibindo seu `DemoMarqueeControl`. Selecione o botão **Iniciar** para iniciar a animação. Você deve ver o texto piscando e as luzes se movendo ao redor da borda.

## <a name="next-steps"></a>Próximas etapas

O `MarqueeControlLibrary` demonstra uma implementação simples de controles personalizados e designers associados. Você pode tornar esse exemplo mais sofisticado de várias maneiras:

- Altere os valores da propriedade para `DemoMarqueeControl` no designer. Adicione mais controles `MarqueBorder` e encaixe-os dentro das respectivas instâncias pai para criar um efeito aninhado. Faça experiências com configurações diferentes para o `UpdatePeriod` e as propriedades relacionadas à luz.

- Crie suas próprias implementações de `IMarqueeWidget`. Você pode, por exemplo, criar uma "sinalização de neon" piscando ou uma sinalização animada com várias imagens.

- Personalize ainda mais a experiência de tempo de design. Você pode tentar sombrear mais propriedades do que <xref:System.Windows.Forms.Control.Enabled%2A> e <xref:System.Windows.Forms.Control.Visible%2A> , e pode adicionar novas propriedades. Adicione novos verbos do designer para simplificar tarefas comuns, como encaixar controles filho.

- Licencie o `MarqueeControl`.

- Controle o modo como os controles são serializados e como o código é gerado para eles. Para obter mais informações, consulte [Geração e compilação de código-fonte dinâmico](../../reflection-and-codedom/dynamic-source-code-generation-and-compilation.md).

## <a name="see-also"></a>Confira também

- <xref:System.Windows.Forms.UserControl>
- <xref:System.Windows.Forms.Design.ParentControlDesigner>
- <xref:System.Windows.Forms.Design.DocumentDesigner>
- <xref:System.ComponentModel.Design.IRootDesigner>
- <xref:System.ComponentModel.Design.DesignerVerb>
- <xref:System.Drawing.Design.UITypeEditor>
- <xref:System.ComponentModel.BackgroundWorker>
