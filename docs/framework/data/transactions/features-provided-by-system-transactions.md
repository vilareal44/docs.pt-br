---
title: Recursos fornecidos por System.Transactions
description: Examine os recursos fornecidos pelo namespace System. Transactions no .NET para escrever seu próprio aplicativo de transação e o Gerenciador de recursos.
ms.date: 03/30/2017
ms.assetid: e458cef9-63b5-4401-b448-1536dcd9d9e5
ms.openlocfilehash: 0278e9248305572c6156c6500f1fe51a8b3f3338
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141856"
---
# <a name="features-provided-by-systemtransactions"></a>Recursos fornecidos por System.Transactions
Esta seção descreve como você pode usar os recursos fornecidos pelo <xref:System.Transactions> namespace para escrever seu próprio Gerenciador de recursos e dos aplicativos transacional. Especificamente, esta seção aborda como criar e participar de uma transação (local ou distribuída) com um ou vários participantes.  
  
## <a name="overview-of-systemtransactions"></a>Visão geral do System. Transactions  
 A infra-estrutura fornecida pelas classes de <xref:System.Transactions> namespace torna transacional de programação simples e eficiente, oferecendo suporte a transações iniciadas no SQL Server, ADO.NET, enfileiramento de mensagens (MSMQ) e Microsoft Distributed Transaction coordenador (MSDTC). O <xref:System.Transactions> namespace fornece dois um modelo de programação explícito com base no <xref:System.Transactions.Transaction> classe, bem como um modelo de programação implícito usando a <xref:System.Transactions.TransactionScope> classe, em que as transações são automaticamente gerenciadas pela infra-estrutura. Para obter mais informações sobre como criar um aplicativo transacional usando esses dois modelos, consulte [escrevendo um aplicativo transacional](writing-a-transactional-application.md).  
  
 O <xref:System.Transactions> namespace também fornece tipos para implementar um Gerenciador de recursos. Um Gerenciador de recursos gerencia dados duráveis ou voláteis usados em uma transação e trabalhar em cooperação com o Gerenciador de transações para fornecer o aplicativo com uma garantia de atomicidade e isolamento. O Gerenciador de transações fornecida pelo <xref:System.Transactions> infra-estrutura oferece suporte a transações que envolvem vários recursos voláteis ou um único recurso durável. Para obter mais informações sobre como implementar um Gerenciador de recursos, consulte [implementando um Gerenciador de recursos](implementing-a-resource-manager.md).  
  
 O Gerenciador de transações também transparentemente escala transações locais para transações distribuídas através da coordenação com um Gerenciador de transações baseadas em disco como o DTC, quando um Gerenciador de recursos adicionais de durável inscreve-se com uma transação. Há duas maneiras principais que o <xref:System.Transactions> infra-estrutura proporciona um desempenho aprimorado.  
  
- Escalonamento dinâmico, o que garante que o <xref:System.Transactions> infra-estrutura apenas emprega o MSDTC quando uma transação engloba vários recursos distribuídos. Para obter mais informações sobre o escalonamento dinâmico. consulte o tópico [escalonamento de gerenciamento de transações](transaction-management-escalation.md) .  
  
- Podem ser promovidas inscrições, que permite que um recurso, como um banco de dados, apropriar-se da transação se for a única entidade participa da transação. Posteriormente, se necessário, o <xref:System.Transactions> infra-estrutura ainda pode escalonar o gerenciamento da transação para o MSDTC. Isso reduz ainda mais a possibilidade de usar o MSDTC. As inlistagens promocionais são abordadas em detalhes no tópico[otimização usando a confirmação de fase única e a notificação de fase única de promoçãotable](optimization-spc-and-promotable-spn.md).  
  
 O <xref:System.Transactions> namespace define três níveis de confiança - AllowPartiallyTrustedCallers (APTCA), DistributedTransactionPermission(DTP) e confiança total - que restringem o acesso aos tipos de recursos ele expõe. Para obter mais informações sobre os vários níveis de confiança, consulte [níveis de confiança de segurança em Acessando recursos](security-trust-levels-in-accessing-resources.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
### <a name="writing-a-transactional-application"></a>Escrevendo um aplicativo transacional  
 O <xref:System.Transactions> namespace fornece dois modelos para criar aplicativos transacionais. [Implementar uma transação implícita usando o escopo da transação](implementing-an-implicit-transaction-using-transaction-scope.md) descreve como o <xref:System.Transactions> namespace dá suporte à criação de transações implícitas usando a <xref:System.Transactions.TransactionScope> classe.  
  
 [Implementar uma transação explícita usando CommittableTransaction](implementing-an-explicit-transaction-using-committabletransaction.md) descreve como o <xref:System.Transactions> namespace dá suporte à criação de transações explícitas usando a <xref:System.Transactions.CommittableTransaction> classe.  
  
 Para obter tópicos adicionais sobre como escrever um aplicativo transacional, consulte [escrevendo um aplicativo transacional](writing-a-transactional-application.md).  
  
### <a name="implementing-a-resource-manager"></a>Implementar um Gerenciador de recursos  
 Para implementar um Gerenciador de recursos que pode participar de uma transação, consulte [implementando um Gerenciador de recursos](implementing-a-resource-manager.md). Esta seção aborda a inscrição de um recurso, confirmar uma transação, a recuperação após falha e práticas recomendadas de otimização.
