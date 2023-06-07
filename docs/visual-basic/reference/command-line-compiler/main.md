---
title: -main
ms.date: 03/13/2018
helpviewer_keywords:
- main compiler option [Visual Basic]
- /main compiler option [Visual Basic]
- -main compiler option [Visual Basic]
ms.assetid: 83fc339d-6652-415d-b205-b5133319b5b0
ms.openlocfilehash: 5530da4c784346df4a1088998b8d2027feee08e3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403155"
---
# <a name="-main"></a>-main
Especifica a classe ou o módulo que contém o procedimento `Sub Main`.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-main:location  
```  
  
## <a name="arguments"></a>Argumentos  
 `location`  
 Obrigatórios. O nome da classe ou do módulo que contém o `Sub Main` procedimento a ser chamado quando o programa for iniciado. Isso pode estar no formato **-principal: módulo** ou **-principal: namespace. Module**.  
  
## <a name="remarks"></a>Comentários  
 Use essa opção quando você criar um arquivo executável ou programa executável do Windows. Se a opção **-Main** for omitida, o compilador pesquisará um compartilhamento válido `Sub Main` em todos os módulos e classes públicas.  
  
 Consulte o [procedimento principal no Visual Basic](../../programming-guide/program-structure/main-procedure.md) para obter uma discussão sobre as várias formas do `Main` procedimento.  
  
 Quando `location` é uma classe que herda de <xref:System.Windows.Forms.Form> , o compilador fornece um `Main` procedimento padrão que inicia o aplicativo se a classe não tiver nenhum `Main` procedimento. Isso permite que você compile o código na linha de comando que foi criada no ambiente de desenvolvimento.  
  
 [!code-vb[VbVbalrCompiler#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/Class1.vb#16)]  
  
### <a name="to-set--main-in-the-visual-studio-integrated-development-environment"></a>Para definir o principal no ambiente de desenvolvimento integrado do Visual Studio  
  
1. Selecione um projeto no **Gerenciador de Soluções**. No menu **Projeto** , clique em **Propriedades**.  
  
2. Clique na guia **Aplicativo**.  
  
3. Verifique se a caixa de seleção **habilitar estrutura de aplicativo** não está marcada.  
  
4. Modifique o valor na caixa **objeto de inicialização** .  
  
## <a name="example"></a>Exemplo  
 O código a seguir compila `T2.vb` e `T3.vb` , especificando que o `Sub Main` procedimento será encontrado na `Test2` classe.  
  
```console
vbc t2.vb t3.vb -main:Test2  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-Target (Visual Basic)](target.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [Procedimento principal no Visual Basic](../../programming-guide/program-structure/main-procedure.md)
