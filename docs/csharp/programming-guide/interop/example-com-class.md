---
title: Exemplo de classe COM – Guia de Programação em C#
description: Saiba como expor uma classe como um objeto COM em C#. Este exemplo adiciona código em arquivos. cs a um projeto e define o registro para a propriedade de interoperabilidade COM.
ms.date: 07/20/2015
helpviewer_keywords:
- examples [C#], COM classes
- COM, exposing Visual C# objects to
ms.assetid: 6504dea9-ad1c-4993-a794-830fec5270af
ms.openlocfilehash: 4ea66ba26595c5bae2e579d1cc85c4b0d58616df
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303030"
---
# <a name="example-com-class-c-programming-guide"></a>Exemplo de classe COM (Guia de Programação em C#)
A seguir está um exemplo de uma classe que você poderia expor como um objeto COM. Depois que esse código é colocado em um arquivo .cs e adicionado ao seu projeto, defina a propriedade **Registrar para interoperabilidade COM** como **True**. Para obter mais informações, consulte [Como registrar um componente para interoperabilidade COM](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/w29wacsy(v=vs.100)).
  
 A exposição de objetos do Visual C# para COM requer a declaração de uma interface de classe, de uma interface de eventos se necessário e da própria classe. Os membros de classe devem seguir estas regras para ficarem visíveis ao COM:  
  
- A classe deve ser pública.  
  
- As propriedades, os métodos e os eventos devem ser públicos.  
  
- As propriedades e os métodos devem ser declarados na interface de classe.  
  
- Os eventos devem ser declarados na interface de eventos.  
  
 Outros membros públicos na classe que não são declarados nessas interfaces não estarão visíveis para COM, mas eles ficarão visíveis para outros objetos .NET.  
  
 Para expor propriedades e métodos ao COM, você deve declará-los na interface de classe e marcá-los com um atributo `DispId` e implementá-los na classe. A ordem na qual os membros são declarados na interface é a ordem usada para a vtable do COM.  
  
 Para expor eventos de sua classe, você deve declará-los na interface de eventos e marcá-los com um atributo `DispId`. A classe não deve implementar essa interface.  
  
 A classe implementa a interface de classe. Ela pode implementar mais de uma interface, mas a primeira implementação será a interface de classe padrão. Implemente os métodos e propriedades expostos ao COM aqui. Eles devem ser marcados como públicos e devem corresponder às declarações na interface de classe. Além disso, declare aqui os eventos acionados pela classe. Eles devem ser marcados como públicos e devem corresponder às declarações na interface de eventos.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[csProgGuideInterop#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInterop/CS/ExampleCOM.cs#8)]  
  
## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Interoperabilidade](./index.md)
- [Página de Build, Designer de Projeto (C#)](/visualstudio/ide/reference/build-page-project-designer-csharp)
