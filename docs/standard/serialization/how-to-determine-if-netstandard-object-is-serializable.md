---
title: Como determinar se um objeto de .NET Standard é serializável
description: Mostra como determinar se um tipo de .NET Standard pode ser serializado em tempo de execução.
ms.date: 10/20/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- serializing objects
- objects, serializing steps
ms.openlocfilehash: b6791ae0666aeb0ac02d8a38ca419d7de2b263cf
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289597"
---
# <a name="how-to-determine-if-a-net-standard-object-is-serializable"></a>Como determinar se um objeto de .NET Standard é serializável

O .NET Standard é uma especificação que define os tipos e membros que devem estar presentes em implementações específicas do .NET que estão em conformidade com essa versão do padrão. No entanto, o .NET Standard não define se um tipo é serializável. Os tipos definidos na biblioteca de .NET Standard não são marcados com o <xref:System.SerializableAttribute> atributo. Em vez disso, implementações específicas do .NET, como o .NET Framework e o .NET Core, são livres para determinar se um tipo específico é serializável.

Se você tiver desenvolvido uma biblioteca que se destina ao .NET Standard, sua biblioteca poderá ser consumida por qualquer implementação do .NET que ofereça suporte ao .NET Standard. Isso significa que você não pode saber com antecedência se um tipo específico é serializável; Você só pode determinar se ele é serializável em tempo de execução.

Você pode determinar se um objeto é serializável em tempo de execução recuperando o valor da <xref:System.Type.IsSerializable> propriedade de um <xref:System.Type> objeto que representa o tipo desse objeto. O exemplo a seguir fornece uma implementação. Ele define um `IsSerializable(Object)` método de extensão que indica se qualquer <xref:System.Object> instância pode ser serializada.

[!code-csharp[is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/csharp/program.cs#2)]
[!code-vb[is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/vb/library.vb#2)]

Em seguida, você pode passar qualquer objeto para o método para determinar se ele pode ser serializado e desserializado na implementação atual do .NET, como mostra o exemplo a seguir:

[!code-csharp[test-is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/csharp/program.cs#1)]
[!code-vb[test-is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/vb/program.vb#1)]

## <a name="see-also"></a>Confira também

- [Serialização binária](binary-serialization.md)
- <xref:System.SerializableAttribute?displayProperty=nameWithType>
- <xref:System.Type.IsSerializable?displayProperty=nameWithType>
