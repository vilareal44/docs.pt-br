---
title: Exemplos de distribuidor de tabela de UriTemplate
ms.date: 03/30/2017
ms.assetid: 3b32975d-ba90-4c5c-83bc-2fbb48f11c0c
ms.openlocfilehash: 97e916aaf9d137eb7931470f9565797b03620d28
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591066"
---
# <a name="uritemplate-table-dispatcher-sample"></a>Exemplos de distribuidor de tabela de UriTemplate
A <xref:System.UriTemplateTable> classe fornece uma estrutura de tabela associativa semelhante a um dicionário para trabalhar com um conjunto de <xref:System.UriTemplate> instâncias. Este exemplo demonstra um mecanismo de expedição básico criado usando `UriTemplateTable` o, um cenário de uso comum para a `UriTemplateTable` classe.  
  
 Este exemplo demonstra os seguintes conceitos principais para a `UriTemplateTable` classe:  
  
- Associando delegados a `UriTemplates` em um `UriTemplateTable` .  
  
- Usando <xref:System.UriTemplateTable.MatchSingle%2A> para obter o delegado de manipulador correto para um URI específico.  
  
- Invocar o delegado de manipulador para processar a solicitação.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
2. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Web\UriTemplateDispatcher`  
  
## <a name="see-also"></a>Confira também

- [Tabela de UriTemplate](uritemplate-table-sample.md)
- [UriTemplate](uritemplate-sample.md)
