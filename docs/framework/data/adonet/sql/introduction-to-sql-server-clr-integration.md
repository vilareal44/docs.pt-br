---
title: Introdução à Integração do SQL Server CLR
description: A integração CLR com o SQL Server dá suporte a procedimentos armazenados, gatilhos, funções definidas pelo usuário, tipos definidos pelo usuário e agregações definidas pelo usuário em código gerenciado.
ms.date: 03/30/2017
ms.assetid: 551d2290-ed80-49be-b377-44b32444da1c
ms.openlocfilehash: fa2ef68792d09cf94b3e0680a14bd79f9b593999
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286424"
---
# <a name="introduction-to-sql-server-clr-integration"></a>Introdução à Integração do SQL Server CLR
O CLR (Common Language Runtime) é o centro do Microsoft .NET Framework; ele fornece o ambiente de execução para todo o código do .NET Framework. O código executado no CLR é chamado de código gerenciado. O CLR fornece diversas funções e serviços necessários para a execução de programas, incluindo a compilação JIT (Just-In-Time), alocação e gerenciamento de memória, imposição de segurança de tipos, tratamento de exceções, gerenciamento de threads e segurança.  
  
 Com o CLR hospedado no Microsoft SQL Server (a chamada integração CLR), você pode criar procedimentos armazenados, gatilhos, funções definidas pelo usuário, tipos definidos pelo usuário e agregações definidas pelo usuário no código gerenciado. Como o código gerenciado é compilado em código nativo antes da execução, você pode obter aumentos significativos de desempenho em alguns cenários.  
  
 O código gerenciado usa CAS (segurança de acesso do código), links de código e domínios de aplicativo para impedir que os assemblies executem determinadas operações. O SQL Server usa CAS para ajudar a proteger o código gerenciado e evitar o comprometimento do sistema operacional ou servidor de banco de dados.  
  
 Esta seção é destinada para fornecer somente as informações suficientes para começar a programar com a integração de CLR do SQL Server, e não para ser abrangente. Para obter informações detalhadas, consulte a versão dos Manuais Online do SQL Server da versão do SQL Server que você está usando.  
  
 **Documentação do SQL Server**  
  
- [Visão geral da integração CLR (Common Language Runtime)](/sql/relational-databases/clr-integration/common-language-runtime-integration-overview)  
  
## <a name="enabling-clr-integration"></a>Habilitando integração CLR  
 O recurso de integração do CLR (Common Language Runtime) está desativado por padrão no Microsoft SQL Server e deve ser habilitado para usar os objetos que são implementados usando a integração de CLR. Para habilitar a integração de CLR usando Transact-SQL, use a opção `clr enabled` do procedimento armazenado `sp_configure` conforme mostrado:  
  
```sql  
sp_configure 'clr enabled', 1  
GO  
RECONFIGURE  
GO  
```  
  
 Você pode desabilitar a integração de CLR configurando a opção `clr enabled` como 0. Quando você desabilita a integração de CLR, o SQL Server para de executar todas rotinas de CLR e descarrega todos os domínios de aplicativo.  
  
 Para obter informações detalhadas, consulte a versão dos Manuais Online do SQL Server da versão do SQL Server que você está usando.  
  
 **Documentação do SQL Server**  
  
- [Habilitando integração CLR](/sql/relational-databases/clr-integration/clr-integration-enabling)  
  
## <a name="deploying-a-clr-assembly"></a>Implantando um assembly de CLR  
 Quando testados e verificados no servidor de teste, os métodos do CLR podem ser distribuídos para servidores de produção usando um script de implantação. O script de implantação pode ser gerado manualmente ou usando o SQL Server Management Studio. Para obter informações mais detalhadas, consulte a versão da documentação do SQL Server para a versão do SQL Server que você está usando.  
  
 **Documentação do SQL Server**  
  
1. [Implantando objetos de banco de dados CLR](/sql/relational-databases/clr-integration/deploying-clr-database-objects)  
  
## <a name="clr-integration-security"></a>Segurança da integração CLR  
 O modelo de segurança de integração do Microsoft SQL Server com o CLR (Common Language Runtime) do Microsoft .NET Framework gerencia e protege o acesso entre diferentes tipos de objetos CLR e não CLR que são executados no SQL Server. Esses objetos podem ser chamados por uma instrução Transact-SQL ou outro objeto CLR que é executado no servidor.  
  
 Para obter informações detalhadas, consulte a versão dos Manuais Online do SQL Server da versão do SQL Server que você está usando.  
  
 **Documentação do SQL Server**  
  
- [Segurança da integração CLR](/sql/relational-databases/clr-integration/security/clr-integration-security)  
  
## <a name="debugging-a-clr-assembly"></a>Depurando um assembly de CLR  
 O Microsoft SQL Server fornece suporte para depurar o Transact-SQL e objetos CLR (Common Language Runtime) no banco de dados. Depurar funciona entre linguagens: os usuários podem entrar perfeitamente em objetos CLR de Transact-SQL e vice-versa.  
  
 Para obter informações detalhadas, consulte a versão dos Manuais Online do SQL Server da versão do SQL Server que você está usando.  
  
 **Documentação do SQL Server**  
  
- [Depurando objetos de banco de dados CLR](/sql/relational-databases/clr-integration/debugging-clr-database-objects)  
  
## <a name="see-also"></a>Veja também

- [Segurança de acesso do código e o ADO.NET](../code-access-security.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
