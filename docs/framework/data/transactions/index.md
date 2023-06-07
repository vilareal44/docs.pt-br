---
title: Processamento de transações
description: Examine o processamento de transações no .NET. As transações garantem que os recursos orientados a dados não sejam atualizados permanentemente a menos que todas as operações sejam concluídas com êxito.
ms.date: 03/30/2017
ms.assetid: effdc8e6-accf-41eb-98a5-431603ba218b
ms.openlocfilehash: 30d69c55d968865cc80b8633bdbc2442f6d216de
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141908"
---
# <a name="transaction-processing"></a>Processamento de transações
Ao comprar um livro de livrarias online, você troca money (na forma de crédito) de um livro. Se o crédito é boa, uma série de operações relacionadas garante que você obtenha o livro e a livraria obtém seu dinheiro. No entanto, se uma única operação na série falhar durante a troca, toda a troca falhará. Você não obtiver o livro e a livraria não obter seu dinheiro.  
  
 A tecnologia responsável por fazer o intercâmbio equilibrado e previsível é chamada de processamento de transações. As transações garantem que recursos orientados a dados não são permanentemente atualizados, a menos que todas as operações dentro da unidade transacional foi concluída com êxito. Ao combinar um conjunto de operações relacionadas em uma unidade que completamente obtenha êxito ou falha completamente, você pode simplificar a recuperação de erro e tornar seu aplicativo mais confiável.  
  
 Sistemas de processamento de transação consistem em hardware e software que hospeda um aplicativo orientado a transações que executa as transações de rotina necessárias para conduzir os negócios. Os exemplos incluem sistemas de gerenciamento de entrada de ordem de venda, reservas de viagens, folha de pagamento, registros de funcionários, produção e a remessa.  
  
 Esta seção fornece informações gerais sobre o processamento de transações e informações específicas sobre como escrever aplicativos transacionais e gerenciadores de recursos usando o Microsoft .NET Framework.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Conceitos básicos de transação](transaction-fundamentals.md)  
 Apresenta os termos e conceitos de processamento de transações básicas.  
  
 [Recursos fornecidos por System.Transactions](features-provided-by-system-transactions.md)  
 Descreve como usar recursos do System. Transactions para escrever seu próprio aplicativo transacional.  
  
## <a name="reference"></a>Referência  
 <xref:System.Transactions>  
 Fornece classes que permitem que o seu código para participar de transações. As classes de suportam a transações com vários participantes distribuídos, várias notificações de fase e inscrições duráveis.
