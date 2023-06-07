---
title: Estruturas e classes
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic], vs. structures
- structures [Visual Basic]
- classes [Visual Basic]
- structures [Visual Basic], compared to classes
- structures [Visual Basic], structure variables
- structure variables [Visual Basic]
ms.assetid: a221e74a-ffcf-4bdc-a0f6-a088a9bf26cc
ms.openlocfilehash: d252d9216a9b825ad0663a5779d7ce7f81fa9011
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393566"
---
# <a name="structures-and-classes-visual-basic"></a>Estruturas e classes (Visual Basic)
Visual Basic unifica a sintaxe para estruturas e classes, com o resultado de que ambas as entidades dão suporte à maioria dos mesmos recursos. No entanto, também há diferenças importantes entre estruturas e classes.  
  
 Classes têm a vantagem de ser tipos de referência — passar uma referência é mais eficiente do que passar uma variável de estrutura com todos os seus dados. Por outro lado, as estruturas não exigem a alocação de memória no heap global.  
  
 Como não é possível herdar de uma estrutura, as estruturas devem ser usadas somente para objetos que não precisam ser estendidos. Use estruturas quando o objeto que você deseja criar tiver um tamanho de instância pequeno e leve em conta as características de desempenho de classes versus estruturas.  
  
## <a name="similarities"></a>Semelhanças  
 Estruturas e classes são semelhantes nos seguintes aspectos:  
  
- Ambos são tipos de *contêineres* , o que significa que eles contêm outros tipos como membros.  
  
- Ambos têm Membros, que podem incluir construtores, métodos, propriedades, campos, constantes, enumerações, eventos e manipuladores de eventos. No entanto, não confunda esses membros com os *elementos* declarados de uma estrutura.  
  
- Os membros de ambos podem ter níveis de acesso individualizados. Por exemplo, um membro pode ser declarado `Public` e outro `Private` .  
  
- Ambos podem implementar interfaces.  
  
- Ambos podem ter construtores compartilhados, com ou sem parâmetros.  
  
- Ambos podem expor uma *propriedade padrão*, desde que a propriedade aceite pelo menos um parâmetro.  
  
- Ambos podem declarar e gerar eventos, e ambos podem declarar delegados.  
  
## <a name="differences"></a>Diferenças  
 Estruturas e classes diferem nas seguintes particularidades:  
  
- Estruturas são *tipos de valor*; classes são *tipos de referência*. Uma variável de um tipo de estrutura contém os dados da estrutura, em vez de conter uma referência aos dados como um tipo de classe.  
  
- Estruturas usam alocação de pilha; as classes usam alocação de heap.  
  
- Todos os elementos de estrutura são `Public` por padrão; variáveis de classe e constantes são `Private` por padrão, enquanto outros membros de classe são `Public` por padrão. Esse comportamento para membros de classe fornece compatibilidade com o sistema Visual Basic 6,0 de padrões.  
  
- Uma estrutura deve ter pelo menos uma variável não compartilhada ou um elemento de evento não compartilhado e não-personalizado; uma classe pode estar completamente vazia.  
  
- Elementos de estrutura não podem ser declarados como `Protected` ; membros de classe podem.  
  
- Um procedimento de estrutura pode manipular eventos somente se for um [Shared](../../../language-reference/modifiers/shared.md) `Sub` procedimento compartilhado e apenas por meio da [instrução AddHandler](../../../language-reference/statements/addhandler-statement.md); qualquer procedimento de classe pode manipular eventos, usando a palavra-chave [Handles](../../../language-reference/statements/handles-clause.md) ou a `AddHandler` instrução. Para obter mais informações, consulte [Eventos](../events/index.md).  
  
- Declarações de variável de estrutura não podem especificar inicializadores ou tamanhos iniciais para matrizes; declarações de variável de classe podem.  
  
- As estruturas herdam implicitamente da <xref:System.ValueType?displayProperty=nameWithType> classe e não podem herdar de nenhum outro tipo; as classes podem herdar de qualquer classe ou classes diferentes de <xref:System.ValueType?displayProperty=nameWithType> .  
  
- Estruturas não são herdáveis; as classes são.  
  
- As estruturas nunca são encerradas, portanto, o Common Language Runtime (CLR) nunca chama o <xref:System.Object.Finalize%2A> método em qualquer estrutura; as classes são encerradas pelo GC (coletor de lixo), que chama <xref:System.Object.Finalize%2A> uma classe quando detecta que não há nenhuma referência ativa restante.  
  
- Uma estrutura não requer um construtor; uma classe faz.  
  
- Estruturas só podem ter construtores não compartilhados se eles usam parâmetros; as classes podem tê-las com ou sem parâmetros.  
  
 Cada estrutura tem um construtor público implícito sem parâmetros. Esse construtor inicializa todos os elementos de dados da estrutura para seus valores padrão. Você não pode redefinir esse comportamento.  
  
## <a name="instances-and-variables"></a>Instâncias e variáveis  
 Como estruturas são tipos de valor, cada variável de estrutura é permanentemente associada a uma instância de estrutura individual. Mas classes são tipos de referência, e uma variável de objeto pode se referir a várias instâncias de classe em momentos diferentes. Essa distinção afeta o uso de estruturas e classes das seguintes maneiras:  
  
- **Initialization.** Uma variável de estrutura inclui implicitamente uma inicialização dos elementos usando o construtor sem parâmetros da estrutura. Portanto, `Dim s As struct1` é equivalente a `Dim s As struct1 = New struct1()` .  
  
- **Atribuindo variáveis.** Quando você atribui uma variável de estrutura a outra ou passa uma instância de estrutura para um argumento de procedimento, os valores atuais de todos os elementos variáveis são copiados para a nova estrutura. Quando você atribui uma variável de objeto a outra ou passa uma variável de objeto para um procedimento, somente o ponteiro de referência é copiado.  
  
- **Atribuindo nada.** Você pode atribuir o valor [Nothing](../../../language-reference/nothing.md) a uma variável de estrutura, mas a instância continua a ser associada à variável. Você ainda pode chamar seus métodos e acessar seus elementos de dados, embora elementos variáveis sejam reinicializados pela atribuição.  
  
     Por outro lado, se você definir uma variável de objeto como `Nothing` , você a dissocia de qualquer instância de classe e não poderá acessar nenhum membro por meio da variável até que você atribua outra instância a ela.  
  
- **Várias instâncias.** Uma variável de objeto pode ter diferentes instâncias de classe atribuídas a ela em momentos diferentes, e várias variáveis de objeto podem se referir à mesma instância de classe ao mesmo tempo. As alterações feitas nos valores dos membros da classe afetam esses membros quando acessados por meio de outra variável que aponta para a mesma instância.  
  
     Os elementos de estrutura, no entanto, são isolados em sua própria instância. As alterações em seus valores não são refletidas em nenhuma outra variável de estrutura, mesmo em outras instâncias da mesma `Structure` declaração.  
  
- **Igualmente.** O teste de igualdade de duas estruturas deve ser executado com um teste de elemento por elemento. Duas variáveis de objeto podem ser comparadas usando o <xref:System.Object.Equals%2A> método. <xref:System.Object.Equals%2A>indica se as duas variáveis apontam para a mesma instância.  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados](index.md)
- [Tipos de dados compostos](composite-data-types.md)
- [Tipos de valor e referência](value-types-and-reference-types.md)
- [Estruturas](structures.md)
- [Solução de problemas de tipos de dados](troubleshooting-data-types.md)
- [Estruturas e outros elementos de programação](structures-and-other-programming-elements.md)
- [Objetos e classes](../objects-and-classes/index.md)
