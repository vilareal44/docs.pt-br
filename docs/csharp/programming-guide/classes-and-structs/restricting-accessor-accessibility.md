---
title: Como restringir a acessibilidade ao acessador – Guia de Programação em C#
description: Os acessadores get e set de uma propriedade em C# têm a mesma visibilidade ou nível de acesso, por padrão, como Propriedade à qual pertencem. Você pode restringir o acesso.
ms.date: 07/20/2015
helpviewer_keywords:
- read-only properties [C#]
- read-only indexers [C#]
- accessors [C#]
- properties [C#], read-only
- asymmetric accessor accessibility [C#]
- indexers [C#], read-only
ms.assetid: 6e655798-e112-4301-a680-6310a6e012e1
ms.openlocfilehash: 18fd1d58dc6125b5180118b2e0d3edc885a4b971
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86863962"
---
# <a name="restricting-accessor-accessibility-c-programming-guide"></a>Restringindo a acessibilidade ao acessador (Guia de Programação em C#)
As partes [get](../../language-reference/keywords/get.md) e [set](../../language-reference/keywords/set.md) de uma propriedade ou de um indexador são chamadas *acessadores*. Por padrão, esses acessadores têm a mesma visibilidade ou nível de acesso da propriedade ou do indexador aos quais pertencem. Para obter mais informações, consulte [níveis de acessibilidade](../../language-reference/keywords/accessibility-levels.md). No entanto, às vezes é útil restringir o acesso a um desses acessadores. Normalmente, isso envolve restringir a acessibilidade do acessador `set` e manter o acessador `get` publicamente acessível. Por exemplo:  
  
 [!code-csharp[csProgGuideIndexers#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#6)]  
  
 Neste exemplo, uma propriedade chamada `Name` define um acessador `get` e `set`. O acessador `get` recebe o nível de acessibilidade da propriedade em si, `public` nesse caso, embora o `set` acessador esteja restrito explicitamente ao aplicar o modificador de acesso [protegido](../../language-reference/keywords/protected.md) ao acessador em si.  
  
## <a name="restrictions-on-access-modifiers-on-accessors"></a>Restrições em modificadores de acesso nos acessadores  
 O uso dos modificadores de acesso em propriedades ou indexadores está sujeito a estas condições:  
  
- Não é possível usar os modificadores de acessador em uma interface ou em uma implementação de membro de [interface](../../language-reference/keywords/interface.md) explícita.  
  
- É possível usar os modificadores de acessador somente se a propriedade ou o indexador tiver os acessadores `set` e `get`. Nesse caso, o modificador é permitido em apenas um dos dois acessadores.  
  
- Se a propriedade ou o indexador tiver um modificador [substituir](../../language-reference/keywords/override.md), o modificador de acessador deverá corresponder o acessador do acessador substituído, se houver.  
  
- O nível de acessibilidade do acessador deve ser mais restritivo do que o nível de acessibilidade na propriedade ou no indexador em si.  
  
## <a name="access-modifiers-on-overriding-accessors"></a>Modificadores de acesso em acessadores de substituição  
 Quando você substitui uma propriedade ou indexador, os acessadores substituídos devem estar acessíveis ao código de substituição. Além disso, a acessibilidade da propriedade/indexador, e seus acessadores, devem corresponder à propriedade/indexador substituído e seus acessadores. Por exemplo:  
  
 [!code-csharp[csProgGuideIndexers#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#7)]  
  
## <a name="implementing-interfaces"></a>Implementando interfaces  
 Quando você usa um acessador para implementar uma interface, o acessador pode não ter um modificador de acesso. No entanto, se você implementar a interface usando um acessador, como `get`, o outro acessador poderá ter um modificador de acesso, como no exemplo a seguir:  
  
 [!code-csharp[csProgGuideIndexers#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#8)]  
  
## <a name="accessor-accessibility-domain"></a>Domínio de acessibilidade do acessador  
 Se você usar um modificador de acesso no acessador, o [domínio de acessibilidade](../../language-reference/keywords/accessibility-domain.md) do acessador será determinado por esse modificador.  
  
 Se você não usou um modificador de acesso no acessador, o domínio de acessibilidade do acessador é determinado pelo nível de acessibilidade da propriedade ou do indexador.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir contém três classes, `BaseClass`, `DerivedClass` e `MainClass`. Há duas propriedades no `BaseClass`, `Name` e `Id` em ambas as classes. O exemplo demonstra como a propriedade `Id` no `DerivedClass` pode ser oculta pela propriedade `Id` no `BaseClass` quando você usa um modificador de acesso restritivo como [protegido](../../language-reference/keywords/protected.md) ou [privado](../../language-reference/keywords/private.md). Portanto, em vez disso, quando você atribui valores a essa propriedade, a propriedade na classe `BaseClass` é chamada. Substituindo o modificador de acesso por [público](../../language-reference/keywords/public.md) tornará a propriedade acessível.  
  
 O exemplo também demonstra que um modificador de acesso restritivo, como `private` ou `protected`, no acessador `set` da propriedade `Name` no `DerivedClass` impede o acesso ao acessador e gera um erro quando você atribui a ele.  
  
 [!code-csharp[csProgGuideIndexers#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideIndexers/CS/Indexers.cs#5)]  
  
## <a name="comments"></a>Comentários  
 Observe que, se você substituir a declaração `new private string Id` por `new public string Id`, você obterá a saída:  
  
 `Name and ID in the base class: Name-BaseClass, ID-BaseClass`  
  
 `Name and ID in the derived class: John, John123`  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Propriedades](./properties.md)
- [Indexadores](../indexers/index.md)
- [Modificadores de acesso](./access-modifiers.md)
