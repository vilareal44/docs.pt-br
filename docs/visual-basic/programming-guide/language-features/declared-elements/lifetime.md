---
title: Tempo de vida
ms.date: 07/20/2015
helpviewer_keywords:
- static variables [Visual Basic], lifetime
- static variables [Visual Basic], Visual Basic
- declared elements [Visual Basic], lifetime
- Shared variable lifetime [Visual Basic]
- lifetime [Visual Basic], declared elements
- lifetime [Visual Basic], Visual Basic
- lifetime [Visual Basic]
ms.assetid: bd91e390-690a-469a-9946-8dca70bc14e7
ms.openlocfilehash: 377f0e0b5240c3da931dc4af5439aba8924f1e81
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84357135"
---
# <a name="lifetime-in-visual-basic"></a>Tempo de vida no Visual Basic
O tempo de *vida* de um elemento declarado é o período durante o qual ele está disponível para uso. As variáveis são os únicos elementos que têm tempo de vida. Para essa finalidade, o compilador trata os parâmetros de procedimento e a função retorna como casos especiais de variáveis. O tempo de vida de uma variável representa o período durante o qual ele pode conter um valor. Seu valor pode mudar ao longo de seu tempo de vida, mas sempre tem algum valor.  
  
## <a name="different-lifetimes"></a>Tempos de vida diferentes  
 Uma *variável de membro* (declarada em nível de módulo, fora de qualquer procedimento) normalmente tem a mesma vida útil que o elemento no qual ela é declarada. Uma variável não compartilhada declarada em uma classe ou estrutura existe como uma cópia separada para cada instância da classe ou estrutura na qual ela é declarada. Cada variável desse tipo tem a mesma vida útil que sua instância. No entanto, uma `Shared` variável tem apenas uma única vida útil, que dura todo o tempo em que seu aplicativo está em execução.  
  
 Uma *variável local* (declarada dentro de um procedimento) existe somente enquanto o procedimento no qual ele está declarado está em execução. Isso se aplica também aos parâmetros desse procedimento e a qualquer função retornada. No entanto, se esse procedimento chamar outros procedimentos, as variáveis locais manterão seus valores enquanto os procedimentos chamados estiverem em execução.  
  
## <a name="beginning-of-lifetime"></a>Início do tempo de vida  
 O tempo de vida de uma variável local começa quando o controle entra no procedimento em que é declarado. Cada variável local é inicializada com o valor padrão para seu tipo de dados assim que o procedimento começa a ser executado. Quando o procedimento encontra uma `Dim` instrução que especifica valores iniciais, ele define essas variáveis para esses valores, mesmo que seu código já tenha atribuído outros valores a eles.  
  
 Cada membro de uma variável de estrutura é inicializado como se fosse uma variável separada. Da mesma forma, cada elemento de uma variável de matriz é inicializado individualmente.  
  
 Variáveis declaradas dentro de um bloco dentro de um procedimento (como um `For` loop) são inicializadas na entrada para o procedimento. Essas inicializações entram em vigor independentemente de o seu código executar ou não o bloco.  
  
## <a name="end-of-lifetime"></a>Fim da vida útil  
 Quando um procedimento é encerrado, os valores de suas variáveis locais não são preservados e Visual Basic recupera sua memória. Na próxima vez que você chamar o procedimento, todas as suas variáveis locais serão criadas novamente e reinicializadas.  
  
 Quando uma instância de uma classe ou estrutura é encerrada, suas variáveis não compartilhadas perdem sua memória e seus valores. Cada nova instância da classe ou estrutura cria e reinicializa suas variáveis não compartilhadas. No entanto, `Shared` as variáveis são preservadas até que o aplicativo pare de ser executado.  
  
## <a name="extension-of-lifetime"></a>Extensão do tempo de vida  
 Se você declarar uma variável local com a `Static` palavra-chave, sua vida útil será maior do que o tempo de execução de seu procedimento. A tabela a seguir mostra como a declaração de procedimento determina por quanto tempo uma `Static` variável existe.  
  
