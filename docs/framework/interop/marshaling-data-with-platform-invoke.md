---
title: Marshaling de dados com invocação de plataforma
description: Realize marshaling de dados com a invocação de plataforma no .NET. Veja uma lista de tipos de dados usados em APIs do Windows e funções em estilo C e localize seus equivalentes de tipo gerenciado do .NET.
ms.date: 03/20/2019
dev_langs:
- cpp
helpviewer_keywords:
- platform invoke, marshaling data
- data marshaling, platform invoke
- marshaling, platform invoke
ms.assetid: dc5c76cf-7b12-406f-b79c-d1a023ec245d
ms.openlocfilehash: 27045790780c1eef9537bdf7e52deb579e2b467c
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904098"
---
# <a name="marshaling-data-with-platform-invoke"></a>Marshaling de dados com invocação de plataforma

Para chamar funções exportadas de uma biblioteca não gerenciada, um aplicativo do .NET Framework requer um protótipo de função em código gerenciado que representa a função não gerenciada. Para criar um protótipo que habilita a invocação de plataforma a realizar marshaling de dados corretamente, você deve fazer o seguinte:

- Aplique o atributo <xref:System.Runtime.InteropServices.DllImportAttribute> à função estática ou método em código gerenciado.

- Substitua os tipos de dados gerenciados por tipos de dados não gerenciados.

Você pode usar a documentação fornecida com uma função não gerenciada para construir um protótipo gerenciado equivalente, aplicando o atributo com os respectivos campos opcionais e substituindo os tipos de dados gerenciados por tipos não gerenciados. Para obter instruções sobre como aplicar o <xref:System.Runtime.InteropServices.DllImportAttribute>, confira [Consumindo funções de DLL não gerenciadas](consuming-unmanaged-dll-functions.md).

Esta seção fornece amostras que demonstram como criar protótipos de função gerenciada para passar argumentos para e receber valores retornados de funções exportadas por bibliotecas não gerenciadas. As amostras também demonstram quando usar o atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> e a classe <xref:System.Runtime.InteropServices.Marshal> para realizar marshaling de dados explicitamente.

## <a name="platform-invoke-data-types"></a>Tipos de dados de invocação de plataforma

A tabela a seguir lista os tipos de dados usados nas APIs do Windows e nas funções de estilo C. Muitas bibliotecas não gerenciadas contêm funções que passam esses tipos de dados como parâmetros e valores retornados. A terceira coluna lista a classe ou tipo de valor interno correspondente do .NET Framework que você usa em código gerenciado. Em alguns casos, você pode substituir um tipo do mesmo tamanho pelo tipo listado na tabela.

|Tipo não gerenciado nas APIs do Windows|Tipo de linguagem C não gerenciado|Tipo gerenciado|Description|
|--------------------------------|-------------------------------|------------------------|-----------------|
|`VOID`|`void`|<xref:System.Void?displayProperty=nameWithType>|Aplicado a uma função que não retorna um valor.|
|`HANDLE`|`void *`|<xref:System.IntPtr?displayProperty=nameWithType> ou <xref:System.UIntPtr?displayProperty=nameWithType>|32 bits em Sistemas Operacionais Windows 32 bits, 64 bits em Sistemas Operacionais Windows 64 bits.|
|`BYTE`|`unsigned char`|<xref:System.Byte?displayProperty=nameWithType>|8 bits|
|`SHORT`|`short`|<xref:System.Int16?displayProperty=nameWithType>|16 bits|
|`WORD`|`unsigned short`|<xref:System.UInt16?displayProperty=nameWithType>|16 bits|
|`INT`|`int`|<xref:System.Int32?displayProperty=nameWithType>|32 bits|
|`UINT`|`unsigned int`|<xref:System.UInt32?displayProperty=nameWithType>|32 bits|
|`LONG`|`long`|<xref:System.Int32?displayProperty=nameWithType>|32 bits|
|`BOOL`|`long`|<xref:System.Boolean?displayProperty=nameWithType> ou <xref:System.Int32?displayProperty=nameWithType>|32 bits|
|`DWORD`|`unsigned long`|<xref:System.UInt32?displayProperty=nameWithType>|32 bits|
|`ULONG`|`unsigned long`|<xref:System.UInt32?displayProperty=nameWithType>|32 bits|
|`CHAR`|`char`|<xref:System.Char?displayProperty=nameWithType>|Decore com ANSI.|
|`WCHAR`|`wchar_t`|<xref:System.Char?displayProperty=nameWithType>|Decore com Unicode.|
|`LPSTR`|`char *`|<xref:System.String?displayProperty=nameWithType> ou <xref:System.Text.StringBuilder?displayProperty=nameWithType>|Decore com ANSI.|
|`LPCSTR`|`const char *`|<xref:System.String?displayProperty=nameWithType> ou <xref:System.Text.StringBuilder?displayProperty=nameWithType>|Decore com ANSI.|
|`LPWSTR`|`wchar_t *`|<xref:System.String?displayProperty=nameWithType> ou <xref:System.Text.StringBuilder?displayProperty=nameWithType>|Decore com Unicode.|
|`LPCWSTR`|`const wchar_t *`|<xref:System.String?displayProperty=nameWithType> ou <xref:System.Text.StringBuilder?displayProperty=nameWithType>|Decore com Unicode.|
|`FLOAT`|`float`|<xref:System.Single?displayProperty=nameWithType>|32 bits|
|`DOUBLE`|`double`|<xref:System.Double?displayProperty=nameWithType>|64 bits|

Para obter os tipos correspondentes em Visual Basic, C# e C++, confira a [Introdução à biblioteca de classes .NET Framework](../../standard/class-library-overview.md#system-namespace).

## <a name="pinvokelibdll"></a>PinvokeLib.dll

O código a seguir define as funções de biblioteca fornecidas por Pinvoke.dll. Muitos exemplos descritos nesta seção chamam essa biblioteca.

### <a name="example"></a>Exemplo

[!code-cpp[PInvokeLib#1](../../../samples/snippets/cpp/VS_Snippets_CLR/pinvokelib/cpp/pinvokelib.cpp#1)]

[!code-cpp[PInvokeLib#2](../../../samples/snippets/cpp/VS_Snippets_CLR/pinvokelib/cpp/pinvokelib.h#2)]
