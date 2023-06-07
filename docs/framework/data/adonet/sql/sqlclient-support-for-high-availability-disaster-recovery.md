---
title: Suporte do SqlClient para alta disponibilidade, recuperação de desastre
description: Saiba mais sobre o suporte a aplicativos SqlClient para alta disponibilidade e recuperação de desastre em SQL Server, usando Grupos de Disponibilidade AlwaysOn.
ms.date: 03/30/2017
ms.assetid: 61e0b396-09d7-4e13-9711-7dcbcbd103a0
ms.openlocfilehash: eba243d37db8262970d161cfa786d3aee4462950
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286204"
---
# <a name="sqlclient-support-for-high-availability-disaster-recovery"></a>Suporte do SqlClient para alta disponibilidade, recuperação de desastre
Este tópico discute o suporte a SqlClient (adicionado no .NET Framework 4,5) para alta disponibilidade, recuperação de desastre-Grupos de Disponibilidade AlwaysOn.  O recurso dos Grupos de Disponibilidade AlwaysOn foi adicionado ao SQL Server 2012. Para obter mais informações sobre os Grupos de Disponibilidade AlwaysOn, confira os Manuais Online do SQL Server.  
  
 Agora você pode especificar o ouvinte de grupo de disponibilidade de um grupo de disponibilidade (de alta disponibilidade e recuperação de desastres) (AG) ou uma instância de cluster de failover do SQL Server 2012 na propriedade de conexão. Se um aplicativo SqlClient estiver conectado a um banco de dados AlwaysOn que efetua failover, a conexão original será interrompida e o aplicativo deverá abrir uma nova conexão para continuar o trabalho após o failover.  
  
 Se você não estiver conectado a um ouvinte de grupo de disponibilidade ou instância de cluster de failover do SQL Server 2012, e se houver vários endereços IP associados a um nome de host, o SqlClient percorrerá sequencialmente todos os endereços IP associados à entrada DNS. Isso pode demorar muito se o primeiro endereço IP retornado pelo servidor DNS não estiver associado a nenhuma NIC (placa de interface de rede). Ao se conectar a um ouvinte de grupo de disponibilidade ou instância de cluster de failover do SQL Server 2012, o SqlClient tenta estabelecer conexões com todos os endereços IP em paralelo e, se uma tentativa de conexão for bem-sucedida, o driver descartará tentativas pendentes de conexão.  
  
> [!NOTE]
> O aumento do tempo limite de conexão e a implementação de lógica de repetição de conexão aumentarão a probabilidade de um aplicativo se conectar a um grupo de disponibilidade. Além disso, como uma conexão pode falhar devido a um failover, você deve implementar lógica de repetição de conexão, repetindo uma conexão com falha até se reconectar.  
  
 As propriedades de conexão a seguir foram adicionadas ao SqlClient no .NET Framework 4,5:  
  
- `ApplicationIntent`  
  
- `MultiSubnetFailover`  
  
 você pode modificar programaticamente essas palavras-chave de cadeia de conexão com:  
  
1. <xref:System.Data.SqlClient.SqlConnectionStringBuilder.ApplicationIntent%2A>  
  
2. <xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A>  

> [!NOTE]
> `MultiSubnetFailover`A configuração para `true` não é necessária com .NET Framework 4.6.1 ou versões posteriores.
  
## <a name="connecting-with-multisubnetfailover"></a>Conectando-se ao MultiSubnetFailover  
 Sempre especifique `MultiSubnetFailover=True` quando for se conectar a um ouvinte de grupo de disponibilidade do SQL Server 2012 ou uma instância de cluster de failover do SQL Server 2012. `MultiSubnetFailover` permite o failover mais rápido para todos os grupos de disponibilidade e as instâncias de cluster de failover do SQL Server 2012, e reduz significativamente o tempo de failover para topologias AlwaysOn de uma ou várias sub-redes. Durante um failover de várias sub-redes, o cliente tentará conexões em paralelo. Durante um failover de sub-rede, ele tentará novamente a conexão TCP de forma agressiva.  
  
 A propriedade de conexão `MultiSubnetFailover` indica que o aplicativo está sendo implantado em um grupo de disponibilidade ou Instância do Cluster de Failover do SQL Server 2012 e que o SqlClient tentará se conectar ao banco de dados na instância principal do SQL Server tentando se conectar a todos os endereços IP. Quando `MultiSubnetFailover=True` é especificado para uma conexão, o cliente repete as tentativas de conexão TCP mais rápido do que os intervalos de retransmissão TCP padrão do sistema operacional. Isto permite uma reconexão mais rápida depois de failover de um Grupo de disponibilidade AlwaysOn ou uma Instância de Cluster de Failover AlwaysOn e é aplicável a Grupos de disponibilidade único e de várias sub-redes e Instâncias de Cluster de Failover.  
  
 Para obter mais informações sobre as palavras-chave de cadeia de conexão no SqlClient, confira <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>.  
  
 Especificar `MultiSubnetFailover=True` ao conectar-se a algo diferente de um ouvinte de grupo de disponibilidade ou de uma Instância de Cluster de Failover do SQL Server 2012 pode resultar em um impacto negativo no desempenho, e não há suporte para isso.  
  
 Use as diretrizes a seguir para conectar-se a um servidor em um grupo de disponibilidade ou Instância de Cluster de Failover do SQL Server 2012:  
  
- Use a propriedade de conexão `MultiSubnetFailover` para conectar-se a uma ou várias sub-redes; isso melhorará o desempenho de ambos.  
  
