---
title: Classe UnsafeNclNativeMethods (System.Net)
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.UnsafeNclNativeMethods
- System.Net.UnsafeNclNativeMethods.NativePKI
- System.Net.UnsafeNclNativeMethods.NativePKI.FindClientCertificates
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 46756a0d1d69b4768dbb8dcdd7ab098d3f1849bf
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990293"
---
# <a name="unsafenclnativemethods-class"></a>Classe UnsafeNclNativeMethods

Contém classes que importam métodos de rede nativas não seguros. Esta classe não pode ser herdada.

```csharp
internal static class UnsafeNclNativeMethods
```

> [!WARNING]
> Essa classe é interna e não deve ser usada diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="nativepki-class"></a>Classe NativePKI

Contém métodos importados do crypt32.dll. Esses métodos manipulam certificados ao usar o protocolo HTTPS (Hypertext Transfer Protocol Secure). Esta classe não pode ser herdada.

```csharp
internal static class NativePKI
```

## <a name="nativepkifindclientcertificates-method"></a>Método NativePKI. FindClientCertificates

Descobre os certificados de cliente disponíveis para enviar ao servidor.

```csharp
internal static X509CertificateCollection FindClientCertificates
```

### <a name="return-value"></a>Valor retornado

<xref:System.Security.Cryptography.X509Certificates.X509CertificateCollection?displayProperty=nameWithType>

Uma coleção de certificados de cliente disponíveis.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)
