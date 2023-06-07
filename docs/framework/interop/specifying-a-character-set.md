---
title: Especificando um conjunto de caracteres
description: Saiba como especificar um conjunto de caracteres que usa codificação estreita (ANSI) ou larga (Unicode). Alternativamente, você pode especificar a seleção automática de tempo de execução.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- platform invoke, attribute fields
- attribute fields in platform invoke, CharSet
- CharSet field
ms.assetid: a8347eb1-295f-46b9-8a78-63331f9ecc50
ms.openlocfilehash: 789753742d8714e481f038e323407cbab0499f6c
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309788"
---
# <a name="specify-a-character-set"></a>Especificar um conjunto de caracteres

O campo <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet?displayProperty=nameWithType> controla o marshaling de cadeia de caracteres e determina como a invocação de plataforma localiza os nomes de função em uma DLL. Este tópico descreve os dois comportamentos.  
  
 Algumas APIs exportam duas versões de funções que usam argumentos de cadeia de caracteres: estreita (ANSI) e larga (Unicode). A API do Windows, por exemplo, inclui os seguintes nomes do ponto de entrada para a função **MessageBox**:  
  
- **MessageBoxA**  
  
     Fornece a formatação ANSI de caracteres de 1 byte, diferenciada por um “A” acrescentado ao nome do ponto de entrada. Chamadas para **MessageBoxA** sempre realizam marshaling das cadeias de caracteres no formato ANSI.  
  
- **MessageBoxW**  
  
     Fornece a formatação Unicode de caracteres de 2 bytes, diferenciada por um “W” acrescentado ao nome do ponto de entrada. Chamadas para **MessageBoxW** sempre realizam marshaling das cadeias de caracteres no formato Unicode.  
  
## <a name="string-marshaling-and-name-matching"></a>Marshaling de cadeia de caracteres e correspondência de nomes  
 O campo `CharSet` aceita os seguintes valores:  
  
 <xref:System.Runtime.InteropServices.CharSet.Ansi> (valor padrão)  
  
- Marshaling de cadeia de caracteres  
  
     A invocação de plataforma realiza marshaling das cadeias de caracteres de seu formato gerenciado (Unicode) para o formato ANSI.  
  
- Correspondência de nomes  
  
     Quando o campo <xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling?displayProperty=nameWithType> é `true`, como é o padrão no Visual Basic, a invocação de plataforma procura somente o nome especificado. Por exemplo, se você especificar **MessageBox**, a invocação de plataforma procurará **MessageBox** e falhará quando não conseguir localizar a ortografia exata.  
  
     Quando o campo `ExactSpelling` é `false`, que é o padrão no C++ e no C#, a invocação de plataforma procura o alias não danificado primeiro (**MessageBox**) e, em seguida, o nome danificado (**MessageBoxA**), se o alias não danificado não é encontrado. Observe que o comportamento de correspondência de nomes do ANSI é diferente do comportamento de correspondência de nomes do Unicode.  
  
 <xref:System.Runtime.InteropServices.CharSet.Unicode>  
  
- Marshaling de cadeia de caracteres  
  
     A invocação de plataforma copia as cadeias de caracteres de seu formato gerenciado (Unicode) para o formato Unicode.  
  
- Correspondência de nomes  
  
     Quando o campo `ExactSpelling` é `true`, como é o padrão no Visual Basic, a invocação de plataforma procura somente o nome especificado. Por exemplo, se você especificar **MessageBox**, a invocação de plataforma procurará **MessageBox** e falhará se não conseguir localizar a ortografia exata.  
  
     Quando o campo `ExactSpelling` é `false`, que é o padrão no C++ e no C#, a invocação de plataforma procura o nome danificado primeiro (**MessageBoxW**) e, em seguida, o alias não danificado (**MessageBox**), se o nome danificado não é encontrado. Observe que o comportamento de correspondência de nomes do Unicode é diferente do comportamento de correspondência de nomes do ANSI.  
  
 <xref:System.Runtime.InteropServices.CharSet.Auto>  
  
- A invocação de plataforma escolhe entre os formatos ANSI e Unicode em tempo de execução, com base na plataforma de destino.  
  
## <a name="specify-a-character-set-in-visual-basic"></a>Especifique um conjunto de caracteres em Visual Basic

Você pode especificar o comportamento do conjunto de caracteres no Visual Basic adicionando `Ansi` a `Unicode` `Auto` palavra-chave, ou à instrução de declaração. Se você omitir a palavra-chave de conjunto de caracteres, o <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet?displayProperty=nameWithType> campo usa como padrão o conjunto de caracteres ANSI.

O exemplo a seguir declara a função **MessageBox** três vezes, cada vez com um comportamento diferente do conjunto de caracteres. A primeira instrução omite a palavra-chave de conjunto de caracteres, de modo que o conjunto de caracteres usa ANSI como padrão. A segunda e terceira instruções especificam explicitamente um conjunto de caracteres com uma palavra-chave.

```vb
Friend Class NativeMethods
    Friend Declare Function MessageBoxA Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer

    Friend Declare Unicode Function MessageBoxW Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer

    Friend Declare Auto Function MessageBox Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer
End Class
```
  
## <a name="specify-a-character-set-in-c-and-c"></a>Especificar um conjunto de caracteres em C# e C++

O campo <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet?displayProperty=nameWithType> identifica o conjunto de caracteres subjacente como ANSI ou Unicode. O conjunto de caracteres controla como deve ser realizado o marshaling dos argumentos de cadeia de caracteres para um método. Use um dos seguintes formatos para indicar o conjunto de caracteres:  
  
```csharp
[DllImport("DllName", CharSet = CharSet.Ansi)]
[DllImport("DllName", CharSet = CharSet.Unicode)]
[DllImport("DllName", CharSet = CharSet.Auto)]
```
  
```cpp
[DllImport("DllName", CharSet = CharSet::Ansi)]
[DllImport("DllName", CharSet = CharSet::Unicode)]
[DllImport("DllName", CharSet = CharSet::Auto)]
```
  
 O exemplo a seguir mostra três definições gerenciadas da função **MessageBox** atribuída para especificar um conjunto de caracteres. Na primeira definição, por sua omissão, o campo `CharSet` usa como padrão o conjunto de caracteres ANSI.  
  
```csharp  
using System;
using System.Runtime.InteropServices;

internal static class NativeMethods
{
    [DllImport("user32.dll")]
    internal static extern int MessageBoxA(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);

    [DllImport("user32.dll", CharSet = CharSet.Unicode)]
    internal static extern int MessageBoxW(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);

    [DllImport("user32.dll", CharSet = CharSet.Auto)]
    internal static extern int MessageBox(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);
}
```  
  
```cpp
typedef void* HWND;

// Can use MessageBox or MessageBoxA.
[DllImport("user32")]
extern "C" int MessageBox(
    HWND hWnd, String* lpText, String* lpCaption, unsigned int uType);

// Can use MessageBox or MessageBoxW.
[DllImport("user32", CharSet = CharSet::Unicode)]
extern "C" int MessageBoxW(
    HWND hWnd, String* lpText, String* lpCaption, unsigned int uType);

// Must use MessageBox.
[DllImport("user32", CharSet = CharSet::Auto)]
extern "C" int MessageBox(
    HWND hWnd, String* lpText, String* lpCaption, unsigned int uType);
```
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.InteropServices.DllImportAttribute>
- [Criando protótipos em código gerenciado](creating-prototypes-in-managed-code.md)
- [Exemplos de invocação de plataforma](platform-invoke-examples.md)
- [Marshaling de dados com invocação de plataforma](marshaling-data-with-platform-invoke.md)
