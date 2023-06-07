---
title: ConfigurationCodeGenerator
ms.date: 03/30/2017
ms.assetid: 3913aae8-165f-4014-9262-7fe426f90cb2
ms.openlocfilehash: d64be95f71f840e08ede63e1c1f14ee08e52ce97
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84592470"
---
# <a name="configurationcodegenerator"></a>ConfigurationCodeGenerator
O ConfigurationCodeGenerator é uma ferramenta que você pode usar para expor suas implementações de canal personalizadas para o sistema de configuração. Isso permite que os usuários do seu canal personalizado configurem seu canal usando um arquivo. config, assim como configurariam uma associação fornecida pelo sistema, como `NetTcpBinding` ou uma ligação personalizada usando o `TcpTransportBindingElement` .  
  
 Quando você escreve um canal personalizado e o expõe para o modelo de programação usando um novo `BindingElement` ou `Binding` , você deve criar um conjunto de classes para tornar o `BindingElement` ou `Binding` configurável usando um arquivo. config. Você pode usar a ferramenta ConfigurationCodeGenerator para gerar essas classes e aprimorar a experiência do cliente.  
  
### <a name="to-build-the-tool"></a>Para criar a ferramenta  
  
1. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
2. A criação da solução gera um arquivo: ConfigurationCodeGenerator. exe. O arquivo SampleRun. cmd tem uma linha de comando de exemplo que mostra como usar essa ferramenta para gerar as classes para o exemplo de [transporte: UDP](transport-udp.md) .  
  
### <a name="to-run-the-tool"></a>Para executar a ferramenta  
  
1. No prompt de comando, digite o seguinte se você tiver um `BindingElement` tipo personalizado e um `Binding` tipo personalizado:  
  
    ```console  
    ConfigurationCodeGenerator.exe /be:YourCustomBindingElementTypeName /sb:YourCustomStdBindingTypeName /dll:TheAssemblyWhereTheseTypesAreDefined  
    ```  
  
     Ou digite o seguinte se você tiver apenas um `BindingElement` tipo personalizado:  
  
    ```console  
    ConfigurationCodeGenerator.exe /be:YourCustomBindingElementTypeName /dll: TheAssemblyWhereThisTypeIsDefined  
    ```  
  
     Ou digite o seguinte se você tiver apenas um `Binding` tipo personalizado:  
  
    ```console  
    ConfigurationCodeGenerator.exe /sb:YourCustomStdBindingTypeName /dll:TheAssemblyWhereThisTypeIsDefined  
    ```  
  
     O comando gera três arquivos. cs para o `BindingElement` (se você especificou a opção/be:), cinco arquivos. cs para o padrão `Binding` (se você especificou a opção/SB:) e um arquivo. xml.  
  
    1. Se você usou a opção/be, um dos arquivos. cs implementa o `BindingElementExtensionSection` para seu elemento Binding. Esse código expõe seu `BindingElement` para o sistema de configuração, para que outras associações personalizadas possam usar seu elemento de associação. Os outros arquivos têm classes que representam padrões e constantes. Os arquivos têm `//TODO` comentários para lembrá-lo de atualizar os valores padrão.  
  
    2. Se você especificou a opção/SB, dois dos arquivos. cs implementam um `StandardBindingElement` e um `StandardBindingCollectionElement` , respectivamente, que expõem a ligação padrão com o sistema de configuração. Os outros arquivos têm classes que representam padrões e constantes. Os arquivos têm `//TODO` comentários para lembrá-lo de atualizar os valores padrão.  
  
         Se você especificou a opção/SB: o CodeToAddTo \<*YourStdBinding*> . cs tem código que você deve adicionar manualmente à classe que implementa sua associação padrão.  
  
     O arquivo SampleConfig. xml contém o código de configuração que você deve adicionar ao arquivo de configuração que registra os manipuladores definidos na etapa 1 ou 2 anterior.  
