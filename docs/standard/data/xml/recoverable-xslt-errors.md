---
title: Erros recuperáveis XSLT
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 484929b0-fefb-4629-87ee-ebdde70ff1f8
ms.openlocfilehash: ada0b352cd867417ed3ecf86291df023ca7c579e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289090"
---
# <a name="recoverable-xslt-errors"></a>Erros recuperáveis XSLT
A recomendação de versão 1,0 do W3C de transformações XSL (XSLT) inclui as áreas no qual o provedor de implementação pode decidir como manipular uma situação. Essas áreas são consideradas como comportamento arbitrário. Por exemplo, em instruções de processamento criadoras da seção 7,3, XSLT 1,0 estados de recomendação que é um erro se criar uma instância do conteúdo de `xsl:processing-instruction` cria nós diferentes de nós de texto. Para alguns problemas, a recomendação XSLT 1,0 indica o que a decisão deve ser feita se o processador decidir recuperar de erro. Para o problema da seção 7,3, o W3C com a implementação pode recuperar esse erro ignorando os nós e seu conteúdo.  
  
## <a name="discretionary-behaviors"></a>Comportamentos arbitrários  
 A tabela a seguir lista cada um dos comportamentos arbitrários permitidas pela recomendação XSLT 1,0, e como esses comportamentos são tratados pela classe de <xref:System.Xml.Xsl.XslCompiledTransform> .  
  
- Recupere indica que a classe de <xref:System.Xml.Xsl.XslCompiledTransform> recuperar esse erro. O evento de <xref:System.Xml.Xsl.XsltArgumentList.XsltMessageEncountered?displayProperty=nameWithType> pode ser usado para relatar todos os eventos do processador XSLT.  
  
- O erro indica que uma exceção é gerada para essa condição.  
  
- As referências da seção podem ser encontradas em [Recomendação de transformações XSL (XSLT) de W3C da versão 1.0](https://www.w3.org/TR/xslt) e a [Errata da especificação de transformações XSL (XSLT) de W3C da versão 1.0](https://www.w3.org/1999/11/REC-xslt-19991116-errata/).  
  
|Condição XSLT|Seção|Comportamento de XslCompiledTransform|  
|--------------------|-------------|-----------------------------------|  
|Um nó de texto corresponde `xsl:strip-space` e `xsl:preserve-space`.|3.4|Recuperar|  
|Um nó de origem corresponde mais de uma regra de modelo.|5.5|Recuperar|  
|O URI de um namespace é declarada para ser um alias para vários namespace URIs, tudo que tem o mesmo precedência de importação.|7.1.1|Recuperar|  
|O atributo de `name` em `xsl:attribute` e em `xsl:element` gerado de um valor de atributo não é um QName.|7.1.2, 7.1.3|Erro*|  
|Dois conjuntos de atributos com o mesmo nome expandir- importação e possuem um atributo em comum e não há nenhum outro atributo definida que contém o atributo comuns que tem o mesmo nome com importância mais alta.|7.1.4|Recuperar|  
|Adicionando um atributo a um elemento após os filhos lhe foram adicionados.|7.1.3|Erro*|  
|Criando um atributo com o nome “xmlns”|7.1.3|Erro*|  
|Adicionando um atributo a um nó que não é um elemento.|7.1.3|Erro*|  
|Criando nós diferentes de nós de texto durante instanciação de conteúdo de atributo de `xsl:attribute` .|7.1.3|Erro*|  
|O atributo de `name` de `xsl:processing-instruction` não produz um NCName e um destino de instrução de processamento.|7.3|Erro*|  
|Criar uma instância do conteúdo de `xsl:processing-instruction` cria nós diferentes de nós de texto.|7.3|Erro*|  
|O resultado de instanciar o conteúdo do `xsl:processing-instruction` contém a cadeia de caracteres "?>"|7.3|Recuperar|  
|O resultado de criar uma instância do conteúdo de `xsl:processing-instruction` contém a cadeia de caracteres “--” ou termina com “-”.|7.4|Recuperar|  
|O resultado de criar uma instância do conteúdo de `xsl:comment` cria nós diferentes de nós de texto.|7.4|Erro*|  
|O modelo dentro de um elemento de variável associação retorna um nó de atributo ou um nó de namespace.|11.2|Erro*|  
|Há um erro que recupera o recurso URI passado na função do documento.|12.1|Erro|  
|A referência URI na função do documento contém um identificador de fragmento e há um erro que processa o identificador do fragmento.|12.1|Recover*|  
|Há vários atributos com o mesmo nome, mas os valores diferentes, que não são nomeados elementos de cdata- seção em `xsl:output` com a mesma precedência de importação.|16|Recuperar|  
|O processador não oferece suporte a codificação em `xsl:output` que codifica o atributo.|16.1|Recuperar|  
|Desativando saída que escapam para um nó de texto que é usado para algo diferente de um nó de texto na árvore de resultado.|16.4|Recover*|  
|Convertendo uma árvore de resultado fragmente a um número ou cadeia se o fragmento da árvore de resultado contém um nó de texto com o escape de saída ativado.|16.4|Recover*|  
|O escape de saída é desativado para um caractere que não pode ser representado na codificação que o processador XSLT está usando para saída.|16.4|Recover*|  
|Adicionando um nó de namespace para um elemento após os filhos lhe foram adicionados ou após os atributos lhe foram adicionados.|errata 25|Erro*|  
|O atributo de `value` de `xsl:number` é NAN, infinito ou menor que 0,5|errata 24|Recuperar|  
|O segundo argumento nó- definida como a função do documento está vazia e a referência URI é relativo.|erratas 14|Recuperar|  
  
 <sup>*</sup>Esse comportamento é diferente daquele da <xref:System.Xml.Xsl.XslTransform> classe. Para saber mais, confira [Implementação de comportamentos discricionários na classe XslTransform](implementation-of-discretionary-behaviors-in-the-xsltransform-class.md).  
  
## <a name="see-also"></a>Veja também

- [Transformações XSLT](xslt-transformations.md)