|Local e compartilhamento do procedimento|Tempo de vida da variável estática iniciado|Término do tempo de vida da variável estática|  
|------------------------------------|-------------------------------------|-----------------------------------|  
|Em um módulo (compartilhado por padrão)|Na primeira vez que o procedimento é chamado|Quando o aplicativo para de ser executado|  
|Em uma classe, `Shared` (o procedimento não é um membro de instância)|Na primeira vez que o procedimento é chamado em uma instância específica ou no próprio nome de classe ou estrutura|Quando o aplicativo para de ser executado|  
|Em uma instância de uma classe, não `Shared` (o procedimento é um membro de instância)|Na primeira vez que o procedimento é chamado na instância específica|Quando a instância é liberada para coleta de lixo (GC)|  
  
## <a name="static-variables-of-the-same-name"></a>Variáveis estáticas do mesmo nome  
 Você pode declarar variáveis estáticas com o mesmo nome em mais de um procedimento. Se você fizer isso, o compilador Visual Basic considerará que cada variável é um elemento separado. A inicialização de uma dessas variáveis não afeta os valores das outras. O mesmo se aplica se você definir um procedimento com um conjunto de sobrecargas e declarar uma variável estática com o mesmo nome em cada sobrecarga.  
  
## <a name="containing-elements-for-static-variables"></a>Elementos continentes para variáveis estáticas  
 Você pode declarar uma variável local estática dentro de uma classe, ou seja, dentro de um procedimento nessa classe. No entanto, você não pode declarar uma variável local estática dentro de uma estrutura, como um membro de estrutura ou como uma variável local de um procedimento dentro dessa estrutura.  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir declara uma variável com a palavra-chave [static](../../../language-reference/modifiers/static.md) . (Observe que você não precisa da `Dim` palavra-chave quando a [instrução Dim](../../../language-reference/statements/dim-statement.md) usa um modificador, como `Static` .)  
  
### <a name="code"></a>Código  
 [!code-vb[VbVbalrKeywords#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/class7.vb#13)]  
  
### <a name="comments"></a>Comentários  
 No exemplo anterior, a variável `applesSold` continua existindo depois que o procedimento `runningTotal` retorna para o código de chamada. Na próxima vez que `runningTotal` for chamado, o `applesSold` manterá seu valor calculado anteriormente.  
  
 Se `applesSold` tiver sido declarado sem usar `Static` , os valores acumulados anteriores não seriam preservados entre chamadas para `runningTotal` . Na próxima vez `runningTotal` que for chamado, `applesSold` teria sido recriado e inicializado como 0 e `runningTotal` teria simplesmente retornado o mesmo valor com o qual ele foi chamado.  
  
### <a name="compile-the-code"></a>Compilar o código  
 Você pode inicializar o valor de uma variável local estática como parte de sua declaração. Se você declarar uma matriz como sendo `Static` , poderá inicializar sua classificação (número de dimensões), o comprimento de cada dimensão e os valores dos elementos individuais.  
  
### <a name="security"></a>Segurança  
 No exemplo anterior, você pode produzir o mesmo tempo de vida declarando `applesSold` no nível do módulo. Se você alterou o escopo de uma variável dessa forma, no entanto, o procedimento não teria mais acesso exclusivo a ele. Como outros procedimentos podem acessar `applesSold` e alterar seu valor, o total acumulado pode ser não confiável e o código poderia ser mais difícil de manter.  
  
## <a name="see-also"></a>Confira também

- [Compartilhado](../../../language-reference/modifiers/shared.md)
- [Nada](../../../language-reference/nothing.md)
- [Nomes de elementos declarados](declared-element-names.md)
- [Referências a elementos declarados](references-to-declared-elements.md)
- [Escopo no Visual Basic](scope.md)
- [Níveis de acesso no Visual Basic](access-levels.md)
- [Variáveis](../variables/index.md)
- [Declaração de Variável](../variables/variable-declaration.md)
- [Solução de problemas de tipos de dados](../data-types/troubleshooting-data-types.md)
- [Estático](../../../language-reference/modifiers/static.md)
