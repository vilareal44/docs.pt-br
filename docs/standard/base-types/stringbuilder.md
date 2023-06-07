---
title: Uso da classe StringBuilder no .NET
description: Saiba como usar a classe StringBuilder no .NET. Use essa classe para modificar uma cadeia de caracteres sem criar um novo objeto.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Remove method
- strings [.NET Framework], capacities
- StringBuilder object
- Replace method
- AppendFormat method
- Append method
- Insert method
- strings [.NET Framework], StringBuilder object
ms.assetid: 5c14867c-9a99-45bc-ae7f-2686700d377a
ms.openlocfilehash: 83d4b9327b55c511e2a46486e519e3cd0c77b1a3
ms.sourcegitcommit: 1eae045421d9ea2bfc82aaccfa5b1ff1b8c9e0e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84803224"
---
# <a name="using-the-stringbuilder-class-in-net"></a>Uso da classe StringBuilder no .NET
O objeto <xref:System.String> é imutável. Sempre que usa um dos métodos na classe <xref:System.String?displayProperty=nameWithType>, você cria um novo objeto de cadeia de caracteres na memória, o que requer uma nova alocação de espaço para esse novo objeto. Em situações em que você precisa realizar repetidas modificações em uma cadeia de caracteres, a sobrecarga associada à criação de um novo objeto <xref:System.String> pode ser dispendiosa. A classe <xref:System.Text.StringBuilder?displayProperty=nameWithType> pode ser usada quando você deseja modificar uma cadeia de caracteres sem criar um novo objeto. Por exemplo, o uso da classe <xref:System.Text.StringBuilder> pode melhorar o desempenho ao concatenar várias cadeias de caracteres em um loop.  
  
