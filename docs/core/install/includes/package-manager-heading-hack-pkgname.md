---
ms.openlocfilehash: 7723892a33bf7dd8e475b2f696db5d9ab287e182
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602988"
---

Os pacotes adicionados aos feeds do Gerenciador de pacotes são nomeados em um formato hackable: `{product}-{type}-{version}` .

- **remessa**\
O tipo de produto .NET a ser instalado. As opções válidas são:

  - dotnet
  - aspnetcore

- **Escreva**\
Escolhe o SDK ou o tempo de execução. As opções válidas são:

  - sdk
  - runtime

- **Versão**\
A versão do SDK ou do tempo de execução a ser instalado. Este artigo sempre fornecerá as instruções para a versão mais recente com suporte. As opções válidas são qualquer versão lançada, como:

  - 3.1
  - 3.0
  - 2.1

  É possível que o SDK/tempo de execução que você está tentando baixar não esteja disponível para sua distribuição do Linux. Para obter uma lista de distribuições com suporte, consulte [dependências e requisitos do .NET Core](../linux.md).

### <a name="examples"></a>Exemplos

- Instale o tempo de execução do ASP.NET Core 3,1:`aspnetcore-runtime-3.1`
- Instale o tempo de execução do .NET Core 2,1:`dotnet-runtime-2.1`
- Instale o SDK do .NET Core 3,1:`dotnet-sdk-3.1`

### <a name="package-missing"></a>Pacote ausente

Se a combinação de versão do pacote não funcionar, ela não estará disponível. Por exemplo, não há um SDK ASP.NET Core, os componentes do SDK estão incluídos no SDK do .NET Core. O valor `aspnetcore-sdk-2.2` está incorreto e deve ser `dotnet-sdk-2.2` . Para obter uma lista de distribuições do Linux com suporte no .NET Core, confira [dependências e requisitos do .NET Core](../linux.md).
