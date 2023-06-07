---
title: + Operador
ms.date: 07/20/2015
f1_keywords:
- vb.+
helpviewer_keywords:
- arithmetic operators [Visual Basic], addition
- + operator
- concatenation operators [Visual Basic], syntax
- strings [Visual Basic], concatenating
- sum operator [Visual Basic]
ms.assetid: 5694778f-0a2c-4539-8009-f66f318fb46d
ms.openlocfilehash: 6ae3feae6ecb63b82426f2aa69359625bbffcec8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84372110"
---
# <a name="-operator-visual-basic"></a>Operador + (Visual Basic)
Adiciona dois números ou retorna o valor positivo de uma expressão numérica. Também pode ser usado para concatenar duas expressões de cadeia de caracteres.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb
expression1 + expression2
```
  
ou

```vb  
+expression1  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`expression1`|Obrigatórios. Qualquer expressão numérica ou de cadeia de caracteres.|  
|`expression2`|Necessário, a menos que o `+` operador esteja calculando um valor negativo. Qualquer expressão numérica ou de cadeia de caracteres.|  
  
## <a name="result"></a>Result  
 Se `expression1` e `expression2` forem ambos numéricos, o resultado será sua soma aritmética.  
  
 Se `expression2` estiver ausente, o `+` operador será o operador de identidade *unário* para o valor inalterado de uma expressão. Nesse sentido, a operação consiste em reter o sinal de `expression1` , portanto, o resultado será negativo se `expression1` for negativo.  
  
 Se `expression1` e `expression2` forem as duas cadeias de caracteres, o resultado será a concatenação de seus valores.  
  
 Se `expression1` e `expression2` forem de tipos mistos, a ação executada dependerá de seus tipos, do seu conteúdo e da configuração da [instrução Option Strict](../statements/option-strict-statement.md). Para obter mais informações, consulte as tabelas em "Comentários".  
  
## <a name="supported-types"></a>Tipos com suporte  
 Todos os tipos numéricos, incluindo os tipos de ponto flutuante e não assinado e e `Decimal` `String` .  
  
## <a name="remarks"></a>Comentários  
 Em geral, `+` o executa a adição aritmética quando possível e concatena somente quando ambas as expressões são cadeias de caracteres.  
  
 Se nenhuma expressão for uma `Object` , Visual Basic executará as seguintes ações.  
  
|Tipos de dados de expressões|Ação por compilador|  
|---|---|  
|As duas expressões são tipos de dados numéricos (,,,,,,, `SByte` `Byte` `Short` `UShort` `Integer` `UInteger` `Long` `ULong` , `Decimal` , `Single` ou `Double` )|Adicionar. O tipo de dados de resultado é um tipo numérico apropriado para os tipos de dados de `expression1` e `expression2` . Consulte as tabelas "aritmética de inteiros" em [tipos de dados de resultados do operador](data-types-of-operator-results.md).|  
|Ambas as expressões são do tipo`String`|Concatenar.|  
|Uma expressão é um tipo de dados numérico e a outra é uma cadeia de caracteres|Se `Option Strict` for `On` , gere um erro do compilador.<br /><br /> Se `Option Strict` for `Off` , converta implicitamente o `String` para `Double` e adicione.<br /><br /> Se o `String` não puder ser convertido em `Double` , lance uma <xref:System.InvalidCastException> exceção.|  
|Uma expressão é um tipo de dados numérico e a outra é [Nothing](../nothing.md)|Adicionar, com `Nothing` valor igual a zero.|  
|Uma expressão é uma cadeia de caracteres e a outra é`Nothing`|Concatenar, com `Nothing` valor "".|  
  
 Se uma expressão for uma `Object` expressão, Visual Basic executará as seguintes ações.  
  
