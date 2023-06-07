---
title: private protected – Referência de C#
ms.date: 11/15/2017
f1_keywords:
- privateprotected_CSharpKeyword
author: sputier
ms.openlocfilehash: 94ef55d7e13841f81b036f52659b215e22a3a0d7
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301795"
---
# <a name="private-protected-c-reference"></a>private protected (referência do C#)

A combinação de palavras-chave `private protected` é um modificador de acesso de membro. Um membro particular protegido é acessível por tipos derivados da classe recipiente, mas apenas dentro de seu assembly recipiente. Para obter uma comparação de `private protected` com os outros modificadores de acesso, consulte [Níveis de acessibilidade](accessibility-levels.md).

> [!NOTE]
> O modificador de acesso `private protected` é válido no C# versão 7.2 e posterior.

## <a name="example"></a>Exemplo

Um membro particular protegido de uma classe base é acessível de tipos derivados em seu assembly recipiente apenas se o tipo estático da variável é o tipo da classe derivada. Por exemplo, considere o seguinte segmento de código:

```csharp
public class BaseClass
{
    private protected int myValue = 0;
}

public class DerivedClass1 : BaseClass
{
    void Access()
    {
        var baseObject = new BaseClass();

        // Error CS1540, because myValue can only be accessed by
        // classes derived from BaseClass.
        // baseObject.myValue = 5;

        // OK, accessed through the current derived class instance
        myValue = 5;
    }
}
```

```csharp
// Assembly2.cs
// Compile with: /reference:Assembly1.dll
class DerivedClass2 : BaseClass
{
    void Access()
    {
        // Error CS0122, because myValue can only be
        // accessed by types in Assembly1
        // myValue = 10;
    }
}
```

Este exemplo contém dois arquivos, `Assembly1.cs` e `Assembly2.cs`.
O primeiro arquivo contém uma classe base pública, `BaseClass`, e um tipo derivado dela, `DerivedClass1`. `BaseClass` tem um membro particular protegido, `myValue`, que `DerivedClass1` tenta acessar de duas maneiras. A primeira tentativa de acessar `myValue` por meio de uma instância de `BaseClass` produzirá um erro. No entanto, a tentativa de usá-lo como um membro herdado em `DerivedClass1` terá êxito.

No segundo arquivo, uma tentativa de acessar `myValue` como um membro herdado de `DerivedClass2` produzirá um erro, pois ele é acessível apenas por tipos derivados em Assembly1.

Se `Assembly1.cs` contiver um <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> nome que `Assembly2` , a classe derivada terá `DerivedClass1` acesso aos `private protected` membros declarados em `BaseClass` . `InternalsVisibleTo`torna `private protected` os membros visíveis para classes derivadas em outros assemblies.

Membros de struct não podem ser `private protected` porque o struct não pode ser herdado.

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
