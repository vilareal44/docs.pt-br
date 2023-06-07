---
title: Marshaling padrão para cadeias de caracteres
description: Examine o comportamento de marshaling padrão para cadeias de caracteres em interfaces, invocação de plataforma, estruturas, & buffers de cadeia de comprimento fixo no .NET.
ms.date: 03/20/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- strings, interop marshaling
- interop marshaling, strings
ms.assetid: 9baea3ce-27b3-4b4f-af98-9ad0f9467e6f
ms.openlocfilehash: 440a49730f351b820cd68a741e79f94434f585c8
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904111"
---
# <a name="default-marshaling-for-strings"></a>Marshaling padrão para cadeias de caracteres

As classes <xref:System.String?displayProperty=nameWithType> e <xref:System.Text.StringBuilder?displayProperty=nameWithType> têm comportamentos de marshaling semelhantes.

As cadeias de caracteres têm o marshaling realizado como um tipo `BSTR` de estilo COM ou como uma cadeia de caracteres terminada em nulo (uma matriz de caracteres que termina com um caractere nulo). Os caracteres na cadeia de caracteres podem ter o marshaling realizado como Unicode (o padrão em sistemas Windows) ou ANSI.

## <a name="strings-used-in-interfaces"></a>Cadeias de caracteres usadas em interfaces

A tabela a seguir mostra as opções de marshaling para o tipo de dados String ao realizar marshaling dele como um argumento de método para um código não gerenciado. O atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> fornece vários valores de enumeração <xref:System.Runtime.InteropServices.UnmanagedType> para realizar marshaling de cadeias de caracteres para interfaces COM.

|Tipo de enumeração|Descrição do formato não gerenciado|
|----------------------|-------------------------------------|
|`UnmanagedType.BStr` (padrão)|Um `BSTR` de estilo COM com um tamanho prefixado e caracteres Unicode.|
|`UnmanagedType.LPStr`|Um ponteiro para uma matriz de caracteres ANSI terminada em nulo.|
|`UnmanagedType.LPWStr`|Um ponteiro para uma matriz de caracteres Unicode terminada em nulo.|

Esta tabela aplica-se a <xref:System.String>. Para <xref:System.Text.StringBuilder>, as únicas opções permitidas são `UnmanagedType.LPStr` e `UnmanagedType.LPWStr`.

O exemplo a seguir mostra as cadeias de caracteres declaradas na interface `IStringWorker`.

```csharp
public interface IStringWorker
{
    void PassString1(string s);
    void PassString2([MarshalAs(UnmanagedType.BStr)] string s);
    void PassString3([MarshalAs(UnmanagedType.LPStr)] string s);
    void PassString4([MarshalAs(UnmanagedType.LPWStr)] string s);
    void PassStringRef1(ref string s);
    void PassStringRef2([MarshalAs(UnmanagedType.BStr)] ref string s);
    void PassStringRef3([MarshalAs(UnmanagedType.LPStr)] ref string s);
    void PassStringRef4([MarshalAs(UnmanagedType.LPWStr)] ref string s);
}
```

```vb
Public Interface IStringWorker
    Sub PassString1(s As String)
    Sub PassString2(<MarshalAs(UnmanagedType.BStr)> s As String)
    Sub PassString3(<MarshalAs(UnmanagedType.LPStr)> s As String)
    Sub PassString4(<MarshalAs(UnmanagedType.LPWStr)> s As String)
    Sub PassStringRef1(ByRef s As String)
    Sub PassStringRef2(<MarshalAs(UnmanagedType.BStr)> ByRef s As String)
    Sub PassStringRef3(<MarshalAs(UnmanagedType.LPStr)> ByRef s As String)
    Sub PassStringRef4(<MarshalAs(UnmanagedType.LPWStr)> ByRef s As String)
End Interface
```

O exemplo a seguir mostra a interface correspondente descrita em uma biblioteca de tipos.

