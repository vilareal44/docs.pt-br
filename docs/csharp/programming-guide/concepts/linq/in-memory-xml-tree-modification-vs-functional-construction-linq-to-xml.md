---
title: Modificação da árvore XML na memória versus construção funcional (LINQ to XML) (C#)
description: Esses exemplos alteram a forma de um documento XML modificando-o no local e usando LINQ to XML construção funcional em C#.
ms.date: 07/20/2015
ms.assetid: b5afc31d-a325-4ec6-bf17-0ff90a20ffca
ms.openlocfilehash: 7a2e3d2ddcd452cf6a58e9d5cc886f3e8b8dd325
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165786"
---
# <a name="in-memory-xml-tree-modification-vs-functional-construction-linq-to-xml-c"></a>Modificação da árvore XML na memória versus construção funcional (LINQ to XML) (C#)
Modificar uma árvore XML no local é uma abordagem tradicional para alterar a forma de um documento XML. Um aplicativo típico carregar um documento em um armazenamento de dados como os DOM ou [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]; usa uma interface de programação para inserir nós, nós, excluir ou modificar o conteúdo dos nós; e então salva XML para um arquivo ou fluxo em uma rede.  
  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permite outra abordagem que é útil em muitos cenários: *construção funcional*. Deleites funcionais de compilação que modificam dados como um problema de transformação, em vez de como tratamento detalhada de um armazenamento de dados. Se você pode ter uma representação dos dados e a transformação com eficiência de um formulário para outro, o resultado é o mesmo como se você recebe um armazenamento de dados e o manipulou de alguma maneira para executar outra forma. Uma chave para a abordagem de construção funcional é passar os resultados de consultas para os construtores <xref:System.Xml.Linq.XDocument> e <xref:System.Xml.Linq.XElement>.  
  
 Em muitos casos você pode escrever código transformacional em uma fração de tempo que iria para manipular o armazenamento de dados, e que o código é mais fácil e mais robusto manter. Nesses casos, mesmo que a abordagem transformacional pode levar mais avançados de processamento, é mais eficiente para modificar dados. Se um desenvolvedor estiver familiarizado com a abordagem funcional, o código resultante é em muitos casos mais fácil de entender. É mais fácil de localizar o código que altera cada parte da árvore.  
  
 A abordagem onde você altera uma árvore XML no local é mais familiar para muitos desenvolvedores DOM, enquanto o código escrito usando a abordagem funcional pode parecer estranha a um desenvolvedor que não entende ainda essa abordagem. Se você precisa apenas fazer uma alteração pequena a uma grande árvore XML, a abordagem onde você altera uma árvore no lugar em muitos casos terá menos processador central - tempo.  
  
 Este tópico fornece um exemplo que é implementado com as duas abordagens.  
  
## <a name="transforming-attributes-into-elements"></a>Transformando atributos em elementos  
 Para esse exemplo, suponha que você deseja modificar o seguinte documento XML simples de modo que os atributos se transformem elementos. Este tópico apresenta primeiro a abordagem no lugar tradicional de alteração. Mostra a abordagem funcional de compilação.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<Root Data1="123" Data2="456">  
  <Child1>Content</Child1>  
</Root>  
```  
  
### <a name="modifying-the-xml-tree"></a>Alterando a árvore XML  
 Você pode escrever qualquer código procedural para criar elementos de atributos, e exclui os atributos, como segue:  
  
```csharp  
XElement root = XElement.Load("Data.xml");  
foreach (XAttribute att in root.Attributes()) {  
    root.Add(new XElement(att.Name, (string)att));  
}  
root.Attributes().Remove();  
Console.WriteLine(root);  
```  
  
 Este código produz a seguinte saída:  
  
```xml  
<Root>  
  <Child1>Content</Child1>  
  <Data1>123</Data1>  
  <Data2>456</Data2>  
</Root>  
```  
  
### <a name="functional-construction-approach"></a>Abordagem funcional de compilação  
 Por outro lado, uma abordagem funcional consiste no código para formar uma nova árvore, a recortagem e escolha elementos e atributos de árvore de origem, e transformar-los tão apropriados como são adicionados à nova árvore. Os aspectos funcionais de abordagem parece com o seguinte:  
  
```csharp  
XElement root = XElement.Load("Data.xml");  
XElement newTree = new XElement("Root",  
    root.Element("Child1"),  
    from att in root.Attributes()  
    select new XElement(att.Name, (string)att)  
);  
Console.WriteLine(newTree);  
```  
  
 Saída deste exemplo mesmo XML que o primeiro exemplo. No entanto, observe que você pode realmente consulte a estrutura XML resultante de novo na abordagem funcional. Você pode ver a criação do elemento de `Root` , o código que recebe o elemento de `Child1` de árvore de origem, e o código que transforma os atributos de árvore de origem aos elementos na árvore novo.  
  
 O exemplo funcional não nesse caso é menor que o primeiro exemplo, e não é realmente mais simples. No entanto, se você tiver muitas alterações para fazer a uma árvore XML, a abordagem não funcional será muito complexa e um pouco obtuso. Por outro lado, ao usar a abordagem funcional, você ainda forma apenas XML desejado, inserindo consultas e expressões apropriadas, para receber dentro do conteúdo desejado. Os passa funcionais de abordagem código que é mais fácil de manter.  
  
 Observe que neste caso a abordagem funcional provavelmente não deseja executar muito bem como a abordagem de manipulação de árvore. A principal problema é que a abordagem funcional cria um objetos mais breves. No entanto, as troca são eficiente usar a abordagem funcional permite maior produtividade do programador.  
  
 Este é um exemplo muito simples, mas serve para mostrar a diferença na filosofia entre as duas abordagens. A abordagem mais funcional fornece ganhos de produtividade para transformar documentos XML maiores.  
  