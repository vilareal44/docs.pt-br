---
title: Processo de execução gerenciada
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- source code language
- code, managed execution process
- runtime, managed execution process
- compiling source code, managed execution process
- managed execution process
- common language runtime, managed execution process
ms.assetid: 476b03dc-2b12-49a7-b067-41caeaa2f533
ms.openlocfilehash: 3cfe66f188c5abf245370f841d4b4d31e7b6db8b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290065"
---
# <a name="managed-execution-process"></a>Processo de execução gerenciada
<a name="introduction"></a> O processo de execução gerenciada inclui as seguintes etapas, que serão discutidas em detalhes mais adiante neste tópico:  
  
1. [Escolhendo um compilador](#choosing_a_compiler).  
  
     Para obter as vantagens fornecidas pelo Common Language Runtime, você deve usar um ou mais compiladores de linguagem que selecionam o tempo de execução.  
  
2. [Compilando seu código em MSIL](#compiling_to_msil).  
  
     A compilação converte seu código-fonte em MSIL (Microsoft Intermediate Language) e gera os metadados necessários.  
  
3. [Compilando MSIL em código nativo](#compiling_msil_to_native_code).  
  
     No tempo de execução, uma compilação JIT (just-in-time) converte o MSIL em código nativo. Durante essa compilação, o código deve passar por um processo de verificação que examina o MSIL e os metadados para descobrir se o código pode ser considerado fortemente tipado.  
  
4. [Código em execução](#running_code).  
  
     O Common Language Runtime fornece a infraestrutura que permite que a execução ocorra e os serviços que podem ser usados durante a execução.  
  
<a name="choosing_a_compiler"></a>
## <a name="choosing-a-compiler"></a>Escolhendo um compilador  
 Para obter as vantagens fornecidas pelo CLR (Common Language Runtime), você deve usar um ou mais compiladores de linguagem que direcionam o tempo de execução, como Visual Basic, C#, Visual C++, F# ou um dos muitos compiladores de terceiros como um Eiffel, um Perl ou um compilador COBOL.  
  
 Por ser um ambiente de execução multilíngue, o runtime dá suporte a uma grande variedade de tipos de dados e recursos de linguagem. O compilador de linguagem que você usa determina quais recursos do runtime estão disponíveis, e você cria seu código usando esses recursos. O seu compilador, e não o runtime, estabelece a sintaxe que seu código deve usar. Se o componente deve ser completamente utilizável por componentes escritos em outras linguagens, os tipos exportados do seu componente devem expor somente recursos de linguagem incluídos na [Independência de linguagem e componentes independentes de linguagem](language-independence-and-language-independent-components.md) (CLS). Você pode usar o atributo <xref:System.CLSCompliantAttribute> para garantir que seu código seja compatível com CLS. Para saber mais, veja [Componentes de independência de linguagem e componentes independentes da linguagem](language-independence-and-language-independent-components.md).  
  
 [Voltar ao início](#introduction)  
  
<a name="compiling_to_msil"></a>
## <a name="compiling-to-msil"></a>Compilando para MSIL  
 Quando compila para o código gerenciado, o compilador converte seu código fonte em MSIL, que é um conjunto de instruções independente de CPU que pode ser convertido em código nativo com eficiência. O MSIL inclui instruções para carregamento, armazenamento, inicialização e chamada de métodos em objetos, bem como instruções para operações aritméticas e lógicas, fluxo de controle, acesso direto à memória, tratamento de exceções e outras operações. Para o código ser executado, a MSIL deve ser convertida em código específico de CPU, geralmente por um [Compilador JIT (Just-In-Time)](#compiling_msil_to_native_code). Como o Common Language Runtime fornece um ou mais compiladores JIT para cada arquitetura de computador para a qual dá suporte, o mesmo conjunto de MSIL pode ser compilado por JIT e executado em qualquer arquitetura compatível.  
  
 Quando um compilador produz MSIL, ele também produz metadados. Metadados descrevem os tipos no seu código, incluindo a definição de cada tipo, as assinaturas de membros de cada tipo, os membros que seu código referencia e outros dados que o runtime usa no runtime. O MSIL e os metadados estão contidos em um arquivo PE (Portable Executable) com base no e que estende o arquivo PE Microsoft publicado e o COFF (Common Object File Format) usados historicamente no conteúdo executável. Esse formato de arquivo, que acomoda MSIL ou código nativo, bem como metadados, permite ao sistema operacional reconhecer imagens do Common Language Runtime. A presença de metadados no arquivo com MSIL permite que seu código se descreva, o que significa que não há necessidade de bibliotecas de tipos ou IDL (Interface Definition Language). O runtime localiza e extrai os metadados do arquivo conforme necessário durante a execução.  
  
 [Voltar ao início](#introduction)  
  
<a name="compiling_msil_to_native_code"></a>
## <a name="compiling-msil-to-native-code"></a>Compilando MSIL para código nativo  
 Para executar o MSIL, ele deve ser compilado no CLR para código nativo da arquitetura do computador de destino. O .NET Framework fornece duas maneiras para realizar essa conversão:  
  
- Um compilador JIT (just-in-time) do .NET Framework.  
  
- O [Ngen.exe (Gerador de Imagem Nativa)](../framework/tools/ngen-exe-native-image-generator.md) do .NET Framework.  
  
### <a name="compilation-by-the-jit-compiler"></a>Compilação pelo compilador JIT  
 A compilação JIT converte MSIL para código nativo sob demanda no tempo de execução do aplicativo, quando o conteúdo de um assembly é carregado e executado. Como o CLR fornece um compilador JIT para cada arquitetura de CPU compatível, os desenvolvedores podem compilar um conjunto de assemblies MSIL que pode ser compilado pelo JIT e executado em diferentes computadores com arquiteturas diferentes. No entanto, se seu código gerenciado chama APIs nativas específicas da plataforma ou uma biblioteca de classes específica da plataforma, ele será executado somente nesse sistema operacional.  
  
 A compilação JIT leva em conta a possibilidade de um código jamais ser chamado durante a execução. Em vez de usar o tempo e a memória para converter todo o MSIL em um arquivo PE para código nativo, ele converte o MSIL conforme necessário durante a execução e armazena o código nativo resultante na memória de forma que seja acessível para chamadas subsequentes no contexto desse processo. O carregador cria e anexa um stub para cada método em um tipo quando o tipo é carregado e inicializado. Quando um método é chamado pela primeira vez, o stub passa o controle para o compilador JIT, que converte o MSIL para esse método em código nativo e modifica o stub para apontar diretamente para o código nativo gerado. Portanto, as chamadas subsequentes para o método compilado por JIT vão diretamente para o código nativo.  
  
### <a name="install-time-code-generation-using-ngenexe"></a>Geração de código do tempo de instalação usando NGen.exe  
 Como o compilador JIT converte o MSIL do assembly para código nativo quando métodos individuais definidos nesse assembly são chamados, ele afeta o desempenho negativamente no tempo de execução. Na maioria dos casos, esse desempenho diminuído é aceitável. Mais importante, o código gerado pelo compilador JIT é associado ao processo que disparou a compilação. Ele não pode ser compartilhado por vários processos. Para permitir que o código gerado seja compartilhado por várias invocações de um aplicativo ou por vários processos que compartilham um conjunto de assemblies, o CLR dá suporte a um modo de compilação antecipado. Esse modo de compilação Ahead Of Time usa o [Ngen.exe (Gerador de Imagem Nativa)](../framework/tools/ngen-exe-native-image-generator.md) para converter assemblies MSIL em código nativo de maneira semelhante ao compilador JIT. No entanto, a operação de Ngen.exe é diferente da operação do compilador JIT de três maneiras:  
  
- Ele executa a conversão do MSIL em código nativo antes de executar o aplicativo, e não durante a execução do aplicativo.  
  
- Ele cria um assembly inteiro por vez, em vez de um método de cada vez.  
  
- Ele mantém o código gerado no Cache de Imagem Nativa como um arquivo no disco.  
  
### <a name="code-verification"></a>Verificação de código  
 Como parte de sua compilação para código nativo, o código MSIL deve passar por um processo de verificação, a menos que um administrador estabeleça uma política de segurança permitindo que o código ignore a verificação. A verificação examina o MSIL e os metadados para descobrir se o código é fortemente tipado, o que significa que ele acessa apenas os locais de memória que está autorizado a acessar. A segurança de tipo ajuda a isolar objetos uns dos outros e a protegê-los de danos não intencionais ou corrupção mal-intencionada. Ela também dá garantia de que restrições de segurança no código possam ser aplicadas confiavelmente.  
  
 O runtime depende das seguintes declarações serem verdadeiras para o código que é comprovadamente fortemente tipado:  
  
- Uma referência a um tipo é estritamente compatível com o tipo que está sendo referenciado.  
  
- Somente operações definidas adequadamente são invocadas em um objeto.  
  
- Identidades são o que elas afirmam ser.  
  
 Durante o processo de verificação, o código MSIL é examinado em uma tentativa de confirmar que o código possa acessar locais de memória e chamar métodos apenas por tipos corretamente definidos. Por exemplo, o código não pode permitir que campos de um objeto sejam acessados de uma maneira que permita que locais de memória sejam saturados. Além disso, a verificação inspeciona o código para determinar se o MSIL foi corretamente gerado, porque MSIL incorreto pode levar a uma violação das regras de segurança de tipos. O processo de verificação passa um conjunto bem definido de código fortemente tipado, e ele passa apenas código fortemente tipado. Entretanto, alguns códigos fortemente tipados podem não passar nessa verificação devido às limitações do processo de verificação, e algumas linguagens, por projeto, não produzem código fortemente tipado verificável. Se o código fortemente tipado for exigido pela política de segurança mas o código não passar pela verificação, uma exceção será lançada quando o código for executado.  
  
 [Voltar ao início](#introduction)  
  
<a name="running_code"></a>
## <a name="running-code"></a>Executando o código  
 O CLR fornece a infraestrutura que permite que a runtime gerenciada ocorra e os serviços que podem ser usados durante a runtime. Para um método ser executado, ele deve ser compilado para o código específico do processador. Cada método para MSIL gerado é compilado por JIT quando é chamado pela primeira vez e executado. Na próxima vez em que o método for executado, o código nativo compilado por JIT existente será executado. O processo de compilação por JIT e a execução do código é repetido até a execução ser concluída.  
  
 Durante a execução, o código gerenciado recebe serviços como coleta de lixo, segurança, interoperabilidade com código não gerenciado, suporte à depuração entre linguagens e suporte avançado à implantação e ao controle de versão.  
  
 No Microsoft Windows Vista, o carregador do sistema operacional verifica módulos gerenciados examinando um pouco no cabeçalho COFF. Se o bit estiver definido, o módulo será gerenciado. Se o carregador detecta módulos gerenciados, ele carrega mscoree.dll e `_CorValidateImage`, e `_CorImageUnloading` notifica o carregador quando as imagens do módulo gerenciado são carregadas e descarregadas. `_CorValidateImage` executa as seguintes ações:  
  
1. Garante que o código seja um código gerenciado válido.  
  
2. Altera o ponto de entrada na imagem para um ponto de entrada no ambiente de runtime.  
  
 No Windows 64 bits, `_CorValidateImage` modifica a imagem que está na memória transformando-a do formato PE32 para o formato PE32+.  
  
 [Voltar ao início](#introduction)  
  
## <a name="see-also"></a>Veja também

- [Visão geral](../framework/get-started/overview.md)
- [Componentes de independência de linguagem e componentes independentes da linguagem](language-independence-and-language-independent-components.md)
- [Metadados e componentes autodescritivos](metadata-and-self-describing-components.md)
- [Ilasm. exe (assembler IL)](../framework/tools/ilasm-exe-il-assembler.md)
- [Segurança](security/index.md)
- [Interoperação com código não gerenciado](../framework/interop/index.md)
- [Implantação](../framework/deployment/net-framework-applications.md)
- [Assemblies no .NET](assembly/index.md)
- [Domínios do aplicativo](../framework/app-domains/application-domains.md)
