---
title: Segurança e serialização
description: Leia sobre segurança e serialização. Use SecurityPermission com o sinalizador SerializationFormatter especificado para ver ou modificar dados de instância de objeto.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- code security, serialization
- serialization, security
- secure coding, serialization
- security [.NET Framework], serialization
ms.assetid: b921bc94-bd3a-4c91-9ede-2c8d4f78ea9a
ms.openlocfilehash: 393e334e8165f55812681848070929bdfb03a2a5
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855680"
---
# <a name="security-and-serialization"></a>Segurança e serialização

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

Como a serialização pode permitir que outro código veja ou modifique dados de instância de objeto que, de outra forma, seriam inacessíveis, uma permissão especial é necessária para o código que executa a serialização: <xref:System.Security.Permissions.SecurityPermission> com o <xref:System.Security.Permissions.SecurityPermissionFlag.SerializationFormatter> sinalizador especificado. De acordo com a política padrão, essa permissão não é dada ao código da intranet ou baixado da Internet; somente o código no computador local recebe essa permissão.  
  
 Normalmente, todos os campos de uma instância de objeto são serializados, o que significa que os dados são representados nos dados serializados da instância. É possível que o código possa interpretar o formato para determinar quais são os valores de dados, independentemente da acessibilidade do membro. Da mesma forma, a desserialização extrai dados da representação serializada e define o estado do objeto diretamente, independentemente das regras de acessibilidade.  
  
 Qualquer objeto que possa conter dados sensíveis à segurança deve se tornar não serializável, se possível. Se ele precisar ser serializável, tente criar campos específicos que contenham dados confidenciais não serializáveis. Se esses campos não puderem ser transserializáveis, os dados confidenciais serão expostos a qualquer código que tenha permissão para serializar. Certifique-se de que nenhum código mal-intencionado possa obter essa permissão.  
  
 A <xref:System.Runtime.Serialization.ISerializable> interface destina-se ao uso somente pela infraestrutura de serialização. No entanto, se desprotegido, ele poderá liberar informações confidenciais. Se você fornecer serialização personalizada implementando **ISerializable**, certifique-se de tomar as seguintes precauções:  
  
- O <xref:System.Runtime.Serialization.ISerializable.GetObjectData%2A> método deve ser explicitamente protegido, exigindo a permissão **SecurityPermission** com **SerializationFormatter** especificada ou garantindo que nenhuma informação confidencial seja lançada com a saída do método. Por exemplo:  
  
    ```vb  
    Public Overrides<SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter := True)>  _  
    Sub GetObjectData(info As SerializationInfo, context As StreamingContext)  
    End Sub  
    ```  
  
    ```csharp  
    [SecurityPermissionAttribute(SecurityAction.Demand,SerializationFormatter
    =true)]  
    public override void GetObjectData(SerializationInfo info,
    StreamingContext context)  
    {  
    }  
    ```  
  
- O Construtor especial usado para serialização também deve executar uma validação de entrada completa e deve ser protegido ou privado para ajudar a proteger contra uso indevido de código mal-intencionado. Ele deve impor as mesmas verificações de segurança e as permissões necessárias para obter uma instância de tal classe por outros meios, como criar explicitamente a classe ou criá-la indiretamente por meio de algum tipo de fábrica.  
  
## <a name="see-also"></a>Veja também

- [Diretrizes de codificação segura](../../standard/security/secure-coding-guidelines.md)
