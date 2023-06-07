---
title: Regasm.exe (Ferramenta de Registro de Assembly)
description: Use Regasm.exe, a ferramenta de registro do assembly. Ler metadados do assembly e adicionar as entradas necessárias ao registro, permitindo que clientes COM criem classes .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- Assembly Registration tool
- assemblies [.NET Framework], registering
- Regasm.exe
- registering assemblies
ms.assetid: e190e342-36ef-4651-a0b4-0e8c2c0281cb
ms.openlocfilehash: a3af6a0fbc7c9c695e18448da285977f04fd72ef
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517250"
---
# <a name="regasmexe-assembly-registration-tool"></a>Regasm.exe (Ferramenta de Registro de Assembly)

A ferramenta de Registro do Assembly lê os metadados dentro de um assembly e adiciona as entradas necessárias ao Registro, que permite que clientes COM para criar classes do .NET Framework de maneira transparente. Depois de uma classe ser registrada, qualquer cliente COM poderá usá-la, mesmo que a classe seja uma classe COM. A classe é registrada apenas uma vez, quando o assembly é instalado. As instâncias de classes dentro do assembly não poderão ser criadas com base em COM até serem efetivamente registradas.

Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio. Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).

No prompt de comando, digite o seguinte:

## <a name="syntax"></a>Sintaxe

