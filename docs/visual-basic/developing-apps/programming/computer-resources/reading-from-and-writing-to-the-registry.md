---
title: Ler e gravar no Registro
ms.date: 07/20/2015
helpviewer_keywords:
- My.Computer.Registry object, tasks
- registry [Visual Basic], writing to
- registry [Visual Basic], reading
ms.assetid: a13da106-185b-41d7-b23c-416da65e21e4
ms.openlocfilehash: bb400ef89edaa4eb743aee3a7f2cc5b9dfec4534
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84360053"
---
# <a name="reading-from-and-writing-to-the-registry-visual-basic"></a>Lendo e gravando a partir do Registro (Visual Basic)

Este tópico descreve as tarefas e tópicos conceituais associados ao Registro.  
  
 Ao programar em Visual Basic, você pode optar por acessar o Registro por meio das funções fornecidas pelo Visual Basic ou das classes de Registro do .NET Framework. O Registro hospeda informações do sistema operacional, bem como informações de aplicativos hospedados no computador. Trabalhar com o Registro pode comprometer a segurança ao permitir o acesso inadequado aos recursos do sistema ou a informações protegidas.  
  
## <a name="in-this-section"></a>Nesta seção  

 [Como: Criar uma chave do Registro e definir seu valor](how-to-create-a-registry-key-and-set-its-value.md)  
 Descreve como usar os métodos `CreateSubKey` e `SetValue` do objeto `My.Computer.Registry` para criar uma chave do Registro e definir seu valor.  
  
 [Como: ler um valor de uma chave do Registro](how-to-read-a-value-from-a-registry-key.md)  
 Descreve como usar o método `GetValue` do objeto `My.Computer.Registry` para ler um valor de uma chave do Registro.  
  
 [Como: Excluir uma Chave do Registro](how-to-delete-a-registry-key.md)  
 Descreve como usar o método `DeleteSubKey` da propriedade `My.Computer.Registry.CurrentUser` para excluir uma chave do Registro.  
  
 [Ler e gravar no Registro usando o namespace Microsoft.Win32](reading-from-and-writing-to-the-registry-using-the-microsoft-win32-namespace.md)  
 Descreve como usar as classes `Registry` e `RegistryKey` do .NET Framework para acessar o Registro.  
  
 [Segurança e Registro](security-and-the-registry.md)  
 Aborda questões de segurança que envolvem o Registro.  
  
## <a name="related-sections"></a>Seções relacionadas  

 <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>  
 Lista e explica membros do objeto `My.Computer.Registry`.  
  
 <xref:Microsoft.Win32.Registry>  
 Apresenta uma visão geral da classe `Registry`, bem como links para membros e chaves individuais.
