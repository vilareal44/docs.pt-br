---
title: Opções de compilador listadas por categoria
ms.date: 04/12/2018
helpviewer_keywords:
- Visual Basic compiler, options
ms.assetid: fbe36f7a-7cfa-4f77-a8d4-2be5958568e3
ms.openlocfilehash: 533d3da2f76854d311262ce97b43f240acab5f7d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408745"
---
# <a name="visual-basic-compiler-options-listed-by-category"></a>Visual Basic opções de compilador listadas por categoria
O compilador de linha de comando Visual Basic é fornecido como uma alternativa à compilação de programas de dentro do ambiente de desenvolvimento integrado (IDE) do Visual Studio. Veja a seguir uma lista de Visual Basic opções do compilador de linha de comando classificadas por categoria funcional.  

[!INCLUDE[compiler-options](~/includes/compiler-options.md)]
  
## <a name="compiler-output"></a>Saída do compilador  
  
|Opção|Finalidade|  
|---|---|  
|[-nologo](nologo.md)|Suprime as informações da faixa do compilador.|  
|[-utf8output](utf8output.md)|Exibe a saída do compilador usando a codificação UTF-8.|  
|[-Detalhado](verbose.md)|Gera informações extras durante a compilação.|  
|`-modulename:<string>`|Especificar o nome do módulo de origem|  
|[-preferreduilang](../../../csharp/language-reference/compiler-options/preferreduilang-compiler-option.md)|Especifique uma linguagem para a saída do compilador.|
  
## <a name="optimization"></a>Optimization  
  
|Opção|Finalidade|  
|---|---|  
|[-filealign](filealign.md)|Especifica onde alinhar as seções do arquivo de saída.|  
|[-otimizar](optimize.md)|Habilita/desabilita otimizações.|  
  
## <a name="output-files"></a>Arquivos de saída  
  
|Opção|Finalidade|  
|---|---|  
|[-doc](doc.md)|Processa comentários de documentação em um arquivo XML.|  
|[-deterministic](deterministic.md)|Faz com que o compilador gere um assembly de conteúdo binário idêntico entre compilações se as entradas são idênticas.|
|[-netcf](netcf.md)|Define o compilador para o .NET Compact Framework de destino.|  
|[-out](out.md)|Especifica um arquivo de saída.|  
|[-refonly](refonly-compiler-option.md)|Gera apenas um assembly de referência.|
|[-refout](refout-compiler-option.md)|Especifica o caminho de saída de um assembly de referência.|
|[-target](target.md)|Especifica o formato da saída.|  
  
## <a name="net-assemblies"></a>Assemblies .NET  
  
|Opção|Finalidade|  
|---|---|  
|[-addmodule](addmodule.md)|Faz com que o compilador verifique todos os tipos de informações de arquivos especificados disponíveis para o projeto que você está compilando.|  
|[-delaysign](delaysign.md)|Especifica se o assembly será assinado total ou parcialmente.|  
|[-imports](imports.md)|Importa um namespace de um assembly especificado.|  
|[-keycontainer](keycontainer.md)|Especifica um nome de contêiner de chave para um par de chaves para dar a um assembly um nome forte.|  
|[-keyfile](keyfile.md)|Especifica um arquivo que contém uma chave ou um par de chaves para fornecer um nome forte ao assembly.|  
|[-libpath](libpath.md)|Especifica o local dos assemblies referenciados pela opção [-Reference](reference.md) .|  
|[-referência](reference.md)|Importa metadados de um assembly.|  
|[-moduleassemblyname](moduleassemblyname.md)|Especifica o nome do assembly do qual um módulo fará parte.|  
|`-analyzer`|Executar os analisadores com basse nesse assembly (forma abreviada: -a)|  
|`-additionalfile`|Nomeia outros arquivos que não afetam diretamente a geração de código, mas podem ser usados por analisadores para produzir erros ou avisos.|  
  
## <a name="debuggingerror-checking"></a>Depuração/verificação de erros  
  
|Opção|Finalidade|  
|---|---|  
|[-bugreport](bugreport.md)|Cria um arquivo que contém informações que tornam mais fácil relatar um bug.|  
|[-Depurar](debug.md)|Produz informações de depuração.|  
|[-nowarn](nowarn.md)|Suprime a capacidade do compilador de gerar avisos.|  
|[-quiet](quiet.md)|Impede que o compilador exiba código para erros e avisos relacionados à sintaxe.|  
|[-removeintchecks](removeintchecks.md)|Desabilita a verificação de estouro de inteiros.|  
|[-warnaserror](warnaserror.md)|Promove avisos a erros.|  
|`-ruleset:<file>`|Especifique um arquivo de conjunto de regras que desabilita o diagnóstico específico.|  
  
