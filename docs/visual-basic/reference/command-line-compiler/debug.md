---
title: -debug
ms.date: 03/10/2018
helpviewer_keywords:
- debug compiler switches
- /debug compiler option [Visual Basic]
- -debug compiler option [Visual Basic]
- debug compiler option [Visual Basic]
ms.assetid: c2b0bea5-1d5e-499f-9bd5-4f6c6b715ea2
ms.openlocfilehash: 60c6e512a648f093bb9c70b5af86d5719e544adc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408719"
---
# <a name="-debug-visual-basic"></a>-debug (Visual Basic)

Faz com que o compilador gere informações de depuração e coloque-as nos arquivos de saída.

## <a name="syntax"></a>Sintaxe

```console
-debug[+ | -]
```

ou

```console
-debug:[full | pdbonly]
```

## <a name="arguments"></a>Argumentos

|Termo|Definição|
|---|---|
|`+` &#124; `-`|Opcional. Especificar `+` ou `-debug` faz com que o compilador gere informações de depuração e coloque-as em um arquivo. pdb. Especificar `-` tem o mesmo efeito que não especificar `-debug` .|
|`full` &#124; `pdbonly`|Opcional. Especifica o tipo de informações de depuração geradas pelo compilador. Se você não especificar `-debug:pdbonly` , o padrão é `full` , que permite anexar um depurador ao programa em execução. O `pdbonly` argumento permite a depuração de código-fonte quando o programa é iniciado no depurador, mas exibe o código de linguagem de assembly somente quando o programa em execução está anexado ao depurador.|

## <a name="remarks"></a>Comentários

Use essa opção para criar builds de depuração. Se você não especificar `-debug` , `-debug+` , ou `-debug:full` , não será possível depurar o arquivo de saída do programa.

Por padrão, as informações de depuração não são emitidas ( `-debug-` ). Para emitir informações de depuração, especifique `-debug` ou `-debug+` .

Para obter informações sobre como configurar o desempenho de depuração de um aplicativo, consulte [Facilitando a depuração de uma imagem](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md).

|Para definir-depurar no ambiente de desenvolvimento integrado do Visual Studio|
|---|
|1. com um projeto selecionado em **Gerenciador de soluções**, no menu **projeto** , clique em **Propriedades**. <br />2. Clique na guia **Compilar** .<br />3. clique em **Opções de compilação avançadas**.<br />4. modifique o valor na caixa **gerar informações de depuração** .|

## <a name="example"></a>Exemplo

O exemplo a seguir coloca as informações de depuração no arquivo de saída `App.exe` .

```console
vbc -debug -out:app.exe test.vb
```

## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-bugreport](bugreport.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
