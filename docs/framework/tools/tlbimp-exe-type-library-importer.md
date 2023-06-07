---
title: Tlbimp.exe (Importador de Biblioteca de Tipos)
description: Use Tlbimp.exe, o importador da biblioteca de tipos. Essa ferramenta converte definições de tipo encontradas em uma biblioteca de tipos COM em definições equivalentes em um assembly CLR.
ms.date: 03/30/2017
helpviewer_keywords:
- type libraries [.NET Framework], importing
- importing type library
- Tlbimp.exe
- type definition conversion
- Type Library Importer
- type libraries
- converting type definitions
ms.assetid: ec0a8d63-11b3-4acd-b398-da1e37e97382
ms.openlocfilehash: f1e50336e6c159ae56b393098868e4b8f5310b49
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87516990"
---
# <a name="tlbimpexe-type-library-importer"></a>Tlbimp.exe (Importador de Biblioteca de Tipos)
O Importador da Biblioteca de Tipos converte as definições de tipo encontradas dentro de uma biblioteca de tipos COM em definições equivalentes em um assembly do Common Language Runtime. A saída de Tlbimp.exe é um arquivo binário (um assembly) que contém os metadados do runtime para os tipos definidos na biblioteca de tipos original. É possível examinar esse arquivo com ferramentas como [Ildasm.exe](ildasm-exe-il-disassembler.md).  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
tlbimp tlbFile [options]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|*tlbFile*|O nome de qualquer arquivo que contenha uma biblioteca de tipos COM.|  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/asmversion:** *versionnumber*|Especifica o número de versão do assembly a ser produzido. Especifique *versionnumber* no formato *principal.secundária.build.revisão*.|  
|**/Company:**`companyinformation`|Adiciona informações da empresa ao assembly de saída.|  
|**/direitos autorais:**`copyrightinformation`|Adiciona informações de direito autoral ao assembly de saída. Essas informações podem ser exibidas na caixa de diálogo **Propriedades de arquivo** do assembly.|  
|**/delaysign**|Especifica Tlbimp.exe para assinar o assembly resultante com um nome forte usando a assinatura com atraso. Você deve especificar essa opção com a opção **/keycontainer:**, **/keyfile:** ou **/publickey:**. Para obter mais informações sobre o processo de assinatura com atraso, consulte [Assinatura com Atraso de um Assembly](../../standard/assembly/delay-sign.md).|  
|**/help**|Exibe sintaxe de comando e opções para a ferramenta.|  
|**/keycontainer:** *containername*|Assina o assembly resultante com um nome forte usando o par de chaves pública/privada encontrado no contêiner de chave especificado por *containername*.|  
|**/keyfile:** *filename*|Assina o assembly resultante com um nome forte usando o par de chaves pública/privada oficial do editor encontrado em *filename*.|  
|**/Machine:**`machinetype`|Cria um assembly com o tipo de computador especificado como alvo (microprocessador). Tipos de computador compatíveis: x86, x64, Itanium e Agnostic.|  
|**/namespace:** *namespace*|Especifica o namespace no qual produzir o assembly.|  
|**/noclassmembers**|Impede que Tlbimp.exe adicione membros a classes. Isso evita um <xref:System.TypeLoadException>em potencial.|  
|**/nologo**|Suprime a exibição do banner de inicialização da Microsoft.|  
|**/out:** *nome de arquivo*|Especifica o nome do arquivo de saída, o assembly e o namespace no qual gravar as definições de metadados. A opção **/out** não afetará o namespace do assembly se a biblioteca de tipos especificar o atributo personalizado de linguagem IDL que controla explicitamente o namespace do assembly. Se você não especificar essa opção, Tlbimp.exe gravará os metadados em um arquivo com o mesmo nome da biblioteca de tipos real definido no arquivo de entrada e o atribuirá a uma extensão .dll. Se o arquivo de saída tiver o mesmo nome do arquivo de entrada, a ferramenta gerará um erro para evitar a substituição da biblioteca de tipos.|  
|**/primary**|Produz um assembly de interoperabilidade primário para a biblioteca de tipos especificada. As informações são adicionadas ao assembly, o que indica que o editor da biblioteca de tipos produziu o assembly. Especificando um assembly de interoperabilidade primária, você diferencia o assembly de um editor de todos os outros assemblies criados na biblioteca de tipos usando Tlbimp.exe. Use a opção **/primary** somente se você for o editor da biblioteca de tipos que está sendo importado com Tlbimp.exe. Você deve assinar um assembly de interoperabilidade primário com um [nome forte](../../standard/assembly/strong-named.md). Para obter mais informações, consulte [Assemblies de Interoperabilidade Primários](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aax7sdch(v=vs.100)).|  
|**/Product:**`productinformation`|Adiciona informações do produto ao assembly de saída. Essas informações podem ser exibidas na caixa de diálogo **Propriedades de arquivo** do assembly.|  
|**/ProductVersion:**`productversioninformation`|Adiciona informações da versão ao assembly de saída. Não há restrições de formato. Essas informações podem ser exibidas na caixa de diálogo **Propriedades de arquivo** do assembly.|  
|**/publickey:** *filename*|Especifica o arquivo que contém a chave pública a ser usada na assinatura do assembly resultante. Se você especificar a opção **/keyfile:** ou **/keycontainer:** em vez de **/publickey:**, Tlbimp.exe gerará a chave pública do par de chaves pública/privada fornecido com **/keyfile:** ou **/keycontainer:**. A opção **/publickey:** dá suporte aos cenários de chave de teste e de assinatura com atraso. O arquivo está no formato gerado por Sn.exe. Para obter mais informações, consulte a opção **-p** de Sn.exe em [Ferramenta Nome Forte (Sn.exe)](sn-exe-strong-name-tool.md).|  
|**/reference:** *filename*|Especifica o arquivo do assembly a ser usado para resolver referências para tipos definidos fora da biblioteca de tipos atual. Se você não especificar a opção **/reference**, Tlbimp.exe importará recursivamente de maneira automática qualquer biblioteca de tipos externa a que a biblioteca de tipos está fazendo referência. Se você especificar a opção **/reference**, a ferramenta tentará resolver tipos externos nos assemblies referenciados antes de importar outras bibliotecas de tipos.|  
|**/silence:** `warningnumber`|Suprime a exibição do aviso especificado. Essa opção não pode ser usada com **/silent**.|  
|**/Silent**|Suprime a exibição de mensagens de sucesso. Essa opção não pode ser usada com **/silence**.|  
|**/strictref**|Não importará uma biblioteca de tipos se a ferramenta não puder resolver todas as referências dentro do assembly atual, os assemblies especificados com a opção **/reference** ou PIAs (assemblies de interoperabilidade primários).|  
|**/strictref:nopia**|Igual a **/strictref**, mas ignora PIAs.|  
|**/sysarray**|Especifica a ferramenta para importar um estilo COM SafeArray como um tipo gerenciado <xref:System.Array>.|  
|**/tlbreference:** *filename*|Especifica o arquivo de bibliotecas de tipos a ser usado para resolver referências da biblioteca de tipos sem consultar o Registro.<br /><br /> Essa opção não carregará alguns formatos da biblioteca de tipos anteriores.  No entanto, ainda é possível carregar formatos da biblioteca de tipos anteriores implicitamente por meio do Registro ou diretório atual.|  
|**/Trademark:**`trademarkinformation`|Adiciona informações de marca ao assembly de saída. Essas informações podem ser exibidas na caixa de diálogo **Propriedades de arquivo** do assembly.|  
|**/transform:** *transformname*|Transforma metadados conforme especificado pelo parâmetro *transformname*.<br /><br /> Especifique **dispret** para o parâmetro *transformname* para transformar parâmetros [out, retval] de métodos em interfaces apenas de expedição (dispinterfaces) em valores retornados.<br /><br /> Para obter mais informações sobre essa opção, consulte os exemplos mais à frente neste tópico.|  
|**/UNSAFE**|Produz interfaces sem verificações de segurança do .NET Framework. A chamada de um método exposto dessa maneira pode oferecer um risco à segurança. Você não deve usar essa opção, a menos que saiba os riscos de expor esse código.|  
|**/verbose**|Especifica o modo detalhado; exibe informações adicionais sobre a biblioteca de tipos importada.|  
|**/VariantBoolFieldToBool**|Converte campos `VARIANT_BOOL` em estruturas para <xref:System.Boolean>.|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
  
