---
title: Introdução à Interoperabilidade COM
ms.date: 07/20/2015
helpviewer_keywords:
- interop assemblies
- COM interop [Visual Basic], about COM interop
ms.assetid: 8bd62e68-383d-407f-998b-29aa0ce0fd67
ms.openlocfilehash: 6c7caf266514c43e40135b33d848a688546acf1c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396773"
---
# <a name="introduction-to-com-interop-visual-basic"></a>Introdução à interoperabilidade COM (Visual Basic)
A Component Object Model (COM) permite que um objeto exponha sua funcionalidade a outros componentes e hospede aplicativos. Embora os objetos COM tenham sido fundamentais para a programação do Windows por muitos anos, os aplicativos projetados para o Common Language Runtime (CLR) oferecem muitas vantagens.  
  
 Os aplicativos .NET Framework eventualmente substituirão aqueles desenvolvidos com com. Até lá, talvez você precise usar ou criar objetos COM usando o Visual Studio. A interoperabilidade com com ou a *interoperabilidade com*permite que você use objetos com existentes ao fazer a transição para o .NET Framework em seu próprio ritmo.  
  
 Usando o .NET Framework para criar componentes COM, você pode usar a interoperabilidade COM sem registro. Isso permite que você controle qual versão de DLL está habilitada quando mais de uma versão está instalada em um computador e permite que os usuários finais usem o XCOPY ou FTP para copiar seu aplicativo para um diretório apropriado em seu computador onde ele pode ser executado. Para obter mais informações, consulte [interoperabilidade com sem registro](../../../framework/interop/registration-free-com-interop.md).  
  
## <a name="managed-code-and-data"></a>Código e dados gerenciados  
 O código desenvolvido para o .NET Framework é conhecido como *código gerenciado*e contém metadados que são usados pelo CLR. Os dados usados por .NET Framework aplicativos são chamados de *dados gerenciados* porque o tempo de execução gerencia tarefas relacionadas a dados, como alocar e recuperar memória e executar a verificação de tipo. Por padrão, Visual Basic .NET usa código e dados gerenciados, mas você pode acessar o código não gerenciado e os dados de objetos COM usando assemblies de interoperabilidade (descritos posteriormente nesta página).  
  
## <a name="assemblies"></a>Assemblies  
 Um assembly é o bloco de construção principal de um aplicativo .NET Framework. É uma coleção de funcionalidades que são criadas, com controle de versão e implantadas como uma única unidade de implementação que contém um ou mais arquivos. Cada assembly contém um manifesto do assembly.  
  
## <a name="type-libraries-and-assembly-manifests"></a>Bibliotecas de tipos e manifestos de assembly  
 Bibliotecas de tipos descrevem características de objetos COM, como nomes de membros e tipos de dados. Os manifestos de assembly executam a mesma função para aplicativos .NET Framework. Eles incluem informações sobre o seguinte:  
  
- Identidade do assembly, versão, cultura e assinatura digital.  
  
- Arquivos que compõem a implementação do assembly.  
  
- Tipos e recursos que compõem o assembly. Isso inclui os que são exportados a partir dele.  
  
- Dependências de tempo de compilação em outros assemblies.  
  
- Permissões necessárias para que o assembly seja executado corretamente.  
  
 Para obter mais informações sobre assemblies e manifestos de assembly, consulte [assemblies no .net](../../../standard/assembly/index.md).  
  
### <a name="importing-and-exporting-type-libraries"></a>Importando e exportando bibliotecas de tipos  
 O Visual Studio contém um utilitário, Tlbimp, que permite que você importe informações de uma biblioteca de tipos para um aplicativo .NET Framework. Você pode gerar bibliotecas de tipos de assemblies usando o utilitário Tlbexp.  
  
 Para obter informações sobre Tlbimp e TlbExp, consulte [Tlbimp. exe (tipo de importador de biblioteca de tipos)](../../../framework/tools/tlbimp-exe-type-library-importer.md) e [Tlbexp. exe (tipo de exportador de biblioteca de tipos)](../../../framework/tools/tlbexp-exe-type-library-exporter.md).  
  
## <a name="interop-assemblies"></a>Assemblies de interoperabilidade  
 Os assemblies de interoperabilidade são .NET Framework assemblies que fazem a ponte entre códigos gerenciados e não gerenciados, mapeando membros de objeto COM para membros gerenciados .NET Framework equivalentes. Os assemblies de interoperabilidade criados pelo Visual Basic .NET lidam com muitos dos detalhes do trabalho com objetos COM, como o marshaling de interoperabilidade.  
  
## <a name="interoperability-marshaling"></a>Marshaling de interoperabilidade  
 Todos os aplicativos de .NET Framework compartilham um conjunto de tipos comuns que permitem a interoperabilidade de objetos, independentemente da linguagem de programação usada. Os parâmetros e valores de retorno de objetos COM às vezes usam tipos de dados que diferem daqueles usados em código gerenciado. O *marshaling de interoperabilidade* é o processo de empacotamento de parâmetros e valores de retorno em tipos de dados equivalentes à medida que eles se movem para e de objetos com. Para obter mais informações, consulte [Interop Marshaling](../../../framework/interop/interop-marshaling.md).  
  
## <a name="see-also"></a>Confira também

- [Interoperabilidade COM](index.md)
- [Passo a passo: Implementação de herança com objetos COM](walkthrough-implementing-inheritance-with-com-objects.md)
- [Interoperação com código não gerenciado](../../../framework/interop/index.md)
- [Solução de problemas de Interoperabilidade](troubleshooting-interoperability.md)
- [Assemblies no .NET](../../../standard/assembly/index.md)
- [Tlbimp. exe (tipo de importador de biblioteca de tipos)](../../../framework/tools/tlbimp-exe-type-library-importer.md)
- [TlbExp. exe (tipo de exportador da biblioteca de tipos)](../../../framework/tools/tlbexp-exe-type-library-exporter.md)
- [Realizando marshaling de interoperabilidade](../../../framework/interop/interop-marshaling.md)
- [Interoperabilidade COM sem registro](../../../framework/interop/registration-free-com-interop.md)
