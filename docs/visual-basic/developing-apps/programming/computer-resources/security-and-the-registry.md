---
title: Segurança e Registro
ms.date: 07/20/2015
helpviewer_keywords:
- security [Visual Basic], registry
- registry [Visual Basic], security issues
ms.assetid: 9980aff7-2f69-492b-8f66-29a9a76d3df5
ms.openlocfilehash: 2eba9d177769b22de3f931bc7af4905a80e316b5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359962"
---
# <a name="security-and-the-registry-visual-basic"></a>Segurança e Registro (Visual Basic)

Esta página discute as implicações de segurança de armazenar dados no Registro.  
  
## <a name="permissions"></a>Permissões  

 Não é seguro armazenar segredos, como senhas, no Registro como texto sem formatação, mesmo se a chave do Registro estiver protegida por ACLs (listas de controle de acesso).  
  
 Trabalhar com o Registro pode comprometer a segurança ao permitir o acesso inadequado aos recursos do sistema ou a informações protegidas. Para usar essas propriedades, é necessário ter lido e gravado permissões da enumeração <xref:System.Security.Permissions.RegistryPermissionAccess>, que controla o acesso a variáveis do Registro. Qualquer código sendo executado com confiança total (nos termos da política de segurança padrão, é qualquer código instalado no disco rígido local do usuário) tem as permissões necessárias para acessar o Registro. Para obter mais informações, consulte a classe <xref:System.Security.Permissions.RegistryPermission>.  
  
 As variáveis de Registro não devem ser armazenadas em locais de memória em que o código sem <xref:System.Security.Permissions.RegistryPermission> possa acessá-los. Da mesma forma, ao conceder permissões, conceda os privilégios mínimos necessários para realizar o trabalho.  
  
 Os valores de acesso de permissão ao Registro são definidos pela enumeração <xref:System.Security.Permissions.RegistryPermissionAccess>. A tabela a seguir detalha seus membros.  
  
|Valor|Acesso às variáveis de Registro|  
|-----------|----------------------------------|  
|`AllAccess`|Criar, ler e gravar|  
|`Create`|Criar|  
|`NoAccess`|Sem acesso|  
|`Read`|Ler|  
|`Write`|Gravar|  
  
## <a name="checking-values-in-registry-keys"></a>Verificando valores nas chaves do Registro  

 Ao criar um valor de Registro, é necessário decidir o que fazer se esse valor já existir. Outro processo, talvez um mal-intencionado, pode já ter criado o valor e tem acesso a ele. Ao colocar dados no valor de Registro, os dados estarão disponíveis para o outro processo. Para impedir isso, use o método `GetValue`. Ele retornará `Nothing` se a chave ainda não existir.  
  
> [!IMPORTANT]
> Ao ler o Registro em um aplicativo Web, a identidade do usuário atual depende da autenticação e da representação implementadas no aplicativo Web.  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- [Ler e gravar no Registro](reading-from-and-writing-to-the-registry.md)
