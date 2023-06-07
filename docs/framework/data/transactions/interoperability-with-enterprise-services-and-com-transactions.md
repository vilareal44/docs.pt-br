---
title: Interoperabilidade com serviços corporativos e transações COM+
description: Entenda a interoperabilidade com os serviços corporativos e as transações COM+ no .NET usando o namespace System. Transactions.
ms.date: 03/30/2017
ms.assetid: d0fd0d26-fe86-443b-b208-4d57d39fa4aa
ms.openlocfilehash: ebd6166fbd99ef102cf10ba1bcef9e3eb8aaa5da
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141895"
---
# <a name="interoperability-with-enterprise-services-and-com-transactions"></a>Interoperabilidade com serviços corporativos e transações COM+
O <xref:System.Transactions> namespace oferece suporte a interoperabilidade entre objetos de transação criados usando este namespace e transações criadas por meio do COM+.  
  
 Você pode usar o <xref:System.Transactions.EnterpriseServicesInteropOption> enumeração quando você cria um novo <xref:System.Transactions.TransactionScope> instância para especificar o nível de interoperabilidade com COM+.  
  
 Por padrão, quando o código do aplicativo verifica a <xref:System.Transactions.Transaction.Current%2A> propriedade estática, o <xref:System.Transactions> tenta procurar uma transação que, de outra forma, é atual, ou um <xref:System.Transactions.TransactionScope> objeto que dita que <xref:System.Transactions.Transaction.Current%2A> é **nulo**. Se não for possível encontrar qualquer um deles, <xref:System.Transactions> consulta o contexto de COM+ para uma transação. Observe que, embora <xref:System.Transactions> pode localizar uma transação de contexto de COM+, ele ainda favorece transações que são nativas do <xref:System.Transactions>.  
  
## <a name="interoperability-levels"></a>Níveis de interoperabilidade  
 O <xref:System.Transactions.EnterpriseServicesInteropOption> enumeração define os seguintes níveis de interoperabilidade —<xref:System.Transactions.EnterpriseServicesInteropOption.None>, <xref:System.Transactions.EnterpriseServicesInteropOption.Full> e <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>.  
  
 O <xref:System.Transactions.TransactionScope> classe fornece construtores que aceitam <xref:System.Transactions.EnterpriseServicesInteropOption> como um parâmetro.  
  
 <xref:System.Transactions.EnterpriseServicesInteropOption.None>, como o nome implica, significa que não há interoperabilidade entre <xref:System.EnterpriseServices> contextos e escopos de transação. Depois de criar um <xref:System.Transactions.TransactionScope> objeto com <xref:System.Transactions.EnterpriseServicesInteropOption.None>, todas as alterações <xref:System.Transactions.Transaction.Current%2A> não são refletidas no contexto de COM+. Da mesma forma, as alterações a transação no contexto de COM+ não são refletidas no <xref:System.Transactions.Transaction.Current%2A>. Este é o modo mais rápido de operação para <xref:System.Transactions> porque não há nenhuma sincronização adicional necessária. <xref:System.Transactions.EnterpriseServicesInteropOption.None>é o valor padrão usado pelo <xref:System.Transactions.TransactionScope> com todos os construtores que não aceitam <xref:System.Transactions.EnterpriseServicesInteropOption> como parâmetro.  
  
 Se você deseja combinar <xref:System.EnterpriseServices> transações com sua transação de ambiente, você precisa usar um <xref:System.Transactions.EnterpriseServicesInteropOption.Full> ou <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>. Esses valores dependem de um recurso chamado serviços sem componentes e, portanto, você deve estar em execução no Windows XP Service Pack 2 ou Windows Server 2003 quando usá-los.  
  
 <xref:System.Transactions.EnterpriseServicesInteropOption.Full>Especifica que as transações de ambiente para <xref:System.Transactions> e <xref:System.EnterpriseServices> são sempre as mesmas. Isso resulta na criação de um novo <xref:System.EnterpriseServices> contexto transacional e aplicando a transação atual para o <xref:System.Transactions.TransactionScope> atual para esse contexto. Como tal, a transação no <xref:System.Transactions.Transaction.Current%2A> é completamente em sincronia com a transação em <xref:System.EnterpriseServices.ContextUtil.Transaction%2A>. Esse valor apresenta uma penalidade de desempenho porque novos COM+ contextos precisa ser criado.  
  
 <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>Especifica os seguintes requisitos:  
  
- Quando <xref:System.Transactions.Transaction.Current%2A> estiver marcada, <xref:System.Transactions> deve oferecer suporte a transações no contexto de COM+ se detectar que ele seja executado em um contexto diferente o contexto padrão. Observe que o contexto padrão não pode conter uma transação. Portanto, no contexto padrão, mesmo com <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>, a transação armazenada no armazenamento local de thread usado pelo <xref:System.Transactions> é retornado para <xref:System.Transactions.Transaction.Current%2A>.  
  
