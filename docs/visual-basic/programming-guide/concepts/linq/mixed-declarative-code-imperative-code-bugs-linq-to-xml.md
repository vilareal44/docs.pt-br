---
title: Bugs misturados de código declarativo/código obrigatório (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: f12b1ab4-bb92-4b92-a648-0525e45b3ce7
ms.openlocfilehash: e5526a64805b19ea293d3ef28636738923d03662
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84361067"
---
# <a name="mixed-declarative-codeimperative-code-bugs-linq-to-xml-visual-basic"></a>Código declarativo misto/bugs de código imperativo (LINQ to XML) (Visual Basic)
[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] contém vários métodos que permitem que você modifique uma árvore XML diretamente. Você pode adicionar elementos, excluir elementos, modifica o conteúdo de um elemento, adiciona atributos, e assim por diante. Essa interface de programação é descrita em [modificando árvores XML (LINQ to XML) (Visual Basic)](modifying-xml-trees-linq-to-xml.md). Se você estiver iterando com um dos eixos, como <xref:System.Xml.Linq.XContainer.Elements%2A>, e você está alterando a árvore XML como você itera através do eixo, você pode acabar com alguns erros estranhas.  
  
 Esse problema é às vezes conhecido como “o problema do Dia De Bruxas”.  
  
## <a name="definition-of-the-problem"></a>Definição do problema  
 Quando você escrever qualquer código usando LINQ que itera através de uma coleção, você estiver escrevendo código em um estilo declarativo. É mais próximo a descrever *o que* você deseja, em vez de *como* deseja que isso seja feito. Se você escreve o código que 1) obtém o primeiro elemento, 2) testá-la para alguma condição, 3) altera-a, e 4) coloque-a de novo na lista, então este código seria obrigatório. Você está informando o computador *como* deseja que isso seja feito.  
  
 Misturar esses estilos de código na mesma operação é o que resulta em problemas. Considere o seguinte:  
  
 Suponha que você tenha uma lista vinculada com três itens nele (a, b, e c#):  
  
 `a -> b -> c`  
  
 Agora, suponha que você deseja mover através da lista vinculada, adicionando novos itens três (a, b, e c#). Você deseja a lista vinculada resultante para ter esta aparência:  
  
 `a -> a' -> b -> b' -> c -> c'`  
  
 Assim você escreve o código que itera através da lista, e para cada item, adicione um novo item mesmo após ele. O que acontece são que seu código verá o primeiro elemento de `a` , e inserção `a'` após ele. Agora, seu código se o nó seguir na lista, que agora é `a'`! Felizmente adicionar um novo item à lista, `a''`.  
  
 Como você determinaria este no mundo real? Bem, você pode fazer uma cópia do original associado para listar, e criar uma lista completamente nova. Ou se você estiver escrevendo código puramente obrigatório, você pode localizar o primeiro item, adiciona o novo item, e então avanço duas vezes na lista vinculada, adiantando sobre o elemento que você adicionou.  
  
## <a name="adding-while-iterating"></a>Adicionar a iterar  
 Por exemplo, suponha que você deseja escrever qualquer código que para cada elemento em uma árvore, você deseja criar um elemento duplicado:  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
For Each e As XElement In root.Elements()  
    root.Add(New XElement(e.Name, e.Value))  
Next  
```  
  
 Esse código entra em um loop infinito. A declaração de `foreach` itera através do eixo de `Elements()` , adicionando novos elementos para o elemento de `doc` . Acaba também iterar através dos elementos que acabou de adicionar. E como atribuir novos objetos com cada iteração do loop, consumirá se houver qualquer memória disponível.  
  
 Você pode corrigir este problema recebendo a coleção na memória usando o operador padrão de consulta de <xref:System.Linq.Enumerable.ToList%2A> , como segue:  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
For Each e As XElement In root.Elements().ToList()  
    root.Add(New XElement(e.Name, e.Value))  
Next  
Console.WriteLine(root)  
```  
  
 Agora o código. A árvore XML resultante é a seguinte:  
  
```xml  
<Root>  
  <A>1</A>  
  <B>2</B>  
  <C>3</C>  
  <A>1</A>  
  <B>2</B>  
  <C>3</C>  
</Root>  
```  
  
## <a name="deleting-while-iterating"></a>Excluir para iterar  
 Se você deseja excluir todos os nós em um determinado nível, você pode ter tentado escrever código como o seguinte:  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
For Each e As XElement In root.Elements()  
    e.Remove()  
Next  
Console.WriteLine(root)  
```  
  
 No entanto, isso não faz o que você deseja. Nesta situação, depois que você removesse o primeiro elemento, A, é removido da árvore XML contida na raiz, e o código no método dos elementos que está fazendo iterar não pode localizar o elemento seguir.  
  
 O código anterior gerencia a saída a seguir:  
  
```xml  
<Root>  
  <B>2</B>  
  <C>3</C>  
</Root>  
```  
  
 A solução é novamente chamar <xref:System.Linq.Enumerable.ToList%2A> para materializar a coleção, como segue:  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
For Each e As XElement In root.Elements().ToList()  
    e.Remove()  
Next  
Console.WriteLine(root)  
```  
  
 Isso gera a saída a seguir:  
  
```xml  
<Root />  
```  
  
 Como alternativa, você pode eliminar a iteração completamente chamando <xref:System.Xml.Linq.XElement.RemoveAll%2A> no elemento pai:  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
root.RemoveAll()  
Console.WriteLine(root)  
```  
  
## <a name="why-cant-linq-automatically-handle-this"></a>Por que não pode automaticamente LINQ manipular esse?  
 Uma abordagem seria sempre trazer tudo na memória em vez de fazer a avaliação lazy. No entanto, seria muito cara em termos de uso de desempenho e de memória. De fato, se LINQ e LINQ to (XML) foi tomar essa abordagem, falharia em situações do mundo real.  
  
 Outra abordagem seria possível colocar o meio em alguma sintaxe de transação em LINQ, e tem a tentativa de compilador analisar o código e de determinar se as coleções específicas de precisa ser materializada. No entanto, tentar determinar qualquer código que tiver efeitos colaterais é incredibly complexa. Considere o código a seguir:  
  
```vb  
Dim z = _  
    From e In root.Elements() _  
    Where (TestSomeCondition(e)) _  
    Select DoMyProjection(e)  
```  
  
 Esse código de análise precisaria analisar métodos TestSomeCondition e DoMyProjection, e todos os métodos que esses métodos chamados a partir, para determinar se qualquer código tinha efeitos colaterais. Mas o código de análise não pode apenas procurar qualquer código que tem efeitos colaterais. Precisaria para selecionar apenas o código que tinha efeitos colaterais em elementos filho de `root` nesta situação.  
  
 LINQ to XML não tenta fazer uma análise.  
  
 Você pode para evitar esses problemas.  
  
## <a name="guidance"></a>Diretrizes  
 Primeiro, não mistura o código declarativo e obrigatório.  
  
 Mesmo se você souber exatamente a semântica das coleções e semântica dos métodos que modificam a árvore XML, se você escrever qualquer código inteligente que impede essas categorias de problemas, seu código deverá ser mantido no futuro por outros desenvolvedores, e não podem ser como o espaço livre nos problemas. Se você mistura estilos declarativo e obrigatórias de codificação, seu código será mais frágil.  
  
 Se você escreve o código que materializa uma coleção para que esses problemas são impedidos, observar-la com comentários apropriadas em seu código, para que os desenvolvedores de aplicativos compreendam o problema.  
  
 Segundo, se o desempenho e outras considerações reservam, use somente o código declarativo. Não altere sua árvore XML existente. Gerencia um novo.  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
Dim newRoot As XElement = New XElement("Root", _  
    root.Elements(), root.Elements())  
Console.WriteLine(newRoot)  
```  
  
## <a name="see-also"></a>Confira também

- [Programação de LINQ to XML avançada (Visual Basic)](advanced-linq-to-xml-programming.md)