```cpp
interface IStringWorker : IDispatch
{
    HRESULT PassString1([in] BSTR s);
    HRESULT PassString2([in] BSTR s);
    HRESULT PassString3([in] LPStr s);
    HRESULT PassString4([in] LPWStr s);
    HRESULT PassStringRef1([in, out] BSTR *s);
    HRESULT PassStringRef2([in, out] BSTR *s);
    HRESULT PassStringRef3([in, out] LPStr *s);
    HRESULT PassStringRef4([in, out] LPWStr *s);
};
```

## <a name="strings-used-in-platform-invoke"></a>Cadeias de caracteres usadas na invocação de plataforma

A invocação de plataforma copia argumentos de cadeia de caracteres, convertendo-os do formato do .NET Framework (Unicode) para o formato não gerenciado da plataforma. As cadeias de caracteres são imutáveis e não são copiadas novamente da memória não gerenciada para a memória gerenciada quando a chamada é retornada.

A tabela a seguir lista as opções de marshaling para cadeias de caracteres ao realizar marshaling delas como um argumento de método de uma chamada de invocação de plataforma. O atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> fornece vários valores de enumeração <xref:System.Runtime.InteropServices.UnmanagedType> para realizar marshaling de cadeias de caracteres.

|Tipo de enumeração|Descrição do formato não gerenciado|
|----------------------|-------------------------------------|
|`UnmanagedType.AnsiBStr`|Um `BSTR` de estilo COM com um tamanho prefixado e caracteres ANSI.|
|`UnmanagedType.BStr`|Um `BSTR` de estilo COM com um tamanho prefixado e caracteres Unicode.|
|`UnmanagedType.LPStr` (padrão)|Um ponteiro para uma matriz de caracteres ANSI terminada em nulo.|
|`UnmanagedType.LPTStr`|Um ponteiro para uma matriz terminada em nulo de caracteres dependentes de plataforma.|
|`UnmanagedType.LPUTF8Str`|Um ponteiro para uma matriz terminada em nulo de caracteres codificados em UTF-8.|
|`UnmanagedType.LPWStr`|Um ponteiro para uma matriz de caracteres Unicode terminada em nulo.|
|`UnmanagedType.TBStr`|Um `BSTR` de estilo COM com um tamanho prefixado e caracteres dependentes de plataforma.|
|`VBByRefStr`|Um valor que permite que o Visual Basic .NET altere uma cadeia de caracteres no código não gerenciado, refletindo os resultados no código gerenciado. Há suporte para esse valor apenas na invocação de plataforma. Esse é o valor padrão no Visual Basic para cadeias de caracteres `ByVal`.|

Esta tabela aplica-se a <xref:System.String>. Para <xref:System.Text.StringBuilder>, as únicas opções permitidas são `LPStr`, `LPTStr` e `LPWStr`.

A definição de tipo a seguir mostra o uso correto de `MarshalAsAttribute` para chamadas de invocação de plataforma.

```csharp
class StringLibAPI
{
    [DllImport("StringLib.dll")]
    public static extern void PassLPStr([MarshalAs(UnmanagedType.LPStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPWStr([MarshalAs(UnmanagedType.LPWStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPTStr([MarshalAs(UnmanagedType.LPTStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassLPUTF8Str([MarshalAs(UnmanagedType.LPUTF8Str)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassBStr([MarshalAs(UnmanagedType.BStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassAnsiBStr([MarshalAs(UnmanagedType.AnsiBStr)] string s);
    [DllImport("StringLib.dll")]
    public static extern void PassTBStr([MarshalAs(UnmanagedType.TBStr)] string s);
}
```

