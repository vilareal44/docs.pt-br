---
title: Serialização seletiva
description: Este artigo mostra como marcar campos com o atributo NonSerialized, que impede que esse campo seja serializado.
ms.date: 08/07/2017
dev_langs:
- CSharp
helpviewer_keywords:
- serialization, selective serialization
- binary serialization, selective serialization
ms.assetid: 39c56635-95d2-4afd-aff1-b022e7649bb3
ms.openlocfilehash: 74113979f0ebe77319ae308c2a669e91d8cb4209
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84278409"
---
# <a name="selective-serialization"></a>Serialização seletiva
Uma classe geralmente contém os campos que não devem ser serializados. Por exemplo, presuma que uma classe armazene uma ID de thread em uma variável de membro. Quando a classe é desserializada, o thread que armazenou a ID de quando a classe foi serializada pode não estar mais em execução; portanto, serializar esse valor não faz sentido. Você pode impedir que variáveis de membros sejam serializadas marcando-as com o atributo [NonSerialized](xref:System.NonSerializedAttribute) da maneira a seguir.  
  
```csharp  
[Serializable]  
public class MyObject
{  
  public int n1;  
  [NonSerialized] public int n2;  
  public String str;  
}  
```

Se possível, crie um objeto que possa conter dados confidenciais de segurança não serializáveis. Se o objeto tiver que ser serializado, aplique o atributo `NonSerialized` em campos específicos que armazenam dados confidenciais. Se você não excluir esses campos da serialização, esteja ciente de que os dados que eles armazenam serão expostos em qualquer código que tenha permissão para serializar. Para obter mais informações sobre como escrever um código de serialização segura, consulte [Segurança e serialização](../../framework/misc/security-and-serialization.md).

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
## <a name="see-also"></a>Confira também

- [Serialização binária](binary-serialization.md)
- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
- [Segurança e serialização](../../framework/misc/security-and-serialization.md)