> [!NOTE]
> As opções de linha de comando para Tlbimp.exe não diferenciam maiúsculas de minúsculas e podem ser fornecidas em qualquer ordem. Você só precisa especificar o suficiente da opção para identificá-la com exclusividade. Portanto, **/n** é equivalente a **/nologo** e **/ou:** *outfile.dll* é equivalente a **/out:** *outfile.dll*.  
  
## <a name="remarks"></a>Comentários  
 Tlbimp.exe realiza conversões em uma biblioteca de tipos ao mesmo tempo. Não é possível usar a ferramenta para gerar informações de tipo para um subconjunto dos tipos definidos dentro de uma única biblioteca de tipos.  
  
 Costuma ser útil ou necessário atribuir [nomes fortes](../../standard/assembly/strong-named.md) a assemblies. Por isso, Tlbimp.exe inclui opções para fornecer as informações necessárias à geração dos assemblies fortemente nomeados. As opções **/keyfile:** e **/keycontainer:** assinam assemblies com nomes fortes. Por isso, é lógico fornecer apenas uma dessas opções por vez.  
  
 É possível especificar vários assemblies de referência usando a opção **/reference** várias vezes.

 Devido à maneira na qual Tlbimp.exe gera assemblies, não é possível redirecionar um assembly para uma versão `mscorlib` diferente. Por exemplo, se desejar gerar um assembly que direciona o .NET Framework 2.0, o Tlbimp.exe que acompanha o SDK do .NET Framework 2.0/3.0/3.5 deverá ser usado. Para direcionar o .NET Framework 4.x, o Tlbimp.exe que acompanha um SDK do .NET Framework 4.x deverá ser usado.

 Uma ID de recurso pode ser acrescentada a um arquivo da biblioteca de tipos durante a importação de uma biblioteca de tipos de um módulo que contém várias bibliotecas de tipos. Tlbimp.exe só poderá localizar esse arquivo se estiver no diretório atual ou se você especificar o caminho completo. Consulte o exemplo mais à frente neste tópico.  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir gera um assembly com o mesmo nome da biblioteca de tipos encontrada em `myTest.tlb` e com a extensão .dll.  
  
