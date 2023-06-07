---
title: Funções de servidor e banco de dados no SQL Server
description: Saiba mais sobre o servidor fixo e as funções de banco de dados fixas, que têm um conjunto fixo de permissões atribuídas a eles. SQL Server usa a segurança baseada em função.
ms.date: 03/30/2017
ms.assetid: 5482dfdb-e498-4614-8652-b174829eed13
ms.openlocfilehash: 9c3725b0404a5b3c754a53fa56f4a22497afee70
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286230"
---
# <a name="server-and-database-roles-in-sql-server"></a>Funções de servidor e banco de dados no SQL Server
Todas as versões do SQL Server usam segurança baseada em função, o que permite que você atribua permissões para uma função ou grupo de usuários, em vez de usuários individuais. O servidor fixo e as funções de banco de dados fixas têm um conjunto fixo de permissões atribuídas a eles.  
  
## <a name="fixed-server-roles"></a>Funções de servidor fixas  
 As funções de servidor fixas têm um conjunto fixo de permissões e escopo no servidor. Elas são destinadas para uso em administrar o SQL Server e as permissões atribuídas a elas não podem ser modificadas. Os logons podem ser atribuídos às funções de servidor fixas sem ter uma conta de usuário em um banco de dados.  
  
> [!IMPORTANT]
> A função de servidor fixa `sysadmin` aborda todas as outras funções e tem escopo ilimitado. Não adicione entidades de segurança a essa função a menos que elas sejam altamente confiáveis. Os membros de função `sysadmin` têm privilégios administrativos irrevogáveis em todos os bancos de dados e recursos do servidor.  
  
 Seja seletivo ao adicionar usuários a funções de servidor fixas. Por exemplo, a função `bulkadmin` permite que os usuários insiram o conteúdo do qualquer arquivo local em uma tabela, o que pode prejudicar a integridade de dados. Consulte Manuais Online do SQL Server para obter a lista completa de funções e permissões de servidor fixas.  
  
## <a name="fixed-database-roles"></a>Funções de banco de dados fixas  
 As funções de banco de dados fixas têm um conjunto pré-definido de permissões que são criadas para permitir que você gerencie grupos de permissões com facilidade. Os membros da função `db_owner` podem executar todas as atividades de configuração e manutenção no banco de dados.  
  
 Para obter mais informações sobre as funções predefinidas do SQL Server, consulte os seguintes recursos.  
  
|Recurso|Descrição|  
|--------------|-----------------|  
|[Funções de nível de servidor](/sql/relational-databases/security/authentication-access/server-level-roles)|Descreve as funções de servidor fixas e as permissões associadas a elas no SQL Server.|  
|[Funções de nível de banco de dados](/sql/relational-databases/security/authentication-access/database-level-roles)|Descreve funções de banco de dados fixas e as permissões associadas a elas|  
  
## <a name="database-roles-and-users"></a>Funções e usuários do banco de dados  
 Os logons devem ser mapeados para contas de usuário do banco de dados para funcionar com objetos de banco de dados. Os usuários do banco de dados podem então ser adicionados às funções de banco de dados, herdando todos os conjuntos de permissões associados a essas funções. Todas as permissões podem ser concedidas.  
  
 Você também deve considerar a função `public`, a conta de usuário `dbo` e a conta `guest` ao projetar a segurança de seu aplicativo.  
  
### <a name="the-public-role"></a>A função pública  
 A função `public` está contida em cada banco de dados, incluindo bancos de dados do sistema. Ela não pode ser descartada e você não pode adicionar ou remover usuários dela. As permissões concedidas à função `public` são herdadas por todos os outros usuários e funções porque pertencem à função `public` por padrão. Conceda a `public` somente as permissões que você deseja que todos os usuários tenham.  
  
### <a name="the-dbo-user-account"></a>A conta de usuário dbo  
 O `dbo`, ou o proprietário do banco de dados, é uma conta de usuário que tem permissões implícitas para executar todas as atividades no banco de dados. Os membros da função de servidor fixa `sysadmin` são mapeados automaticamente para `dbo`.  
  
> [!NOTE]
> `dbo`também é o nome de um esquema, conforme discutido em [propriedade e separação de esquema de usuário em SQL Server](ownership-and-user-schema-separation-in-sql-server.md).  
  
 A conta de usuário `dbo` é confundida frequentemente com a função de banco de dados fixa `db_owner`. O escopo do `db_owner` é um banco de dados; o escopo do `sysadmin` é o servidor inteiro. A associação na função de `db_owner` não confere privilégios do usuário de `dbo`.  
  
### <a name="the-guest-user-account"></a>A conta de usuário guest  
 Após um usuário ter sido autenticado e permitido fazer logon em uma instância do SQL Server, uma conta de usuário separada deve existir em cada banco de dados que o usuário precise acessar. Exigir uma conta de usuário em cada banco de dados impede que os usuários se conectem a uma instância do SQL Server e acessem todos os bancos de dados em um servidor. A existência de uma conta de usuário `guest` no banco de dados contorna esse requisito permitindo que um logon sem uma conta de usuário do banco de dados acesse um banco de dados.  
  
 A conta `guest` é uma conta interna em todas as versões do SQL Server. Por padrão, ela é desabilitada em novos bancos de dados. Se estiver ativada, você poderá desativá-la revogando sua permissão CONNECT executando a instrução REVOKE CONNECT FROM GUEST do Transact-SQL.  
  
> [!IMPORTANT]
> Evite usar a conta `guest`; todos os logons sem suas próprias permissões de banco de dados obtêm as permissões de banco de dados concedidas a essa conta. Se você usar a conta `guest`, conceda permissões mínimas.  
  
 Para obter mais informações sobre logons, usuários e funções do SQL Server, consulte os seguintes recursos.  
  
|Recurso|Descrição|  
|--------------|-----------------|  
|[Guia de Introdução às permissões do mecanismo de banco de dados](/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions)|Contém links para tópicos que descrevem entidades de segurança, funções, credenciais, protegíveis e permissões.|  
|[Principals](/sql/relational-databases/security/authentication-access/principals-database-engine)|Descreve as entidades de segurança e contém links para tópicos que descrevem funções de servidor e banco de dados.|  
  
## <a name="see-also"></a>Veja também

- [Protegendo aplicativos ADO.NET](../securing-ado-net-applications.md)
- [Cenários de Segurança de Aplicativo no SQL Server](application-security-scenarios-in-sql-server.md)
- [Autenticação no SQL Server](authentication-in-sql-server.md)
- [Propriedade e separação do esquema do usuário no SQL Server](ownership-and-user-schema-separation-in-sql-server.md)
- [Autorização e permissões no SQL Server](authorization-and-permissions-in-sql-server.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
