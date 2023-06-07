---
title: Exemplo de tabela de UriTemplate
ms.date: 03/30/2017
ms.assetid: 5dd1d38f-1989-4c64-820d-821f5a02216a
ms.openlocfilehash: ff88bdfe0c8c32da6f07f2b22de54af437376c51
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596429"
---
# <a name="uritemplate-table-sample"></a>Exemplo de tabela de UriTemplate
A <xref:System.UriTemplateTable> classe fornece uma estrutura de tabela associativa semelhante a um dicionário para trabalhar com um conjunto de `UriTemplate` instâncias. Identificadores de recursos uniformes (URIs) específicos podem ser correspondidos com eficiência em todos os modelos na tabela, e os dados associados ao modelo correspondente podem ser recuperados.  
  
 Este exemplo demonstra os seguintes conceitos principais relacionados à `UriTemplateTable` classe:  
  
- Sintaxe para instanciar um `UriTemplateTable` .  
  
- Populando um `UriTemplateTable` com um conjunto de pares chave/valor.  
  
- Correspondendo um URI candidato à tabela usando <xref:System.UriTemplateTable.MatchSingle%2A> .  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
2. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Web\UriTemplateTable`  
  
## <a name="see-also"></a>Consulte também

- [Dispatcher de tabela de UriTemplate](uritemplate-table-dispatcher-sample.md)
- [UriTemplate](uritemplate-sample.md)