```console
regasm assemblyFile [options]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------------|-----------------|
|*assemblyFile*|O assembly a ser registrado usando-se COM.|

|Opção|Descrição|
|------------|-----------------|
|**/codebase**|Cria uma entrada Codebase no Registro. A entrada codebase especifica o caminho do arquivo para um assembly que não está instalado no cache de assembly global. Não especifique essa opção se você instalar subseqüentemente o assembly que está registrando no cache de assembly global. O argumento *assemblyFile* especificado com a opção **/codebase** deve ser um [assembly de nome forte](../../standard/assembly/strong-named.md).|
|**/registered**|Especifica que essa ferramenta só fará referência a bibliotecas de tipos já registradas.|
|**/asmpath: diretório**|Especifica um diretório que contém referências de assembly. Deve ser usado com a opção **/regfile**.|
|**/nologo**|Suprime a exibição do banner de inicialização da Microsoft.|
|**/regfile** [**:** *regfile*]|Gera o arquivo .reg especificado para o assembly, que contém as entradas do Registro necessárias. A especificação dessa opção não altera o Registro. Não é possível usar essa opção com as opções **/u** ou **/tlb**.|
|**/Silent** ou **/s**|Suprime a exibição de mensagens de sucesso.|
|**/tlb** [**:** *TypeLibFile*]|Gera uma biblioteca de tipos com base no assembly especificado que contém definições dos tipos acessíveis definidos dentro do assembly.|
|**/unregister** ou **/u**|Cancela o registro das classes criáveis encontradas em *assemblyFile*. A omissão dessa opção faz Regasm.exe registrar as classes criáveis no assembly.|
|**/verbose**|Especifica o modo detalhado, exibe uma lista de todos os assemblies referenciados para os quais uma biblioteca de tipos precisa ser gerada, quando especificado com a opção **/tlb**.|
|**/?** ou **/Help**|Exibe sintaxe de comando e opções para a ferramenta.|

> [!NOTE]
> As opções de linha de comando de Regasm.exe não diferenciam maiúsculas de minúsculas. Você só precisa fornecer o suficiente da opção para identificá-la com exclusividade. Por exemplo, **/n** equivale a **/nologo** e **/t:** *outfile.tlb* equivale a **/tlb:** *outfile.tlb*.

## <a name="remarks"></a>Comentários

É possível usar a opção **/regfile** para gerar um arquivo .reg que contém as entradas do Registro em vez de fazer as alterações diretamente no Registro. É possível atualizar o Registro em um computador importando-se o arquivo .reg com a ferramenta Editor do Registro (Regedit.exe). O arquivo. reg não contém nenhuma atualização de registro que possa ser feita por funções de registro definidas pelo usuário. A opção **/regfile** só emite entradas de registro para classes gerenciadas. Essa opção não emite entradas para `TypeLibID`s ou `InterfaceID`S.

Quando você especifica a opção **/tlb**, Regasm.exe gera e registra uma biblioteca de tipos que descreve os tipos encontrados no assembly. Regasm.exe coloca as bibliotecas de tipos geradas no diretório de trabalho atual ou no diretório especificado para o arquivo de saída. A geração de uma biblioteca de tipos para um assembly que faça referência a outros assemblies pode fazer várias bibliotecas de tipos serem geradas de uma só vez. É possível usar a biblioteca de tipos para fornecer informações de tipo para ferramentas de desenvolvimento como o Visual Studio. Não use a opção **/tlb** se o assembly que você está registrando foi produzido pelo tipo importador da biblioteca de tipos ([Tlbimp.exe](tlbimp-exe-type-library-importer.md)). Não é possível exportar uma biblioteca de tipos com base em um assembly que foi importado de uma biblioteca de tipos. O uso da opção **/tlb** tem o mesmo efeito do uso do Exportador da Biblioteca de Tipos ([Tlbexp.exe](tlbexp-exe-type-library-exporter.md)) e de Regasm.exe, com a exceção de Tlbexp.exe não registra a biblioteca de tipos produzida.  Se você usar a opção **/tlb** para registrar uma biblioteca de tipos, poderá usar a opção **/tlb** com a opção **/Unregister** para cancelar o registro da biblioteca de tipos. O uso das duas opções juntas cancelará o registro das entradas da biblioteca de tipos e da interface, que podem limpar o Registro consideravelmente.

Quando você registra um assembly a ser usado pelo COM, Regasm.exe adiciona entradas ao Registro no computador local. Mais especificamente, ele cria chaves do Registro que dependem da versão que permitem a execução de várias versões do mesmo assembly lado a lado em um computador. Na primeira vez que um assembly é registrado, uma chave de nível superior é criada para o assembly e uma subchave exclusiva é criada para a versão específica. Sempre que você registra uma nova versão do assembly, Regasm.exe cria uma subchave para a nova versão.

Por exemplo, leve em consideração um cenário em que você registra o componente gerenciado, myComp.dll, versão 1.0.0.0 a ser usado por COM. Posteriormente, você registra myComp.dll, versão 2.0.0.0. Você determina que todos os aplicativos cliente COM no computador estão usando myComp.dll versão 2.0.0.0 e opta por cancelar o registro de myComponent.dll versão 1.0.0.0. Esse esquema do Registro permite cancelar o registro de myComp.dll versão 1.0.0.0 porque apenas a subchave versão 1.0.0.0 é removida.

Depois de registrar um assembly usando Regasm.exe, você poderá instalá-lo no [cache de assembly global](../app-domains/gac.md) de forma que possa ser ativado de qualquer cliente COM. Se o assembly for ativado apenas por um único aplicativo, será possível colocá-lo no diretório desse aplicativo.

## <a name="examples"></a>Exemplos

O comando a seguir registra todas as classes públicas contidas em `myTest.dll`.

```console
regasm myTest.dll
```

O comando a seguir gera o arquivo `myTest.reg`, que contém todas as entradas do Registro necessárias. Esse comando não atualiza o Registro.

```console
regasm myTest.dll /regfile:myTest.reg
```

O comando a seguir registra todas as classes públicas contidas em `myTest.dll`, além de gerar e registrar a biblioteca de tipos `myTest.tlb`, que contém definições de todos os tipos públicos definidos em `myTest.dll`.

```console
regasm myTest.dll /tlb:myTest.tlb
```

## <a name="see-also"></a>Consulte também

- [Ferramentas](index.md)
- [Tlbexp.exe (exportador de biblioteca de tipos)](tlbexp-exe-type-library-exporter.md)
- [Tlbimp.exe (tipo de importador de biblioteca de tipos)](tlbimp-exe-type-library-importer.md)
- [Registrando assemblies com o COM](../interop/registering-assemblies-with-com.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
