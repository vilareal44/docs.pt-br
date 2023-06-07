---
title: Processando o arquivo XML
ms.date: 07/20/2015
helpviewer_keywords:
- XML comments [Visual Basic], parsing [Visual Basic]
ms.assetid: 78a15cd0-7708-4e79-85d1-c154b7a14a8c
ms.openlocfilehash: 81d2c8d305e828b2963a0af9d97ec35b1745197a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398325"
---
# <a name="processing-the-xml-file-visual-basic"></a>Processando o arquivo XML (Visual Basic)
O compilador gera uma cadeia de identificação para cada constructo no seu código marcado para gerar a documentação. (Para obter informações sobre como marcar seu código, consulte [marcas de comentário XML](../../language-reference/xmldoc/index.md).) A cadeia de caracteres de ID identifica exclusivamente a construção. Programas que processam o arquivo XML podem usar a cadeia de caracteres de ID para identificar o item de metadados/reflexão .NET Framework correspondente.  
  
 O arquivo XML não é uma representação hierárquica do seu código; é uma lista simples com uma ID gerada para cada elemento.  
  
 O compilador observa as seguintes regras quando gera as cadeias de identificação:  
  
- Nenhum espaço em branco é colocado na cadeia de caracteres.  
  
- A primeira parte da cadeia de identificação identifica o tipo de membro que está sendo identificado, com um único caractere seguido por dois-pontos. Os seguintes tipos de membro são usados.  
  
|Caractere|Descrição|  
|---|---|  
|N|namespace<br /><br /> Você não pode adicionar comentários de documentação a um namespace, mas pode fazer referências CREF a eles, quando houver suporte.|  
|T|tipo: `Class` , `Module` , `Interface` , `Structure` , `Enum` ,`Delegate`|  
|F|campo`Dim`|  
|P|Propriedade: `Property` (incluindo propriedades padrão)|  
|M|método: `Sub` , `Function` , `Declare` ,`Operator`|  
|E|circunstância`Event`|  
|!|cadeia de caracteres de erro<br /><br /> O restante da cadeia de caracteres fornece informações sobre o erro. O compilador Visual Basic gera informações de erro para links que não podem ser resolvidos.|  
  
- A segunda parte do `String` é o nome totalmente qualificado do item, começando na raiz do namespace. O nome do item, seus tipos de delimitador e o namespace são separados por pontos. Se o nome do próprio item contiver pontos, eles serão substituídos pelo sinal numérico (#). Supõe-se que nenhum item tenha um sinal de número diretamente em seu nome. Por exemplo, o nome totalmente qualificado do `String` Construtor seria `System.String.#ctor` .  
  
- Para propriedades e métodos, se houver argumentos para o método, seguirá a lista de argumentos entre parênteses. Se não houver nenhum argumento, não haverá parênteses. Os argumentos são separados por vírgulas. A codificação de cada argumento segue diretamente como ele é codificado em uma assinatura .NET Framework.  
  
## <a name="example"></a>Exemplo  
 O código a seguir mostra como as cadeias de caracteres de ID para uma classe e seus membros são gerados.  
  
 [!code-vb[VbVbcnXmlDocComments#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#10)]  
  
## <a name="see-also"></a>Confira também

- [-doc](../../reference/command-line-compiler/doc.md)
- [Como criar documentação XML](how-to-create-xml-documentation.md)
