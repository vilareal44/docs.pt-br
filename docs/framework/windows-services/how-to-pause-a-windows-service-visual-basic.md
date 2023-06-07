---
title: 'Como: Pausar um Serviço Windows (Visual Basic)'
description: Leia como usar o componente ServiceController para pausar um serviço do Windows (como o serviço de administração do IIS) em um computador local com Visual Basic.
ms.date: 03/30/2017
dev_langs:
- vb
f1_keywords:
- ServiceController.Pause
helpviewer_keywords:
- Windows Service applications, pausing
- pausing Windows Service applications
ms.assetid: eddb9409-942b-46b6-a2ce-fbd4c65f2790
author: ghogen
ms.openlocfilehash: 628cc2e896f7f8a289e52674b721c4aef605854c
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925560"
---
# <a name="how-to-pause-a-windows-service-visual-basic"></a>Como: Pausar um Serviço Windows (Visual Basic)
Este exemplo usa o componente <xref:System.ServiceProcess.ServiceController> para pausar o serviço de administração do IIS no computador local.  
  
## <a name="example"></a>Exemplo  
 [!code-vb[VbRadconService#11](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#11)]  
[!code-vb[VbRadconService#12](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#12)]  
  
 Este exemplo de código também está disponível como um snippet de código do IntelliSense. No selecionador de snippet de código, ele está localizado em **Sistema Operacional Windows &gt; Serviços Windows**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Uma referência do projeto para System.serviceprocess.dll.  
  
- Acesso aos membros do namespace <xref:System.ServiceProcess>. Adicione uma instrução `Imports` se você não está qualificando totalmente os nomes de membros em seu código. Para obter mais informações, consulte [Instrução Imports (tipo e namespace .NET)](../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md).  
  
## <a name="robust-programming"></a>Programação robusta  
 A propriedade <xref:System.ServiceProcess.ServiceController.MachineName%2A> da classe <xref:System.ServiceProcess.ServiceController> é o computador local por padrão. Para referenciar os Serviços Windows em outro computador, altere a propriedade <xref:System.ServiceProcess.ServiceController.MachineName%2A> para o nome desse computador.  
  
 As seguintes condições podem causar uma exceção:  
  
- O serviço não pode ser colocado em pausa. (<xref:System.InvalidOperationException>)  
  
- Ocorreu um erro ao acessar uma API do sistema. (<xref:System.ComponentModel.Win32Exception>)  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 O controle de serviços no computador pode ser restringido usando o <xref:System.ServiceProcess.ServiceControllerPermissionAccess> para definir permissões no <xref:System.ServiceProcess.ServiceControllerPermission>.  
  
 O acesso a informações de serviço pode ser restringido usando o <xref:System.Security.Permissions.PermissionState> para definir permissões em <xref:System.Security.Permissions.SecurityPermission>.  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceProcess.ServiceController>
- <xref:System.ServiceProcess.ServiceControllerStatus>
- <xref:System.ServiceProcess.ServiceController.WaitForStatus%2A>
- [Como: Continuar um serviço Windows (Visual Basic)](how-to-continue-a-windows-service-visual-basic.md)
