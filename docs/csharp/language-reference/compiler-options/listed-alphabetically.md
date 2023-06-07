---
title: Opções do compilador de C# listadas em ordem alfabética
ms.date: 06/04/2020
helpviewer_keywords:
- compiler options [C#], listed alphabetically
- C# language, compiler options listed alphabetically
- Visual C# compiler, options listed alphabetically
- Visual C#, compiler options listed alphabetically
ms.assetid: 43535ea0-ca47-4a15-b528-615087a86092
ms.openlocfilehash: eb3a591ba7b58e187eb03e65a3da6dfb47c9475c
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86473975"
---
# <a name="c-compiler-options-listed-alphabetically"></a>Opções do compilador de C# listadas em ordem alfabética

As opções do compilador a seguir estão em ordem alfabética. Para obter uma lista categórica, consulte [Opções de Compilador de C# Listadas por Categoria](listed-by-category.md).

|Opção|Finalidade|
|------------|-------------|
|[@](response-file-compiler-option.md)|Lê um arquivo de resposta para obter mais opções.|
|[-?](help-compiler-option.md)|Exibe uma mensagem de uso para stdout.|
|-additionalfile|Nomeia outros arquivos que não afetam diretamente a geração de código, mas podem ser usados por analisadores para produzir erros ou avisos.|
|[-addmodule](addmodule-compiler-option.md)|Vincula os módulos especificados nesse assembly|
|-analyzer|Executar os analisadores com basse nesse assembly (forma abreviada: -a)|
|[-appconfig](appconfig-compiler-option.md)|Especifica o local do app.config em tempo de associação do assembly.|
|[-baseaddress](baseaddress-compiler-option.md)|Especifica o endereço básico para a biblioteca a ser criada.|
|[-bugreport](bugreport-compiler-option.md)|Cria um arquivo 'Bug Report'. Esse arquivo será enviado junto com informações de falha se for usado com -errorreport:prompt ou -errorreport:send.|
|[-checked](checked-compiler-option.md)|Faz com que o compilador gere verificações de estouro.|
|-checksumalgorithm:\<alg>|Especifica o algoritmo para calcular a soma de verificação do arquivo de origem armazenada no PDB.  Os valores com suporte são: SHA256 (padrão) ou SHA1.<br>Em razão de problemas de colisão com SHA1, a Microsoft recomenda SHA256. |
|[-página de código](codepage-compiler-option.md)|Especifica a página de código a ser usada ao abrir arquivos de origem.|
|[-Depurar](debug-compiler-option.md)|Emite informações de depuração.|
|[-define](define-compiler-option.md)|Define símbolos de compilação condicional.|
|[-delaysign](delaysign-compiler-option.md)|Atrasa a assinatura do assembly usando somente a parte pública da chave de nome forte.|
|[-determinístico](deterministic-compiler-option.md)|Faz com que o compilador gere um assembly de conteúdo binário idêntico entre compilações se as entradas são idênticas.|
|[-doc](doc-compiler-option.md)|Especifica um arquivo de documentação XML a ser gerado.|
|-embed|Insere todos os arquivos de origem no PDB.|
|Inser\<file list>|Insere arquivos específicos no PDB.|
|-errorendlocation|Coluna e linha de saída do local final de cada erro.|
|erros\<file>|Especifique um arquivo para registrar todos os diagnósticos do compilador e do analisador.|
|[-errorreport](errorreport-compiler-option.md)|Especifica como tratar erros internos do compilador: prompt, send ou none. O padrão é none.|
|[-filealign](filealign-compiler-option.md)|Especifica o alinhamento usado em seções de arquivo de saída.|
|[-fullpaths](fullpaths-compiler-option.md)|Faz com que o compilador gere caminhos totalmente qualificados.|
|[-ajuda](help-compiler-option.md)|Exibe uma mensagem de uso para stdout.|
|[-highentropyva](highentropyva-compiler-option.md)|Especifica que há suporte para ASLR de alta entropia.|
|-incremental|Habilita a compilação incremental [obsoleta].|
|[-keycontainer](keycontainer-compiler-option.md)|Especifica um contêiner de chave de nome forte.|
|[-keyfile](keyfile-compiler-option.md)|Especifica um arquivo de chave de nome forte.|
|[-langversion:\<string>](langversion-compiler-option.md)|Especificar a versão da linguagem: padrão, ISO-1, ISO-2, 3, 4, 5, 6, 7, 7.1, 7.2, 7.3 ou mais recente |
|[-lib](lib-compiler-option.md)|Especifica diretórios adicionais para pesquisar referências.|
|[-link](link-compiler-option.md)|Disponibiliza informações de tipo COM em assemblies especificados para o projeto.|
|[-linkresource](linkresource-compiler-option.md)|Vincula o recurso especificado a esse assembly.|
|[-main](main-compiler-option.md)|Especifica o tipo que contém o ponto de entrada (ignorar todos os outros pontos de entrada possíveis).|
|[-moduleassemblyname](moduleassemblyname-compiler-option.md)|Especifica um assembly cujos tipos não públicos podem ser acessados por um .netmodule.|
|ModuleName\<string>|Especificar o nome do módulo de origem|
|[-noconfig](noconfig-compiler-option.md)|Instrui o compilador a não incluir automaticamente o arquivo CSC.RSP.|
|[-nologo](nologo-compiler-option.md)|Suprime a mensagem de direitos autorais do compilador.|
|[-nostdlib](nostdlib-compiler-option.md)|Instrui o compilador a não referenciar a biblioteca padrão (mscorlib.dll).|
|[-nowarn](nowarn-compiler-option.md)|Desabilita mensagens de aviso específicas|
|[-nowin32manifest](nowin32manifest-compiler-option.md)|Instrui o compilador não inserir um manifesto de aplicativo no arquivo executável.|
|[– permite valor nulo](nullable-compiler-option.md)|Especifica a opção de contexto anulável.|
|[-otimizar](optimize-compiler-option.md)|Habilita/desabilita otimizações.|
|[-out](out-compiler-option.md)|Especifica o nome do arquivo de saída (padrão: nome base do arquivo com classe main ou primeiro arquivo).|
|-parallel[+&#124;-]|Especifica se deve o build simultâneo deve ser usado (+).|
|[-pathmap](pathmap-compiler-option.md)|Especifica um mapeamento para os nomes de caminho de origem emitidos pelo compilador.|
|[-pdb](pdb-compiler-option.md)|Especifica o nome de arquivo e a localização do arquivo .pdb.|
|[-plataforma](platform-compiler-option.md)|Limita as plataformas em que este código pode ser executado: x86, Itanium, x64, anycpu ou anycpu32bitpreferred. O padrão é anycpu.|
|[-preferreduilang](preferreduilang-compiler-option.md)|Especifica o idioma a ser usado para a saída do compilador.|
|[-publicsign](publicsign-compiler-option.md)|Aplicar uma chave pública sem assinar o assembly, mas definir o bit no assembly indicando que o assembly está assinado.|
|[-recurse](recurse-compiler-option.md)|Inclui todos os arquivos no diretório e subdiretórios atuais de acordo com as especificações curinga.|
|[-referência](reference-compiler-option.md)|Referencia metadados dos arquivos do assembly especificado.|
|[-refout](refout-compiler-option.md)|Gere um assembly de referência além de um assembly principal.|
|[-refonly](refonly-compiler-option.md)|Gere um assembly de referência em vez de um assembly principal.|
|-reportanalyzer|Reporte informações adicionais do analisador, como o tempo de execução.|
|[-recurso](resource-compiler-option.md)|Insere o recurso especificado.|
|regras\<file>|Especifique um arquivo de conjunto de regras que desabilita o diagnóstico específico.|
|[-subsystemversion](subsystemversion-compiler-option.md)|Especifica a versão mínima do subsistema que o arquivo executável pode usar.|
|[-destino](target-compiler-option.md)|Especifica o formato do arquivo de saída usando uma das seguintes opções: [-target: appcontainerexe](target-appcontainerexe-compiler-option.md), [-target: exe](target-exe-compiler-option.md), [-target: library](target-library-compiler-option.md), [-target: módulo](target-module-compiler-option.md), [-target: winexe](target-winexe-compiler-option.md), [-target: winmdobj](target-winmdobj-compiler-option.md).|
|[-unsafe](unsafe-compiler-option.md)|Permite o código [não seguro](../keywords/unsafe.md).|
|[-utf8output](utf8output-compiler-option.md)|Produz mensagens do compilador em codificação UTF-8.|
|-version|Exiba o número de versão do compilador e saia.|
|[-warn](warn-compiler-option.md)|Define o nível de aviso (0-4).|
|[-warnaserror](warnaserror-compiler-option.md)|Relata avisos específicos como erros.|
|[-win32icon](win32icon-compiler-option.md)|Usa este ícone para saída.|
|[-win32manifest](win32manifest-compiler-option.md)|Especifica um arquivo de manifesto win32 personalizado.|
|[-win32res](win32res-compiler-option.md)|Especifica um arquivo de recurso win32 (.res).|

## <a name="see-also"></a>Consulte também

- [Opções do compilador C#](index.md)
- [Opções do compilador de C# listadas por categoria](listed-by-category.md)
- [Como configurar variáveis de ambiente para a linha de comando do Visual Studio](how-to-set-environment-variables-for-the-visual-studio-command-line.md)
- [\<compiler>Elementos](../../../framework/configure-apps/file-schema/compiler/compiler-element.md)
