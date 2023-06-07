---
title: Modificadores de acesso – Guia de Programação em C#
description: Todos os tipos e membros de tipo em C# têm um nível de acessibilidade que controla se eles podem ser usados de outro código. Examine esta lista de modificadores de acesso.
ms.date: 03/08/2020
helpviewer_keywords:
- C# Language, access modifiers
- access modifiers [C#], about
ms.assetid: 6e81ee82-224f-4a12-9baf-a0dca2656c5b
ms.openlocfilehash: 557f5d9f302b08d32896b462e86ce1d96710ff36
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474521"
---
# <a name="access-modifiers-c-programming-guide"></a>Modificadores de acesso (Guia de Programação em C#)

Todos os tipos e membros de tipo têm um nível de acessibilidade. O nível de acessibilidade controla se eles podem ser usados de outro código em seu assembly ou de outros assemblies. Use os seguintes modificadores de acesso para especificar a acessibilidade de um tipo ou membro ao declará-lo:

- [público](../../language-reference/keywords/public.md): o tipo ou membro pode ser acessado por qualquer outro código no mesmo assembly ou em outro assembly que faça referência a ele.
- [particular](../../language-reference/keywords/private.md): o tipo ou membro pode ser acessado somente pelo código no mesmo `class` ou `struct` .
- [protegido](../../language-reference/keywords/protected.md): o tipo ou membro pode ser acessado somente pelo código no mesmo `class` ou em um `class` que seja derivado dele `class` .
- [interno](../../language-reference/keywords/internal.md): o tipo ou membro pode ser acessado por qualquer código no mesmo assembly, mas não de outro assembly.
- [interno protegido](../../language-reference/keywords/protected-internal.md): o tipo ou membro pode ser acessado por qualquer código no assembly no qual ele é declarado ou de dentro de um derivado `class` em outro assembly.
- [protegido particular](../../language-reference/keywords/private-protected.md): o tipo ou membro pode ser acessado somente dentro de seu assembly declarativo, pelo código no mesmo `class` ou em um tipo que é derivado dele `class` .

Os exemplos a seguir demonstram como especificar modificadores de acesso em um tipo e membro:

[!code-csharp[PublicAccess](~/samples/snippets/csharp/objectoriented/accessmodifiers.cs#PublicAccess)]

Nem todos os modificadores de acesso são válidos para todos os tipos ou membros em todos os contextos. Em alguns casos, a acessibilidade de um membro de tipo é restrita pela acessibilidade de seu tipo recipiente.

## <a name="class-and-struct-accessibility"></a>Acessibilidade de classe e struct  

Classes e structs declaradas diretamente dentro de um namespace (em outras palavras, que não são aninhadas em outras classes ou structs) podem ser `public` ou `internal` . `Internal`será o padrão se nenhum modificador de acesso for especificado.  

Membros de struct, incluindo classes aninhadas e structs, podem ser declarados `public` , `internal` ou `private` . Membros de classe, incluindo classes aninhadas e structs, podem ser `public` , `protected internal` , `protected` ,, `internal` `private protected` ou `private` . Membros de classe e struct, incluindo classes aninhadas e estruturas, têm `private` acesso por padrão. Tipos aninhados privados não podem ser acessados de fora do tipo recipiente.

Classes derivadas não podem ter mais acessibilidade do que seus tipos base. Você não pode declarar uma classe pública `B` que deriva de uma classe interna `A` . Se permitido, ele teria o efeito de tornar o `A` público, porque todos `protected` ou `internal` membros do `A` são acessíveis da classe derivada.

Você pode habilitar outros assemblies específicos para acessar seus tipos internos usando o `InternalsVisibleToAttribute` . Para obter mais informações, consulte [Assemblies amigáveis](../../../standard/assembly/friend.md).

## <a name="class-and-struct-member-accessibility"></a>Acessibilidade de classe e membro de struct  

Os membros de classe (incluindo as classes e os structs aninhados) podem ser declarados com qualquer um dos seis tipos de acesso. Membros de struct não podem ser declarados como `protected` , `protected internal` ou `private protected` porque structs não dão suporte à herança.

Normalmente, a acessibilidade de um membro não é maior do que a acessibilidade do tipo que o contém. No entanto, um `public` membro de uma classe interna pode ser acessível de fora do assembly se o membro implementar métodos de interface ou substituir métodos virtuais que são definidos em uma classe base pública.

O tipo de qualquer campo de membro, propriedade ou evento deve ser pelo menos acessível como o próprio membro. Da mesma forma, o tipo de retorno e os tipos de parâmetro de qualquer método, indexador ou delegado devem ser pelo menos tão acessíveis quanto o próprio membro. Por exemplo, você não pode ter um `public` método `M` que retorne uma classe `C` a menos que `C` também seja `public` . Da mesma forma, você não poderá ter uma `protected` Propriedade do tipo `A` se `A` for declarada como `private` .

Os operadores definidos pelo usuário sempre devem ser declarados como `public` e `static` . Para obter mais informações, consulte [Sobrecarga de operador](../../language-reference/operators/operator-overloading.md).

Os finalizadores não podem ter modificadores de acessibilidade.

Para definir o nível de acesso para `class` um `struct` membro ou, adicione a palavra-chave apropriada à declaração de membro, conforme mostrado no exemplo a seguir.

[!code-csharp[MethodAccess](~/samples/snippets/csharp/objectoriented/accessmodifiers.cs#MethodAccess)]

## <a name="other-types"></a>Outros tipos

As interfaces declaradas diretamente dentro de um namespace podem ser `public` ou `internal` e, assim como classes e structs, têm como padrão o `internal` acesso. Membros de interface são `public` por padrão porque a finalidade de uma interface é permitir que outros tipos acessem uma classe ou struct. Declarações de membro de interface podem incluir qualquer modificador de acesso. Isso é mais útil para métodos estáticos para fornecer implementações comuns necessárias para todos os implementadores de uma classe.

Os membros de enumeração são sempre `public` e nenhum modificador de acesso pode ser aplicado.

Delegados se comportam como classes e structs. Por padrão, eles têm `internal` acesso quando declarados diretamente dentro de um namespace e são `private` acessados quando aninhados.

## <a name="c-language-specification"></a>especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  

## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [Classes e structs](./index.md)
- [Interfaces](../interfaces/index.md)
- [pessoal](../../language-reference/keywords/private.md)
- [público](../../language-reference/keywords/public.md)
- [interno](../../language-reference/keywords/internal.md)
- [protegidos](../../language-reference/keywords/protected.md)
- [internos protegidos](../../language-reference/keywords/protected-internal.md)
- [privado protegido](../../language-reference/keywords/private-protected.md)
- [class](../../language-reference/keywords/class.md)
- [struct](../../language-reference/builtin-types/struct.md)
- [interface](../../language-reference/keywords/interface.md)