|Tipos de dados de expressões|Ação por compilador|  
|---|---|  
|`Object`a expressão contém um valor numérico e o outro é um tipo de dados numérico|Se `Option Strict` for `On` , gere um erro do compilador.<br /><br /> Se `Option Strict` for `Off` , adicione.|  
|`Object`a expressão contém um valor numérico e o outro é do tipo`String`|Se `Option Strict` for `On` , gere um erro do compilador.<br /><br /> Se `Option Strict` for `Off` , converta implicitamente o `String` para `Double` e adicione.<br /><br /> Se o `String` não puder ser convertido em `Double` , lance uma <xref:System.InvalidCastException> exceção.|  
|`Object`a expressão contém uma cadeia de caracteres e a outra é um tipo de dados numérico|Se `Option Strict` for `On` , gere um erro do compilador.<br /><br /> Se `Option Strict` for `Off` , converta implicitamente a cadeia de caracteres `Object` para `Double` e adicione.<br /><br /> Se a cadeia de caracteres `Object` não puder ser convertida em `Double` , lance uma <xref:System.InvalidCastException> exceção.|  
|`Object`a expressão contém uma cadeia de caracteres e a outra é do tipo`String`|Se `Option Strict` for `On` , gere um erro do compilador.<br /><br /> Se `Option Strict` for `Off` , então converta implicitamente `Object` `String` e concatene.|  
  
 Se ambas as expressões forem `Object` expressões, Visual Basic executará as seguintes ações ( `Option Strict Off` somente).  
  
|Tipos de dados de expressões|Ação por compilador|  
|---|---|  
|Ambas as `Object` expressões contêm valores numéricos|Adicionar.|  
|Ambas as `Object` expressões são do tipo`String`|Concatenar.|  
|Uma `Object` expressão contém um valor numérico e a outra contém uma cadeia de caracteres|Converta implicitamente a cadeia de caracteres `Object` para `Double` e adicione.<br /><br /> Se a cadeia de caracteres `Object` não puder ser convertida em um valor numérico, lance uma <xref:System.InvalidCastException> exceção.|  
  
 Se qualquer `Object` expressão for avaliada como [Nothing](../nothing.md) ou <xref:System.DBNull> , o `+` operador a tratará como um `String` com um valor de "".  
  
> [!NOTE]
> Ao usar o `+` operador, talvez você não consiga determinar se a adição ou a concatenação de cadeia de caracteres ocorrerá. Use o `&` operador para concatenação para eliminar ambigüidade e fornecer código de autodocumentação.  
  
## <a name="overloading"></a>Sobrecarga  
 O `+` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `+` operador para adicionar números. Se os operandos forem numéricos, Visual Basic calculará o resultado aritmético. O resultado aritmético representa a soma dos dois operandos.  
  
 [!code-vb[VbVbalrOperators#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#6)]  
  
 Você também pode usar o `+` operador para concatenar cadeias de caracteres. Se os operandos forem as duas cadeias de caracteres, Visual Basic as concatena. O resultado da concatenação representa uma única cadeia de caracteres que consiste no conteúdo dos dois operandos um após o outro.  
  
 Se os operandos forem de tipos mistos, o resultado dependerá da configuração da [instrução Option Strict](../statements/option-strict-statement.md). O exemplo a seguir ilustra o resultado quando `Option Strict` é `On` .  
  
 [!code-vb[VbVbalrOperators#53](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class3.vb#53)]  
  
 [!code-vb[VbVbalrOperators#50](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#50)]  
[!code-vb[VbVbalrOperators#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#51)]  
  
 O exemplo a seguir ilustra o resultado quando `Option Strict` é `Off` .  
  
 [!code-vb[VbVbalrOperators#54](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#54)]  
  
 [!code-vb[VbVbalrOperators#50](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#50)]  
[!code-vb[VbVbalrOperators#52](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#52)]  
  
 Para eliminar a ambiguidade, você deve usar o `&` operador em vez de `+` para concatenação.  
  
## <a name="see-also"></a>Confira também

- [Operador de&](concatenation-operator.md)
- [Operadores de concatenação](concatenation-operators.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores aritméticos no Visual Basic](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [Instrução Option Strict](../statements/option-strict-statement.md)
