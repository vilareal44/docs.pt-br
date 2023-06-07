---
title: Diferenças entre Propriedades e Variáveis
ms.date: 07/20/2015
helpviewer_keywords:
- property values [Visual Basic]
- variables [Visual Basic]
- Visual Basic code, procedures
- values [Visual Basic], properties
- variables [Visual Basic], definition
- Visual Basic code, variables
- Visual Basic code, properties
- properties [Visual Basic], values
- properties [Visual Basic], defined
- variables [Visual Basic], and properties
- properties [Visual Basic], and variables
ms.assetid: 7a03a8be-5381-431f-bd7c-16e887e4e07b
ms.openlocfilehash: 162bd71eaebdf55f6be89e0c5dce7acc1b975d79
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403298"
---
# <a name="differences-between-properties-and-variables-in-visual-basic"></a>Diferenças entre propriedades e variáveis no Visual Basic
Variáveis e propriedades representam valores que você pode acessar. No entanto, há diferenças no armazenamento e na implementação.  
  
## <a name="variables"></a>Variáveis  
 Uma *variável* corresponde diretamente a um local da memória. Você define uma variável com uma única instrução de declaração. Uma variável pode ser uma *variável local*, definida dentro de um procedimento e disponível somente dentro desse procedimento, ou pode ser uma *variável de membro*, definida em um módulo, classe ou estrutura, mas não dentro de nenhum procedimento. Uma variável de membro também é chamada de *campo*.  
  
## <a name="properties"></a>Propriedades  
 Uma *Propriedade* é um elemento de dados definido em um módulo, classe ou estrutura. Você define uma propriedade com um bloco de código entre `Property` as `End Property` instruções e. O bloco de código contém um `Get` procedimento, um `Set` procedimento ou ambos. Esses procedimentos são chamados de *procedimentos de propriedade* ou *acessadores de propriedade*. Além de recuperar ou armazenar o valor da propriedade, eles também podem executar ações personalizadas, como atualizar um contador de acesso.  
  
## <a name="differences"></a>Diferenças  
 A tabela a seguir mostra algumas diferenças importantes entre variáveis e propriedades.  
  
|Ponto de diferença|Variável|Propriedade|  
|-------------------------|--------------|--------------|  
|Declaração|Instrução de declaração única|Série de instruções em um bloco de código|  
|Implementação|Local de armazenamento único|Código executável (procedimentos de propriedade)|  
|Armazenamento|Diretamente associado ao valor da variável|Normalmente, o armazenamento interno não está disponível fora da classe ou do módulo que contém a propriedade<br /><br /> O valor da propriedade pode ou não existir como um elemento armazenado <sup>1</sup>|  
|Código executável|Nenhum|Deve ter pelo menos um procedimento|  
|Acesso de leitura e gravação|Leitura/gravação ou somente leitura|Leitura/gravação, somente leitura ou somente gravação|  
|Ações personalizadas (além de aceitar ou retornar valor)|Impossível|Pode ser executado como parte da configuração ou da recuperação do valor da propriedade|  
  
 <sup>1</sup> ao contrário de uma variável, o valor de uma propriedade pode não corresponder diretamente a um único item de armazenamento. O armazenamento pode ser dividido em partes para conveniência ou segurança, ou o valor pode ser armazenado em um formato criptografado. Nesses casos, o `Get` procedimento montaria as partes ou descriptografaria o valor armazenado, e o `Set` procedimento criptografaria o novo valor ou o dividiria no armazenamento de constituintes. Um valor de propriedade pode ser efêmero, como a hora do dia, caso em que o `Get` procedimento o calcularia em tempo real sempre que você acessar a propriedade.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos de propriedade](./property-procedures.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Instrução Property](../../../language-reference/statements/property-statement.md)
- [Instrução Dim](../../../language-reference/statements/dim-statement.md)
- [Como criar uma propriedade](./how-to-create-a-property.md)
- [Como declarar uma propriedade com níveis de acesso mistos](./how-to-declare-a-property-with-mixed-access-levels.md)
- [Como chamar um procedimento de propriedade](./how-to-call-a-property-procedure.md)
- [Como declarar e chamar uma propriedade padrão no Visual Basic](./how-to-declare-and-call-a-default-property.md)
- [Como inserir um valor em uma propriedade](./how-to-put-a-value-in-a-property.md)
- [Como obter um valor a partir de uma propriedade](./how-to-get-a-value-from-a-property.md)
