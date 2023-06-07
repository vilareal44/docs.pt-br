---
title: Alterações significativas na biblioteca de classes base
description: Lista as alterações significativas nas principais bibliotecas do .NET.
ms.date: 07/27/2020
ms.openlocfilehash: 9190fc2fc8dddc4fb4be8409915cf24c92a97daf
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558162"
---
# <a name="core-net-libraries-breaking-changes"></a>Principais alterações significativas nas bibliotecas do .NET

As principais bibliotecas do .NET fornecem os primitivos e outros tipos gerais usados pelo .NET Core.

As seguintes alterações significativas estão documentadas nesta página:

| Alteração significativa | Versão introduzida |
| - | :-: |
| [Environment. OSVersion retorna a versão correta do sistema operacional](#environmentosversion-returns-the-correct-operating-system-version) | 5.0 |
| [Complexidade do LINQ OrderBy. primeiro {OrDefault} aumentou](#complexity-of-linq-orderbyfirstordefault-increased) | 5.0 |
| [IntPtr e UIntPtr implementam IFormattable](#intptr-and-uintptr-implement-iformattable) | 5.0 |
| [PrincipalPermissionattribute está obsoleto como erro](#principalpermissionattribute-is-obsolete-as-error) | 5.0 |
| [Os métodos de serialização BinaryFormatter são obsoletos e proibidos em aplicativos ASP.NET](#binaryformatter-serialization-methods-are-obsolete-and-prohibited-in-aspnet-apps) | 5.0 |
| [Os caminhos de código UTF-7 estão obsoletos](#utf-7-code-paths-are-obsolete) | 5.0 |
| [Vetor \<T> sempre gera NotSupportedException para tipos sem suporte](#vectort-always-throws-notsupportedexception-for-unsupported-types) | 5.0 |
| [O ActivityIdFormat padrão é W3C](#default-activityidformat-is-w3c) | 5.0 |
| [Alteração de comportamento para vector2. Lerp e Vector4. Lerp](#behavior-change-for-vector2lerp-and-vector4lerp) | 5.0 |
| [Os métodos SSE e SSE2 CompareGreaterThan manipulam corretamente as entradas NaN](#sse-and-sse2-comparegreaterthan-methods-properly-handle-nan-inputs) | 5.0 |
| [CounterSet. CreateCounterSetInstance agora gera InvalidOperationException se a instância já existir](#countersetcreatecountersetinstance-now-throws-invalidoperationexception-if-instance-already-exists) | 5.0 |
| [Pacote Microsoft. DotNet. PlatformAbstractions removido](#microsoftdotnetplatformabstractions-package-removed) | 5.0 |
| [As APIs que relatam versão agora relatam produto e não versão de arquivo](#apis-that-report-version-now-report-product-and-not-file-version) | 3.0 |
| [Instâncias EncoderFallbackBuffer personalizadas não podem retornar recursivamente](#custom-encoderfallbackbuffer-instances-cannot-fall-back-recursively) | 3.0 |
| [Alterações de comportamento de análise e formatação de ponto flutuante](#floating-point-formatting-and-parsing-behavior-changed) | 3.0 |
| [Operações de análise de ponto flutuante não falham mais ou geram uma estourexception](#floating-point-parsing-operations-no-longer-fail-or-throw-an-overflowexception) | 3.0 |
| [InvalidAsynchronousStateException movido para outro assembly](#invalidasynchronousstateexception-moved-to-another-assembly) | 3.0 |
| [A substituição de sequências de bytes UTF-8 mal formados segue as diretrizes de Unicode](#replacing-ill-formed-utf-8-byte-sequences-follows-unicode-guidelines) | 3.0 |
| [TypeDescriptionProviderAttribute movido para outro assembly](#typedescriptionproviderattribute-moved-to-another-assembly) | 3.0 |
| [O ZipArchiveEntry não lida mais com os arquivos mortos com tamanhos de entrada inconsistentes](#ziparchiveentry-no-longer-handles-archives-with-inconsistent-entry-sizes) | 3.0 |
| [FieldInfo. SetValue gera uma exceção para campos estáticos somente de inicialização](#fieldinfosetvalue-throws-exception-for-static-init-only-fields) | 3.0 |
| [Campos privados adicionados aos tipos de struct internos](#private-fields-added-to-built-in-struct-types) | 2.1 |
| [Alteração no valor padrão de UseShellExecute](#change-in-default-value-of-useshellexecute) | 2.1 |
| [Versões do OpenSSL no macOS](#openssl-versions-on-macos) | 2.1 |
| [UnauthorizedAccessException gerado por FileSystemInfo. Attributes](#unauthorizedaccessexception-thrown-by-filesysteminfoattributes) | 1.0 |
| [Tratamento de exceções de estado de processo corrompido não é suportado](#handling-corrupted-state-exceptions-is-not-supported) | 1.0 |
| [As propriedades UriBuilder não precedem mais os caracteres à esquerda](#uribuilder-properties-no-longer-prepend-leading-characters) | 1.0 |
| [Process. StartInfo gera InvalidOperationException para processos que você não iniciou](#processstartinfo-throws-invalidoperationexception-for-processes-you-didnt-start) | 1.0 |

## <a name="net-50"></a>.NET 5,0

[!INCLUDE [environment-osversion-returns-correct-version](../../../includes/core-changes/corefx/5.0/environment-osversion-returns-correct-version.md)]

***

[!INCLUDE [orderby-firstordefault-complexity-increase](../../../includes/core-changes/corefx/5.0/orderby-firstordefault-complexity-increase.md)]

***

[!INCLUDE [intptr-uintptr-implement-iformattable](../../../includes/core-changes/corefx/5.0/intptr-uintptr-implement-iformattable.md)]

***

[!INCLUDE [principalpermissionattribute-obsolete](../../../includes/core-changes/corefx/5.0/principalpermissionattribute-obsolete.md)]

***

[!INCLUDE [binaryformatter-serialization-obsolete](../../../includes/core-changes/corefx/5.0/binaryformatter-serialization-obsolete.md)]

***

[!INCLUDE [utf-7-code-paths-obsolete](../../../includes/core-changes/corefx/5.0/utf-7-code-paths-obsolete.md)]

***

[!INCLUDE [vectort-throws-notsupportedexception](../../../includes/core-changes/corefx/5.0/vectort-throws-notsupportedexception.md)]

***

[!INCLUDE [default-activityidformat-changed](../../../includes/core-changes/corefx/5.0/default-activityidformat-changed.md)]

***

[!INCLUDE [vector-lerp-behavior-change](../../../includes/core-changes/corefx/5.0/vector-lerp-behavior-change.md)]

***

[!INCLUDE [sse-comparegreaterthan-intrinsics](../../../includes/core-changes/corefx/5.0/sse-comparegreaterthan-intrinsics.md)]

***

[!INCLUDE [createcountersetinstance-throws-invalidoperation](../../../includes/core-changes/corefx/5.0/createcountersetinstance-throws-invalidoperation.md)]

***

[!INCLUDE [platformabstractions-package-removed](../../../includes/core-changes/corefx/5.0/platformabstractions-package-removed.md)]

***

## <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[APIs that report version now report product and not file version](~/includes/core-changes/corefx/3.0/version-information-changes.md)]

***

[!INCLUDE[Custom EncoderFallbackBuffer instances cannot fall back recursively](~/includes/core-changes/corefx/3.0/custom-encoderfallbackbuffer-cannot-be-recursive.md)]

***

[!INCLUDE[Floating point formatting and parsing behavior changes](~/includes/core-changes/corefx/3.0/floating-point-changes.md)]

***

[!INCLUDE[Floating-point parsing operations no longer fail or throw an OverflowException](~/includes/core-changes/corefx/3.0/floating-point-parsing-does-not-overflow.md)]

***

[!INCLUDE[InvalidAsynchronousStateException moved to another assembly](~/includes/core-changes/corefx/3.0/move-invalidasynchronousstateexception.md)]

***

[!INCLUDE[NET Core 3.0 follows Unicode best practices when replacing ill-formed UTF-8 byte sequences](~/includes/core-changes/corefx/3.0/net-core-3-0-follows-unicode-utf8-best-practices.md)]

***

[!INCLUDE[TypeDescriptionProviderAttribute moved to another assembly](~/includes/core-changes/corefx/3.0/move-typedescriptionproviderattribute.md)]

***

[!INCLUDE[ZipArchiveEntry no longer handles archives with inconsistent entry sizes](~/includes/core-changes/corefx/3.0/ziparchiveentry-and-inconsistent-entry-sizes.md)]

***

[!INCLUDE [FieldInfo.SetValue throws exception for static, init-only fields](~/includes/core-changes/corefx/3.0/fieldinfo-setvalue-exception.md)]

***

## <a name="net-core-21"></a>.NET Core 2.1

[!INCLUDE[Private fields added to built-in struct types](~/includes/core-changes/corefx/2.1/instantiate-struct.md)]

***

[!INCLUDE[Change in default value of UseShellExecute](~/includes/core-changes/corefx/2.1/process-start-changes.md)]

***

[!INCLUDE [OpenSSL versions on macOS](../../../includes/core-changes/corefx/openssl-dependencies-macos.md)]

***

## <a name="net-core-10"></a>.NET Core 1.0

[!INCLUDE [UnauthorizedAccessException thrown by FileSystemInfo.Attributes](~/includes/core-changes/corefx/1.0/filesysteminfo-attributes-exceptions.md)]

***

[!INCLUDE [corrupted-state-exceptions](~/includes/core-changes/corefx/1.0/corrupted-state-exceptions.md)]

***

[!INCLUDE [uribuilder-behavior-changes](../../../includes/core-changes/corefx/1.0/uribuilder-behavior-changes.md)]

***

[!INCLUDE [startinfo-throws-exception](../../../includes/core-changes/corefx/1.0/startinfo-throws-exception.md)]

***
