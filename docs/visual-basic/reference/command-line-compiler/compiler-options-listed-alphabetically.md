---
title: Opções do compilador listadas em ordem alfabética
ms.date: 04/12/2018
helpviewer_keywords:
- Visual Basic compiler, options
ms.assetid: e67febba-bacf-4e1f-a143-c141e063f90e
ms.openlocfilehash: 19e14953c08f90ea1ab245fa3124a462ccba162f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408732"
---
# <a name="visual-basic-compiler-options-listed-alphabetically"></a>Visual Basic opções de compilador listadas em ordem alfabética
O compilador de linha de comando Visual Basic é fornecido como uma alternativa à compilação de programas do IDE (ambiente de desenvolvimento integrado) do Visual Studio. Veja a seguir uma lista de Visual Basic opções do compilador de linha de comando classificadas em ordem alfabética.  

[!INCLUDE[compiler-options](~/includes/compiler-options.md)]
  
|Opção|Finalidade|  
|------------|-------------|  
|[@ (especificar arquivo de resposta)](specify-response-file.md)|Especifica um arquivo de resposta.|  
|[-?](help.md)|Exibe as opções do compilador. Esse comando é o mesmo que especificar a `-help` opção. Nenhuma compilação ocorre.|  
|`-additionalfile`|Nomeia outros arquivos que não afetam diretamente a geração de código, mas podem ser usados por analisadores para produzir erros ou avisos.|  
|[-addmodule](addmodule.md)|Faz com que o compilador verifique todos os tipos de informações de arquivos especificados disponíveis para o projeto que você está compilando.|  
|`-analyzer`|Executar os analisadores com basse nesse assembly (forma abreviada: -a)|  
|[-baseaddress](baseaddress.md)|Especifica o endereço base de uma DLL.|  
|[-bugreport](bugreport.md)|Cria um arquivo que contém informações que tornam mais fácil relatar um bug.|  
|`-checksumalgorithm:<alg>`|Especifique o algoritmo para calcular a soma de verificação do arquivo de origem armazenada no PDB.  Os valores com suporte são: SHA1 (padrão) ou SHA256. <br>Devido a problemas de colisão com o SHA1, a Microsoft recomenda SHA256 ou melhor.|  
|[-codepage](codepage.md)|Especifica a página de código a ser usada para todos os arquivos de código-fonte na compilação.|  
|[-Depurar](debug.md)|Produz informações de depuração.|  
|[-define](define.md)|Define os símbolos para a compilação condicional.|  
|[-delaysign](delaysign.md)|Especifica se o assembly será assinado total ou parcialmente.|  
|[-deterministic](deterministic.md)|Faz com que o compilador gere um assembly de conteúdo binário idêntico entre compilações se as entradas são idênticas.|
|[-doc](doc.md)|Processa comentários de documentação para um arquivo XML.|  
|[-errorreport](errorreport.md)|Especifica como o compilador de Visual Basic deve relatar erros internos do compilador.|  
|[-filealign](filealign.md)|Especifica onde alinhar as seções do arquivo de saída.|  
|[-ajuda](help.md)|Exibe as opções do compilador. Esse comando é o mesmo que especificar a `-?` opção. Nenhuma compilação ocorre.|  
|[-highentropyva](highentropyva.md)|Indica se um executável específico dá suporte a alta possibilidade de seleção de layout de espaço de endereço de entropia (ASLR).|  
|[-imports](imports.md)|Importa um namespace de um assembly especificado.|  
|[-keycontainer](keycontainer.md)|Especifica um nome de contêiner de chave para um par de chaves para dar a um assembly um nome forte.|  
|[-keyfile](keyfile.md)|Especifica um arquivo que contém uma chave ou um par de chaves para fornecer um nome forte ao assembly.|  
|[-langversion](langversion.md)|Especifique a versão do idioma: 9&#124;9,0&#124;10&#124;10,0&#124;11&#124;11,0.|  
|[-libpath](libpath.md)|Especifica o local dos assemblies referenciados pela opção [-Reference](reference.md) .|  
|[-linkresource](linkresource.md)|Cria um link a um recurso gerenciado.|  
|[-main](main.md)|Especifica a classe que contém o `Sub Main` procedimento a ser usado na inicialização.|  
|[-moduleassemblyname](moduleassemblyname.md)|Especifica o nome do assembly do qual um módulo fará parte.|  
|`-modulename:<string>`|Especificar o nome do módulo de origem|  
|[-netcf](netcf.md)|Define o compilador para o .NET Compact Framework de destino.|  
|[-noconfig](noconfig.md)|Não compile com Vbc. rsp.|  
|[-nologo](nologo.md)|Suprime as informações da faixa do compilador.|  
|[-nostdlib](nostdlib.md)|Faz com que o compilador não referencie as bibliotecas padrão.|  
|[-nowarn](nowarn.md)|Suprime a capacidade do compilador de gerar avisos.|  
|[-nowin32manifest](nowin32manifest.md)|Instrui o compilador a não inserir nenhum manifesto de aplicativo no arquivo executável.|  
|[-otimizar](optimize.md)|Habilita/desabilita a otimização de código.|  
|[-optioncompare](optioncompare.md)|Especifica se as comparações de cadeia de caracteres devem ser binárias ou usar a semântica de texto específica à localidade.|  
|[-optionexplicit](optionexplicit.md)|Impõe uma declaração explícita de variáveis.|  
|[-optioninfer](optioninfer.md)|Permite o uso de inferência de tipo local nas declarações de variáveis.|  
|[-optionstrict](optionstrict.md)|Impõe semântica de linguagem estrita.|  
|[-out](out.md)|Especifica um arquivo de saída.|  
|<code>-parallel[+&#124;-]</code>|Especifica se deve o build simultâneo deve ser usado (+).|  
|[-plataforma](platform.md)|Especifica a plataforma do processador que o compilador tem como destino para o arquivo de saída.|  
|`-preferreduilang`|Especifique o nome de idioma de saída preferencial.|  
|[-quiet](quiet.md)|Impede que o compilador exiba código para erros e avisos relacionados à sintaxe.|  
|[-recurse](recurse.md)|Pesquisa em subdiretórios arquivos de código-fonte a serem compilados.|  
|[-referência](reference.md)|Importa metadados de um assembly.|  
|[-refonly](refonly-compiler-option.md)|Gera apenas um assembly de referência.|
|[-refout](refout-compiler-option.md)|Especifica o caminho de saída de um assembly de referência.|
|[-removeintchecks](removeintchecks.md)|Desabilita a verificação de estouro de inteiros.|  
|[-recurso](resource.md)|Insere um recurso gerenciado em um assembly.|  
|[-rootnamespace](rootnamespace.md)|Especifica um namespace para todas as declarações de tipo.|  
|`-ruleset:<file>`|Especifique um arquivo de conjunto de regras que desabilita o diagnóstico específico.|  
|[-sdkpath](sdkpath.md)|Especifica o local de mscorlib. dll e Microsoft. VisualBasic. dll.|  
|[-subsystemversion](subsystemversion.md)|Especifica a versão mínima do subsistema que o arquivo executável gerado pode usar.|  
|[-target](target.md)|Especifica o formato do arquivo de saída.|  
|[-utf8output](utf8output.md)|Exibe a saída do compilador usando a codificação UTF-8.|  
|[-vbruntime](vbruntime.md)|Especifica que o compilador deve compilar sem uma referência para a biblioteca de tempo de execução Visual Basic ou com uma referência a uma biblioteca de tempo de execução específica.|  
|[-Detalhado](verbose.md)|Gera informações extras durante a compilação.|  
|[-warnaserror](warnaserror.md)|Promove avisos a erros.|  
|[-win32icon](win32icon.md)|Insere um arquivo. ico no arquivo de saída.|  
|[-win32manifest](win32manifest.md)|Identifica um arquivo de manifesto do aplicativo Win32 definido pelo usuário para ser inserido no arquivo executável portátil (PE) de um projeto.|  
|[-win32resource](win32resource.md)|Insere um recurso do Win32 no arquivo de saída.|  
  
## <a name="see-also"></a>Confira também

- [Opções de compilador do Visual Basic listadas por categoria](compiler-options-listed-by-category.md)
- [Gerenciar propriedades do projeto e da solução](/visualstudio/ide/managing-project-and-solution-properties)
