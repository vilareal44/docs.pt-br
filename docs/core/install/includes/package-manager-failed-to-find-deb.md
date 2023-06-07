---
ms.openlocfilehash: 7d398df060c031ae891218b82a2712d74f4c33b7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602981"
---

Se você receber uma mensagem de erro semelhante a **não é possível localizar o pacote {NetCore-Package}**, execute os comandos a seguir.

Há dois espaços reservados no seguinte conjunto de comandos.

- `{dotnet-package}`\
Isso representa o pacote .NET Core que você está instalando, como `aspnetcore-runtime-3.1` . Isso é usado no `sudo apt-get install` comando a seguir.

- `{os-version}`\
Isso representa a versão do Linux em que você está. Isso é usado no `wget` comando a seguir.

Tente limpar a lista de pacotes:

```bash
sudo dpkg --purge packages-microsoft-prod && sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install {dotnet-package}
```

Se isso não funcionar, você poderá executar uma instalação manual com os seguintes comandos:
