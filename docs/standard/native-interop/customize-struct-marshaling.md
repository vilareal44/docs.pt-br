---
title: Personalização do marshaling de estrutura – .NET
description: Saiba como personalizar como o .NET empacota estruturas para uma representação nativa.
ms.date: 01/18/2019
dev_langs:
- csharp
- cpp
ms.openlocfilehash: c82e0099c44b8033cad241d69bdd284243711a50
ms.sourcegitcommit: e7748001b1cee80ced691d8a76ca814c0b02dd9b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86374527"
---
# <a name="customize-structure-marshaling"></a>Personalizar marshaling de estruturas

Às vezes, as regras de marshaling padrão para estruturas não são exatamente o que você precisa. Os runtimes do .NET fornecem alguns pontos de extensão para você personalizar o layout de sua estrutura e como os campos têm o marshaling realizado.

## <a name="customizing-structure-layout"></a>Personalização do layout de estrutura

O .NET fornece o atributo <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> e a enumeração <xref:System.Runtime.InteropServices.LayoutKind?displayProperty=nameWithType> para permitir que você personalize como os campos são colocados na memória. As diretrizes a seguir ajudarão a evitar problemas comuns.

✔️ CONSIDERE usar `LayoutKind.Sequential` sempre que possível.

✔️ Só usar `LayoutKind.Explicit` no marshaling quando sua estrutura nativa também tiver um layout explícito, como uma Union.

❌Evite usar `LayoutKind.Explicit` ao realizar marshaling de estruturas em plataformas não Windows se você precisar direcionar os tempos de execução antes do .NET Core 3,0. O tempo de execução do .NET Core antes de 3,0 não dá suporte à passagem de estruturas explícitas por valor para funções nativas em sistemas Intel ou AMD de 64 bits que não sejam Windows. No entanto, o runtime dá suporte à passagem de estruturas explícitas por referência em todas as plataformas.

## <a name="customizing-boolean-field-marshaling"></a>Personalização do marshaling de campo booliano

O código nativo tem muitas representações boolianas diferentes. Somente no Windows, há três formas de representar valores boolianos. O runtime não conhece a definição nativa de sua estrutura, portanto, o melhor que ele pode fazer é estimar como realizar marshal de seus valores boolianos. O runtime do .NET fornece uma maneira de indicar como realizar marshal de seu campo booliano. Os exemplos a seguir mostram como realizar marshal do .NET `bool` para diferentes tipos boolianos nativos.