- Para conectar-se a um grupo de disponibilidade, especifique o ouvinte do grupo de disponibilidade como o servidor em sua cadeia de conexão.  
  
- Conectar-se a uma instância do SQL Server configurada com mais de 64 endereços IP causará uma falha de conexão.  
  
- O comportamento de um aplicativo que usa a propriedade de conexão `MultiSubnetFailover` não é afetado com base no tipo de autenticação: Autenticação do SQL Server, Autenticação Kerberos ou Autenticação do Windows.  
  
- Aumente o valor de `Connect Timeout` para acomodar o tempo de failover e reduza as tentativas de repetição da conexão do aplicativo.  
  
- Não há suporte para transações distribuídas.  
  
 Se o roteamento somente leitura não estiver em ação, conectar-se a um local de réplica secundária apresentará falha nas seguintes situações:  
  
1. Se o local de réplica secundário não for configurado para aceitar conexões.  
  
2. Se um aplicativo usar `ApplicationIntent=ReadWrite` (abordado abaixo) e o local da réplica secundária estiver configurado para acesso somente leitura.  
  
 O <xref:System.Data.SqlClient.SqlDependency> não é compatível com réplicas secundárias somente leitura.  
  
 Uma conexão falhará se uma réplica primária estiver configurada para rejeitar cargas de trabalho somente leitura e a cadeia de conexão contiver `ApplicationIntent=ReadOnly`.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Atualizando para usar clusters de várias sub-redes a partir do espelhamento de banco de dados  
 Um erro de conexão (<xref:System.ArgumentException>) ocorrerá se as palavras-chave de conexão `MultiSubnetFailover` e `Failover Partner` estiverem presentes na cadeia de conexão ou se `MultiSubnetFailover=True` e um protocolo diferente de TCP forem usados. Um erro (<xref:System.Data.SqlClient.SqlException>) também ocorrerá se `MultiSubnetFailover` for usada e o SQL Server retornar uma resposta de parceiro de failover indicando que ela faz parte de um par de espelhamento de banco de dados.  
  
 Se você atualizar um aplicativo SqlClient que atualmente usa o espelhamento de banco de dados em um cenário de várias sub-redes, deverá remover a propriedade de conexão `Failover Partner` e substituí-la por `MultiSubnetFailover` definido como `True` e substituir o nome do servidor da cadeia de conexão por um ouvinte de grupo de disponibilidade. Se a cadeia de conexão usar `Failover Partner` e `MultiSubnetFailover=True`, o driver gerará um erro. No entanto, se uma cadeia de conexão usar `Failover Partner` e `MultiSubnetFailover=False` (ou `ApplicationIntent=ReadWrite`), o aplicativo usará o espelhamento de banco de dados.  
  
 O driver retornará um erro se o espelhamento de banco de dados for usado no banco de dados primário no AG e se `MultiSubnetFailover=True` for usado na cadeia de conexão que se conecta a um banco de dados primário, e não a um ouvinte de grupo de disponibilidade.  
  
## <a name="specifying-application-intent"></a>Especificando a intenção do aplicativo  
 Quando `ApplicationIntent=ReadOnly`, o cliente solicita uma carga de trabalho leitura ao se conectar a um banco de dados habilitado para AlwaysOn. O servidor irá impor a intenção no momento da conexão e durante uma instrução USE de banco de dados, mas somente para um banco de dados habilitado para AlwaysOn.  
  
 A palavra-chave `ApplicationIntent` não funciona com bancos de dados somente leitura de versões anteriores.  
  
 Um banco de dados pode permitir ou não cargas de trabalho de leitura no banco de dados AlwaysOn de destino. (Isso é feito com a cláusula `ALLOW_CONNECTIONS` das instruções `PRIMARY_ROLE` e `SECONDARY_ROLE`Transact-SQL.)  
  
 A palavra-chave `ApplicationIntent` é usada para habilitar o roteamento somente leitura.  
  
## <a name="read-only-routing"></a>Roteamento somente leitura  
 O roteamento somente leitura é um recurso que pode assegurar a disponibilidade de uma réplica somente leitura de um banco de dados. Para habilitar roteamento somente leitura:  
  
1. Você deve conectar-se a um ouvinte de grupo de disponibilidade AlwaysOn.  
  
2. A palavra-chave de cadeia de conexão `ApplicationIntent` deve ser definida como `ReadOnly`.  
  
3. O grupo de disponibilidade deve ser configurado pelo administrador de banco de dados para permitir o roteamento somente leitura.  
  
 É possível que nem todas as várias conexões que usam roteamento somente leitura se conectem à mesma réplica somente leitura. Alterações na sincronização de banco de dados ou alterações na configuração de roteamento de servidor podem resultar em conexões de cliente com réplicas somente leitura diferentes. Para garantir que todas as solicitações somente leitura conectem-se à mesma réplica somente leitura, não transmita um ouvinte de grupo de disponibilidade à palavra-chave de cadeia de conexão `Data Source`. Em vez disso, especifique o nome da instância somente leitura.  
  
 O roteamento somente leitura pode demorar mais tempo do que a conexão ao primário porque o roteamento somente leitura conecta-se primeiramente ao primário e depois procura o melhor secundário legível disponível. Por isso, você deve aumentar seu tempo limite de logon.  
  
## <a name="see-also"></a>Veja também

- [Recursos de SQL Server e ADO.NET](sql-server-features-and-adonet.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
