---
title: Exemplo de serviço de AJAX utilizando tipos complexos
ms.date: 03/30/2017
ms.assetid: 88242b99-4811-4cbe-8201-52ddf48fb174
ms.openlocfilehash: 4227446e8844accd06490d8e7cf933da43d875a6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575908"
---
# <a name="ajax-service-using-complex-types-sample"></a>Exemplo de serviço de AJAX utilizando tipos complexos

Este exemplo demonstra como usar o Windows Communication Foundation (WCF) para criar um serviço de JavaScript e XML (AJAX) assíncrono do ASP.NET que cria instâncias de tipos complexos e os envia entre o serviço e o cliente como JavaScript Object Notation (JSON). Você pode acessar um serviço AJAX usando o código JavaScript de um cliente de navegador da Web. Este exemplo baseia-se no exemplo de [serviço básico Ajax](basic-ajax-service.md) .

O suporte ao AJAX no WCF é otimizado para uso com o ASP.NET AJAX por meio do <xref:System.Web.UI.ScriptManager> controle. Para obter um exemplo de como usar o WCF com o ASP.NET AJAX, consulte os [exemplos do AJAX](ajax.md).

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

O serviço no exemplo a seguir é um serviço WCF sem código específico do AJAX. Como o <xref:System.ServiceModel.Web.WebGetAttribute> atributo não é aplicado, o verbo HTTP padrão ("post") é usado. O serviço tem uma operação, `DoMath` , que retorna um tipo complexo chamado `MathResult` . O tipo complexo é um tipo de contrato de dados padrão, que também não contém nenhum código específico do AJAX.

```csharp
[DataContract]
public class MathResult
{
    [DataMember]
    public double sum;
    [DataMember]
    public double difference;
    [DataMember]
    public double product;
    [DataMember]
    public double quotient;
}
```

Crie um ponto de extremidade AJAX no serviço usando o <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> , assim como no exemplo de serviço básico Ajax.

A página da Web do cliente ComplexTypeClientPage. aspx contém o código ASP.NET e JavaScript para invocar o serviço quando o usuário clica no botão **executar cálculo** na página. O código para invocar o serviço constrói um corpo JSON e o envia usando HTTP POST, semelhante ao [serviço AJAX usando o exemplo http post](ajax-service-using-http-post.md) .

Depois que a chamada de serviço for realizada com sucesso, você poderá acessar os membros de dados individuais ( `sum` , `difference` `product` e `quotient` ) no objeto JavaScript resultante.

```javascript
function onSuccess(mathResult){
     document.getElementById("sum").value = mathResult.sum;
     document.getElementById("difference").value = mathResult.difference;
     document.getElementById("product").value = mathResult.product;
     document.getElementById("quotient").value = mathResult.quotient;
}
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Crie a solução ComplexTypeAjaxService. sln, conforme descrito em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Navegue até `http://localhost/ServiceModelSamples/ComplexTypeClientPage.aspx` (não abra ComplexTypeClientPage. aspx no navegador do diretório do projeto).

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\ComplexTypeAjaxService`

## <a name="see-also"></a>Consulte também

- [Serviço AJAX básico](basic-ajax-service.md)
