---
title: Adotando o Windows Communication Foundation
ms.date: 03/30/2017
ms.assetid: 49ba71e2-9468-4082-84c5-cf8daf95e34a
ms.openlocfilehash: a31bd5382e67565bd54272c5c7f400eacd5297d6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576532"
---
# <a name="adopt-windows-communication-foundation"></a>Adote Windows Communication Foundation

Você pode optar por usar o Windows Communication Foundation (WCF) para um novo desenvolvimento enquanto continua a manter os aplicativos existentes desenvolvidos usando o ASP.NET. Como o WCF deve ser a opção mais adequada para facilitar a comunicação com aplicativos criados com o .NET Framework em qualquer cenário, ele pode servir como uma ferramenta padrão para resolver uma ampla variedade de problemas de comunicação de software de uma maneira que o ASP.NET não pode.

Novos aplicativos WCF podem ser implantados nos mesmos computadores que os serviços Web ASP.NET existentes. Se os serviços Web do ASP.NET existentes usarem uma versão do .NET Framework anterior à versão 2,0, você poderá usar a ferramenta de registro do IIS ASP.NET para implantar seletivamente o .NET Framework 2,0 em aplicativos do IIS nos quais novos aplicativos WCF devem ser hospedados. Essa ferramenta está documentada em [ASP.net IIS Registration Tool (Aspnet_regiis. exe)](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90))e tem uma interface do usuário incorporada ao console de gerenciamento do IIS 6,0.

O WCF pode ser usado para adicionar novos recursos aos serviços Web ASP.NET existentes adicionando os serviços WCF configurados para serem executados no modo de compatibilidade ASP.NET para aplicativos de serviço Web ASP.NET existentes no IIS. Devido ao modo de compatibilidade do ASP.NET, o código para os novos serviços WCF pode acessar e atualizar as mesmas informações de estado do aplicativo que o código ASP.NET pré-existente, usando a <xref:System.Web.HttpContext> classe. Os aplicativos também podem compartilhar as mesmas bibliotecas de classes.

Os clientes WCF podem usar os serviços Web do ASP.NET. Os serviços WCF que são configurados com o <xref:System.ServiceModel.BasicHttpBinding> podem ser usados por clientes do serviço Web ASP.net. Os serviços Web do ASP.NET podem coexistir com os aplicativos WCF, e o WCF pode até ser usado para adicionar recursos aos serviços Web existentes do ASP.NET. Considerando todas essas maneiras em que os serviços Web do WCF e do ASP.NET podem ser usados juntos, talvez você queira migrar os serviços Web do ASP.NET para o WCF somente se precisar de recursos fornecidos pelo WCF e não pelo ASP.NET Web Services.

Mesmo nos poucos casos em que é necessário, a migração de código de uma tecnologia para outra raramente é a abordagem correta. O motivo para adotar a nova tecnologia é atender a novos requisitos que não podem ser atendidos com a tecnologia anterior e, nesse caso, a coisa certa a fazer é criar uma nova solução para atender ao conjunto de requisitos recentemente expandido. O novo design beneficia-se da sua experiência com o sistema existente e da sabedoria obtida desde que o sistema foi projetado. O novo design também pode usar os recursos completos das novas tecnologias em vez de reproduzir o design antigo na nova plataforma. Após a criação de protótipos dos principais elementos do novo design, fica mais fácil reutilizar o código do sistema existente dentro do novo.

## <a name="see-also"></a>Consulte também

- [Como recuperar metadados e implementar um serviço em conformidade](how-to-retrieve-metadata-and-implement-a-compliant-service.md)
