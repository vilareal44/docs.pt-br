---
title: Usando variação em interfaces para Coleções Genéricas (C#)
description: Saiba como usar as interfaces covariantes e contravariant para coleções genéricas. Consulte exemplos de conversão e comparação de coleções genéricas.
ms.date: 07/20/2015
ms.assetid: a44f0708-10fa-4c76-82cd-daa6e6b31e8e
ms.openlocfilehash: c2ce849e32520cb91422ff36173e418a010476bd
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105671"
---
# <a name="using-variance-in-interfaces-for-generic-collections-c"></a>Usando variação em interfaces para Coleções Genéricas (C#)
Uma interface de covariante permite que seus métodos retornem mais tipos derivados daquelas especificadas na interface. Uma interface de contravariante permite que seus métodos aceitem parâmetros de tipos menos derivados do que os especificados na interface.  
  
 No .NET Framework 4, várias interfaces existentes se tornaram covariantes e contravariantes. Eles incluem <xref:System.Collections.Generic.IEnumerable%601> e <xref:System.IComparable%601>. Isso permite que você reutilize métodos que operam com coleções genéricas de tipos base para coleções de tipos derivados.  
  
 Para obter uma lista de interfaces variantes no .NET, consulte [variação em interfaces genéricas (C#)](./variance-in-generic-interfaces.md).  
  
## <a name="converting-generic-collections"></a>Convertendo coleções genéricas  
 O exemplo a seguir ilustra os benefícios do suporte à covariância na interface <xref:System.Collections.Generic.IEnumerable%601>. O método `PrintFullName` aceita uma coleção do tipo `IEnumerable<Person>` como um parâmetro. No entanto, você pode reutilizá-lo para uma coleção do tipo `IEnumerable<Employee>` porque `Employee` herda `Person`.  
  
```csharp  
// Simple hierarchy of classes.  
public class Person  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
}  
  
public class Employee : Person { }  
  
class Program  
{  
    // The method has a parameter of the IEnumerable<Person> type.  
    public static void PrintFullName(IEnumerable<Person> persons)  
    {  
        foreach (Person person in persons)  
        {  
            Console.WriteLine("Name: {0} {1}",  
            person.FirstName, person.LastName);  
        }  
    }  
  
    public static void Test()  
    {  
        IEnumerable<Employee> employees = new List<Employee>();  
  
        // You can pass IEnumerable<Employee>,
        // although the method expects IEnumerable<Person>.  
  
        PrintFullName(employees);  
  
    }  
}  
```  
  
## <a name="comparing-generic-collections"></a>Comparando coleções genéricas  
 O exemplo a seguir ilustra os benefícios do suporte à contravariância na interface <xref:System.Collections.Generic.IComparer%601>. A classe `PersonComparer` implementa a interface `IComparer<Person>`. No entanto, você pode reutilizar essa classe para comparar uma sequência de objetos do tipo `Employee` porque `Employee` herda `Person`.  
  
```csharp  
// Simple hierarchy of classes.  
public class Person  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
}  
  
public class Employee : Person { }  
  
// The custom comparer for the Person type  
// with standard implementations of Equals()  
// and GetHashCode() methods.  
class PersonComparer : IEqualityComparer<Person>  
{  
    public bool Equals(Person x, Person y)  
    {
        if (Object.ReferenceEquals(x, y)) return true;  
        if (Object.ReferenceEquals(x, null) ||  
            Object.ReferenceEquals(y, null))  
            return false;
        return x.FirstName == y.FirstName && x.LastName == y.LastName;  
    }  
    public int GetHashCode(Person person)  
    {  
        if (Object.ReferenceEquals(person, null)) return 0;  
        int hashFirstName = person.FirstName == null  
            ? 0 : person.FirstName.GetHashCode();  
        int hashLastName = person.LastName.GetHashCode();  
        return hashFirstName ^ hashLastName;  
    }  
}  
  
class Program  
{  
  
    public static void Test()  
    {  
        List<Employee> employees = new List<Employee> {  
               new Employee() {FirstName = "Michael", LastName = "Alexander"},  
               new Employee() {FirstName = "Jeff", LastName = "Price"}  
            };  
  
        // You can pass PersonComparer,
        // which implements IEqualityComparer<Person>,  
        // although the method expects IEqualityComparer<Employee>.  
  
        IEnumerable<Employee> noduplicates =  
            employees.Distinct<Employee>(new PersonComparer());  
  
        foreach (var employee in noduplicates)  
            Console.WriteLine(employee.FirstName + " " + employee.LastName);  
    }  
}  
```  
  
## <a name="see-also"></a>Confira também

- [Variância em interfaces genéricas (C#)](./variance-in-generic-interfaces.md)
