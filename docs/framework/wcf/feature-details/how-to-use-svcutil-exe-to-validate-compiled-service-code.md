---
title: Como usar o Svcutil.exe para validar o código de serviço compilado
ms.date: 03/30/2017
ms.assetid: d0d820fb-41c2-45b8-8f22-0fa5aeebbbaa
ms.openlocfilehash: 6f2064c696e3186c3208a7e57dc51655056d23ea
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595343"
---
# <a name="how-to-use-svcutilexe-to-validate-compiled-service-code"></a>Como usar o Svcutil.exe para validar o código de serviço compilado
Você pode usar a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para detectar erros em implementações de serviço e configurações sem hospedar o serviço.  
  
### <a name="to-validate-a-service"></a>Para validar um serviço  
  
1. Compile seu serviço em um arquivo executável e em um ou mais assemblies dependentes.  
  
2. Abrir um prompt de comando do SDK  
  
3. No prompt de comando, inicie a ferramenta svcutil. exe usando o formato a seguir. Para obter mais informações sobre os vários parâmetros, consulte o Validationsection de serviço do tópico [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) .  
  
    ```console
    svcutil.exe /validate /serviceName:<serviceConfigName>  <assemblyPath>*  
    ```  
  
     Você deve usar a `/serviceName` opção para indicar o nome da configuração do serviço que deseja validar.  
  
     O `assemblyPath` argumento especifica o caminho para o arquivo executável para o serviço e um ou mais assemblies que contêm os tipos de serviço a serem validados. O assembly executável deve ter um arquivo de configuração associado para fornecer a configuração do serviço. Você pode usar curingas de linha de comando padrão para fornecer vários assemblies.  
  
## <a name="example"></a>Exemplo  
 O comando a seguir o serviço myServiceName implementado no arquivo executável myservicehost. exe.  O arquivo de configuração para o serviço (myservicehost. exe. config) é carregado automaticamente.  
  
```console  
svcutil /validate /serviceName:myServiceName myServiceHost.exe  
```  
  
## <a name="see-also"></a>Consulte também

- [Ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