## <a name="help"></a>Ajuda  
  
|Opção|Finalidade|  
|---|---|  
|[-?](help.md)|Exibe as opções do compilador. Esse comando é o mesmo que especificar a `-help` opção. Nenhuma compilação ocorre.|  
|[-ajuda](help.md)|Exibe as opções do compilador. Esse comando é o mesmo que especificar a `-?` opção. Nenhuma compilação ocorre.|  
  
## <a name="language"></a>Idioma  
  
|Opção|Finalidade|  
|---|---|  
|[-langversion](langversion.md)|Especifique a versão do idioma: 9&#124;9,0&#124;10&#124;10,0&#124;11&#124;11,0.|  
|[-optionexplicit](optionexplicit.md)|Impõe uma declaração explícita de variáveis.|  
|[-optionstrict](optionstrict.md)|Impõe semântica de tipo estrito.|  
|[-optioncompare](optioncompare.md)|Especifica se as comparações de cadeia de caracteres devem ser binárias ou usar a semântica de texto específica à localidade.|  
|[-optioninfer](optioninfer.md)|Permite o uso de inferência de tipo local nas declarações de variáveis.|  
  
## <a name="preprocessor"></a>Pré-processador  
  
|Opção|Finalidade|  
|---|---|  
|[-define](define.md)|Define os símbolos para a compilação condicional.|  
  
## <a name="resources"></a>Recursos  
  
|Opção|Finalidade|  
|---|---|  
|[-linkresource](linkresource.md)|Cria um link a um recurso gerenciado.|  
|[-recurso](resource.md)|Insere um recurso gerenciado em um assembly.|  
|[-win32icon](win32icon.md)|Insere um arquivo. ico no arquivo de saída.|  
|[-win32resource](win32resource.md)|Insere um recurso do Win32 no arquivo de saída.|  
  
## <a name="miscellaneous"></a>Diversos  
  
|Opção|Finalidade|  
|---|---|  
|[@ (especificar arquivo de resposta)](specify-response-file.md)|Especifica um arquivo de resposta.|  
|[-baseaddress](baseaddress.md)|Especifica o endereço base de uma DLL.|  
|[-codepage](codepage.md)|Especifica a página de código a ser usada para todos os arquivos de código-fonte na compilação.|  
|[-errorreport](errorreport.md)|Especifica como o compilador de Visual Basic deve relatar erros internos do compilador.|  
|[-highentropyva](highentropyva.md)|Informa ao kernel do Windows se um executável específico dá suporte a alta (randomização de layout de espaço de endereço) de entropia.|  
|[-main](main.md)|Especifica a classe que contém o `Sub Main` procedimento a ser usado na inicialização.|  
|[-noconfig](noconfig.md)|Não compilar com Vbc. rsp|  
|[-nostdlib](nostdlib.md)|Faz com que o compilador não referencie as bibliotecas padrão.|  
|[-nowin32manifest](nowin32manifest.md)|Instrui o compilador a não inserir nenhum manifesto de aplicativo no arquivo executável.|  
|[-plataforma](platform.md)|Especifica a plataforma do processador que o compilador tem como destino para o arquivo de saída.|  
|[-recurse](recurse.md)|Pesquisa em subdiretórios arquivos de código-fonte a serem compilados.|  
|[-rootnamespace](rootnamespace.md)|Especifica um namespace para todas as declarações de tipo.|  
|[-sdkpath](sdkpath.md)|Especifica o local de mscorlib. dll e Microsoft. VisualBasic. dll.|  
|[-vbruntime](vbruntime.md)|Especifica que o compilador deve compilar sem uma referência para a biblioteca de tempo de execução Visual Basic ou com uma referência a uma biblioteca de tempo de execução específica.|  
|[-win32manifest](win32manifest.md)|Identifica um arquivo de manifesto do aplicativo Win32 definido pelo usuário para ser inserido no arquivo executável portátil (PE) de um projeto.|  
|`-parallel[+&#124;-]`|Especifica se deve o build simultâneo deve ser usado (+).|  
|`-checksumalgorithm:<alg>`|Especifique o algoritmo para calcular a soma de verificação do arquivo de origem armazenada no PDB.  Os valores com suporte são: SHA1 (padrão) ou SHA256. <br>Devido a problemas de colisão com o SHA1, a Microsoft recomenda SHA256 ou melhor.|  
  
## <a name="see-also"></a>Confira também

- [Opções do compilador do Visual Basic listadas em ordem alfabética](compiler-options-listed-alphabetically.md)
- [Gerenciar propriedades do projeto e da solução](/visualstudio/ide/managing-project-and-solution-properties)