- Se um novo <xref:System.Transactions.TransactionScope> objeto é criado e a criação ocorre em um contexto diferente do contexto padrão, a transação atual para o <xref:System.Transactions.TransactionScope> objeto deve ser refletido no COM+. Nesse caso, <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic> se comporta como <xref:System.Transactions.EnterpriseServicesInteropOption.Full> que ele cria um novo contexto de COM+.  
  
 Além disso quando <xref:System.Transactions.Transaction.Current%2A> é definido em ambos <xref:System.Transactions.EnterpriseServicesInteropOption.Full> e <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>, esses dois modos significam que <xref:System.Transactions.Transaction.Current%2A> não pode ser definida diretamente.  Qualquer tentativa de definir <xref:System.Transactions.Transaction.Current%2A> diferente de criar diretamente uma <xref:System.Transactions.TransactionScope> resulta em uma <xref:System.InvalidOperationException>. O <xref:System.Transactions.EnterpriseServicesInteropOption> valor de enumeração é herdada por novos escopos de transação que não especificar explicitamente o valor a ser usado. Por exemplo, se você criar um novo <xref:System.Transactions.TransactionScope> objeto com <xref:System.Transactions.EnterpriseServicesInteropOption.Full>e, em seguida, crie um segundo <xref:System.Transactions.TransactionScope> do objeto, mas não especificam um <xref:System.Transactions.EnterpriseServicesInteropOption> valor, a segunda <xref:System.Transactions.TransactionScope> objeto também tem um <xref:System.Transactions.EnterpriseServicesInteropOption.Full>.  
  
 Em resumo, as seguintes regras se aplicam ao criar um novo escopo de transação:  
  
1. <xref:System.Transactions.Transaction.Current%2A>é verificado para ver se há uma transação. Essa verificação resulta em:  
  
    - Uma verificação para ver se há um escopo.  
  
    - Se houver um escopo, o valor da <xref:System.Transactions.EnterpriseServicesInteropOption> enumeração passada quando o escopo foi criado inicialmente é verificada.  
  
    - Se o <xref:System.Transactions.EnterpriseServicesInteropOption> enumeração é definida como <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>, a transação COM+ (<xref:System.EnterpriseServices> transação) tem precedência sobre o <xref:System.Transactions> transações no armazenamento local de thread gerenciado.  
  
         Se o valor for definido como <xref:System.Transactions.EnterpriseServicesInteropOption.None>, o <xref:System.Transactions> transações no armazenamento local de thread gerenciado tem precedência.  
  
         Se o valor for <xref:System.Transactions.EnterpriseServicesInteropOption.Full>, há somente uma transação e é uma transação COM+.  
  
2. O valor da <xref:System.Transactions.TransactionScopeOption> enumeração passado pelo <xref:System.Transactions.TransactionScope> construtor é verificada. Determina se uma nova transação deve ser criada.  
  
3. Se uma nova transação deve ser criado, os seguintes valores de <xref:System.Transactions.EnterpriseServicesInteropOption> resultar em:  
  
    - <xref:System.Transactions.EnterpriseServicesInteropOption.Full>: uma transação associada a um contexto COM+ é criada.  
  
    - <xref:System.Transactions.EnterpriseServicesInteropOption.None>: uma <xref:System.Transactions> transação é criada.  
  
    - <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>: se houver um contexto COM+, uma transação será criada e anexada ao contexto.  
  
 A tabela a seguir ilustra o contexto do Enterprise Services (ES) e um escopo transacional que exige uma transação usando o <xref:System.Transactions.EnterpriseServicesInteropOption> enumeração.  
  
|Contexto de ES|Nenhum|Automático|Completo|  
|----------------|----------|---------------|----------|  
|Contexto padrão|Contexto padrão|Contexto padrão|Create new <br />contexto transacional|  
|Contexto de não-padrão|Manter o contexto do cliente|Criar um novo contexto transacional|Criar um novo contexto transacional|  
  
 A tabela a seguir ilustra o que a transação ambiente é, dado um determinado <xref:System.EnterpriseServices> contexto e um escopo transacional que exige uma transação usando o <xref:System.Transactions.EnterpriseServicesInteropOption> enumeração.  
  
|Contexto de ES|Nenhum|Automático|Completo|  
|----------------|----------|---------------|----------|  
|Contexto padrão|ST|ST|ES|  
|Contexto de não-padrão|ST|ES|ES|  
  
 Na tabela anterior:  
  
- ST significa que a transação de ambiente do escopo seja gerenciada por <xref:System.Transactions>separada de qualquer <xref:System.EnterpriseServices> transação do contexto que pode estar presente.  
  
- ES significa que a transação de ambiente do escopo é igual a <xref:System.EnterpriseServices> transação do contexto.
