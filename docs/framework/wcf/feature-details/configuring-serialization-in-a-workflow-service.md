---
title: Configurando a serialização em um serviço de fluxo de trabalho
ms.date: 03/30/2017
ms.assetid: aa70b290-a2ee-4c3c-90ea-d0a7665096ae
ms.openlocfilehash: 5076f3d377a656cb96909cf8df01591dc6ab72b7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597534"
---
# <a name="configuring-serialization-in-a-workflow-service"></a>Configurando a serialização em um serviço de fluxo de trabalho
Os serviços de fluxo de trabalho são Windows Communication Foundation (WCF) e, portanto, têm a opção de usar o <xref:System.Runtime.Serialization.DataContractSerializer> (o padrão) ou o <xref:System.Xml.Serialization.XmlSerializer> . Ao escrever serviços que não são de fluxo de trabalho, o tipo de serializador a ser usado é especificado no contrato de operação ou serviço. Ao criar serviços de fluxo de trabalho WCF, você não especifica esses contratos no código, mas sim eles são gerados em tempo de execução por inferência de contrato. Para obter mais informações sobre a inferência de contrato, consulte [usando contratos no fluxo de trabalho](using-contracts-in-workflow.md).  O serializador é especificado usando a <xref:System.ServiceModel.Activities.Receive.SerializerOption%2A> propriedade. Isso pode ser definido no designer, conforme mostrado na ilustração a seguir.  
  
 ![Definindo a Propriedade SerializerOption na janela Propriedades.](./media/configuring-serialization-in-a-workflow-service/setting-serializer-property.png)  
  
 O serializador também pode ser definido no código, conforme mostrado no exemplo a seguir,  
  
```csharp  
Receive approveExpense = new Receive  
            {  
                OperationName = "ApproveExpense",  
                CanCreateInstance = true,  
                ServiceContractName = "FinanceService",  
                SerializerOption = SerializerOption.DataContractSerializer,  
                Content = ReceiveContent.Create(new OutArgument<Expense>(expense))  
            };  
```  
  
  Tipos conhecidos também podem ser especificados em serviços de fluxo de trabalho. Para obter mais informações sobre tipos conhecidos, consulte [tipos conhecidos de contrato de dados](data-contract-known-types.md). Tipos conhecidos podem ser especificados no designer ou no código. Para especificar tipos conhecidos no designer, clique no botão de reticências ao lado da propriedade KnownTypes na **janela Propriedades** de uma <xref:System.ServiceModel.Activities.Receive> atividade, conforme mostrado na ilustração a seguir.
  
 ![Captura de tela da caixa de diálogo Propriedade KnownTypes.](./media/configuring-serialization-in-a-workflow-service/known-types-properties.png)  
  
 Isso exibe o editor de coleções de tipos que permite pesquisar e especificar tipos conhecidos.  
  
 ![Captura de tela do editor de coleções de tipos.](./media/configuring-serialization-in-a-workflow-service/type-collection-editor.gif)  
  
 Clique no link **Adicionar novo tipo** e use a lista suspensa para selecionar ou pesquisar um tipo para adicionar à coleção de tipos conhecidos. Para especificar tipos conhecidos no código, use a <xref:System.ServiceModel.Activities.Receive.KnownTypes%2A> propriedade, conforme mostrado no exemplo a seguir.  
  
```csharp
Receive approveExpense = new Receive  
            {  
                OperationName = "ApproveExpense",  
                CanCreateInstance = true,  
                ServiceContractName = "FinanceService",  
                SerializerOption = SerializerOption.DataContractSerializer,  
                Content = ReceiveContent.Create(new OutArgument<Expense>(expense))  
            };  
            approveExpense.KnownTypes.Add(typeof(Travel));  
            approveExpense.KnownTypes.Add(typeof(Meal));  
```