```vb
Class StringLibAPI
    Public Declare Auto Sub PassLPStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPStr)> s As String)
    Public Declare Auto Sub PassLPWStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPWStr)> s As String)
    Public Declare Auto Sub PassLPTStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPTStr)> s As String)
    Public Declare Auto Sub PassLPUTF8Str Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.LPUTF8Str)> s As String)
    Public Declare Auto Sub PassBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.BStr)> s As String)
    Public Declare Auto Sub PassAnsiBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.AnsiBStr)> s As String)
    Public Declare Auto Sub PassTBStr Lib "StringLib.dll" (
        <MarshalAs(UnmanagedType.TBStr)> s As String)
End Class
```

## <a name="strings-used-in-structures"></a>Cadeias de caracteres usadas em estruturas

Cadeias de caracteres são membros válidos de estruturas; no entanto, buffers do <xref:System.Text.StringBuilder> são inválidos em estruturas. A tabela a seguir mostra as opções de marshaling para o <xref:System.String> quando o tipo de dados é empacotado como um campo. O atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> fornece vários valores de enumeração <xref:System.Runtime.InteropServices.UnmanagedType> para realizar marshaling de cadeias de caracteres para um campo.

|Tipo de enumeração|Descrição do formato não gerenciado|
|----------------------|-------------------------------------|
|`UnmanagedType.BStr`|Um `BSTR` de estilo COM com um tamanho prefixado e caracteres Unicode.|
|`UnmanagedType.LPStr` (padrão)|Um ponteiro para uma matriz de caracteres ANSI terminada em nulo.|
|`UnmanagedType.LPTStr`|Um ponteiro para uma matriz terminada em nulo de caracteres dependentes de plataforma.|
|`UnmanagedType.LPUTF8Str`|Um ponteiro para uma matriz terminada em nulo de caracteres codificados em UTF-8.|
|`UnmanagedType.LPWStr`|Um ponteiro para uma matriz de caracteres Unicode terminada em nulo.|
|`UnmanagedType.ByValTStr`|Uma matriz de comprimento fixo de caracteres; o tipo da matriz é determinado pelo conjunto de caracteres da estrutura que o contém.|

O tipo `ByValTStr` é usado para matrizes de caracteres embutidas e de comprimento fixo que aparecem em uma estrutura. Outros tipos aplicam as referências de cadeia de caracteres contidas em estruturas que contêm ponteiros para cadeias de caracteres.

O argumento `CharSet` do <xref:System.Runtime.InteropServices.StructLayoutAttribute> que é aplicado à estrutura recipiente determina o formato do caractere de cadeias de caracteres em estruturas. As estruturas de exemplo a seguir contêm referências de cadeia de caracteres e cadeias de caracteres embutidas, bem como caracteres ANSI, Unicode e dependentes de plataforma. A representação dessas estruturas em uma biblioteca de tipos é mostrada no código C++ a seguir:

```cpp
struct StringInfoA
{
    char *  f1;
    char    f2[256];
};

struct StringInfoW
{
    WCHAR * f1;
    WCHAR   f2[256];
    BSTR    f3;
};

struct StringInfoT
{
    TCHAR * f1;
    TCHAR   f2[256];
};
```

