---
title: Classes – Guia de Programação em C#
description: Saiba mais sobre tipos de classes e como criá-las
ms.date: 08/21/2018
helpviewer_keywords:
- classes [C#]
- C# language, classes
ms.assetid: e8848524-7273-429f-8aba-c658d5eff5ad
ms.openlocfilehash: d726ab3a882d2e6913fa69c7b82f1d6db78dd47d
ms.sourcegitcommit: 73aa9653547a1cd70ee6586221f79cc29b588ebd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82102041"
---
# <a name="classes-c-programming-guide"></a>Classes (Guia de Programação em C#)

## <a name="reference-types"></a>Tipos de referência  
Um tipo que é definido como uma [classe](../../language-reference/keywords/class.md) é um *tipo de referência*. No tempo de execução, quando você declara uma variável de um tipo de referência, a variável contém o valor [null](../../language-reference/keywords/null.md) até que você crie explicitamente uma instância da classe usando o operador [new](../../language-reference/operators/new-operator.md) ou atribua a ela um objeto de um tipo compatível que foi criado em outro lugar, conforme mostrado no exemplo a seguir:

```csharp
//Declaring an object of type MyClass.
MyClass mc = new MyClass();

//Declaring another object of the same type, assigning it the value of the first object.
MyClass mc2 = mc;
```

Quando o objeto é criado, memória suficiente é alocada no heap gerenciado para o objeto específico, e a variável contém apenas uma referência para o local do objeto. Os tipos no heap gerenciado requerem sobrecarga quando são alocados e quando são recuperados pela funcionalidade de gerenciamento automático de memória do CLR, que é conhecida como *coleta de lixo*. No entanto, a coleta de lixo também é altamente otimizada e, na maioria dos cenários, não cria um problema de desempenho. Para obter mais informações sobre a coleta de lixo, consulte [Gerenciamento automático de memória e coleta de lixo](../../../standard/garbage-collection/fundamentals.md).  
  
## <a name="declaring-classes"></a>Declarando Classes

 As classes são declaradas usando a palavra-chave [class](../../language-reference/keywords/class.md), seguida por um identificador exclusivo, conforme mostrado no exemplo a seguir:

 ```csharp
//[access modifier] - [class] - [identifier]
 public class Customer
 {
    // Fields, properties, methods and events go here...
 }
```

 A palavra-chave `class` é precedida pelo nível de acesso. Como [público](../../language-reference/keywords/public.md) é usado nesse caso, qualquer pessoa pode criar instâncias dessa classe. O nome da classe segue a palavra-chave `class`. O nome da classe deve ser um [nome do identificador](../inside-a-program/identifier-names.md) válido em C#. O restante da definição é o corpo da classe, em que o comportamento e os dados são definidos. Campos, propriedades, métodos e eventos em uma classe são coletivamente denominados de *membros de classe*.  
  
## <a name="creating-objects"></a>Criando objetos

Embora eles sejam usados algumas vezes de maneira intercambiável, uma classe e um objeto são coisas diferentes. Uma classe define um tipo de objeto, mas não é um objeto em si. Um objeto é uma entidade concreta com base em uma classe e, às vezes, é conhecido como uma instância de uma classe.  
  
 Os objetos podem ser criados usando a palavra-chave [new](../../language-reference/operators/new-operator.md), seguida pelo nome da classe na qual ele se baseará, dessa maneira:  

 ```csharp
 Customer object1 = new Customer();
 ```

 Quando uma instância de uma classe é criada, uma referência ao objeto é passada de volta para o programador. No exemplo anterior, `object1` é uma referência a um objeto que é baseado em `Customer`. Esta referência refere-se ao novo objeto, mas não contém os dados de objeto. Na verdade, você pode criar uma referência de objeto sem criar um objeto:  

```csharp
 Customer object2;
```

 Não recomendamos a criação de referências de objeto como essa, que não faz referência a um objeto, porque tentar acessar um objeto por meio de uma referência desse tipo falhará em tempo de execução. Entretanto, essa referência pode ser feita para se referir a um objeto, criando um novo objeto ou atribuindo-a a um objeto existente, como abaixo:  

 ```csharp
 Customer object3 = new Customer();
 Customer object4 = object3;
```
  
 Esse código cria duas referências de objeto que fazem referência ao mesmo objeto. Portanto, qualquer alteração no objeto feita por meio de `object3` será refletida no usos posteriores de `object4`. Como os objetos que são baseados em classes são referenciados por referência, as classes são conhecidas como tipos de referência.  
  
## <a name="class-inheritance"></a>Herança de classe  

As classes dão suporte completo à *herança*, uma característica fundamental da programação orientada a objetos. Ao criar uma classe, você pode herdar de outra interface ou classe que não está definida como [selada](../../language-reference/keywords/sealed.md), e outras classes podem herdar de sua classe e substituir seus métodos virtuais.

A herança é realizada usando uma *derivação*, o que significa que uma classe é declarada usando uma *classe base*, da qual ela herda o comportamento e os dados. Uma classe base é especificada ao acrescentar dois-pontos e o nome de classe base depois do nome de classe derivada, dessa maneira:  

 ```csharp
 public class Manager : Employee
 {
     // Employee fields, properties, methods and events are inherited
     // New Manager fields, properties, methods and events go here...
 }
 ```

Quando uma classe declara uma classe base, ela herda todos os membros da classe base, exceto os construtores. Para obter mais informações, consulte [Herança](inheritance.md).
  
Ao contrário do C++, uma classe no C# só pode herdar diretamente de uma classe base. No entanto, como uma classe base pode herdar de outra classe, uma classe pode herdar indiretamente várias classes base. Além disso, uma classe pode implementar diretamente mais de uma interface. Para obter mais informações, consulte [interfaces](../interfaces/index.md).  
  
Uma classe pode ser declarada [abstract](../../language-reference/keywords/abstract.md). Uma classe abstrata contém métodos abstratos que têm uma definição de assinatura, mas não têm implementação. As classes abstratas não podem ser instanciadas. Elas só podem ser usadas por meio de classes derivadas que implementam os métodos abstratos. Por outro lado, uma classe [lacrada](../../language-reference/keywords/sealed.md) não permite que outras classes sejam derivadas dela. Para obter mais informações, consulte [classes abstratas e lacradas e membros de classe](abstract-and-sealed-classes-and-class-members.md).  
  
As definições de classe podem ser divididas entre arquivos de origem diferentes. Para obter mais informações, consulte [Classes parciais e métodos](partial-classes-and-methods.md).  
  
## <a name="example"></a>Exemplo

No exemplo a seguir, é definida uma classe pública que contém uma [propriedade autoimplementada](auto-implemented-properties.md), um método e um método especial chamado construtor. Para obter mais informações, consulte os tópicos [Propriedades](properties.md), [Métodos](methods.md), e [Construtores](constructors.md). As instâncias da classe são então instanciadas com a palavra-chave `new`.  
  
[!code-csharp[Class Example](~/samples/snippets/csharp/programming-guide/classes-and-structs/class-example.cs)]
  
## <a name="c-language-specification"></a>Especificação da Linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Programação orientada a objeto](../concepts/object-oriented-programming.md)
- [Polimorfismo](polymorphism.md)
- [Nomes de identificadores](../inside-a-program/identifier-names.md)
- [Membros](members.md)
- [Métodos](methods.md)
- [Construtores](constructors.md)
- [Finalizadores](destructors.md)
- [Objetos](objects.md)
