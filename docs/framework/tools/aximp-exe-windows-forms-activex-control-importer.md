---
title: Aximp.exe (Importador de Controle ActiveX do Windows Forms)
description: Entenda Aximp.exe, o Windows Forms o importador do controle ActiveX. Essa ferramenta converte definições de tipo em uma biblioteca de tipos COM do ActiveX em Windows Forms.
ms.date: 03/30/2017
helpviewer_keywords:
- ActiveX controls, hosting in Windows Forms
- ActiveX Control Importer
- type definitions, converting
- Aximp.exe
- Windows Forms ActiveX Control Importer
ms.assetid: 482c0d83-7144-4497-b626-87d2351b78d0
ms.openlocfilehash: d4fd6762195078963b43392178996a61f90feb94
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167346"
---
# <a name="aximpexe-windows-forms-activex-control-importer"></a>Aximp.exe (Importador de Controle ActiveX do Windows Forms)
O Importador de Controle ActiveX converte definições de tipo em uma biblioteca de tipos COM para um controle ActiveX em um controle do Windows Forms.  
  
 O Windows Forms só pode hospedar controles do Windows Forms — ou seja, classes derivadas de <xref:System.Windows.Forms.Control>. Aximp.exe gerencia uma classe wrapper para um controle ActiveX que pode ser hospedado no Windows Form. Isso permite usar o mesmo suporte de tempo de design e a mesma metodologia de programação aplicável a outros controles do Windows Forms.  
  
 Para hospedar o controle ActiveX, você deve gerar um controle wrapper derivado de <xref:System.Windows.Forms.AxHost>. Esse controle de wrapper contém uma instância do controle ActiveX subjacente. Ele sabe como se comunicar com o controle ActiveX, mas é exibido como um controle do Windows Forms. Esse controle gerado hospeda o controle ActiveX e expõe suas propriedades, métodos e eventos como os do controle gerado.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
aximp [options]{file.dll | file.ocx}  
```  
  
## <a name="remarks"></a>Comentários  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|*file*|O nome do arquivo de origem que contém o controle ActiveX a ser convertido. O argumento do arquivo deve ter a extensão .dll ou .ocx.|  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|`/delaysign`|Especifica Aximp.exe para assinar o controle resultante usando-se a assinatura com atraso. Você deve especificar essa opção com a opção `/keycontainer:`, `/keyfile:` ou `/publickey:`. Para obter mais informações sobre o processo de assinatura com atraso, consulte [Assinatura com Atraso de um Assembly](../../standard/assembly/delay-sign.md).|  
|`/help`|Exibe sintaxe de comando e opções para a ferramenta.|  
|`/keycontainer:`*ContainerName*|Assina o controle resultante com um nome forte usando o par de chaves pública/privada encontrado no contêiner de chave especificado por *containerName*.|  
|`/keyfile:`*nome do arquivo*|Assina o controle resultante com um nome forte usando o par de chaves pública/privada oficial do editor encontrado em *filename*.|  
|`/nologo`|Suprime a exibição do banner de inicialização da Microsoft.|  
|`/out:`*nome do arquivo*|Especifica o nome do assembly a ser criado.|  
|`/publickey:`*nome do arquivo*|Assina o controle resultante com um nome forte usando a chave pública encontrada no arquivo especificado por *filename*.|  
|`/rcw:`*nome do arquivo*|Usa o runtime callable wrapper especificado, em vez de gerar um novo. Convém especificar várias instâncias. O diretório atual é usado em caminhos relativos. Para obter mais informações, consulte [RCW (Runtime Callable Wrapper)](../../standard/native-interop/runtime-callable-wrapper.md).|  
|`/silent`|Suprime a exibição de mensagens de sucesso.|  
|`/source`|Gera o código-fonte do C# para o wrapper do Windows Forms.|  
|`/verbose`|Especifica o modo detalhado; exibe informações de andamento adicionais.|  
|`/?`|Exibe sintaxe de comando e opções para a ferramenta.|  
  
 Aximp.exe converte uma biblioteca de tipos inteira do Controle ActiveX de uma só vez e produz um conjunto de assemblies que contêm os metadados de Common Language Runtime e controlam a implementação dos tipos definidos na biblioteca de tipos original. Os arquivos gerados são nomeados de acordo com o seguinte padrão:  
  
 Proxy de Common Language Runtime para tipos COM: *progid*.dll  
  
 Proxy do Windows Forms para controles ActiveX (em que Ax significa ActiveX): Ax*progid*.dll  
  
> [!NOTE]
> Se o nome de um membro do controle ActiveX corresponder a um nome definido no .NET Framework, Aximp.exe prefixará o nome do membro com "Ctl" ao criar a classe derivada AxHost. Por exemplo, se o controle ActiveX tiver um membro chamado "Layout", ele será renomeado "CtlLayout" na classe derivada AxHost porque o evento Layout é definido dentro do .NET Framework.  
  
 É possível examinar esses arquivos gerados com ferramentas como [Ildasm.exe (IL Disassembler)](ildasm-exe-il-disassembler.md).  
  
 O uso de Aximp.exe para gerar um assembly do .NET. para o controle ActiveX WebBrowser (shdocvw.dll) não é compatível.  
  
 Quando você executar Aximp.exe em shdocvw.dll, ele sempre criará outro arquivo chamado shdocvw.dll no diretório no qual a ferramenta é executada. Se esse arquivo gerado for colocado no diretório Documents and Settings, isso causará problemas para Microsoft Internet Explorer e Windows Explorer. Quando o computador é reinicializado, o Windows examina o diretório Documents and Settings antes do diretório system32 para encontrar uma cópia de shdocvw.dll. Ele usará a cópia encontrada em Documents and Settings e tentará carregar os wrappers gerenciados. O Internet Explorer e o Windows Explorer não funcionarão corretamente porque dependem do mecanismo de renderização na versão de shdocvw.dll localizado no diretório system32. Se esse problema ocorrer, exclua a cópia de shdocvw.dll no diretório Documents and Settings e reinicie o computador.  
  
 O uso de Aximp.exe com shdocvw.dll para criar um assembly do .NET. a ser usado no desenvolvimento de aplicativos também pode causar problemas. Nesse caso, o aplicativo carregará a versão do sistema de shdocvw.dll e a versão gerada, além de poder atribuir a prioridade à versão do sistema. Nesse caso, quando você tentar carregar uma página da Web no controle ActiveX WebBrowser, os usuários poderão ser avisados com uma caixa de diálogo Abrir/Salvar. Quando o usuário clica em **Abrir**, a página da Web é aberta no Internet Explorer. Isso ocorre apenas com computadores que estão executando o Internet Explorer versão 6 ou anterior. Para evitar esse problema, use o controle gerenciado <xref:System.Windows.Forms.WebBrowser> ou o Visual Studio para gerar o shdocvw.dll gerenciado conforme descrito em [Como adicionar referências a bibliotecas de tipos](../interop/how-to-add-references-to-type-libraries.md).  
  
## <a name="example"></a>Exemplo  
 O comando a seguir gera MediaPlayer.dll e AxMediaPlayer.dll para o controle `msdxm.ocx` do Media Player.  
  
```console
aximp c:\systemroot\system32\msdxm.ocx  
```  
  
## <a name="see-also"></a>Confira também

- [Ferramentas](index.md)
- [Ildasm.exe (desmontador de IL)](ildasm-exe-il-disassembler.md)
