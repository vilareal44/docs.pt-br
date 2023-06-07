---
title: 'Como: Escrever serviços de forma programática'
description: Veja como escrever serviços programaticamente Configurando a herança e outros elementos de infraestrutura por conta própria.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- services, creating
- Windows Service applications, creating
ms.assetid: 3abbb2ec-78d2-41e6-b9f9-6662d4e2cdc7
author: ghogen
ms.openlocfilehash: 9693e3d387f38319519ab04211d8219fe1e5dda1
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925703"
---
# <a name="how-to-write-services-programmatically"></a>Como: Escrever serviços de forma programática
Quando você opta por não usar o modelo de projeto de Serviço Windows, é possível escrever seus próprios serviços configurando a herança e outros elementos de infraestrutura por conta própria. Ao criar um serviço de forma programática, você precisa executar várias etapas que o modelo executaria para você:  
  
- Você precisa configurar a classe de serviço para ser herdada da classe <xref:System.ServiceProcess.ServiceBase>.  
  
- Você precisa criar um método `Main` para seu projeto de serviço que defina os serviços a serem executados e chame o método <xref:System.ServiceProcess.ServiceBase.Run%2A>neles.  
  
- Você precisa substituir os procedimentos <xref:System.ServiceProcess.ServiceBase.OnStart%2A> e <xref:System.ServiceProcess.ServiceBase.OnStop%2A> e preencher os códigos que desejar que eles executem.  
  
### <a name="to-write-a-service-programmatically"></a>Para escrever um serviço de forma programático  
  
1. Criar um projeto vazio e crie uma referência aos namespaces necessários seguindo estas etapas:  
  
    1. Em **Gerenciador de Soluções**, clique com o botão direito do mouse no nó **Referências** e clique em **Adicionar Referência**.  
  
    2. Na guia **.NET Framework**, role até **System.dll** e clique em **Selecionar**.  
  
    3. Role até **System.ServiceProcess.dll** e clique em **Selecionar**.  
  
    4. Clique em **OK**.  
  
2. Adicione uma classe e configure-a para ser herdada de <xref:System.ServiceProcess.ServiceBase>:  
  
     [!code-csharp[VbRadconService#7](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#7)]
     [!code-vb[VbRadconService#7](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#7)]  
  
3. Adicione o código a seguir para configurar sua classe de serviço:  
  
     [!code-csharp[VbRadconService#8](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#8)]
     [!code-vb[VbRadconService#8](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#8)]  
  
4. Crie um método `Main` para sua classe e use-o para definir o serviço que a classe conterá. `userService1` é o nome da classe:  
  
     [!code-csharp[VbRadconService#9](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#9)]
     [!code-vb[VbRadconService#9](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#9)]  
  
5. Substitua o método <xref:System.ServiceProcess.ServiceBase.OnStart%2A> e defina o processamento que deverá ocorrer quando o serviço for iniciado.  
  
     [!code-csharp[VbRadconService#10](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#10)]
     [!code-vb[VbRadconService#10](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#10)]  
  
6. Substitua quaisquer outros métodos para os quais deseje definir um processamento personalizado e escreva o código para determinar as ações que o serviço deve executar em cada caso.  
  
7. Adicionar os instaladores necessários para seu aplicativo de serviço. Para obter mais informações, confira [Como adicionar instaladores no seu aplicativo de serviço](how-to-add-installers-to-your-service-application.md).  
  
8. Compile o projeto selecionando **Compilar Solução** no menu **Compilar**.  
  
    > [!NOTE]
    > Pressione F5 para executar seu projeto. Você não pode executar um projeto de serviço dessa maneira.  
  
9. Crie um projeto de instalação e as ações personalizadas para instalar o serviço. Para obter um exemplo, confira [Passo a passo: criando um aplicativo de Serviço Windows no designer de componentes](walkthrough-creating-a-windows-service-application-in-the-component-designer.md).  
  
10. Instale o serviço. Para obter mais informações, confira [Como instalar e desinstalar serviços](how-to-install-and-uninstall-services.md).  
  
## <a name="see-also"></a>Veja também

- [Introdução a aplicativos do Serviço Windows](introduction-to-windows-service-applications.md)
- [Como: Criar serviços Windows](how-to-create-windows-services.md)
- [Como: Adicionar instaladores ao aplicativo de serviço](how-to-add-installers-to-your-service-application.md)
- [Como: Registrar em log informações sobre serviços](how-to-log-information-about-services.md)
- [Passo a passo: criando um aplicativo de Serviço Windows no Designer de componentes](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)