O exemplo a seguir mostra como usar o <xref:System.Runtime.InteropServices.MarshalAsAttribute> para definir a mesma estrutura em formatos diferentes.

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
struct StringInfoA
{
    [MarshalAs(UnmanagedType.LPStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
}

[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
struct StringInfoW
{
    [MarshalAs(UnmanagedType.LPWStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
    [MarshalAs(UnmanagedType.BStr)] public string f3;
}

[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Auto)]
struct StringInfoT
{
    [MarshalAs(UnmanagedType.LPTStr)] public string f1;
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 256)] public string f2;
}
```

```vb
<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Ansi)> _
Structure StringInfoA
    <MarshalAs(UnmanagedType.LPStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
End Structure

<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Unicode)> _
Structure StringInfoW
    <MarshalAs(UnmanagedType.LPWStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
<MarshalAs(UnmanagedType.BStr)> Public f3 As String
End Structure

<StructLayout(LayoutKind.Sequential, CharSet := CharSet.Auto)> _
Structure StringInfoT
    <MarshalAs(UnmanagedType.LPTStr)> Public f1 As String
    <MarshalAs(UnmanagedType.ByValTStr, SizeConst := 256)> _
    Public f2 As String
End Structure
```

## <a name="fixed-length-string-buffers"></a>Buffers de cadeia de caracteres de comprimento fixo

Em algumas circunstâncias, um buffer de caracteres de comprimento fixo deve ser passado para um código não gerenciado para ser manipulado. Apenas passar uma cadeia de caracteres não funciona neste caso, porque o receptor não pode modificar o conteúdo do buffer passado. Mesmo se a cadeia de caracteres for passada por referência, não haverá nenhuma maneira de inicializar o buffer em determinado tamanho.

A solução é passar um buffer <xref:System.Text.StringBuilder> como o argumento em vez de um <xref:System.String>. Um `StringBuilder` pode ser desreferenciado e modificado pelo receptor, desde que ele não exceda a capacidade do `StringBuilder`. Ele também pode ser inicializado com um comprimento fixo. Por exemplo, se você inicializar um buffer do `StringBuilder` em uma capacidade de `N`, o marshaler fornecerá um buffer com o tamanho de (`N`+ 1) caracteres. O + 1 leva em conta o fato de que a cadeia de caracteres não gerenciada tem um terminador nulo, ao contrário de `StringBuilder`.

Por exemplo, a função de API do Windows [`GetWindowText`](/windows/desktop/api/winuser/nf-winuser-getwindowtextw) (definida em *WinUser. h*) requer que o chamador passe um buffer de caracteres de comprimento fixo para o qual a função grava o texto da janela. `LpString` aponta para um buffer alocado pelo chamador com o tamanho `nMaxCount`. O chamador deve alocar o buffer e definir o argumento `nMaxCount` com o tamanho do buffer alocado. O exemplo a seguir mostra a declaração de função `GetWindowText`, conforme definida em *winuser.h*.

```cpp
int GetWindowText(
    HWND hWnd,        // Handle to window or control.
    LPTStr lpString,  // Text buffer.
    int nMaxCount     // Maximum number of characters to copy.
);
```

Um `StringBuilder` pode ser desreferenciado e modificado pelo receptor, desde que ele não exceda a capacidade do `StringBuilder`. O exemplo de código a seguir demonstra como `StringBuilder` pode ser inicializado com um comprimento fixo.

```csharp
using System;
using System.Runtime.InteropServices;
using System.Text;

internal static class NativeMethods
{
    [DllImport("User32.dll")]
    internal static extern void GetWindowText(IntPtr hWnd, StringBuilder lpString, int nMaxCount);
}

public class Window
{
    internal IntPtr h;        // Internal handle to Window.
    public String GetText()
    {
        StringBuilder sb = new StringBuilder(256);
        NativeMethods.GetWindowText(h, sb, sb.Capacity + 1);
        return sb.ToString();
    }
}
```

```vb
Imports System.Text

Friend Class NativeMethods
    Friend Declare Auto Sub GetWindowText Lib "User32.dll" _
        (hWnd As IntPtr, lpString As StringBuilder, nMaxCount As Integer)
End Class

Public Class Window
    Friend h As IntPtr ' Friend handle to Window.
    Public Function GetText() As String
        Dim sb As New StringBuilder(256)
        NativeMethods.GetWindowText(h, sb, sb.Capacity + 1)
        Return sb.ToString()
   End Function
End Class
```

## <a name="see-also"></a>Veja também

- [Comportamento de marshaling padrão](default-marshaling-behavior.md)
- [Realizando marshaling de cadeias de caracteres](marshaling-strings.md)
- [Tipos blittable e não blittable](blittable-and-non-blittable-types.md)
- [Atributos direcionais](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100))
- [Copiando e fixando](copying-and-pinning.md)
