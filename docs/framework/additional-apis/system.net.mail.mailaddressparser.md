---
title: Classe MailAddressParser (System.Net)
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.Mail.MailAddressParser
- System.Net.Mail.MailAddressParser.ParseAddress
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: ff83f6946539fa262ccde980052627f98c75601d
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051344"
---
# <a name="mailaddressparser-class"></a>Classe MailAddressParser

Analisa endereços de email conforme descrito em RFC 2822. Esta classe não pode ser herdada.

```csharp
internal static class MailAddressParser
```

> [!WARNING]
> Essa classe é interna e não deve ser usada diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="parseaddress-method"></a>Método ParseAddress

Analisa um único endereço de email da cadeia de caracteres especificada.

```csharp
internal static MailAddress ParseAddress(string data)
```

### <a name="parameters"></a>Parâmetros

`data` <xref:System.String>

A cadeia de caracteres que contém um endereço de email a ser analisado.

### <a name="return-value"></a>Retornar valor

<xref:System.Net.Mail.MailAddress>

Um endereço de email válido.

### <a name="exceptions"></a>Exceções

<xref:System.FormatException?displayProperty=nameWithType>

O endereço de email é inválido.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)
