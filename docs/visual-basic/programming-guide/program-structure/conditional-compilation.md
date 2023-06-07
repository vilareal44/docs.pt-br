---
title: Compilação condicional
ms.date: 07/20/2015
helpviewer_keywords:
- conditional compilation [Visual Basic], about conditional compilation
- compilation [Visual Basic], conditional
ms.assetid: 9c35e55e-7eee-44fb-a586-dad1f1884848
ms.openlocfilehash: c3eb1eb57b3d76e762ed53edb3b168ad96abec39
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403259"
---
# <a name="conditional-compilation-in-visual-basic"></a>Compilação condicional no Visual Basic
Na *compilação condicional*, blocos específicos de código em um programa são compilados seletivamente enquanto outros são ignorados.  
  
 Por exemplo, você pode querer escrever instruções de depuração que comparam a velocidade de diferentes abordagens para a mesma tarefa de programação ou pode desejar localizar um aplicativo para vários idiomas. As instruções de compilação condicional são projetadas para serem executadas durante o tempo de compilação, não em tempo de execução.  
  
 Você denota que blocos de código sejam compilados condicionalmente com a `#If...Then...#Else` diretiva. Por exemplo, para criar versões de idioma francês e alemão do mesmo aplicativo a partir do mesmo código-fonte, você insere segmentos de código específicos da plataforma em `#If...Then` instruções usando as constantes predefinidas `FrenchVersion` e `GermanVersion` . O exemplo a seguir demonstra como:  
  
 [!code-vb[VbVbalrConditionalComp#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#5)]  
  
 Se você definir o valor da `FrenchVersion` constante de compilação condicional como `True` em tempo de compilação, o código condicional para a versão em francês será compilado. Se você definir o valor da `GermanVersion` constante como `True` , o compilador usará a versão em alemão. Se nenhum for definido como `True` , o código no último `Else` bloco será executado.  
  
> [!NOTE]
> O preenchimento automático não funcionará ao editar o código e usar diretivas de compilação condicionais se o código não fizer parte do Branch atual.  
  
## <a name="declaring-conditional-compilation-constants"></a>Declarando constantes de compilação condicional  
 Você pode definir constantes de compilação condicional de uma das três maneiras:  
  
- No **Designer de projeto**  
  
- Na linha de comando ao usar o compilador de linha de comando  
  
- Em seu código  
  
 As constantes de compilação condicional têm um escopo especial e não podem ser acessadas do código padrão. O escopo de uma constante de compilação condicional depende da maneira como ela é definida. A tabela a seguir lista o escopo de constantes declaradas usando cada uma das três formas mencionadas acima.  
  
|Como a constante é definida|Escopo da constante|  
|---|---|  
|**Designer de Projeto**|Público para todos os arquivos no projeto|  
|Linha de comando|Público para todos os arquivos passados para o compilador de linha de comando|  
|`#Const`instrução no código|Privado para o arquivo no qual ele está declarado|  
  
|Para definir constantes no designer de projeto|  
|---|  
|-Antes de criar o arquivo executável, defina constantes no **Designer de projeto** seguindo as etapas fornecidas em [Gerenciando as propriedades do projeto e da solução](/visualstudio/ide/managing-project-and-solution-properties).|  
  
|Para definir constantes na linha de comando|  
|---|  
|-Use a opção **-d** para inserir constantes de compilação condicional, como no exemplo a seguir:<br />     `vbc MyProj.vb /d:conFrenchVersion=–1:conANSI=0`<br />     Nenhum espaço é necessário entre a opção **-d** e a primeira constante. Para obter mais informações, consulte [-define (Visual Basic)](../../reference/command-line-compiler/define.md).<br />     Declarações de linha de comando substituem declarações inseridas no **Designer de projeto**, mas não as apagam. Os argumentos definidos no **Designer de projeto** permanecem em vigor para compilações subsequentes.<br />     Ao escrever constantes no próprio código, não há regras estritas em seu posicionamento, pois seu escopo é o módulo inteiro no qual elas são declaradas.|  
  
|Para definir constantes em seu código|  
|---|  
|-Coloque as constantes no bloco de declaração do módulo no qual elas são usadas. Isso ajuda a manter seu código organizado e mais fácil de ler.|  
  
## <a name="related-topics"></a>Tópicos relacionados  
  
|Title|Descrição|  
|---|---|  
|[Estrutura do programa e convenções de código](program-structure-and-code-conventions.md)|Fornece sugestões para tornar seu código fácil de ler e manter.|  
  
## <a name="reference"></a>Referência  
 [#Const diretiva](../../language-reference/directives/const-directive.md)  
  
 [#If... Diretivas then... #Else](../../language-reference/directives/if-then-else-directives.md)  
  
 [-definir (Visual Basic)](../../reference/command-line-compiler/define.md)
