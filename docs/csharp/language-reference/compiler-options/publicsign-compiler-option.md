---
title: -publicsign (opções do compilador C#)
ms.date: 05/15/2018
f1_keywords:
- /publicsign
helpviewer_keywords:
- -publicsign compiler option [C#]
- publicsign compiler option [C#]
- /publicsign compiler option [C#]
ms.openlocfilehash: 2655e0216a412053e052ab2ec2fcc8c68ea4f968
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88268043"
---
# <a name="-publicsign-c-compiler-options"></a>-publicsign (opções do compilador C#)

Essa opção faz com que o compilador aplique uma chave pública, mas, na verdade, não assina o assembly. A opção **-publicsign** também define um bit no assembly que informa o runtime que o arquivo realmente já está assinado.

## <a name="syntax"></a>Sintaxe

```console
-publicsign
```

## <a name="arguments"></a>Argumentos

Nenhum.

## <a name="remarks"></a>Comentários

A opção **-publicsign** requer o uso de [-keyfile](keyfile-compiler-option.md) ou [-keycontainer](keycontainer-compiler-option.md). As opções **keyfile** ou **keycontainer** especificam a chave pública.

As opções **-publicsign** e **-delaysign** são mutuamente exclusivas.

Às vezes chamada de "assinatura falsa" ou "assinatura de OSS", a assinatura pública inclui a chave pública em um assembly de saída e define o sinalizador "assinado", mas, na verdade, não assina o assembly com uma chave privada. Isso é útil para projetos de software livre em que as pessoas desejam criar assemblies compatíveis com os assemblies "totalmente assinados" lançados, mas não têm acesso à chave privada usada para assinar os assemblies. Como quase nenhum consumidor precisa verificar realmente se o assembly está totalmente assinado, esses assemblies criados publicamente podem ser usados em quase todos os cenários nos quais o assembly totalmente assinado seria usado.

### <a name="to-set-this-compiler-option-in-a-csproj-file"></a>Para definir essa opção de compilador em um arquivo csproj

Abra o arquivo. csproj de um projeto e adicione o seguinte elemento:

```xml
<PublicSign>true</PublicSign>
```

## <a name="see-also"></a>Confira também

- [Opção -delaysign do compilador C#](delaysign-compiler-option.md)
- [Opção -keyfile do compilador C#](keyfile-compiler-option.md)
- [Opção -keycontainer do compilador C#](keycontainer-compiler-option.md)
- [Opções do compilador C#](index.md)
- [Gerenciando propriedades de solução e de projeto](/visualstudio/ide/managing-project-and-solution-properties)