Os valores Boolianos assumem como padrão o marshaling como um valor nativo de 4 bytes [`BOOL`](/windows/desktop/winprog/windows-data-types#BOOL) do Win32, conforme mostrado no exemplo a seguir:

```csharp
public struct WinBool
{
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

Se você quiser ser explícito, use o valor <xref:System.Runtime.InteropServices.UnmanagedType.Bool?displayProperty=nameWithType> para obter o mesmo comportamento descrito acima:

```csharp
public struct WinBool
{
    [MarshalAs(UnmanagedType.Bool)]
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

Usando os valores `UnmanagedType.U1` ou `UnmanagedType.I1` abaixo, você pode informar o runtime para realizar marshal do campo `b` como um tipo `bool` nativo de 1 byte.

```csharp
public struct CBool
{
    [MarshalAs(UnmanagedType.U1)]
    public bool b;
}
```

```cpp
struct CBool
{
    public bool b;
};
```

No Windows, use o valor <xref:System.Runtime.InteropServices.UnmanagedType.VariantBool?displayProperty=nameWithType> para informar o runtime para realizar marshal de seu valor booliano em um valor `VARIANT_BOOL` de 2 bytes:

```csharp
public struct VariantBool
{
    [MarshalAs(UnmanagedType.VariantBool)]
    public bool b;
}
```

```cpp
struct VariantBool
{
    public VARIANT_BOOL b;
};
```

> [!NOTE]
> `VARIANT_BOOL` é diferente da maioria dos tipos de bool em que `VARIANT_TRUE = -1` e `VARIANT_FALSE = 0`. Além disso, todos os valores que não são iguais a `VARIANT_TRUE` são considerados falsos.

## <a name="customizing-array-field-marshaling"></a>Personalização do marshaling de campo de matriz

O .NET também inclui algumas formas de personalizar o marshaling de matriz.

Por padrão, o .NET realiza marshal de matrizes como um ponteiro para uma lista contígua dos elementos:

```csharp
public struct DefaultArray
{
    public int[] values;
}
```

```cpp
struct DefaultArray
{
    int* values;
};
```

Se você estiver interagindo com APIs COM, talvez seja necessário realizar marshal de matrizes como objetos `SAFEARRAY*`. Use o valor <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> e <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> para informar o runtime para realizar marshal de uma matriz como uma `SAFEARRAY*`:

```csharp
public struct SafeArrayExample
{
    [MarshalAs(UnmanagedType.SafeArray)]
    public int[] values;
}
```

```cpp
struct SafeArrayExample
{
    SAFEARRAY* values;
};
```

Se for necessário personalizar o tipo de elemento que está na `SAFEARRAY`, use os campos <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> e <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> para personalizar o tipo de elemento exato da `SAFEARRAY`.

Se você precisar realizar marshal da matriz in-loco, use o valor <xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType> para informar o marshaler para realizar marshal da matriz in-loco. Ao usar esse marshaling, você também precisa fornecer um valor ao campo <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> para o número de elementos na matriz, para que o runtime possa alocar espaço corretamente para a estrutura.

```csharp
public struct InPlaceArray
{
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = 4)]
    public int[] values;
}
```

```cpp
struct InPlaceArray
{
    int values[4];
};
```

> [!NOTE]
> O .NET não dá suporte à realização de marshaling de um campo de matriz de comprimento variável como um Membro de Matriz Flexível C99.

## <a name="customizing-string-field-marshaling"></a>Personalização do marshaling de campo de cadeia de caracteres

O .NET também fornece uma ampla variedade de personalizações para realizar marshaling de campos de cadeia de caracteres.

Por padrão, o .NET realiza marshal de uma cadeia de caracteres como um ponteiro para uma cadeia de caracteres terminada em nulo. A codificação depende do valor do campo <xref:System.Runtime.InteropServices.StructLayoutAttribute.CharSet?displayProperty=nameWithType> no <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType>. Se nenhum atributo for especificado, a codificação será padronizada para uma codificação ANSI.

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char* str;
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

Se você precisar usar codificações diferentes para campos distintos ou apenas prefere ser mais explícito em sua definição de struct, use os valores <xref:System.Runtime.InteropServices.UnmanagedType.LPStr?displayProperty=nameWithType> ou <xref:System.Runtime.InteropServices.UnmanagedType.LPWStr?displayProperty=nameWithType> em um atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType>.

```csharp
public struct AnsiString
{
    [MarshalAs(UnmanagedType.LPStr)]
    public string str;
}
```

```cpp
struct AnsiString
{
    char* str;
};
```

```csharp
public struct UnicodeString
{
    [MarshalAs(UnmanagedType.LPWStr)]
    public string str;
}
```

```cpp
struct UnicodeString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

Se quiser realizar marshal de suas cadeias de caracteres usando a codificação UTF-8, use o valor <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> em seu <xref:System.Runtime.InteropServices.MarshalAsAttribute>.

```csharp
public struct UTF8String
{
    [MarshalAs(UnmanagedType.LPUTF8Str)]
    public string str;
}
```

```cpp
struct UTF8String
{
    char* str;
};
```

> [!NOTE]
> O uso de <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> requer o .NET Framework 4.7 (ou versões posteriores) ou o .NET Core 1.1 (ou versões posteriores). Não está disponível no .NET Standard 2.0.

Se você estiver trabalhando com APIs COM, talvez seja necessário realizar marshal de uma cadeia de caracteres como um `BSTR`. Usando o valor <xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType>, você pode realizar marshal de uma cadeia de caracteres como um `BSTR`.

```csharp
public struct BString
{
    [MarshalAs(UnmanagedType.BStr)]
    public string str;
}
```

```cpp
struct BString
{
    BSTR str;
};
```

Ao usar uma API baseada no WinRT, talvez seja necessário realizar marshal de uma cadeia de caracteres como um `HSTRING`. Usando o valor <xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType>, você pode realizar marshal de uma cadeia de caracteres como um `HSTRING`.

```csharp
public struct HString
{
    [MarshalAs(UnmanagedType.HString)]
    public string str;
}
```

```cpp
struct BString
{
    HSTRING str;
};
```

Se sua API requer que você passe a cadeia de caracteres in-loco na estrutura, use o valor <xref:System.Runtime.InteropServices.UnmanagedType.ByValTStr?displayProperty=nameWithType>. A codificação de uma cadeia de caracteres com marshaling realizado por `ByValTStr` é determinada por meio do atributo `CharSet`. Além disso, ela requer que o comprimento da cadeia de caracteres seja passado pelo campo <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType>.

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char str[4];
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t str[4]; // Could also be wchar_t[4] on Windows.
};
```

## <a name="customizing-decimal-field-marshaling"></a>Personalização do marshaling de campo decimal

Se você estiver trabalhando no Windows, poderá encontrar algumas APIs que usam a estrutura [ `CY` ou `CURRENCY` ](/windows/win32/api/wtypes/ns-wtypes-cy-r1) nativa. Por padrão, o `decimal` tipo .net realiza marshaling na [`DECIMAL`](/windows/win32/api/wtypes/ns-wtypes-decimal-r1) estrutura nativa. No entanto, você pode usar um <xref:System.Runtime.InteropServices.MarshalAsAttribute> com o valor <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=nameWithType> para instruir o marshaler para converter um valor `decimal` para um valor nativo `CY`.

```csharp
public struct Currency
{
    [MarshalAs(UnmanagedType.Currency)]
    public decimal dec;
}
```

```cpp
struct Currency
{
    CY dec;
};
```

## <a name="marshal-systemobject"></a>Efetua`System.Object`

No Windows, você pode realizar marshal dos campos tipados de `object` para o código nativo. Você pode realizar marshal desses campos para um dos três tipos:

- [`VARIANT`](/windows/win32/api/oaidl/ns-oaidl-variant)
- [`IUnknown*`](/windows/desktop/api/unknwn/nn-unknwn-iunknown)
- [`IDispatch*`](/windows/desktop/api/oaidl/nn-oaidl-idispatch)

Por padrão, um campo tipado de `object` terá o marshaling realizado para um `IUnknown*` que encapsula o objeto.

```csharp
public struct ObjectDefault
{
    public object obj;
}
```

```cpp
struct ObjectDefault
{
    IUnknown* obj;
};
```

Se você quiser realizar marshal de um campo de objeto para um `IDispatch*`, adicione um <xref:System.Runtime.InteropServices.MarshalAsAttribute> com o valor <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType>.

```csharp
public struct ObjectDispatch
{
    [MarshalAs(UnmanagedType.IDispatch)]
    public object obj;
}
```

```cpp
struct ObjectDispatch
{
    IDispatch* obj;
};
```

Se você quiser realizar marshal dele como um `VARIANT`, adicione um <xref:System.Runtime.InteropServices.MarshalAsAttribute> com o valor <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType>.

```csharp
public struct ObjectVariant
{
    [MarshalAs(UnmanagedType.Struct)]
    public object obj;
}
```

```cpp
struct ObjectVariant
{
    VARIANT obj;
};
```

A tabela a seguir descreve como diferentes tipos de runtime do campo `obj` são mapeados para os vários tipos armazenados em um `VARIANT`:

| Tipo .NET | Tipo de VARIANTE | | Tipo .NET | Tipo de VARIANTE |
|------------|--------------|-|----------|--------------|
|  `byte`  | `VT_UI1` |     | `System.Runtime.InteropServices.BStrWrapper` | `VT_BSTR` |
| `sbyte`  | `VT_I1`  |     | `object`  | `VT_DISPATCH` |
| `short`  | `VT_I2`  |     | `System.Runtime.InteropServices.UnknownWrapper` | `VT_UNKNOWN` |
| `ushort` | `VT_UI2` |     | `System.Runtime.InteropServices.DispatchWrapper` | `VT_DISPATCH` |
| `int`    | `VT_I4`  |     | `System.Reflection.Missing` | `VT_ERROR` |
| `uint`   | `VT_UI4` |     | `(object)null` | `VT_EMPTY` |
| `long`   | `VT_I8`  |     | `bool` | `VT_BOOL` |
| `ulong`  | `VT_UI8` |     | `System.DateTime` | `VT_DATE` |
| `float`  | `VT_R4`  |     | `decimal` | `VT_DECIMAL` |
| `double` | `VT_R8`  |     | `System.Runtime.InteropServices.CurrencyWrapper` | `VT_CURRENCY` |
| `char`   | `VT_UI2` |     | `System.DBNull` | `VT_NULL` |
| `string` | `VT_BSTR`|
