---
title: 'How to: Replace the WCF URL Reservation with a Restricted Reservation'
ms.date: 03/30/2017
ms.assetid: 2754d223-79fc-4e2b-a6ce-989889f2abfa
ms.openlocfilehash: 780a2c7fe240ed624ff106e8157661f8b76b32bd
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202370"
---
# <a name="how-to-replace-the-wcf-url-reservation-with-a-restricted-reservation"></a>How to: Replace the WCF URL Reservation with a Restricted Reservation

Uma reserva de URL permite restringir quem pode receber mensagens de uma URL ou um conjunto de URLs. Uma reserva consiste em um modelo de URL, uma ACL (lista de controle de acesso) e um conjunto de sinalizadores. O modelo de URL define quais URLs a reserva afeta. Para obter mais informações sobre como os modelos de URL são processados, consulte [Roteamento de solicitações de entrada](/windows/win32/http/routing-incoming-requests). A ACL controla qual usuário ou grupo de usuários tem permissão para receber mensagens das URLs especificadas. Os sinalizadores indicam se a reserva é para conceder a um usuário ou grupo permissão para escutar na URL diretamente ou para delegar a permissão para escutar algum outro processo.  
  
 Como parte da configuração padrão do sistema operacional, o Windows Communication Foundation (WCF) cria uma reserva globalmente acessível para a porta 80 para permitir que todos os usuários executem aplicativos que usam uma associação HTTP dupla para comunicação duplex. Como a ACL nessa reserva é para todos, os administradores não podem explicitamente permitir ou impedir permissão para escutar em uma URL ou conjunto de URLs. Este tópico explica como excluir essa reserva e como recriar a reserva com uma ACL restrita.  
  
No Windows Vista ou no Windows Server 2008, você pode exibir todas as reservas de URL HTTP de um prompt de comando elevado digitando `netsh http show urlacl` . O exemplo a seguir mostra o que uma reserva de URL do WCF deve ser semelhante a:

```
Reserved URL : http://+:80/Temporary_Listen_Addresses/  
        User: \Everyone  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;WD)  
```

 A reserva consiste em um modelo de URL usado quando um aplicativo WCF está usando uma associação HTTP dual para comunicação duplex. As URLs desse formulário são usadas para um serviço WCF enviar mensagens de volta para o cliente WCF ao se comunicar por uma associação HTTP dupla. Todos recebem permissão para escutar na URL, mas não para delegar a escuta a outro processo. Por fim, a ACL é descrita em SSDL (Security Descriptor Definition Language). Para obter mais informações sobre SSDL, consulte [SSDL](/windows/win32/secauthz/security-descriptor-definition-language)  
  
## <a name="to-delete-the-wcf-url-reservation"></a>Para excluir a reserva de URL do WCF  
  
1. Clique **em Iniciar**, aponte **para todos os programas**, clique em **acessórios**, clique com o botão direito do mouse em prompt de **comando** e clique em **Executar como administrador** no menu de contexto que aparece. Clique em **continuar** na janela controle de conta de usuário (UAC) que pode pedir permissões para continuar.  
  
2. Digite na `netsh http delete urlacl url=http://+:80/Temporary_Listen_Addresses/` janela do prompt de comando.  
  
3. Se a reserva for excluída com êxito, a seguinte mensagem será exibida. **Reserva de URL excluída com êxito**  
  
## <a name="creating-a-new-security-group-and-new-restricted-url-reservation"></a>Criando um novo grupo de segurança e nova reserva de URL restrita  
 Para substituir a reserva de URL do WCF por uma reserva restrita, você deve primeiro criar um novo grupo de segurança. Você pode fazer isso de uma das duas maneiras: em um prompt de comando ou no console de gerenciamento do computador. Você só precisa fazer uma.  
  
### <a name="to-create-a-new-security-group-from-a-command-prompt"></a>Para criar um novo grupo de segurança a partir de um prompt de comando  
  
1. Clique **em Iniciar**, aponte **para todos os programas**, clique em **acessórios**, clique com o botão direito do mouse em prompt de **comando** e clique em **Executar como administrador** no menu de contexto que aparece. Clique em **continuar** na janela controle de conta de usuário (UAC) que pode pedir permissões para continuar.  
  
2. Digite no `net localgroup "<security group name>" /comment:"<security group description>" /add` prompt de comando. Substituindo **\<security group name>** pelo nome do grupo de segurança que você deseja criar e **\<security group description>** com uma descrição adequada para o grupo de segurança.  
  
3. Se o grupo de segurança for criado com êxito, a seguinte mensagem será exibida. **O comando foi concluído com êxito.**  
  
### <a name="to-create-a-new-security-group-from-the-computer-management-console"></a>Para criar um novo grupo de segurança no console de gerenciamento do computador  
  
1. Clique em **Iniciar**, em **painel de controle**, em **Ferramentas administrativas**e em **Gerenciamento do computador** para abrir o console de gerenciamento do computador. Clique em **continuar** na janela controle de conta de usuário (UAC) que pode pedir permissões para continuar.  
  
2. Clique **em ferramentas do sistema**, clique em **usuários e grupos locais**, clique com o botão direito do mouse na pasta **grupos** e clique em **novo grupo** no menu de contexto que aparece. Digite o nome do **grupo**desejado, a **Descrição** e outros detalhes desse novo grupo de segurança e clique no botão **criar** para criar o grupo de segurança.  
  
### <a name="to-create-the-restricted-url-reservation"></a>Para criar a reserva de URL restrita  
  
1. Clique **em Iniciar**, aponte **para todos os programas**, clique em **acessórios**, clique com o botão direito do mouse em prompt de **comando** e clique em **Executar como administrador** no menu de contexto que aparece. Clique em **continuar** na janela controle de conta de usuário (UAC) que pode pedir permissões para continuar.  
  
2. Digite no `netsh http add urlacl url=http://+:80/Temporary_Listen_Addresses/ user="<machine name>\<security group name>` prompt de comando. Substituindo **\<machine name>** pelo nome do computador no qual o grupo deve ser criado e **\<security group name>** pelo nome do grupo de segurança que você criou anteriormente.  
  
3. Se a reserva for criada com êxito, a seguinte mensagem será exibida. **Reserva de URL adicionada com êxito**.
