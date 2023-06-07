---
title: Membros – Guia de Programação em C#
description: Classes e structs em C# têm Membros que representam dados e comportamento, incluindo membros declarados na classe e declarados em sua hierarquia de herança.
ms.date: 07/20/2015
helpviewer_keywords:
- types [C#], nested types
- C# language, type members
ms.assetid: 4a30a4ab-d690-4936-9124-92ce9448665a
ms.openlocfilehash: aa4981ea20d86994bfae92d5db8c9abfa7c8f906
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864586"
---
# <a name="members-c-programming-guide"></a>Membros (Guia de Programação em C#)

Classes e structs têm membros que representam seus dados e comportamento. Os membros de uma classe incluem todos os membros declarados na classe, juntamente com todos os membros (exceto construtores e finalizadores) declarados em todas as classes em sua hierarquia de herança. Os membros privados em classes base são herdados, mas não podem ser acessados de classes derivadas.  
  
 A tabela a seguir lista os tipos de membros que uma classe ou struct pode conter:  
  
|Membro|Descrição|  
|------------|-----------------|  
|[Fields](./fields.md)|Os campos são variáveis declaradas no escopo da classe. Um campo pode ser um tipo numérico interno ou uma instância de outra classe. Por exemplo, uma classe de calendário pode ter um campo que contém a data atual.|  
|[Constantes](./constants.md)|Constantes são campos cujo valor é definido em tempo de compilação e não pode ser alterado.|  
|[Propriedades](./properties.md)|As propriedades são métodos de uma classe acessados como se fossem campos dessa classe. Uma propriedade pode fornecer proteção para um campo de classe para evitar que ele seja alterado sem o conhecimento do objeto.|  
|[Métodos](./methods.md)|Os métodos definem as ações que uma classe pode executar. Métodos podem usar parâmetros que fornecem dados de entrada e retornar dados de saída por meio de parâmetros. Os métodos também podem retornar um valor diretamente, sem usar um parâmetro.|  
|[Eventos](../events/index.md)|Os eventos fornecem notificações sobre ocorrências a outros objetos, como cliques de botão ou a conclusão bem-sucedida de um método. Eventos são definidos e disparados pelos delegados.|  
|[Operadores](../../language-reference/operators/index.md)|Os operadores sobrecarregados são considerados membros de tipo. Ao sobrecarregar um operador, ele é definido como um método estático público em um tipo. Para obter mais informações, consulte [Sobrecarga de operador](../../language-reference/operators/operator-overloading.md).|  
|[Indexadores](../indexers/index.md)|Os indexadores permitem que um objeto seja indexado de maneira semelhante às matrizes.|  
|[Construtores](./constructors.md)|Os construtores são os métodos chamados quando o objeto é criado pela primeira vez. Geralmente, eles são usados para inicializar os dados de um objeto.|  
|[Finalizadores](./destructors.md)|Os finalizadores raramente são usados no C#. Eles são métodos chamados pelo mecanismo de runtime quando o objeto está prestes a ser removido da memória. Geralmente, eles são usados para garantir que recursos que devem ser liberados sejam manipulados corretamente.|  
|[Tipos aninhados](./nested-types.md)|Os tipos aninhados são tipos declarados dentro de outro tipo. Geralmente, eles são usados para descrever objetos utilizados somente pelos tipos que os contêm.|  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Classes](./classes.md)
