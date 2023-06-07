---
title: 'Como: Continuar um serviço Windows (Visual Basic)'
description: Leia como usar o componente ServiceController para continuar um serviço do Windows (como o serviço de administração do IIS) em um computador local com Visual Basic.
ms.date: 03/30/2017
dev_langs:
- vb
f1_keywords:
- ServiceController.Continue
helpviewer_keywords:
- Windows Service applications, pausing
- pausing Windows Service applications
ms.assetid: e5d13760-4c83-4b0d-abef-39852677cd7a
author: ghogen
ms.openlocfilehash: 2a04e330ea7dc37552053b2a7915909c011727f8
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925781"
---
# <a name="how-to-continue-a-windows-service-visual-basic"></a>Como: Continuar um serviço Windows (Visual Basic)
Este exemplo usa o componente <xref:System.ServiceProcess.ServiceController> para continuar o serviço de administração do IIS no computador local.  
  
## <a name="example"></a>Exemplo  
 [!code-vb[VbRadconService#11](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#11)]  
[!code-vb[VbRadconService#13](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#13)]  
  
 Este exemplo de código também está disponível como um snippet de código do IntelliSense. No selecionador de snippet de código, ele está localizado em **Sistema Operacional Windows &gt; Serviços Windows**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Uma referência do projeto para System.serviceprocess.dll.  
  
- Acesso aos membros do namespace <xref:System.ServiceProcess>. Adicione uma instrução `Imports` se você não está qualificando totalmente os nomes de membros em seu código. Para obter mais informações, consulte [Instrução Imports (tipo e namespace .NET)](../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md).  
  
## <a name="robust-programming"></a>Programação robusta  
 A propriedade <xref:System.ServiceProcess.ServiceController.MachineName%2A> da classe <xref:System.ServiceProcess.ServiceController> é o computador local por padrão. Para referenciar os Serviços Windows em outro computador, altere a propriedade <xref:System.ServiceProcess.ServiceController.MachineName%2A> para o nome desse computador.  
  
 Não é possível chamar o método <xref:System.ServiceProcess.ServiceController.Continue%2A> em um serviço até que o status do controlador de serviço seja <xref:System.ServiceProcess.ServiceControllerStatus.Paused>.  
  
 As seguintes condições podem causar uma exceção:  
  
- O serviço não pode ser retomado. (<xref:System.InvalidOperationException>)  
  
- Ocorreu um erro ao acessar uma API do sistema. (<xref:System.ComponentModel.Win32Exception>)  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 O controle de serviços no computador pode ser restringido usando a enumeração <xref:System.ServiceProcess.ServiceControllerPermissionAccess> para definir permissões na classe <xref:System.ServiceProcess.ServiceControllerPermission>.  
  
 O acesso a informações de serviço pode ser restringido usando a enumeração <xref:System.Security.Permissions.PermissionState> para definir permissões na classe <xref:System.Security.Permissions.SecurityPermission>.  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceProcess.ServiceController>
- <xref:System.ServiceProcess.ServiceControllerStatus>
- [Como: Pausar um Serviço Windows (Visual Basic)](how-to-pause-a-windows-service-visual-basic.md)
