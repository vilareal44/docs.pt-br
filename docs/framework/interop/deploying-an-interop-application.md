---
title: Implantando um aplicativo de interoperabilidade
description: Implantar um aplicativo de interoperabilidade, que geralmente tem um assembly de cliente .NET, assemblies de interoperabilidade de bibliotecas de tipos COM diferentes e componentes COM registrados.
ms.date: 03/30/2017
helpviewer_keywords:
- deploying applications [.NET Framework], interop
- strong-named assemblies, interop applications
- unsigned assemblies
- private assemblies
- exposing COM components to .NET Framework
- interoperation with unmanaged code, deploying applications
- shared assemblies
- COM interop, deploying applications
- interoperation with unmanaged code, exposing COM components
- signed assemblies
- COM interop, exposing COM components
ms.assetid: ea8a403e-ae03-4faa-9d9b-02179ec72992
ms.openlocfilehash: 744307d4175d151d07acbedd5815e538307c8973
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617477"
---
# <a name="deploying-an-interop-application"></a>Implantando um aplicativo de interoperabilidade
Um aplicativo de interoperabilidade geralmente inclui um assembly de cliente do .NET, um ou mais assemblies de interoperabilidade que representam diferentes bibliotecas de tipos COM e um ou mais componentes COM registrados. O Visual Studio e o SDK do Windows fornecem ferramentas para importar e converter uma biblioteca de tipos em um assembly de interoperabilidade, conforme abordado em [Importar uma biblioteca de tipos como um assembly](importing-a-type-library-as-an-assembly.md). Há duas maneiras de implantar um aplicativo de interoperabilidade:  
  
- Usando tipos de interoperabilidade inseridos: começando com o .NET Framework 4, você pode instruir o compilador a inserir informações de tipo de um assembly de interoperabilidade em seu executável. O compilador insere apenas as informações de tipo usadas pelo aplicativo. Não é necessário implantar o assembly de interoperabilidade com o aplicativo. Essa é a técnica recomendada.  
  
- Implantando assemblies de interoperabilidade: é possível criar uma referência padrão a um assembly de interoperabilidade. Nesse caso, o assembly de interoperabilidade deve ser implantado com o aplicativo. Se você usar essa técnica e não estiver usando um componente COM particular, sempre referencie o PIA (assembly de interoperabilidade primário) publicado pelo autor do componente COM que você pretende incorporar no código gerenciado. Para obter mais informações sobre como produzir e usar assemblies de interoperabilidade primários, consulte [Assemblies de interoperabilidade primários](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aax7sdch(v=vs.100)).  
  
 Se você usar tipos de interoperabilidade inseridos, a implantação será simples. Não há nada especial que precisa ser feito. O restante deste artigo descreve os cenários de implantação de assemblies de interoperabilidade com o aplicativo.  
  
## <a name="deploying-interop-assemblies"></a>Implantando assemblies de interoperabilidade  
 Os assemblies pode ter nomes fortes. Um assembly de nome forte inclui a chave pública do fornecedor, que fornece uma identidade exclusiva. Assemblies que são produzidos pelo [Importador da Biblioteca de Tipos (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md) podem ser assinados pelo fornecedor usando a opção **/keyfile**. É possível instalar assemblies assinados no cache de assembly global. Assemblies não assinados devem ser instalados no computador do usuário como assemblies particulares.  
  
### <a name="private-assemblies"></a>Assemblies particulares  
 Para instalar um assembly a ser usado de forma particular, o executável do aplicativo e o assembly de interoperabilidade que contém tipos COM importados devem ser instalados na mesma estrutura de diretório. A ilustração a seguir mostra um assembly de interoperabilidade não assinado a ser usado de forma particular por Client1.exe e Client2.exe, que residem em diretórios de aplicativo separados. O assembly de interoperabilidade, que é chamado LOANLib.dll neste exemplo, é instalado duas vezes.  
  
 ![Estrutura de diretórios e registro do Windows](./media/deploying-an-interop-application/com-private-deployment.gif "Estrutura do diretório e entradas do Registro de uma implantação particular")  
  
 Todos os componentes COM associados ao aplicativo devem ser instalados no Registro do Windows. Se Client1.exe e Client2.exe na ilustração forem instalados em computadores diferentes, você deverá registrar esses componentes COM em ambos os computadores.  
  
### <a name="shared-assemblies"></a>Assemblies compartilhados  
 Assemblies que são compartilhados por vários aplicativos devem ser instalados em um repositório centralizado chamado cache de assembly global. Os clientes do .NET podem acessar a mesma cópia do assembly de interoperabilidade, que é assinada e instalada no cache de assembly global. Para obter mais informações sobre como produzir e usar assemblies de interoperabilidade primários, consulte [Assemblies de interoperabilidade primários](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aax7sdch(v=vs.100)).  
  
## <a name="see-also"></a>Consulte também

- [Expondo componentes do COM para o .NET Framework](exposing-com-components.md)
- [Importando uma biblioteca de tipos como um assembly](importing-a-type-library-as-an-assembly.md)
- [Usando tipos COM no código gerenciado](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))
- [Compilando um projeto de interoperabilidade](compiling-an-interop-project.md)
