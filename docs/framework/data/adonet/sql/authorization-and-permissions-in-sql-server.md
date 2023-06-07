---
title: Autorização e permissões no SQL Server
description: Saiba como conceder permissões explicitamente para tornar objetos de banco de dados que você cria acessíveis aos usuários em SQL Server com ADO.NET.
ms.date: 03/30/2017
ms.assetid: d340405c-91f4-4837-a3cc-a238ee89888a
ms.openlocfilehash: eb01e29b36da5e1793b9176301a968a42115d19c
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286527"
---
# <a name="authorization-and-permissions-in-sql-server"></a>Autorização e permissões no SQL Server
Ao criar objetos de banco de dados, você deve conceder permissões explicitamente para torná-los acessíveis aos usuários. Cada objeto protegível tem permissões que podem ser concedidas a uma entidade de segurança usando declarações de permissão.  
  
## <a name="the-principle-of-least-privilege"></a>O princípio de privilégios mínimos  
 Desenvolver um aplicativo usando uma abordagem LUA (conta de usuário com privilégios mínimos) é uma parte importante de uma estratégia detalhada defensiva para combater ameaças de segurança. A abordagem LUA garante que os usuários sigam o princípio de privilégios mínimos e sempre façam logon com contas de usuário limitadas. As tarefas administrativas são divididas usando funções de servidor fixas, e o uso da função de servidor fixa `sysadmin` é rigorosamente restrito.  
  
 Siga sempre o princípio de privilégios mínimos para conceder permissões aos usuários do banco de dados. Conceda as permissões mínimas necessárias a um usuário ou a uma função para realizar uma determinada tarefa.  
  
> [!IMPORTANT]
> O desenvolvimento e o teste de um aplicativo usando a abordagem LUA adiciona um grau de dificuldade ao processo de desenvolvimento. É mais fácil criar objetos e escrever código quando conectado como um administrador do sistema ou proprietário do banco de dados do que usando uma conta com LUA. No entanto, o desenvolvimento de aplicativos usando uma conta altamente privilegiada pode ofuscar o impacto da funcionalidade reduzida quando usuários com privilégios mínimos tentarem executar um aplicativo que exija permissões elevadas para funcionar corretamente. Conceder permissões excessivas aos usuários para readquirir funcionalidade perdida pode deixar seu aplicativo vulnerável a ataque. A criação, o desenvolvimento e o teste de seu aplicativo conectado com uma conta com LUA impõe uma abordagem disciplinada ao planejamento de segurança, que elimina surpresas desagradáveis e a tentação de conceder privilégios elevados como uma solução rápida. Você pode usar um logon do SQL Server para testar mesmo que seu aplicativo esteja destinado a ser implantado usando a autenticação do Windows.  
  
## <a name="role-based-permissions"></a>Permissões baseadas em função  
 Conceder permissões a funções em vez de aos usuários simplifica a administração de segurança. Os conjuntos de permissões que são atribuídos às funções são herdados por todos os membros da função. É mais fácil adicionar ou remover usuários de uma função do que recriar conjuntos de permissões separados para usuários individuais. As funções podem ser aninhadas. No entanto, níveis de aninhamento excessivos podem prejudicar o desempenho. Você também pode adicionar usuários a funções de banco de dados fixas para simplificar a atribuição de permissões.  
  
 Você pode conceder permissões no nível de esquema. Os usuários herdam automaticamente permissões em todos os novos objetos criados no esquema. Você não precisa conceder permissões à medida que novos objetos são criados.  
  
## <a name="permissions-through-procedural-code"></a>Permissões por meio de código procedural  
 O encapsulamento do acesso a dados por meio de módulos, como procedimentos armazenados e funções definidas pelo usuário, fornece uma camada adicional de proteção em torno do aplicativo. Você pode impedir que os usuários interajam diretamente com os objetos do banco de dados concedendo permissões somente a procedimentos armazenados ou funções e negando permissões para objetos subjacentes, como tabelas. O SQL Server obtém isso por meio do encadeamento de propriedade.  
  
## <a name="permission-statements"></a>Instruções de permissão  
 As três instruções de permissão Transact-SQL são descritas na tabela a seguir.  
  
