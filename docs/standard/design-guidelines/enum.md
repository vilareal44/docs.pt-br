---
title: Design de enumeração
description: Design para enums, que são um tipo especial de tipo de valor. Enums simples mantêm conjuntos pequenos e fechados de opções. Os enums de sinalizador dão suporte a operações de bits em valores de enumeração.
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- type design guidelines, enumerations
- simple enumerations
- enumerations [.NET Framework], design guidelines
- class library design guidelines [.NET Framework], enumerations
- flags enumerations
ms.assetid: dd53c952-9d9a-4736-86ff-9540e815d545
ms.openlocfilehash: 40a9faf53dc8a03674cd59074244c15cd304bdd2
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768531"
---
# <a name="enum-design"></a>Design de enumeração

Enums são um tipo especial de tipo de valor. Há dois tipos de enums: enumerações simples e enums de sinalizador.

Enums simples representam pequenos conjuntos fechados de opções. Um exemplo comum de enum simples é um conjunto de cores.

Enumerações de sinalizador são projetadas para dar suporte a operações bit a bit nos valores de enumeração. Um exemplo comum de enum Flags é uma lista de opções.

✔️ usar uma enumeração para parâmetros de tipo forte, propriedades e valores de retorno que representam conjuntos de valores.

✔️ favorecer o uso de uma enumeração em vez de constantes estáticas.

❌Não use um enum para conjuntos abertos (como a versão do sistema operacional, os nomes dos seus amigos, etc.).

❌Não forneça valores de enumeração reservados destinados ao uso futuro.

Você sempre pode simplesmente adicionar valores à enumeração existente em um estágio posterior. Consulte [Adicionando valores a enums](#add_value) para obter mais detalhes sobre como adicionar valores a enums. Valores reservados apenas poluim o conjunto de valores reais e tendem a gerar erros do usuário.

❌Evite expor enumerações publicamente com apenas um valor.

Uma prática comum para garantir a futura extensibilidade de APIs C é adicionar parâmetros reservados a assinaturas de método. Esses parâmetros reservados podem ser expressos como enums com um único valor padrão. Isso não deve ser feito em APIs gerenciadas. O método de sobrecarga permite a adição de parâmetros em versões futuras.

❌Não inclua valores de Sentinel em enums.

Embora algumas vezes sejam úteis para os desenvolvedores de estrutura, os valores de sentinela são confusos para os usuários da estrutura. Eles são usados para rastrear o estado da enumeração em vez de ser um dos valores do conjunto representado pela enumeração.

✔️ fornecer um valor de zero em enums simples.

Considere chamar o valor de algo como "None". Se esse valor não for apropriado para esse enum específico, o valor padrão mais comum para a enumeração deverá ser atribuído ao valor subjacente zero.

✔️ Considere o uso do <xref:System.Int32> (o padrão na maioria das linguagens de programação) como o tipo subjacente de um enum, a menos que qualquer uma das seguintes opções seja verdadeira:

- A enumeração é uma enumeração de sinalizadores e você tem mais de 32 sinalizadores ou espera ter mais no futuro.

- O tipo subjacente precisa ser diferente para uma <xref:System.Int32> interoperabilidade mais fácil com código não-gerenciado, esperando diferentes enumerações de tamanho.

- Um tipo subjacente menor resultaria em uma economia significativa no espaço. Se você espera que a enumeração seja usada principalmente como um argumento para o fluxo de controle, o tamanho faz pouca diferença. A economia de tamanho pode ser significativa se:

  - Você espera que a enumeração seja usada como um campo em uma estrutura ou classe instanciada com muita frequência.

  - Você espera que os usuários criem grandes matrizes ou coleções das instâncias de enumeração.

  - Você espera que um grande número de instâncias da enumeração seja serializado.

Para uso na memória, lembre-se de que os objetos gerenciados são sempre `DWORD` alinhados, de modo que você precisa efetivamente de várias enumerações ou outras pequenas estruturas em uma instância para empacotar uma enumeração menor com a fim de fazer uma diferença, pois o tamanho total da instância sempre será arredondado para um `DWORD` .

✔️ as enumerações de sinalizador de nome com substantivos plural ou frases de substantivo e enums simples com substantivos ou frases de substantivo singulares.

❌Não estenda <xref:System.Enum?displayProperty=nameWithType> diretamente.

<xref:System.Enum?displayProperty=nameWithType>é um tipo especial usado pelo CLR para criar enumerações definidas pelo usuário. A maioria das linguagens de programação fornece um elemento de programação que oferece acesso a essa funcionalidade. Por exemplo, em C#, a `enum` palavra-chave é usada para definir uma enumeração.

<a name="design"></a>

### <a name="designing-flag-enums"></a>Criando enums de sinalizador

✔️ aplicar as <xref:System.FlagsAttribute?displayProperty=nameWithType> enumerações de sinalizador to. Não aplique esse atributo a enums simples.

✔️ usar potências de dois para os valores de enumeração de sinalizador para que eles possam ser livremente combinados usando a operação or

✔️ Considere fornecer valores de enumeração especiais para combinações de sinalizadores comumente usados.

As operações de bit a bit são um conceito avançado e não devem ser necessárias para tarefas simples. <xref:System.IO.FileAccess.ReadWrite>é um exemplo de tal valor especial.

❌Evite criar enums de sinalizador em que determinadas combinações de valores são inválidas.

❌Evite usar os valores de enumeração de sinalizador zero, a menos que o valor represente "todos os sinalizadores sejam limpos" e seja nomeado adequadamente, conforme prescrito pela próxima diretriz.

✔️ nomear o valor zero de enumerações de sinalizador `None` . Para uma enumeração de sinalizador, o valor sempre deve significar "todos os sinalizadores são limpos".

<a name="add_value"></a>

### <a name="adding-value-to-enums"></a>Adicionando valor a enums

É muito comum descobrir que você precisa adicionar valores a uma enumeração depois que você já o enviou. Há um problema potencial de compatibilidade de aplicativo quando o valor recém-adicionado é retornado de uma API existente, pois aplicativos mal escritos podem não lidar corretamente com o novo valor.

✔️ CONSIDERAR a adição de valores a enums, apesar de um pequeno risco de compatibilidade.

Se você tiver dados reais sobre incompatibilidades de aplicativos causadas por adições a uma enumeração, considere adicionar uma nova API que retorne os valores novos e antigos e substitua a antiga API, que deve continuar retornando apenas os valores antigos. Isso garantirá que seus aplicativos existentes permaneçam compatíveis.

*Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

*Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de tipo](type.md)
- [Diretrizes de design de estrutura](index.md)
