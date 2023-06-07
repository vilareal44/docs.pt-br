---
title: protected internal (referência do C#)
ms.date: 11/15/2017
f1_keywords:
- protectedinternal_CSharpKeyword
author: sputier
ms.openlocfilehash: 4067da93bcceba0fa3e4a14aa58b4cde812412f3
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301782"
---
# <a name="protected-internal-c-reference"></a>protected internal (referência do C#)

A combinação de palavras-chave `protected internal` é um modificador de acesso de membro. Um membro protegido interno é acessível no assembly atual ou nos tipos que derivam da classe recipiente. Para obter uma comparação de `protected internal` com os outros modificadores de acesso, consulte [Níveis de acessibilidade](accessibility-levels.md).

## <a name="example"></a>Exemplo

Um membro protegido interno de uma classe base é acessível em qualquer tipo em seu assembly recipiente. Ele também é acessível em uma classe derivada localizada em outro assembly somente se o acesso ocorre por meio de uma variável do tipo de classe derivada. Por exemplo, considere o seguinte segmento de código:

```csharp
// Assembly1.cs
// Compile with: /target:library
public class BaseClass
{
   protected internal int myValue = 0;
}

class TestAccess
{
    void Access()
    {
        var baseObject = new BaseClass();
        baseObject.myValue = 5;
    }
}
```

```csharp
// Assembly2.cs
// Compile with: /reference:Assembly1.dll
class DerivedClass : BaseClass
{
    static void Main()
    {
        var baseObject = new BaseClass();
        var derivedObject = new DerivedClass();

        // Error CS1540, because myValue can only be accessed by
        // classes derived from BaseClass.
        // baseObject.myValue = 10;

        // OK, because this class derives from BaseClass.
        derivedObject.myValue = 10;
    }
}
```

Este exemplo contém dois arquivos, `Assembly1.cs` e `Assembly2.cs`.
O primeiro arquivo contém uma classe base pública, `BaseClass`, e outra classe, `TestAccess`. `BaseClass` tem um membro interno protegido, `myValue`, que é acessado pelo tipo `TestAccess`.
No segundo arquivo, uma tentativa de acessar `myValue` por meio de uma instância de `BaseClass` produzirá um erro, enquanto um acesso a esse membro por meio de uma instância de uma classe derivada, `DerivedClass`, terá êxito.

Membros de struct não podem ser `protected internal` porque o struct não pode ser herdado.

## <a name="c-language-specification"></a>especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Veja também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Modificadores de acesso](access-modifiers.md)
- [Níveis de acessibilidade](accessibility-levels.md)
- [Modificadores](index.md)
- [público](public.md)
- [pessoal](private.md)
- [internal](internal.md)
- [Questões de segurança de palavras-chave virtuais internas](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/heyd8kky(v=vs.100))