|Instrução de permissão|Description|  
|--------------------------|-----------------|  
|GRANT|Concede uma permissão.|  
|REVOKE|Revoga uma permissão. Esse é o estado padrão de um novo objeto. Uma permissão revogada de um usuário ou de uma função ainda pode ser herdada de outros grupos ou funções aos quais a entidade de segurança está atribuída.|  
|NEGAR|DENY revoga uma permissão de modo que não possa ser herdada. DENY tem precedência sobre todas as permissões, com a exceção de que DENY não se aplica a proprietários de objetos ou membros de `sysadmin`. Se você NEGAR permissões em um objeto à função `public` ela será negada a todos os usuários e funções com exceção dos proprietários de objetos e membros de `sysadmin`.|  
  
- A instrução GRANT pode atribuir permissões a um grupo ou a uma função, que podem ser herdadas por usuários do banco de dados. No entanto, a instrução DENY tem precedência sobre quaisquer outras instruções de permissão. Portanto, um usuário para o qual uma permissão é negada não pode herdá-la de outra função.  
  
> [!NOTE]
> As permissões não podem ser negadas para membros da função de servidor fixa `sysadmin` e proprietários de objetos.  
  
## <a name="ownership-chains"></a>Cadeias de propriedade  
 O SQL Server garante que somente as entidades de segurança que receberam permissão podem acessar objetos. Quando vários objetos de banco de dados acessam um ao outro, a sequência é conhecida como uma cadeia. Quando o SQL Server está atravessando os links na cadeia, ele avalia as permissões de maneira diferente do que avaliaria se estivesse acessando cada item separadamente. Quando um objeto é acessado por meio de uma cadeia, o SQL Server primeiro compara o proprietário do objeto com o proprietário do objeto de chamada (o link anterior na cadeia). Se os dois objetos tiverem o mesmo proprietário, as permissões do objeto referenciado não serão verificadas. Sempre que um objeto acessa outro objeto que tem um proprietário diferente, a cadeia de propriedade é interrompida e o SQL Server deve verificar o contexto de segurança do chamador.  
  
## <a name="procedural-code-and-ownership-chaining"></a>Código procedural e encadeamento de propriedade  
 Suponha que um usuário receba permissões de execução em um procedimento armazenado que seleciona dados de uma tabela. Se o procedimento armazenado e a tabela tiverem o mesmo proprietário, o usuário não precisará receber nenhuma permissão na tabela e pode até ter permissões negadas. Entretanto, se o procedimento armazenado e a tabela tiverem proprietários diferentes, o SQL Server deverá verificar as permissões do usuário na tabela antes de permitir acesso a dados.  
  
> [!NOTE]
> O encadeamento de propriedade não se aplica no caso de instruções SQL dinâmicas. Para chamar um procedimento que executa uma instrução SQL, o chamador deve ter recebido permissões nas tabelas subjacentes, deixando seu aplicativo vulnerável ao ataque de injeção de SQL. O SQL Server fornece novos mecanismos, como a representação e módulos de assinatura com certificados, que não requerem a concessão de permissões nas tabelas subjacentes. Eles também podem ser usados com procedimentos armazenados CLR.  
  
## <a name="external-resources"></a>Recursos externos  
 Para obter mais informações, consulte os recursos a seguir.  
  
|Recurso|Descrição|  
|--------------|-----------------|  
|[Permissões](/sql/relational-databases/security/permissions-database-engine)|Contém tópicos que descrevem a hierarquia de permissões, os modos de exibição do catálogo e as permissões de funções fixas do servidor e do banco de dados.|
  
## <a name="see-also"></a>Veja também

- [Protegendo aplicativos ADO.NET](../securing-ado-net-applications.md)
- [Cenários de Segurança de Aplicativo no SQL Server](application-security-scenarios-in-sql-server.md)
- [Autenticação no SQL Server](authentication-in-sql-server.md)
- [Servidor e funções de banco de dados no SQL Server](server-and-database-roles-in-sql-server.md)
- [Propriedade e separação do esquema do usuário no SQL Server](ownership-and-user-schema-separation-in-sql-server.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
