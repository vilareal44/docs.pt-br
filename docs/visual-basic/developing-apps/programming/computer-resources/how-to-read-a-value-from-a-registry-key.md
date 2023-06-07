---
title: 'Como: ler um valor de uma chave do Registro'
ms.date: 07/20/2015
helpviewer_keywords:
- registry keys [Visual Basic], determining if a value exists in
- My.Computer.Registry object, examples
- registry [Visual Basic], determining if values exist
- registry keys [Visual Basic], reading from
- registry [Visual Basic], reading
ms.assetid: 775d0a57-68c9-464e-8949-9a39bd29cc64
ms.openlocfilehash: 74069b111f4316eb81c74f5e62c1fbff6ab8f4b8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401843"
---
# <a name="how-to-read-a-value-from-a-registry-key-in-visual-basic"></a>Como ler um valor a partir de uma chave do Registro no Visual Basic

O método `GetValue` do objeto `My.Computer.Registry` pode ser usado para ler valores no Registro do Windows.  
  
 Se a chave, "Software\MyApp" no exemplo a seguir, não existir, uma exceção será gerada. Se o `ValueName`, "Name" no exemplo a seguir, não existir, `Nothing` será retornado.  
  
 O método `GetValue` também pode ser usado para determinar se um determinado valor existe em uma chave do Registro específica.  
  
 Quando o código lê o Registro de um aplicativo Web, o usuário atual será determinado pela autenticação e a representação implementadas no aplicativo Web.  
  
### <a name="to-read-a-value-from-a-registry-key"></a>Para ler um valor de uma chave do Registro  
  
- Use o método `GetValue` (especificando o caminho e nome) para ler um valor de chave do Registro. O exemplo a seguir lê o valor `Name` de `HKEY_CURRENT_USER\Software\MyApp` e o exibe em uma caixa de mensagem.  
  
     [!code-vb[VbResourceTasks#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#4)]  
  
 Este exemplo de código também está disponível como um snippet de código do IntelliSense. No seletor de snippet de código, ele está localizado em **Sistema Operacional Windows &gt; Registro**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).  
  
### <a name="to-determine-whether-a-value-exists-in-a-registry-key"></a>Para determinar se um valor existe em uma chave do Registro  
  
- Use o método `GetValue` para recuperar o valor. O código a seguir verifica se o valor existe e retorna uma mensagem se ele não existir.  
  
     [!code-vb[VbResourceTasks#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#12)]  
  
## <a name="robust-programming"></a>Programação robusta  

 O Registro contém chaves de nível superior ou raiz, que são usadas para armazenar dados. Por exemplo, a chave raiz HKEY_LOCAL_MACHINE é usada para armazenar configurações de nível do computador usadas por todos os usuários, enquanto HKEY_CURRENT_USER é usada para armazenar dados específicos a um usuário individual.  
  
 As seguintes condições podem causar uma exceção:  
  
- O nome da chave é `Nothing` (<xref:System.ArgumentNullException>).  
  
- O usuário não tem permissões para ler nas chaves do Registro (<xref:System.Security.SecurityException>).  
  
- O nome da chave excede o limite de 255 caracteres (<xref:System.ArgumentException>).  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  

 Para executar esse processo, seu assembly exige um nível de privilégio concedido pela classe <xref:System.Security.Permissions.RegistryPermission>. Se você estiver executando em um contexto de confiança parcial, o processo poderá gerar uma exceção em razão dos privilégios insuficientes. Da mesma forma, o usuário deve ter as ACLs corretas para criar ou gravar nas configurações. Por exemplo, um aplicativo local que tem a permissão de segurança de acesso do código pode não ter permissão do sistema operacional. Para obter mais informações, consulte [Noções Básicas da Segurança de Acesso do Código](../../../../framework/misc/code-access-security-basics.md).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- <xref:Microsoft.Win32.RegistryHive>
- [Ler e gravar no Registro](reading-from-and-writing-to-the-registry.md)
