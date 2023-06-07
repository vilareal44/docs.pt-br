---
title: -keycontainer
ms.date: 03/10/2018
helpviewer_keywords:
- -keycontainer compiler option [Visual Basic]
- keycontainer compiler option [Visual Basic]
- /keycontainer compiler option [Visual Basic]
ms.assetid: 6a9bc861-1752-4db1-9f64-b5252f0482cc
ms.openlocfilehash: 575b337c262fbb36a9e118aa293916c296cc2db3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408556"
---
# <a name="-keycontainer"></a>-keycontainer
Especifica um nome de contêiner de chave para um par de chaves para dar a um assembly um nome forte.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-keycontainer:container  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`container`|Obrigatórios. Arquivo de contêiner que contém a chave. Coloque o nome do arquivo entre aspas ("") se o nome contiver um espaço.|  
  
## <a name="remarks"></a>Comentários  
 O compilador cria o componente compartilhável inserindo uma chave pública no manifesto do assembly e assinando o assembly final com a chave privada. Para gerar um arquivo de chave, digite `sn -k file` na linha de comando. A `-i` opção instala o par de chaves em um contêiner. Para obter mais informações, consulte [sn. exe (Strong Name Tool)](../../../framework/tools/sn-exe-strong-name-tool.md)).  
  
 Se você compilar with `-target:module` , o nome do arquivo de chave será mantido no módulo e incorporado ao assembly criado quando você compilar um assembly com o [módulo-](addmodule.md)Add.  
  
 Também é possível especificar essa opção como um atributo personalizado (<xref:System.Reflection.AssemblyKeyNameAttribute>) no código-fonte de qualquer módulo MSIL (Microsoft Intermediate Language).  
  
 Também é possível passar suas informações de criptografia para o compilador com [-keyfile](keyfile.md). Use [-delaysign](delaysign.md) se quiser um assembly parcialmente assinado.  
  
 Consulte [criando e usando assemblies de nome forte](../../../standard/assembly/create-use-strong-named.md) para obter mais informações sobre como assinar um assembly.  
  
> [!NOTE]
> A `-keycontainer` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.  
  
## <a name="example"></a>Exemplo  
 O código a seguir compila o arquivo `Input.vb` de origem e especifica um contêiner de chave.  
  
```console  
vbc -keycontainer:key1 input.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Assemblies no .NET](../../../standard/assembly/index.md)
- [Compilador de linha de comando do Visual Basic](index.md)
- [-keyfile](keyfile.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
