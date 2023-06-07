---
ms.openlocfilehash: 3d8179c5c0e84f8ff1197cce7790c80a5f5a4f6d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619437"
---

Quando você instala o com um Gerenciador de pacotes, essas bibliotecas são instaladas para você. Mas, se você instalar manualmente o .NET Core ou publicar um aplicativo independente, precisará verificar se essas bibliotecas estão instaladas:

- krb5-libs
- libicu
- openssl-libs

Se a versão do OpenSSL do ambiente de tempo de execução de destino for 1,1 ou mais recente, você precisará instalar o **compat-openssl10**.

Para obter mais informações sobre as dependências, consulte [aplicativos do Linux autocontidos](https://github.com/dotnet/core/blob/master/Documentation/self-contained-linux-apps.md).

Para aplicativos .NET Core que usam o assembly *System. Drawing. Common* , você também precisará da seguinte dependência:

- [libgdiplus (versão 6.0.1 ou posterior)](https://www.mono-project.com/docs/gui/libgdiplus/)

  > [!WARNING]
  > Você pode instalar uma versão recente do *libgdiplus* adicionando o repositório do mono ao seu sistema. Para obter mais informações, consulte <https://www.mono-project.com/download/stable/>.
