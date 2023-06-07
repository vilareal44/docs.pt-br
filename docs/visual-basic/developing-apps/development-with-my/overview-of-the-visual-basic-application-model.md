---
title: Visão geral do modelo de aplicativo do Visual Basic
ms.date: 07/20/2015
helpviewer_keywords:
- My.Application object [Visual Basic], Visual Basic application model
- Visual Basic application model
ms.assetid: 17538984-84fe-43c9-82c8-724c9529fe8b
ms.openlocfilehash: 46177d0af7e5df767eb8421caf424880baac615e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411721"
---
# <a name="overview-of-the-visual-basic-application-model"></a>Visão geral do modelo de aplicativo do Visual Basic

Visual Basic fornece um modelo bem definido para controlar o comportamento de aplicativos Windows Forms: o modelo de aplicativo Visual Basic. Esse modelo inclui eventos para manipular a inicialização e o desligamento do aplicativo, bem como eventos para capturar exceções sem tratamento. Ele também oferece suporte para o desenvolvimento de aplicativos de instância única. O modelo de aplicativo é extensível, de modo que os desenvolvedores que precisam de mais controle podem personalizar seus métodos substituíveis.  
  
## <a name="uses-for-the-application-model"></a>Usos para o modelo de aplicativo  

 Um aplicativo típico precisa executar tarefas quando ele é iniciado e desligado. Por exemplo, quando ele é iniciado, o aplicativo pode exibir uma tela inicial, fazer conexões de banco de dados, carregar um estado salvo e assim por diante. Quando o aplicativo é desligado, ele pode fechar conexões de banco de dados, salvar o estado atual e assim por diante. Além disso, o aplicativo pode executar um código específico quando o aplicativo é desligado inesperadamente, como durante uma exceção sem tratamento.  
  
 O modelo de aplicativo Visual Basic torna mais fácil criar um aplicativo *de instância única* . Um aplicativo de instância única é diferente de um aplicativo normal, pois apenas uma instância do aplicativo pode ser executada de cada vez. Uma tentativa de iniciar outra instância de um aplicativo de instância única faz com que a instância original seja notificada — por meio do `StartupNextInstance` evento — que outra tentativa de inicialização foi feita. A notificação inclui os argumentos de linha de comando da instância subsequente. A instância subsequente do aplicativo é fechada antes que qualquer inicialização possa ocorrer.  
  
 Um aplicativo de instância única é iniciado e verifica se ele é a primeira instância ou uma instância subsequente do aplicativo:  
  
- Se for a primeira instância, ela será iniciada como de costume.  
  
- Cada tentativa subsequente de iniciar o aplicativo, enquanto a primeira instância é executada, resulta em um comportamento muito diferente. A tentativa subsequente notifica a primeira instância sobre os argumentos de linha de comando e, em seguida, sai imediatamente. A primeira instância manipula o `StartupNextInstance` evento para determinar quais são os argumentos de linha de comando da instância subsequente e continua a execução.  
  
     Este diagrama mostra como uma instância subsequente sinaliza a primeira instância:  
  
     ![Diagrama que mostra uma imagem de aplicativo de instância única.](./media/overview-of-the-visual-basic-application-model/single-instance-application.gif)  
  
 Ao manipular o `StartupNextInstance` evento, você pode controlar como seu aplicativo de instância única se comporta. Por exemplo, o Microsoft Outlook normalmente é executado como um aplicativo de instância única; Quando o Outlook estiver em execução e você tentar iniciar o Outlook novamente, o foco mudará para a instância original, mas outra instância não abrirá.  
  
## <a name="events-in-the-application-model"></a>Eventos no modelo de aplicativo  

 Os eventos a seguir são encontrados no modelo de aplicativo:  
  
- **Inicialização do aplicativo**. O aplicativo gera o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup> evento quando ele é iniciado. Ao manipular esse evento, você pode adicionar um código que inicializa o aplicativo antes que o formulário principal seja carregado. O `Startup` evento também fornece o cancelamento da execução do aplicativo durante essa fase do processo de inicialização, se desejado.  
  
     Você pode configurar o aplicativo para mostrar uma tela inicial enquanto o código de inicialização do aplicativo é executado. Por padrão, o modelo de aplicativo suprime a tela inicial quando o `/nosplash` `-nosplash` argumento de linha de comando ou é usado.  
  
- **Aplicativos de instância única**. O <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance> evento é gerado quando uma instância subsequente de um aplicativo de instância única é iniciada. O evento passa os argumentos de linha de comando da instância subsequente.  
  
- **Exceções sem tratamento**. Se o aplicativo encontrar uma exceção sem tratamento, ele gerará o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> evento. Seu manipulador para esse evento pode examinar a exceção e determinar se deseja continuar a execução.  
  
     O `UnhandledException` evento não é gerado em algumas circunstâncias. Para obter mais informações, consulte <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>.  
  
- **Alterações de conectividade de rede**. Se a disponibilidade da rede do computador for alterada, o aplicativo gerará o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged> evento.  
  
     O `NetworkAvailabilityChanged` evento não é gerado em algumas circunstâncias. Para obter mais informações, consulte <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>.  
  
- **Aplicativo desligado**. O aplicativo fornece o <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown> evento a ser sinalizado quando ele está prestes a ser desligado. Nesse manipulador de eventos, você pode garantir que as operações que seu aplicativo precisa executar — fechar e salvar, por exemplo, sejam concluídas. Você pode configurar seu aplicativo para desligar quando o formulário principal fechar ou para desligar somente quando todos os formulários forem fechados.  
  
## <a name="availability"></a>Disponibilidade  

 Por padrão, o modelo de aplicativo Visual Basic está disponível para projetos Windows Forms. Se você configurar o aplicativo para usar um objeto de inicialização diferente ou iniciar o código do aplicativo com um personalizado `Sub Main` , então esse objeto ou classe pode precisar fornecer uma implementação da <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> classe para usar o modelo de aplicativo. Para obter informações sobre como alterar o objeto de inicialização, consulte [página do aplicativo, designer de projeto (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.StartupNextInstance>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.NetworkAvailabilityChanged>
- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase>
- [Estendendo o modelo de aplicativo do Visual Basic](../customizing-extending-my/extending-the-visual-basic-application-model.md)
