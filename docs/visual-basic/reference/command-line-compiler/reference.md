---
title: -reference
ms.date: 03/13/2018
helpviewer_keywords:
- /reference compiler option [Visual Basic]
- r compiler option [Visual Basic]
- -reference compiler option [Visual Basic]
- /r compiler option [Visual Basic]
- reference compiler option [Visual Basic]
- -r compiler option [Visual Basic]
ms.assetid: 66bdfced-bbf6-43d1-a554-bc0990315737
ms.openlocfilehash: 633b457106203e213f5d30003e576b7e8132f4d2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400481"
---
# <a name="-reference-visual-basic"></a>-referência (Visual Basic)
Faz com que o compilador transforme informações de tipo nos assemblies especificados disponíveis para o projeto que você está compilando no momento.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-reference:fileList  
```

ou

```console
-r:fileList  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`fileList`|Obrigatórios. Lista delimitada por vírgulas de nomes de arquivo do assembly. Se o nome do arquivo contém um espaço, coloque o nome entre aspas.|  
  
## <a name="remarks"></a>Comentários  
 Os arquivos que você importar devem conter metadados de assembly. Somente tipos públicos são visíveis fora do assembly. A opção [-addmodule](addmodule.md) importa os metadados de um módulo.  
  
 Se você referenciar um assembly (assembly A) que, por sua vez, faz referência a outro assembly (assembly B), você precisará referenciar o assembly B se:  
  
- Um tipo do Assembly A herda de um tipo ou implementa uma interface do Assembly B.  
  
- Um campo, propriedade, evento ou método que tem um tipo de retorno ou de parâmetro do Assembly B é invocado.  
  
 Use [-LIBPATH](libpath.md) para especificar o diretório no qual uma ou mais das suas referências de assembly estão localizadas.  
  
 Para que o compilador reconheça um tipo em um assembly (não um módulo), ele deve ser forçado a resolver o tipo. Um exemplo de como você pode fazer isso é definir uma instância do tipo. Outras maneiras estão disponíveis para resolver nomes de tipo em um assembly para o compilador. Por exemplo, se você herdar de um tipo em um assembly, o nome do tipo será conhecido pelo compilador.  
  
 O arquivo de resposta Vbc. rsp, que faz referência a assemblies de .NET Framework comumente usados, é usado por padrão. Use `-noconfig` se você não quiser que o compilador use Vbc. rsp.  
  
 A forma abreviada de `-reference` é `-r`.  
  
## <a name="example"></a>Exemplo  
 O comando a seguir compila o arquivo `Input.vb` de origem e os assemblies de referência de `Metad1.dll` e `Metad2.dll` para produzir `Out.exe` .  
  
```console
vbc -reference:metad1.dll,metad2.dll -out:out.exe input.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-noconfig](noconfig.md)
- [-Target (Visual Basic)](target.md)
- [Pública](../../language-reference/modifiers/public.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
