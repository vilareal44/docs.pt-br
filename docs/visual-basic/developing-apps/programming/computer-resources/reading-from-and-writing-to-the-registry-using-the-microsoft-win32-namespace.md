---
title: Ler e gravar no Registro usando o namespace Microsoft.Win32
ms.date: 07/20/2015
helpviewer_keywords:
- registry [Visual Basic]
ms.assetid: 4a0dcce0-c27b-4199-baa8-ee4528da6a56
ms.openlocfilehash: 10431b1ad40ed320541a22fb46cc8db6dbb775b0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84360066"
---
# <a name="reading-from-and-writing-to-the-registry-using-the-microsoftwin32-namespace-visual-basic"></a>Lendo e gravando no Registro usando o namespace Microsoft.Win32 (Visual Basic)

Embora `My.Computer.Registry` deva suprir suas necessidades básicas ao programar no Registro, você também pode usar as classes <xref:Microsoft.Win32.Registry> e <xref:Microsoft.Win32.RegistryKey> no namespace <xref:Microsoft.Win32> do .NET Framework.  
  
## <a name="keys-in-the-registry-class"></a>Chaves na classe de Registro  

 A classe <xref:Microsoft.Win32.Registry> fornece as chaves base do Registro que podem ser usadas para acessar subchaves e seus valores. As chaves base em si são somente leitura. A tabela a seguir lista e descreve as sete chaves expostas pela classe <xref:Microsoft.Win32.Registry>.  
  
|**Chave**|**Descrição**|  
|-------------|---------------------|  
|<xref:Microsoft.Win32.Registry.ClassesRoot>|Define os tipos de documentos e as propriedades associadas a esses tipos.|  
|<xref:Microsoft.Win32.Registry.CurrentConfig>|Contém informações de configuração de hardware que não são específicas do usuário.|  
|<xref:Microsoft.Win32.Registry.CurrentUser>|Contém informações sobre as preferências do usuário atual, como variáveis ambientais.|  
|<xref:Microsoft.Win32.Registry.DynData>|Contém dados do registro dinâmico, como aqueles usados por Drivers de dispositivo virtual.|  
|<xref:Microsoft.Win32.Registry.LocalMachine>|Contém cinco subchaves (Hardware, SAM, Segurança, Software e Sistema) que armazenam os dados de configuração do computador local.|  
|<xref:Microsoft.Win32.Registry.PerformanceData>|Contém informações de desempenho de componentes de software.|  
|<xref:Microsoft.Win32.Registry.Users>|Contém informações sobre as preferências do usuário padrão.|  
  
> [!IMPORTANT]
> É mais seguro gravar dados na pasta do usuário atual (<xref:Microsoft.Win32.Registry.CurrentUser>) do que no computador local (<xref:Microsoft.Win32.Registry.LocalMachine>). Uma condição que costuma ser chamada de "squatting" ocorre quando a chave que você está criando foi criada anteriormente por outro processo, possivelmente mal-intencionado. Para evitar que isso ocorra, use um método, como <xref:Microsoft.Win32.RegistryKey.GetValue%2A>, que retornará `Nothing` se a chave ainda não existir.  
  
## <a name="reading-a-value-from-the-registry"></a>Lendo um valor do Registro  

 O código a seguir mostra como ler uma cadeia de caracteres de HKEY_CURRENT_USER.  
  
 [!code-vb[VbResourceTasks#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#20)]  
  
 O código a seguir lê, incrementa e grava uma cadeia de caracteres em HKEY_CURRENT_USER.  
  
 [!code-vb[VbResourceTasks#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#21)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.SystemException>
- <xref:System.ApplicationException>
- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- [Instrução Try...Catch...Finally](../../../language-reference/statements/try-catch-finally-statement.md)
- [Ler e gravar no Registro](reading-from-and-writing-to-the-registry.md)
- [Segurança e Registro](security-and-the-registry.md)
