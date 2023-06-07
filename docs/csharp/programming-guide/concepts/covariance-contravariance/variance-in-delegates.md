---
title: Variação em delegados (C#)
description: Saiba como o suporte à variação no .NET Framework permite que você corresponda a assinaturas de método com tipos delegados em todos os delegados.
ms.date: 07/20/2015
ms.assetid: 19de89d2-8224-4406-8964-2965b732b890
ms.openlocfilehash: ef57a7fa7feaef98a47822e3f1c9242d0205932d
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105659"
---
# <a name="variance-in-delegates-c"></a>Variação em delegados (C#)
O .NET Framework 3.5 introduziu o suporte a variação para assinaturas de método correspondentes com tipos de delegados em todos os delegados do C#. Isso significa que você pode atribuir a delegados não apenas os métodos que têm assinaturas correspondentes, mas também métodos que retornam tipos mais derivados (covariância) ou que aceitam parâmetros que têm tipos menos derivados (contravariância) do que o especificado pelo tipo de delegado. Isso inclui delegados genéricos e não genéricos.  
  
 Por exemplo, considere o código a seguir, que tem duas classes e dois delegados: genérico e não genérico.  
  
```csharp  
public class First { }  
public class Second : First { }  
public delegate First SampleDelegate(Second a);  
public delegate R SampleGenericDelegate<A, R>(A a);  
```  
  
 Quando cria delegados dos tipos `SampleDelegate` ou `SampleGenericDelegate<A, R>`, você pode atribuir qualquer um dos seguintes métodos a esses delegados.  
  
```csharp  
// Matching signature.  
public static First ASecondRFirst(Second second)  
{ return new First(); }  
  
// The return type is more derived.  
public static Second ASecondRSecond(Second second)  
{ return new Second(); }  
  
// The argument type is less derived.  
public static First AFirstRFirst(First first)  
{ return new First(); }  
  
// The return type is more derived
// and the argument type is less derived.  
public static Second AFirstRSecond(First first)  
{ return new Second(); }  
```  
  
 O exemplo de código a seguir ilustra a conversão implícita entre a assinatura do método e o tipo de delegado.  
  
```csharp  
// Assigning a method with a matching signature
// to a non-generic delegate. No conversion is necessary.  
SampleDelegate dNonGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type
// and less derived argument type to a non-generic delegate.  
// The implicit conversion is used.  
SampleDelegate dNonGenericConversion = AFirstRSecond;  
  
// Assigning a method with a matching signature to a generic delegate.  
// No conversion is necessary.  
SampleGenericDelegate<Second, First> dGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type
// and less derived argument type to a generic delegate.  
// The implicit conversion is used.  
SampleGenericDelegate<Second, First> dGenericConversion = AFirstRSecond;  
```  
  
 Para obter mais informações, consulte [Usando variação em delegados (C#)](./using-variance-in-delegates.md) e [Usando variação para os delegados genéricos Func e Action (C#)](./using-variance-for-func-and-action-generic-delegates.md).  
  
## <a name="variance-in-generic-type-parameters"></a>Variação em parâmetros de tipo genérico  
 No .NET Framework 4 ou posterior, agora você pode habilitar a conversão implícita entre delegados, de modo que delegados genéricos que têm tipos diferentes especificados por parâmetros de tipo genérico podem ser atribuídos uns aos outros, se os tipos forem herdados uns dos outros como é obrigatório de acordo com a variação.  
  
 Para habilitar a conversão implícita, você precisa declarar explicitamente os parâmetros genéricos em um delegado como covariante ou contravariante usando a palavra-chave `in` ou `out`.  
  
 O exemplo de código a seguir mostra como você pode criar um delegado que tem um parâmetro de tipo genérico covariante.  
  
```csharp  
// Type T is declared covariant by using the out keyword.  
public delegate T SampleGenericDelegate <out T>();  
  
public static void Test()  
{  
    SampleGenericDelegate <String> dString = () => " ";  
  
    // You can assign delegates to each other,  
    // because the type T is declared covariant.  
    SampleGenericDelegate <Object> dObject = dString;
}  
```  
  
 Se você usar somente o suporte à para fazer a correspondência de assinaturas de método com tipos de delegados e não usar as palavras-chave `in` e `out`, você poderá perceber que, às vezes, é possível instanciar delegados com métodos ou expressões lambda idênticas, mas não é possível atribuir um delegado a outro.  
  
 No exemplo de código a seguir, `SampleGenericDelegate<String>` não pode ser convertido explicitamente em `SampleGenericDelegate<Object>`, embora `String` herde `Object`. Você pode corrigir esse problema marcando o parâmetro genérico `T` com a palavra-chave `out`.  
  
```csharp  
public delegate T SampleGenericDelegate<T>();  
  
public static void Test()  
{  
    SampleGenericDelegate<String> dString = () => " ";  
  
    // You can assign the dObject delegate  
    // to the same lambda expression as dString delegate  
    // because of the variance support for
    // matching method signatures with delegate types.  
    SampleGenericDelegate<Object> dObject = () => " ";  
  
    // The following statement generates a compiler error  
    // because the generic type T is not marked as covariant.  
    // SampleGenericDelegate <Object> dObject = dString;  
  
}  
```  
  
### <a name="generic-delegates-that-have-variant-type-parameters-in-net"></a>Delegados genéricos que têm parâmetros de tipo Variant no .NET

O .NET Framework 4 introduziu o suporte à variação para parâmetros de tipo genérico em diversos delegados genéricos existentes:  
  
- `Action` delega do namespace <xref:System>, por exemplo, <xref:System.Action%601> e <xref:System.Action%602>  
  
- `Func` delega do namespace <xref:System>, por exemplo, <xref:System.Func%601> e <xref:System.Func%602>  
  
- O delegado <xref:System.Predicate%601>  
  
- O delegado <xref:System.Comparison%601>  
  
- O delegado <xref:System.Converter%602>  
  
 Para obter mais informações e exemplos, consulte [Usando variação para delegados genéricos Func e Action (C#)](./using-variance-for-func-and-action-generic-delegates.md).  
  
### <a name="declaring-variant-type-parameters-in-generic-delegates"></a>Declarando parâmetros de tipo variante em delegados genéricos  
 Se um delegado genérico tiver parâmetros de tipo genérico covariantes ou contravariantes, ele poderá ser considerado um *delegado genérico variante*.  
  
 Você pode declarar um parâmetro de tipo genérico covariante em um delegado genérico usando a palavra-chave `out`. O tipo covariante pode ser usado apenas como um tipo de retorno de método e não como um tipo de argumentos de método. O exemplo de código a seguir mostra como declarar um delegado genérico covariante.  
  
```csharp  
public delegate R DCovariant<out R>();  
```  
  
 Você pode declarar um parâmetro de tipo genérico contravariante em um delegado genérico usando a palavra-chave `in`. O tipo contravariante pode ser usado apenas como um tipo de argumentos de método e não como um tipo de retorno de método. O exemplo de código a seguir mostra como declarar um delegado genérico contravariante.  
  
```csharp  
public delegate void DContravariant<in A>(A a);  
```  
  
> [!IMPORTANT]
> Os parâmetros `ref`, `in` e `out` em C# não podem ser marcados como variantes.  
  
 Também é possível dar suporte à variância e à covariância no mesmo delegado, mas para parâmetros de tipo diferente. Isso é mostrado no exemplo a seguir.  
  
```csharp  
public delegate R DVariant<in A, out R>(A a);  
```  
  
### <a name="instantiating-and-invoking-variant-generic-delegates"></a>Instanciando e invocando delegados genéricos variantes  
 Você pode instanciar e invocar delegados variantes da mesma forma como instancia e invoca delegados invariantes. No exemplo a seguir, um delegado é instanciado por uma expressão lambda.  
  
```csharp  
DVariant<String, String> dvariant = (String str) => str + " ";  
dvariant("test");  
```  
  
### <a name="combining-variant-generic-delegates"></a>Combinando delegados genéricos variantes  

Não combine delegados de variante. O método <xref:System.Delegate.Combine%2A> não dá suporte à conversão de delegados variantes e espera que os delegados sejam exatamente do mesmo tipo. Isso pode levar a uma exceção de tempo de execução quando você combina delegados usando o método <xref:System.Delegate.Combine%2A> ou o operador `+`, conforme mostrado no exemplo de código a seguir.  
  
```csharp  
Action<object> actObj = x => Console.WriteLine("object: {0}", x);  
Action<string> actStr = x => Console.WriteLine("string: {0}", x);  
// All of the following statements throw exceptions at run time.  
// Action<string> actCombine = actStr + actObj;  
// actStr += actObj;  
// Delegate.Combine(actStr, actObj);  
```  
  
## <a name="variance-in-generic-type-parameters-for-value-and-reference-types"></a>Variação em parâmetros de tipo genérico para tipos de referência e valor  
 A variação para parâmetros de tipo genérico tem suporte apenas para tipos de referência. Por exemplo, `DVariant<int>` não pode ser convertido implicitamente em `DVariant<Object>` ou `DVariant<long>`, pois inteiro é um tipo de valor.  
  
 O exemplo a seguir demonstra que a variação em parâmetros de tipo genérico não tem suporte para tipos de valor.  
  
```csharp  
// The type T is covariant.  
public delegate T DVariant<out T>();  
  
// The type T is invariant.  
public delegate T DInvariant<T>();  
  
public static void Test()  
{  
    int i = 0;  
    DInvariant<int> dInt = () => i;  
    DVariant<int> dVariantInt = () => i;  
  
    // All of the following statements generate a compiler error  
    // because type variance in generic parameters is not supported  
    // for value types, even if generic type parameters are declared variant.  
    // DInvariant<Object> dObject = dInt;  
    // DInvariant<long> dLong = dInt;  
    // DVariant<Object> dVariantObject = dVariantInt;  
    // DVariant<long> dVariantLong = dVariantInt;
}  
```  
  
## <a name="see-also"></a>Confira também

- [Genéricos](../../../../standard/generics/index.md)
- [Usando variação para delegados genéricos Func e Action (C#)](./using-variance-for-func-and-action-generic-delegates.md)
- [Como combinar delegados (delegados de multicast)](../../delegates/how-to-combine-delegates-multicast-delegates.md)
