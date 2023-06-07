---
title: Tratamento de erros HTTP Web do WCF
ms.date: 03/30/2017
ms.assetid: 02891563-0fce-4c32-84dc-d794b1a5c040
ms.openlocfilehash: b1d41bebafa2795d390b120ad84475417389479b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598639"
---
# <a name="wcf-web-http-error-handling"></a>Tratamento de erros HTTP Web do WCF
O tratamento de erros HTTP Web do Windows Communication Foundation (WCF) permite que você retorne erros de serviços HTTP Web WCF que especificam um código de status HTTP e retornam detalhes do erro usando o mesmo formato que a operação (por exemplo, XML ou JSON).  
  
## <a name="wcf-web-http-error-handling"></a>Tratamento de erros HTTP Web do WCF  
 A <xref:System.ServiceModel.Web.WebFaultException> classe define um construtor que permite que você especifique um código de status http. Esse código de status é retornado para o cliente. Uma versão genérica da <xref:System.ServiceModel.Web.WebFaultException> classe permite que <xref:System.ServiceModel.Web.WebFaultException%601> você retorne um tipo definido pelo usuário que contém informações sobre o erro ocorrido. Esse objeto personalizado é serializado usando o formato especificado pela operação e retornado ao cliente. O exemplo a seguir mostra como retornar um código de status HTTP.  
  
```csharp
public string Operation1()
{
    // Operation logic  
   // ...
   throw new WebFaultException(HttpStatusCode.Forbidden);
}  
```  
  
 O exemplo a seguir mostra como retornar um código de status HTTP e informações extras em um tipo definido pelo usuário. `MyErrorDetail`é um tipo definido pelo usuário que contém informações adicionais sobre o erro ocorrido.  
  
```csharp
public string Operation2()
{
   // Operation logic  
   // ...
   MyErrorDetail detail = new MyErrorDetail()
   {  
      Message = "Error Message",  
      ErrorCode = 123,  
   }  
   throw new WebFaultException<MyErrorDetail>(detail, HttpStatusCode.Forbidden);  
}  
```  
  
 O código anterior retorna uma resposta HTTP com o código de status proibido e um corpo que contém uma instância do `MyErrorDetails` objeto. O formato do `MyErrorDetails` objeto é determinado por:  
  
- O valor do `ResponseFormat` parâmetro do <xref:System.ServiceModel.Web.WebGetAttribute> <xref:System.ServiceModel.Web.WebInvokeAttribute> atributo ou especificado na operação de serviço.  
  
- O valor de <xref:System.ServiceModel.Description.WebHttpBehavior.AutomaticFormatSelectionEnabled%2A>.  
  
- O valor da <xref:System.ServiceModel.Web.OutgoingWebResponseContext.Format%2A> Propriedade acessando o <xref:System.ServiceModel.Web.OutgoingWebResponseContext> .  
  
 Para obter mais informações sobre como esses valores afetam a formatação da operação, consulte [formatação do WCF Web http](wcf-web-http-formatting.md).  
  
 <xref:System.ServiceModel.Web.WebFaultException>é um <xref:System.ServiceModel.FaultException> e, portanto, pode ser usado como o modelo de programação de exceção de falha para serviços que expõem pontos de extremidade SOAP, bem como pontos de extremidade http da Web.  
  
## <a name="see-also"></a>Consulte também

- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
- [Formatação HTTP Web do WCF](wcf-web-http-formatting.md)
- [Definindo e especificando falhas](../defining-and-specifying-faults.md)
- [Tratamento de exceções e falhas](../extending/handling-exceptions-and-faults.md)
- [Enviando e recebendo falhas](../sending-and-receiving-faults.md)
