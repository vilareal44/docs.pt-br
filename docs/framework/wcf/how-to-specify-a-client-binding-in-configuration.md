---
title: Como especificar uma associação de cliente em configuração
description: Saiba como especificar a associação para um cliente WCF declarativamente em um arquivo de configuração. O cliente acessa um serviço neste exemplo.
ms.date: 03/30/2017
ms.assetid: 4a7c79aa-50ee-4991-891e-adc0599323a7
ms.openlocfilehash: 28778b6ae853199c5d7943f329bb087760f4bb11
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244485"
---
# <a name="how-to-specify-a-client-binding-in-configuration"></a>Como especificar uma associação de cliente em configuração
Neste exemplo, um aplicativo de console do cliente é criado para usar um serviço de calculadora e a associação para esse cliente é especificada declarativamente na configuração. O cliente acessa o `CalculatorService` , que implementa a `ICalculator` interface, e o serviço e o cliente usam a <xref:System.ServiceModel.BasicHttpBinding> classe.  
  
 O procedimento descrito pressupõe que o serviço de calculadora está em execução. Para obter informações sobre como criar o serviço, consulte [como especificar uma associação de serviço na configuração](how-to-specify-a-service-binding-in-configuration.md). Ele também usa a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) que o Windows Communication Foundation (WCF) fornece para gerar automaticamente os componentes do cliente. A ferramenta gera o código do cliente e a configuração para acessar o serviço.  
  
 O cliente é criado em duas partes. Svcutil.exe gera o `ClientCalculator` que implementa a `ICalculator` interface. Esse aplicativo cliente é então construído pela construção de uma instância do `ClientCalculator` .  
  
 Geralmente, é a prática recomendada especificar a associação e as informações de endereço de forma declarativa na configuração, em vez de imperativa no código. A definição de pontos de extremidade no código geralmente não é prática porque as associações e os endereços para um serviço implantado são normalmente diferentes daqueles usados enquanto o serviço está sendo desenvolvido. Em geral, manter as informações de vinculação e endereçamento do código permite que elas sejam alteradas sem a necessidade de recompilar ou reimplantar o aplicativo.  
  
 Você pode executar todas as etapas de configuração a seguir usando a [ferramenta do editor de configuração (SvcConfigEditor.exe)](configuration-editor-tool-svcconfigeditor-exe.md).  
  
 Para a cópia de origem deste exemplo, consulte o exemplo de [BasicBinding](./samples/basicbinding.md) .  
  
### <a name="specifying-a-client-binding-in-configuration"></a>Especificando uma associação de cliente na configuração  
  
1. Use Svcutil.exe da linha de comando para gerar código de metadados de serviço.  
  
    ```console  
    Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
    ```  
  
2. O cliente gerado contém a `ICalculator` interface que define o contrato de serviço que a implementação do cliente deve satisfazer.  
  
     [!code-csharp[C_HowTo_ConfigureClientBinding#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureclientbinding/cs/generatedclient.cs#1)]
     [!code-csharp[C_HowTo_ConfigureClientBinding#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureclientbinding/cs/source.cs#1)]  
  
3. O cliente gerado também contém a implementação do `ClientCalculator` .  
  
     [!code-csharp[C_HowTo_ConfigureClientBinding#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureclientbinding/cs/generatedclient.cs#2)]
     [!code-csharp[C_HowTo_ConfigureClientBinding#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureclientbinding/cs/source.cs#2)]  
  
4. Svcutil.exe também gera a configuração para o cliente que usa a <xref:System.ServiceModel.BasicHttpBinding> classe. Ao usar o Visual Studio, nomeie esse arquivo App.config. Observe que as informações de endereço e de associação não são especificadas em nenhum lugar dentro da implementação do serviço. Além disso, o código não precisa ser escrito para recuperar essas informações do arquivo de configuração.  
  
     [!code-xml[C_HowTo_ConfigureClientBinding#100](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureclientbinding/common/client.exe.config#100)]

5. Crie uma instância do `ClientCalculator` em um aplicativo e, em seguida, chame as operações de serviço.  
  
     [!code-csharp[C_HowTo_ConfigureClientBinding#3](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureclientbinding/cs/client.cs#3)]  
  
6. Compile e execute o cliente.  
  
## <a name="see-also"></a>Veja também

- [Usando associações para configurar serviços e clientes](using-bindings-to-configure-services-and-clients.md)
