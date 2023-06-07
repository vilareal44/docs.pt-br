---
title: -warnaserror
ms.date: 03/13/2018
helpviewer_keywords:
- warnaserror compiler option [Visual Basic]
- /warnaserror compiler option [Visual Basic]
- -warnaserror compiler option [Visual Basic]
ms.assetid: 49819f1d-a1bd-4201-affe-5afe6d9712e1
ms.openlocfilehash: 94a8b43a891df9837925869e17fac4536a995264
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414266"
---
# <a name="-warnaserror-visual-basic"></a>-warnaserror (Visual Basic)
Faz com que o compilador trate a primeira ocorrência de um aviso como um erro.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-warnaserror[+ | -][:numberList]  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|+ &#124; -|Opcional. Por padrão, `-warnaserror-` está em vigor; os avisos não impedem que o compilador produza um arquivo de saída. A opção `-warnaserror`, que é o mesmo que `-warnaserror+`, faz com que os avisos sejam tratados como erros.|  
|`numberList`|Opcional. A lista delimitada por vírgulas dos números de identificação do aviso aos quais se aplica a opção `-warnaserror`. Se nenhuma identificação de aviso for especificada, a opção `-warnaserror` se aplica a todos os avisos.|  
  
## <a name="remarks"></a>Comentários  
 A opção `-warnaserror` trata todos os avisos como erros. Quaisquer mensagens que normalmente seriam relatadas como avisos são, em vez disso, relatadas como erros. O compilador relatará as ocorrências subsequentes do mesmo aviso como avisos.  
  
 Por padrão, `-warnaserror-` está em vigor, o que faz com que os avisos sejam apenas informativos. A opção `-warnaserror`, que é o mesmo que `-warnaserror+`, faz com que os avisos sejam tratados como erros.  
  
 Se você deseja que apenas avisos específicos sejam tratados como erros, pode especificar uma lista separada por vírgulas de números de aviso para serem tratados como erros.  
  
> [!NOTE]
> A opção `-warnaserror` não controla como os avisos são exibidos. Use a opção [-nowarn](nowarn.md) para desativar os avisos.  
  
|Para definir -warnaserror para tratar todos os avisos como erros no IDE do Visual Studio|  
|---|  
|1. ter um projeto selecionado em **Gerenciador de soluções**. No menu **Projeto** , clique em **Propriedades**. <br />2. Clique na guia **Compilar** .<br />3. Certifique-se de que a caixa de seleção **desabilitar todos os avisos** esteja desmarcada.<br />4. Marque a caixa de seleção **tratar todos os avisos como erros** .|  
  
|Para definir -warnaserror para tratar avisos específicos como erros no IDE do Visual Studio|  
|---|  
|1. ter um projeto selecionado em **Gerenciador de soluções**. No menu **Projeto** , clique em **Propriedades**.<br />2. Clique na guia **Compilar** .<br />3. Certifique-se de que a caixa de seleção **desabilitar todos os avisos** esteja desmarcada.<br />4. Verifique se a caixa de seleção **tratar todos os avisos como erros** está desmarcada.<br />5. Selecione o **erro** da coluna de **notificação** adjacente ao aviso que deve ser tratado como um erro.|  
  
## <a name="example"></a>Exemplo  
 O código a seguir compila `In.vb` e direciona o compilador para exibir um erro para a primeira ocorrência de cada aviso que ele encontre.  
  
```console
vbc -warnaserror in.vb  
```  
  
## <a name="example"></a>Exemplo  
 O código a seguir compila `T2.vb` e trata apenas o aviso de variáveis locais não utilizadas (42024) como um erro.  
  
```console
vbc -warnaserror:42024 t2.vb  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)
