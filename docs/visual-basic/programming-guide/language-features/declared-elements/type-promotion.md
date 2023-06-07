---
title: Promoção de tipos
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic], scope
- visibility [Visual Basic], declared elements
- Partial keyword [Visual Basic], unexpected results with type promotion
- scope [Visual Basic], declared elements
- scope [Visual Basic], Visual Basic
- type promotion
- declared elements [Visual Basic], visibility
ms.assetid: 035eeb15-e4c5-4288-ab3c-6bd5d22f7051
ms.openlocfilehash: 1e284b99a36cdf0f62aee2c45fd9f3bf544d1d81
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410702"
---
# <a name="type-promotion-visual-basic"></a>Promoção de tipos (Visual Basic)
Quando você declara um elemento de programação em um módulo, Visual Basic promove seu escopo para o namespace que contém o módulo. Isso é conhecido como *promoção de tipos*.  
  
 O exemplo a seguir mostra uma definição de esqueleto de um módulo e dois membros desse módulo.  
  
 [!code-vb[VbVbalrDeclaredElements#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDeclaredElements/VB/Class1.vb#1)]  
  
 No `projModule` , elementos de programação declarados no nível de módulo são promovidos para `projNamespace` . No exemplo anterior, `basicEnum` e `innerClass` são promovidos, mas `numberSub` não, porque não são declarados no nível do módulo.  
  
## <a name="effect-of-type-promotion"></a>Efeito da promoção de tipos  
 O efeito da promoção de tipos é que uma cadeia de caracteres de qualificação não precisa incluir o nome do módulo. O exemplo a seguir faz duas chamadas para o procedimento no exemplo anterior.  
  
 [!code-vb[VbVbalrDeclaredElements#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDeclaredElements/VB/Class1.vb#2)]  
  
 No exemplo anterior, a primeira chamada usa cadeias de caracteres de qualificação completas. No entanto, isso não é necessário devido à promoção de tipos. A segunda chamada também acessa os membros do módulo sem incluir `projModule` nas cadeias de caracteres de qualificação.  
  
## <a name="defeat-of-type-promotion"></a>Derrota da promoção de tipos  
 Se o namespace já tiver um membro com o mesmo nome de um membro de módulo, a promoção de tipos será derrotada por esse membro de módulo. O exemplo a seguir mostra uma definição de esqueleto de uma enumeração e um módulo dentro do mesmo namespace.  
  
 [!code-vb[VbVbalrDeclaredElements#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDeclaredElements/VB/Class1.vb#3)]  
  
 No exemplo anterior, Visual Basic não pode promover `abc` a classe `thisNameSpace` porque já existe uma enumeração com o mesmo nome no nível de namespace. Para acessar `abcSub` o, você deve usar a cadeia de caracteres de qualificação completa `thisNamespace.thisModule.abc.abcSub` . No entanto, `xyz` a classe ainda é promovida e você pode acessar `xyzSub` com a cadeia de caracteres de qualificação mais curta `thisNamespace.xyz.xyzSub` .  
  
### <a name="defeat-of-type-promotion-for-partial-types"></a>Derrota da promoção de tipo para tipos parciais  
 Se uma classe ou estrutura dentro de um módulo usar a palavra-chave [partial](../../../language-reference/modifiers/partial.md) , a promoção de tipos será automaticamente derrotada por essa classe ou estrutura, independentemente de o namespace ter ou não um membro com o mesmo nome. Outros elementos no módulo ainda são elegíveis para promoção de tipos.  
  
 **Consequências.** Derrota da promoção de tipos de uma definição parcial pode causar resultados inesperados e até mesmo erros de compilador. O exemplo a seguir mostra as definições parciais de esqueleto de uma classe, uma das quais está dentro de um módulo.  
  
 [!code-vb[VbVbalrDeclaredElements#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDeclaredElements/VB/Class1.vb#4)]  
  
 No exemplo anterior, o desenvolvedor pode esperar que o compilador mescle as duas definições parciais de `sampleClass` . No entanto, o compilador não considera a promoção para a definição parcial dentro `sampleModule` . Como resultado, ele tenta compilar duas classes separadas e distintas, ambas nomeadas, `sampleClass` mas com caminhos de qualificação diferentes.  
  
 O compilador mescla definições parciais somente quando seus caminhos totalmente qualificados são idênticos.  
  
## <a name="recommendations"></a>Recomendações  
 As recomendações a seguir representam boa prática de programação.  
  
- **Nomes exclusivos.** Quando você tem controle total sobre a nomenclatura de elementos de programação, é sempre uma boa ideia usar nomes exclusivos em todos os lugares. Nomes idênticos exigem qualificação extra e podem tornar seu código mais difícil de ler. Eles também podem levar a erros sutis e resultados inesperados.  
  
- **Qualificação completa.** Quando você está trabalhando com módulos e outros elementos no mesmo namespace, a abordagem mais segura é sempre usar a qualificação completa para todos os elementos de programação. Se a promoção de tipos for derrotada por um membro de módulo e você não qualificar totalmente esse membro, você poderá acessar inadvertidamente um elemento de programação diferente.  
  
## <a name="see-also"></a>Confira também

- [Instrução Module](../../../language-reference/statements/module-statement.md)
- [Instrução Namespace](../../../language-reference/statements/namespace-statement.md)
- [Parcial](../../../language-reference/modifiers/partial.md)
- [Escopo no Visual Basic](scope.md)
- [Como controlar o escopo de uma variável](how-to-control-the-scope-of-a-variable.md)
- [Referências a elementos declarados](references-to-declared-elements.md)
