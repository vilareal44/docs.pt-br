---
title: Passar argumentos por valor e por referência
ms.date: 07/20/2015
helpviewer_keywords:
- ByRef keyword [Visual Basic], passing arguments by reference
- Visual Basic code, procedures
- passing arguments [Visual Basic], by value or by reference
- ByVal keyword [Visual Basic], passing arguments by value
- arguments [Visual Basic], passing by value or by reference
- argument passing [Visual Basic], by value or by reference
ms.assetid: fd8a9de6-7178-44d5-a9bf-458d4ad907c2
ms.openlocfilehash: 3dd4be6ea6de9dfe8eb165e5d4ba9a990fc40585
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363948"
---
# <a name="passing-arguments-by-value-and-by-reference-visual-basic"></a>Passando argumentos por valor e por referência (Visual Basic)
No Visual Basic, você pode passar um argumento para um procedimento *por valor* ou *por referência*. Isso é conhecido como o *mecanismo de passagem*e determina se o procedimento pode modificar o elemento de programação subjacente ao argumento no código de chamada. A declaração de procedimento determina o mecanismo de passagem para cada parâmetro especificando a palavra-chave [ByVal](../../../language-reference/modifiers/byval.md) ou [ByRef](../../../language-reference/modifiers/byref.md) .  
  
## <a name="distinctions"></a>Distinções  
 Ao passar um argumento para um procedimento, lembre-se de várias diferentes distinções que interagem entre si:  
  
- Se o elemento de programação subjacente é modificável ou não modificável  
  
- Se o próprio argumento é modificável ou não modificável  
  
- Se o argumento está sendo passado por valor ou por referência  
  
- Se o tipo de dados do argumento é um tipo de valor ou um tipo de referência  
  
 Para obter mais informações, consulte [diferenças entre argumentos modificáveis e não modificáveis](./differences-between-modifiable-and-nonmodifiable-arguments.md) e [diferenças entre passar um argumento por valor e por referência](./differences-between-passing-an-argument-by-value-and-by-reference.md).  
  
## <a name="choice-of-passing-mechanism"></a>Opção de mecanismo de passagem  
 Você deve escolher o mecanismo de passagem cuidadosamente para cada argumento.  
  
- **Proteção**. Ao escolher entre os dois mecanismos de passagem, o critério mais importante é a exposição da chamada de variáveis a serem alteradas. A vantagem de passar um argumento `ByRef` é que o procedimento pode retornar um valor para o código de chamada por meio desse argumento. A vantagem de passar um argumento `ByVal` é que ele protege uma variável de ser alterada pelo procedimento.  
  
- **Desempenho**. Embora o mecanismo de passagem possa afetar o desempenho do seu código, a diferença geralmente é insignificante. Uma exceção a isso é um tipo de valor passado `ByVal` . Nesse caso, Visual Basic copia todo o conteúdo de dados do argumento. Portanto, para um tipo de valor grande, como uma estrutura, pode ser mais eficiente passá-lo `ByRef` .  
  
     Para tipos de referência, somente o ponteiro para os dados é copiado (quatro bytes em plataformas de 32 bits, oito bytes em plataformas de 64 bits). Portanto, você pode passar argumentos do tipo `String` ou `Object` por valor sem prejudicar o desempenho.  
  
## <a name="determination-of-the-passing-mechanism"></a>Determinação do mecanismo de passagem  
 A declaração de procedimento especifica o mecanismo de passagem para cada parâmetro. O código de chamada não pode substituir um `ByVal` mecanismo.  
  
 Se um parâmetro for declarado com `ByRef` , o código de chamada poderá forçar o mecanismo a `ByVal` colocar o nome do argumento entre parênteses na chamada. Para obter mais informações, consulte [como: forçar um argumento a ser passado por valor](./how-to-force-an-argument-to-be-passed-by-value.md).  
  
 O padrão no Visual Basic é passar argumentos por valor.  
  
## <a name="when-to-pass-an-argument-by-value"></a>Quando passar um argumento por valor  
  
- Se o elemento de código de chamada subjacente ao argumento for um elemento não modificável, declare o parâmetro correspondente [ByVal](../../../language-reference/modifiers/byval.md). Nenhum código pode alterar o valor de um elemento não modificável.  
  
- Se o elemento subjacente for modificável, mas você não quiser que o procedimento seja capaz de alterar seu valor, declare o parâmetro `ByVal` . Somente o código de chamada pode alterar o valor de um elemento modificável passado por valor.  
  
## <a name="when-to-pass-an-argument-by-reference"></a>Quando passar um argumento por referência  
  
- Se o procedimento tiver uma necessidade original de alterar o elemento subjacente no código de chamada, declare o parâmetro correspondente [ByRef](../../../language-reference/modifiers/byref.md).  
  
- Se a execução correta do código depender do procedimento que altera o elemento subjacente no código de chamada, declare o parâmetro `ByRef` . Se você passá-lo por valor ou se o código de chamada substituir o `ByRef` mecanismo de passagem ao colocar o argumento entre parênteses, a chamada de procedimento poderá produzir resultados inesperados.  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir ilustra quando passar argumentos por valor e quando passá-los por referência. O procedimento `Calculate` tem um `ByVal` e um `ByRef` parâmetro. Dada uma taxa de juros, `rate` e uma soma do Money, `debt` , a tarefa do procedimento é calcular um novo valor para `debt` isso é o resultado da aplicação da taxa de juros ao valor original de `debt` . Como `debt` é um `ByRef` parâmetro, o novo total é refletido no valor do argumento no código de chamada que corresponde a `debt` . O parâmetro `rate` é um `ByVal` parâmetro porque `Calculate` não deve alterar seu valor.  
  
### <a name="code"></a>Código  
 [!code-vb[VbVbcnProcedures#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class2.vb#74)]  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Como passar argumentos para um procedimento](./how-to-pass-arguments-to-a-procedure.md)
- [Como alterar o valor de um argumento de procedimento](./how-to-change-the-value-of-a-procedure-argument.md)
- [Como proteger um argumento de procedimento contra alterações de valor](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [Como forçar um argumento a ser passado por Valor](./how-to-force-an-argument-to-be-passed-by-value.md)
- [Passando argumentos por posição e nome](./passing-arguments-by-position-and-by-name.md)
- [Tipos de valor e referência](../data-types/value-types-and-reference-types.md)
