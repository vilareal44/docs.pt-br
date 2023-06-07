---
title: Propriedades e métodos sobrecarregados
ms.date: 07/20/2015
helpviewer_keywords:
- properties [Visual Basic], overloading
- methods [Visual Basic], overloading
- shadowing
- names [Visual Basic], multiple procedures with same
- procedures [Visual Basic], multiples with same name
- classes [Visual Basic], properties
- classes [Visual Basic], methods
- method overloading
- Overloads keyword [Visual Basic], overloaded members
ms.assetid: b686fb97-e7d7-4001-afaa-6650cba08f0d
ms.openlocfilehash: 1672f12773ece012c580253b6dafbf9d0ac8f07c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84389146"
---
# <a name="overloaded-properties-and-methods-visual-basic"></a>Propriedades e métodos sobrecarregados (Visual Basic)

O sobrecarregamento é a criação de mais de um procedimento, Construtor de instância ou propriedade em uma classe com o mesmo nome, mas tipos de argumento diferentes.

## <a name="overloading-usage"></a>Sobrecarregando o uso

O sobrecarregamento é especialmente útil quando o modelo de objeto determina que você emprega nomes idênticos para procedimentos que operam em diferentes tipos de dados. Por exemplo, uma classe que pode exibir vários tipos de dados diferentes pode ter `Display` procedimentos parecidos com estes:

[!code-vb[VbVbalrOOP#64](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#64)]

Sem sobrecarga, você precisaria criar nomes distintos para cada procedimento, embora eles façam a mesma coisa, conforme mostrado a seguir:

[!code-vb[VbVbalrOOP#65](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#65)]

O sobrecarregamento facilita o uso de propriedades ou métodos, pois ele fornece uma opção de tipos de dados que podem ser usados. Por exemplo, o `Display` método sobrecarregado discutido anteriormente pode ser chamado com qualquer uma das seguintes linhas de código:

[!code-vb[VbVbalrOOP#66](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#66)]

Em tempo de execução, Visual Basic chama o procedimento correto com base nos tipos de dados dos parâmetros que você especificar.

## <a name="overloading-rules"></a>Regras de sobrecarga

 Você cria um membro sobrecarregado para uma classe adicionando duas ou mais propriedades ou métodos com o mesmo nome. Exceto para membros derivados sobrecarregados, cada membro sobrecarregado deve ter diferentes listas de parâmetros e os seguintes itens não podem ser usados como um recurso de diferenciação ao sobrecarregar uma propriedade ou um procedimento:

- Modificadores, como `ByVal` ou `ByRef` , que se aplicam a um membro ou parâmetros do membro.

- Nomes de parâmetros

- Tipos de retorno de procedimentos

A `Overloads` palavra-chave é opcional ao sobrecarregar, mas se qualquer membro sobrecarregado usar a `Overloads` palavra-chave, todos os outros Membros sobrecarregados com o mesmo nome também deverão especificar essa palavra-chave.

Classes derivadas podem sobrecarregar membros herdados com membros que têm parâmetros e tipos de parâmetros idênticos, um processo conhecido como *sombreamento por nome e assinatura*. Se a `Overloads` palavra-chave for usada ao sombrear por nome e assinatura, a implementação da classe derivada do membro será usada em vez da implementação na classe base, e todas as outras sobrecargas para esse membro estarão disponíveis para instâncias da classe derivada.

Se a `Overloads` palavra-chave for omitida ao sobrecarregar um membro herdado com um membro que tenha parâmetros e tipos de parâmetros idênticos, o sobrecarregamento será chamado de *sombreamento por nome*. Sombrear por nome substitui a implementação herdada de um membro e faz com que todas as outras sobrecargas não estejam disponíveis para instâncias da classe derivada e seu decedents.

Os `Overloads` `Shadows` modificadores e não podem ser usados com a mesma propriedade ou método.

### <a name="example"></a>Exemplo

O exemplo a seguir cria métodos sobrecarregados que aceitam `String` uma `Decimal` representação or de um valor de dólar e retornam uma cadeia de caracteres que contém o imposto sobre vendas.

#### <a name="to-use-this-example-to-create-an-overloaded-method"></a>Para usar este exemplo para criar um método sobrecarregado

1. Abra um novo projeto e adicione uma classe chamada `TaxClass` .

2. Adicione o código a seguir à classe `TaxClass` .

    [!code-vb[VbVbalrOOP#67](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#67)]

3. Adicione o procedimento a seguir ao formulário.

    [!code-vb[VbVbalrOOP#68](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#68)]

4. Adicione um botão ao formulário e chame o `ShowTax` procedimento do `Button1_Click` evento do botão.

5. Execute o projeto e clique no botão no formulário para testar o `ShowTax` procedimento sobrecarregado.

Em tempo de execução, o compilador escolhe a função sobrecarregada apropriada que corresponde aos parâmetros que estão sendo usados. Quando você clica no botão, o método sobrecarregado é chamado primeiro com um `Price` parâmetro que é uma cadeia de caracteres e a mensagem, "Price é uma cadeia de caracteres. O imposto é $5.12 "é exibido. `TaxAmount`é chamado com um `Decimal` valor da segunda vez e da mensagem, "Price é um decimal. O imposto é $5.12 "é exibido.

## <a name="see-also"></a>Confira também

- [Objetos e classes](index.md)
- [Sombreamento no Visual Basic](../declared-elements/shadowing.md)
- [Instrução Sub](../../../language-reference/statements/sub-statement.md)
- [Noções básicas de herança](inheritance-basics.md)
- [Sombras](../../../language-reference/modifiers/shadows.md)
- [ByVal](../../../language-reference/modifiers/byval.md)
- [ByRef](../../../language-reference/modifiers/byref.md)
- [Sobrecargas](../../../language-reference/modifiers/overloads.md)
- [Sombras](../../../language-reference/modifiers/shadows.md)
