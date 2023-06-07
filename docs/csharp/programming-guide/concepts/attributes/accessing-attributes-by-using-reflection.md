---
title: Acessando atributos usando reflexão (C#)
description: Use a reflexão para obter informações definidas com atributos personalizados em C# usando o método GetCustomAttributes.
ms.date: 07/20/2015
ms.assetid: dce3a696-4ceb-489a-b5e4-322a83052f18
ms.openlocfilehash: 9425141d64fd061d0c1f628228693cce02f7bfa0
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925092"
---
# <a name="accessing-attributes-by-using-reflection-c"></a>Acessando atributos usando reflexão (C#)
O fato de que você pode definir atributos personalizados e colocá-los em seu código-fonte seria de pouco valor sem alguma maneira de recuperar essas informações e tomar ação sobre elas. Por meio de reflexão, você pode recuperar as informações que foram definidas com atributos personalizados. O método principal é o `GetCustomAttributes`, que retorna uma matriz de objetos que são equivalentes, em tempo de execução, aos atributos do código-fonte. Esse método tem várias versões sobrecarregadas. Para obter mais informações, consulte <xref:System.Attribute>.  
  
 Uma especificação de atributo, como:  
  
```csharp  
[Author("P. Ackerman", version = 1.1)]  
class SampleClass  
```  
  
 é conceitualmente equivalente a esta:  
  
```csharp  
Author anonymousAuthorObject = new Author("P. Ackerman");  
anonymousAuthorObject.version = 1.1;  
```  
  
 No entanto, o código não será executado até que `SampleClass` tenha os atributos consultados. Chamar `GetCustomAttributes` na `SampleClass` faz com que um objeto `Author` seja criado e inicializado como acima. Se a classe tiver outros atributos, outros objetos de atributo serão construídos de forma semelhante. Então o `GetCustomAttributes` retornará o objeto `Author` e quaisquer outros objetos de atributo em uma matriz. Você poderá iterar sobre essa matriz, determinar quais atributos foram aplicados com base no tipo de cada elemento da matriz e extrair informações dos objetos de atributo.  
  
## <a name="example"></a>Exemplo  
 Aqui está um exemplo completo. Um atributo personalizado é definido, aplicado a várias entidades e recuperado por meio da reflexão.  
  
```csharp  
// Multiuse attribute.  
[System.AttributeUsage(System.AttributeTargets.Class |  
                       System.AttributeTargets.Struct,  
                       AllowMultiple = true)  // Multiuse attribute.  
]  
public class Author : System.Attribute  
{  
    string name;  
    public double version;  
  
    public Author(string name)  
    {  
        this.name = name;  
  
        // Default value.  
        version = 1.0;  
    }  
  
    public string GetName()  
    {  
        return name;  
    }  
}  
  
// Class with the Author attribute.  
[Author("P. Ackerman")]  
public class FirstClass  
{  
    // ...  
}  
  
// Class without the Author attribute.  
public class SecondClass  
{  
    // ...  
}  
  
// Class with multiple Author attributes.  
[Author("P. Ackerman"), Author("R. Koch", version = 2.0)]  
public class ThirdClass  
{  
    // ...  
}  
  
class TestAuthorAttribute  
{  
    static void Test()  
    {  
        PrintAuthorInfo(typeof(FirstClass));  
        PrintAuthorInfo(typeof(SecondClass));  
        PrintAuthorInfo(typeof(ThirdClass));  
    }  
  
    private static void PrintAuthorInfo(System.Type t)  
    {  
        System.Console.WriteLine("Author information for {0}", t);  
  
        // Using reflection.  
        System.Attribute[] attrs = System.Attribute.GetCustomAttributes(t);  // Reflection.  
  
        // Displaying output.  
        foreach (System.Attribute attr in attrs)  
        {  
            if (attr is Author)  
            {  
                Author a = (Author)attr;  
                System.Console.WriteLine("   {0}, version {1:f}", a.GetName(), a.version);  
            }  
        }  
    }  
}  
/* Output:  
    Author information for FirstClass  
       P. Ackerman, version 1.00  
    Author information for SecondClass  
    Author information for ThirdClass  
       R. Koch, version 2.00  
       P. Ackerman, version 1.00  
*/  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Reflection>
- <xref:System.Attribute>
- [Guia de programação C#](../../index.md)
- [Recuperando informações armazenadas em atributos](../../../../standard/attributes/retrieving-information-stored-in-attributes.md)
- [Reflexão (C#)](../reflection.md)
- [Atributos (C#)](./index.md)
- [Criando atributos personalizados (C#)](./creating-custom-attributes.md)
