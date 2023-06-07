---
title: -keyfile
ms.date: 03/10/2018
helpviewer_keywords:
- /keyfile compiler option [Visual Basic]
- keyfile compiler option [Visual Basic]
- -keyfile compiler option [Visual Basic]
ms.assetid: ffa82a4b-517a-4c6c-9889-5bae7b534bb8
ms.openlocfilehash: 3f476f6b6db1a788002a938eb5ae4bbbed7a5dae
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408569"
---
# <a name="-keyfile"></a>-keyfile
Especifica um arquivo que contém uma chave ou um par de chaves para fornecer um nome forte ao assembly.  
  
## <a name="syntax"></a>Sintaxe  
  
```console
-keyfile:file  
```  
  
## <a name="arguments"></a>Argumentos  
 `file`  
 Obrigatórios. Arquivo que contém a chave. Se o nome do arquivo contiver um espaço, coloque o nome entre aspas ("").  
  
## <a name="remarks"></a>Comentários  
 O compilador insere a chave pública no manifesto do assembly e, em seguida, assina o assembly final com a chave privada. Para gerar um arquivo de chave, digite `sn -k file` na linha de comando. Para obter mais informações, consulte [sn. exe (Strong Name Tool)](../../../framework/tools/sn-exe-strong-name-tool.md)).  
  
 Se você compilar with `-target:module` , o nome do arquivo de chave será mantido no módulo e incorporado ao assembly criado quando você compilar um assembly com o [módulo-](addmodule.md)Add.  
  
 Também é possível passar suas informações de criptografia para o compilador com [-keycontainer](keycontainer.md). Use [-delaysign](delaysign.md) se quiser um assembly parcialmente assinado.  
  
 Você também pode especificar essa opção como um atributo personalizado ( <xref:System.Reflection.AssemblyKeyFileAttribute> ) no código-fonte para qualquer módulo de linguagem intermediária da Microsoft.  
  
 Caso both `-keyfile` e [-keycontainer](keycontainer.md) sejam especificados (por opção de linha de comando ou por atributo personalizado) na mesma compilação, o compilador primeiro tenta o contêiner de chave. Se isso ocorrer, o assembly será assinado com as informações no contêiner de chaves. Se o compilador não encontrar o contêiner de chave, ele tentará o arquivo especificado com `-keyfile` . Se isso for bem sucedido, o assembly será assinado com as informações no arquivo de chave e as informações de chave serão instaladas no contêiner de chave (semelhante a `sn -i` ) para que, na próxima compilação, o contêiner de chave seja válido.  
  
 Observe que um arquivo de chave pode conter somente a chave pública.  
  
 Consulte [criando e usando assemblies de nome forte](../../../standard/assembly/create-use-strong-named.md) para obter mais informações sobre como assinar um assembly.  
  
> [!NOTE]
> A `-keyfile` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.

## <a name="example"></a>Exemplo

O código a seguir compila o arquivo `Input.vb` de origem e especifica um arquivo de chave.

```console
vbc -keyfile:myfile.sn input.vb
```

## <a name="see-also"></a>Confira também

- [Assemblies no .NET](../../../standard/assembly/index.md)
- [Compilador de linha de comando do Visual Basic](index.md)
- [-referência (Visual Basic)](reference.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