```console  
tlbimp myTest.tlb
```  
  
 O comando a seguir gera um assembly com o nome `myTest.dll`.  
  
```console  
tlbimp  myTest.tlb  /out:myTest.dll  
```  
  
 O comando a seguir gera um assembly com o mesmo nome da biblioteca de tipos especificada por `MyModule.dll\1` e com a extensão .dll. `MyModule.dll\1` deve estar no diretório atual.  
  
```console  
tlbimp MyModule.dll\1  
```  
  
 O comando a seguir gera um assembly com o nome `myTestLib.dll` para a biblioteca de tipos `TestLib.dll`. A opção **/transform:dispret** transforma todos os parâmetros [out, retval] de métodos em dispinterfaces na biblioteca de tipos em valores retornados na biblioteca gerenciada.  
  
```console  
tlbimp TestLib.dll /transform:dispret /out:myTestLib.dll  
```  
  
 A biblioteca de tipos `TestLib.dll`, no exemplo anterior, inclui um método dispinterface chamado `SomeMethod` que retorna nulo e tem um parâmetro [out, retval]. O código a seguir é a assinatura do método de biblioteca de tipos de entrada para `SomeMethod` em `TestLib.dll`.  
  
```cpp  
void SomeMethod([out, retval] VARIANT_BOOL*);  
```  
  
 Especificar a opção **/transform:dispret** faz Tlbimp.exe transformar o parâmetro `[out, retval]` de `SomeMethod` em um valor retornado `bool`. Veja a seguir a assinatura do método produzida por Tlbimp.exe para `SomeMethod` na biblioteca gerenciada `myTestLib.dll` quando a opção **/transform:dispret** é especificada.  
  
```csharp  
bool SomeMethod();  
```  
  
 Se você usar Tlbimp.exe para produzir uma biblioteca gerenciada para `TestLib.dll` sem especificar **/transform:dispret**, a ferramenta produzirá a assinatura do método a seguir para `SomeMethod` na biblioteca gerenciada `myTestLib.dll`.  
  
```csharp  
void SomeMethod(out bool x);  
```  
  
## <a name="see-also"></a>Consulte também

- [Ferramentas](index.md)
- [Tlbexp.exe (exportador de biblioteca de tipos)](tlbexp-exe-type-library-exporter.md)
- [Importando uma biblioteca de tipos como um assembly](../interop/importing-a-type-library-as-an-assembly.md)
- [Resumo da conversão de bibliotecas de tipos em assemblies](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))
- [Ildasm.exe (desmontador de IL)](ildasm-exe-il-disassembler.md)
- [Sn.exe (ferramenta de nome forte)](sn-exe-strong-name-tool.md)
- [Assemblies de nome forte](../../standard/assembly/strong-named.md)
- [Atributos para importar bibliotecas de tipos para assemblies de interoperabilidade](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/y6a7ak23(v=vs.100))
- [Prompts de comando](developer-command-prompt-for-vs.md)
