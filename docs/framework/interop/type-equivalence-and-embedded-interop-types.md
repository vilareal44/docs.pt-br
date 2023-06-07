---
title: Equivalência de tipo e tipos de interoperabilidade inseridos
description: Entenda a equivalência de tipo entre os tipos .NET e os membros com um assembly gerenciado e os tipos COM que são inseridos nesse assembly. Para o .NET 4 e superior.
ms.date: 03/30/2017
helpviewer_keywords:
- type equivalence
- embedded interop types
- primary interop assemblies,not necessary in CLR version 4
- NoPIA
ms.assetid: 78892eba-2a58-4165-b4b1-0250ee2f41dc
ms.openlocfilehash: 2d572133c42f01af7d50f6f771588f5173853f9a
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86282002"
---
# <a name="type-equivalence-and-embedded-interop-types"></a>Equivalência de tipo e tipos de interoperabilidade inseridos

A partir do .NET Framework 4, o Common Language Runtime dá suporte à inserção de informações de tipo de COM diretamente em assemblies gerenciados, em vez de exigir que estes obtenham informações de tipo de COM a partir de assemblies de interoperabilidade. Como as informações de tipo inserido incluem somente os tipos e os membros que são realmente usados por um assembly gerenciado, dois assemblies gerenciados podem ter exibições muito diferentes do mesmo tipo COM. Cada assembly gerenciado tem um objeto <xref:System.Type> diferente para representar sua exibição do tipo COM. O Common Language Runtime dá suporte à equivalência de tipo entre essas exibições diferentes para interfaces, estruturas, enumerações e representantes.

Equivalência de tipo significa que um objeto COM que é passado de um assembly gerenciado para outro pode ser convertido no tipo gerenciado apropriado no assembly receptor.

> [!NOTE]
> Equivalência de tipo e tipos de interoperabilidade inseridos simplificam a implantação de aplicativos e suplementos que usam componentes COM, porque não é necessário implantar assemblies de interoperabilidade com os aplicativos. Os desenvolvedores de componentes COM compartilhados ainda precisam criar PIAs (assemblies de interoperabilidade primários) se desejam que seus componentes sejam usados por versões anteriores do .NET Framework.

## <a name="type-equivalence"></a>Equivalência de tipo

 Há suporte à equivalência de tipos COM em interfaces, estruturas, enumerações e representantes. Os tipos COM se qualificarão como equivalentes se todas as seguintes afirmações forem verdadeiras:

- Os tipos são ambas interfaces, ambas estruturas, ambas enumerações ou ambas representantes.

- Os tipos têm a mesma identidade, conforme descrito na próxima seção.

- Ambos os tipos são elegíveis para equivalência de tipo, conforme descrito na seção [marcando tipos com para equivalência de tipo](#marking-com-types-for-type-equivalence) .

### <a name="type-identity"></a>Tipo de identidade

Dois tipos são determinados como tendo a mesma identidade quando seus escopos e identidades correspondem. Em outras palavras, se cada um tem o atributo <xref:System.Runtime.InteropServices.TypeIdentifierAttribute> e os dois atributos têm propriedades <xref:System.Runtime.InteropServices.TypeIdentifierAttribute.Scope%2A> e <xref:System.Runtime.InteropServices.TypeIdentifierAttribute.Identifier%2A> correspondentes. A comparação de <xref:System.Runtime.InteropServices.TypeIdentifierAttribute.Scope%2A> não diferencia maiúsculas de minúsculas.

Se um tipo não tiver o atributo <xref:System.Runtime.InteropServices.TypeIdentifierAttribute> ou se ele tiver um atributo <xref:System.Runtime.InteropServices.TypeIdentifierAttribute> que não especifica o escopo e o identificador, o tipo ainda poderá ser considerado para equivalência da seguinte maneira:

- Para interfaces, o valor do <xref:System.Runtime.InteropServices.GuidAttribute> é usado em vez da propriedade <xref:System.Runtime.InteropServices.TypeIdentifierAttribute.Scope%2A?displayProperty=nameWithType> e a propriedade <xref:System.Type.FullName%2A?displayProperty=nameWithType> (ou seja, o nome do tipo, incluindo o namespace) é usado em vez da propriedade <xref:System.Runtime.InteropServices.TypeIdentifierAttribute.Identifier%2A?displayProperty=nameWithType>.

- Para estruturas, enumerações e representantes, o <xref:System.Runtime.InteropServices.GuidAttribute> do assembly de contenção é usado em vez da propriedade <xref:System.Runtime.InteropServices.TypeIdentifierAttribute.Scope%2A> e a propriedade <xref:System.Type.FullName%2A?displayProperty=nameWithType> é usada em vez da propriedade <xref:System.Runtime.InteropServices.TypeIdentifierAttribute.Identifier%2A>.

### <a name="marking-com-types-for-type-equivalence"></a>Marcando tipos COM para a equivalência de tipo

 É possível marcar um tipo como qualificado para a equivalência de tipo de duas maneiras:

- Aplicar o atributo <xref:System.Runtime.InteropServices.TypeIdentifierAttribute> ao tipo.

- Tornar o tipo um tipo de importação COM. Uma interface é um tipo de importação COM se ela tem o atributo <xref:System.Runtime.InteropServices.ComImportAttribute>. Uma interface, uma estrutura, uma enumeração ou um representante é um tipo de importação COM se o assembly no qual ele é definido tem o atributo <xref:System.Runtime.InteropServices.ImportedFromTypeLibAttribute>.

## <a name="see-also"></a>Confira também

- <xref:System.Type.IsEquivalentTo%2A>
- [Usando tipos COM no código gerenciado](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))
- [Importando uma biblioteca de tipos como um assembly](importing-a-type-library-as-an-assembly.md)