## <a name="importing-the-systemtext-namespace"></a>Importando o namespace System.Text  
 A classe <xref:System.Text.StringBuilder> é encontrada no namespace <xref:System.Text>.  Para evitar ter que fornecer um nome de tipo totalmente qualificado no código, você pode importar o namespace <xref:System.Text>:  
  
 [!code-cpp[Conceptual.StringBuilder#11](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#11)]
 [!code-csharp[Conceptual.StringBuilder#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#11)]
 [!code-vb[Conceptual.StringBuilder#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#11)]  
  
## <a name="instantiating-a-stringbuilder-object"></a>Criando uma instância de um objeto StringBuilder  
 Você pode criar uma nova instância da classe <xref:System.Text.StringBuilder> inicializando sua variável com um dos métodos do construtor sobrecarregados, conforme ilustrado no exemplo a seguir.  
  
 [!code-cpp[Conceptual.StringBuilder#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#1)]
 [!code-csharp[Conceptual.StringBuilder#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#1)]
 [!code-vb[Conceptual.StringBuilder#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#1)]  
  
## <a name="setting-the-capacity-and-length"></a>Definindo a capacidade e o comprimento  
 Embora o <xref:System.Text.StringBuilder> seja um objeto dinâmico que permite que você expanda o número de caracteres na cadeia de caracteres que ele encapsula, você pode especificar um valor para o número máximo de caracteres que ele pode conter. Esse valor é chamado de capacidade do objeto e não deve ser confundido com o comprimento da cadeia de caracteres que o <xref:System.Text.StringBuilder> atual contém. Por exemplo, você pode criar uma nova instância da classe <xref:System.Text.StringBuilder> com a cadeia de caracteres "Hello", que tem um comprimento de 5 caracteres e você pode especificar que o objeto tenha uma capacidade máxima de 25. Quando você modifica o <xref:System.Text.StringBuilder>, ele não realoca tamanho para si mesmo até que a capacidade seja atingida. Quando isso ocorre, o novo espaço é alocado automaticamente e a capacidade é dobrada. Você pode especificar a capacidade da classe <xref:System.Text.StringBuilder> usando um dos construtores sobrecarregados. O exemplo a seguir especifica que o objeto `myStringBuilder` pode ser expandido para um máximo de 25 espaços.  
  
 [!code-cpp[Conceptual.StringBuilder#2](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#2)]
 [!code-csharp[Conceptual.StringBuilder#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#2)]
 [!code-vb[Conceptual.StringBuilder#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#2)]  
  
 Além disso, você pode usar a propriedade <xref:System.Text.StringBuilder.Capacity%2A> de leitura/gravação para definir o comprimento máximo do objeto. O exemplo a seguir usa a propriedade **Capacidade** para definir o comprimento máximo de objeto.  
  
 [!code-cpp[Conceptual.StringBuilder#3](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#3)]
 [!code-csharp[Conceptual.StringBuilder#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#3)]
 [!code-vb[Conceptual.StringBuilder#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#3)]  
  
 O método <xref:System.Text.StringBuilder.EnsureCapacity%2A> pode ser usado para verificar a capacidade do **StringBuilder** atual. Se a capacidade for maior que o valor transmitido, nenhuma alteração é feita; no entanto, se a capacidade for menor do que o valor transmitido, a capacidade atual é alterada para corresponder ao valor transmitido.  
  
 A propriedade <xref:System.Text.StringBuilder.Length%2A> também pode ser exibida ou definida. Se você definir a propriedade **Comprimento** para um valor maior que a propriedade **Capacidade**, a propriedade **Capacidade** será alterada automaticamente para o mesmo valor que a propriedade **Comprimento**. Definir a propriedade **Comprimento** para um valor menor que o comprimento da cadeia de caracteres no **StringBuilder** atual, diminui a cadeia de caracteres.  
  
## <a name="modifying-the-stringbuilder-string"></a>Modificando a cadeia de caracteres do StringBuilder  
 A tabela a seguir lista os métodos que você pode usar para modificar o conteúdo de um **StringBuilder**.  
  
|Nome do método|Use|  
|-----------------|---------|  
|<xref:System.Text.StringBuilder.Append%2A?displayProperty=nameWithType>|Acrescenta informações ao final do **StringBuilder** atual.|  
|<xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>|Substitui um especificador de formato transmitido em uma cadeia de caracteres com texto formatado.|  
|<xref:System.Text.StringBuilder.Insert%2A?displayProperty=nameWithType>|Insere uma cadeia de caracteres ou um objeto no índice especificado do **StringBuilder** atual.|  
|<xref:System.Text.StringBuilder.Remove%2A?displayProperty=nameWithType>|Remove um número especificado de caracteres do **StringBuilder** atual.|  
|<xref:System.Text.StringBuilder.Replace%2A?displayProperty=nameWithType>|Substitui todas as ocorrências de um caractere ou cadeia de caracteres especificada no **StringBuilder** atual por outro caractere ou cadeia de caracteres especificada.|  
  
### <a name="append"></a>Acrescentar  
 O método **Append** pode ser usado para adicionar texto ou uma representação de cadeia de caracteres de um objeto ao final de uma cadeia de caracteres representada pelo **StringBuilder** atual. O exemplo a seguir inicializa um **StringBuilder** para "Hello World" e, em seguida, acrescenta algum texto ao final do objeto. Espaço é alocado automaticamente conforme necessário.  
  
 [!code-cpp[Conceptual.StringBuilder#4](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#4)]
 [!code-csharp[Conceptual.StringBuilder#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#4)]
 [!code-vb[Conceptual.StringBuilder#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#4)]  
  
### <a name="appendformat"></a>AppendFormat  
 O método <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType> adiciona texto ao final do objeto <xref:System.Text.StringBuilder>. Ele dá suporte ao recurso de formatação de composição (para obter mais informações, confira [Formatação de composição](composite-formatting.md)) chamando a implementação <xref:System.IFormattable> do objeto ou objetos a serem formatados. Portanto, aceita as cadeias de caracteres de formato padrão para valores numéricos, data e hora e enumeração, cadeias de caracteres de formato personalizado para valores numéricos e de data e hora e cadeias de caracteres de formato definidas para tipos personalizados. (Para obter informações sobre formatação, consulte [tipos de formatação](formatting-types.md).) Você pode usar esse método para personalizar o formato de variáveis e acrescentar esses valores a um <xref:System.Text.StringBuilder> . O exemplo a seguir usa o método <xref:System.Text.StringBuilder.AppendFormat%2A> para colocar um valor inteiro, formatado como um valor de moeda, no final de um objeto <xref:System.Text.StringBuilder>.  
  
 [!code-cpp[Conceptual.StringBuilder#5](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#5)]
 [!code-csharp[Conceptual.StringBuilder#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#5)]
 [!code-vb[Conceptual.StringBuilder#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#5)]  
  
### <a name="insert"></a>Inserir  
 O método <xref:System.Text.StringBuilder.Insert%2A> adiciona uma cadeia de caracteres ou um objeto a uma posição especificada no objeto <xref:System.Text.StringBuilder> atual. O exemplo a seguir usa esse método para inserir uma palavra na sexta posição de um objeto <xref:System.Text.StringBuilder>.  
  
 [!code-cpp[Conceptual.StringBuilder#6](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#6)]
 [!code-csharp[Conceptual.StringBuilder#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#6)]
 [!code-vb[Conceptual.StringBuilder#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#6)]  
  
### <a name="remove"></a>Remover  
 Você pode usar o método **Remove** para remover um número especificado de caracteres do objeto <xref:System.Text.StringBuilder> atual, começando em um índice com base zero especificado. O exemplo a seguir usa o método **Remove** para reduzir um objeto <xref:System.Text.StringBuilder>.  
  
 [!code-cpp[Conceptual.StringBuilder#7](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#7)]
 [!code-csharp[Conceptual.StringBuilder#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#7)]
 [!code-vb[Conceptual.StringBuilder#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#7)]  
  
### <a name="replace"></a>Substitua  
 O método **Replace** pode ser usado para substituir caracteres dentro do objeto <xref:System.Text.StringBuilder> com outro caractere especificado. O exemplo a seguir usa o método **Replace** para pesquisar, em um objeto <xref:System.Text.StringBuilder>, todas as instâncias do caractere de ponto de exclamação (!) e substituí-los com o caractere de ponto de interrogação (?).  
  
 [!code-cpp[Conceptual.StringBuilder#8](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#8)]
 [!code-csharp[Conceptual.StringBuilder#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#8)]
 [!code-vb[Conceptual.StringBuilder#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#8)]  
  
## <a name="converting-a-stringbuilder-object-to-a-string"></a>Convertendo um objeto StringBuilder em uma cadeia de caracteres  
 Você deve converter o objeto <xref:System.Text.StringBuilder> em um objeto <xref:System.String>para transmitir a cadeia de caracteres representada pelo objeto <xref:System.Text.StringBuilder> para um método que tem um parâmetro <xref:System.String> ou exibi-lo na interface do usuário. Você faz essa conversão chamando o método <xref:System.Text.StringBuilder.ToString%2A?displayProperty=nameWithType>. O exemplo a seguir chama vários métodos <xref:System.Text.StringBuilder> e chama o método <xref:System.Text.StringBuilder.ToString?displayProperty=nameWithType> para exibir a cadeia de caracteres.  
  
 [!code-csharp[Conceptual.StringBuilder#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/tostringexample1.cs#10)]
 [!code-vb[Conceptual.StringBuilder#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/tostringexample1.vb#10)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Text.StringBuilder?displayProperty=nameWithType>
- [Operações básicas de cadeia de caracteres](basic-string-operations.md)
- [Formatar tipos](formatting-types.md)
