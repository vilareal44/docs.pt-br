---
title: Como definir propriedades abstratas – guia de programação C#
description: Saiba como definir propriedades abstratas em C#. Declarar uma propriedade abstract significa que uma classe oferece suporte a uma propriedade. As classes derivadas implementam acessadores.
ms.date: 07/20/2015
helpviewer_keywords:
- properties [C#], abstract
- abstract properties [C#]
ms.assetid: 672a90eb-47b9-4ae0-9914-af53852fddcb
ms.openlocfilehash: 4db71721495857c634e8090b986704d8a592b4e2
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864391"
---
# <a name="how-to-define-abstract-properties-c-programming-guide"></a>Como definir propriedades abstratas (guia de programação C#)
O exemplo a seguir mostra como definir propriedades [abstract](../../language-reference/keywords/abstract.md). Uma declaração de propriedade abstract não fornece uma implementação dos acessadores da propriedade – ela declara que a classe dá suporte às propriedades, mas deixa a implementação do acessador para classes derivadas. O exemplo a seguir demonstra como implementar as propriedades abstract herdadas de uma classe base.  
  
 Esse exemplo consiste em três arquivos, cada um deles é compilado individualmente e seu assembly resultante é referenciado pela próxima compilação:  
  
- abstractshape.cs: a classe `Shape` que contém uma propriedade abstract `Area`.  
  
- shapes.cs: as subclasses da classe `Shape`.  
  
- shapetest.cs: um programa de teste para exibir as áreas de alguns objetos derivados de `Shape`.  
  
 Para compilar o exemplo, use o comando a seguir:  
  
 `csc abstractshape.cs shapes.cs shapetest.cs`  
  
 Isso criará o arquivo executável shapetest.exe.  
  
## <a name="example"></a>Exemplo  
 Esse arquivo declara a classe `Shape` que contém a propriedade `Area` do tipo `double`.  
  
 [!code-csharp[csProgGuideInheritance#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#1)]  
  
- Os modificadores da propriedade são colocados na própria declaração de propriedade. Por exemplo:  
  
    ```csharp  
    public abstract double Area  
    ```  
  
- Ao declarar uma propriedade abstract (como `Area` neste exemplo), você simplesmente indica quais acessadores de propriedade estão disponíveis, mas não os implementa. Neste exemplo, apenas um acessador [get](../../language-reference/keywords/get.md) está disponível, assim, a propriedade é somente leitura.  
  
## <a name="example"></a>Exemplo  
 O código a seguir mostra três subclasses de `Shape` e como elas substituem a propriedade `Area` para fornecer sua própria implementação.  
  
 [!code-csharp[csProgGuideInheritance#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#2)]  
  
## <a name="example"></a>Exemplo  
 O código a seguir mostra um programa de teste que cria uma quantidade de objetos derivados de `Shape` e imprime suas áreas.  
  
 [!code-csharp[csProgGuideInheritance#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#3)]  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Classes e structs](./index.md)
- [Classes e membros de classes abstract e sealed](./abstract-and-sealed-classes-and-class-members.md)
- [Propriedades](./properties.md)
