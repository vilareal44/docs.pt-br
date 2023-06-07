---
title: Informações de chamador
ms.date: 07/20/2015
ms.assetid: 15d556eb-4d0c-4497-98a3-7f60abb7d6a1
ms.openlocfilehash: 93fb1e327d65ac19f293a2f77b7d5712fc5e8d2f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400662"
---
# <a name="caller-information-visual-basic"></a>Informações do chamador (Visual Basic)
Ao usar atributos de informações do chamador, você pode obter informações sobre o chamador de um método. Você pode obter o caminho do arquivo do código-fonte, o número da linha no código-fonte e o nome do membro do chamador. Essas informações são úteis para fins de rastreamento, depuração e criação de ferramentas de diagnóstico.  
  
 Para obter essas informações, você deve usar os atributos que são aplicadas aos parâmetros opcionais, cada qual com um valor padrão. A tabela a seguir lista os atributos de informações do chamador que são definidos no namespace de <xref:System.Runtime.CompilerServices?displayProperty=nameWithType>:  
  
|Atributo|Descrição|Type|  
|---|---|---|  
|<xref:System.Runtime.CompilerServices.CallerFilePathAttribute>|O caminho completo do arquivo de origem que contém o chamador. Esse é o caminho do arquivo no momento da compilação.|`String`|  
|<xref:System.Runtime.CompilerServices.CallerLineNumberAttribute>|Número da linha no arquivo fonte no qual o método é chamado.|`Integer`|  
|<xref:System.Runtime.CompilerServices.CallerMemberNameAttribute>|Nome do método ou da propriedade do chamador. Consulte [Nomes dos membros](#MEMBERNAMES) mais adiante neste tópico.|`String`|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar os atributos de informações do chamador. Em cada chamada para o método `TraceMessage`, as informações do chamador são substituídas como argumentos para os parâmetros opcionais.  
  
```vb  
Private Sub DoProcessing()  
    TraceMessage("Something happened.")  
End Sub  
  
Public Sub TraceMessage(message As String,  
        <System.Runtime.CompilerServices.CallerMemberName> Optional memberName As String = Nothing,  
        <System.Runtime.CompilerServices.CallerFilePath> Optional sourcefilePath As String = Nothing,  
        <System.Runtime.CompilerServices.CallerLineNumber()> Optional sourceLineNumber As Integer = 0)  
  
    System.Diagnostics.Trace.WriteLine("message: " & message)  
    System.Diagnostics.Trace.WriteLine("member name: " & memberName)  
    System.Diagnostics.Trace.WriteLine("source file path: " & sourcefilePath)  
    System.Diagnostics.Trace.WriteLine("source line number: " & sourceLineNumber)  
End Sub  
  
' Sample output:  
'   message: Something happened.  
'   member name: DoProcessing  
'   source file path: C:\Users\username\Documents\Visual Studio 2012\Projects\CallerInfoVB\CallerInfoVB\Form1.vb  
'   source line number: 15  
```  
  
## <a name="remarks"></a>Comentários  
 Você deve especificar um valor padrão explícito para cada parâmetro opcional. Você não pode aplicar atributos de informações do chamador aos parâmetros que não são especificados como opcionais.  
  
 Os atributos de informações do chamador não tornam um parâmetro opcional. Em vez disso, eles afetam o valor padrão que é passado quando o argumento é omitido.  
  
 Os valores de informações do chamador são emitidos como literais em linguagem intermediária (IL) em tempo de compilação. Ao contrário dos resultados da propriedade <xref:System.Exception.StackTrace%2A> para exceções, os resultados não são afetados por ofuscamento.  
  
 Você pode fornecer explicitamente os argumentos opcionais para controlar as informações do chamador ou ocultá-las.  
  
### <a name="member-names"></a><a name="MEMBERNAMES"></a>Nomes de membros  
 Você pode usar o atributo `CallerMemberName` para evitar especificar o nome do membro como um argumento `String` ao método chamado. Ao usar essa técnica, você evita o problema de que a **Refatoração de Renomeação** não altera os valores de `String`. Esse benefício é especialmente útil para as seguintes tarefas:  
  
- Usar rotinas de rastreamento e diagnóstico.  
  
- Implementando a interface <xref:System.ComponentModel.INotifyPropertyChanged> ao associar dados. Essa interface permite que a propriedade de um objeto notifique um controle associado sobre a alteração da propriedade de modo que o controle possa exibir as informações atualizadas. Sem o atributo `CallerMemberName`, você deve especificar o nome da propriedade como um literal.  
  
 O gráfico a seguir mostra os nomes de membros que são retornados quando você usa o atributo `CallerMemberName`.  
  
|As chamadas ocorrem em|Resultado de nome de membro|  
|-------------------------|------------------------|  
|Método, propriedade ou evento|O nome do método, da propriedade ou do evento em que a chamada foi originada.|  
|Construtor|A cadeia de caracteres “.ctor”|  
|Construtor estático|A cadeia de caracteres “.cctor”|  
|Destruidor|A cadeia de caracteres "Finalize"|  
|Operadores usuário ou conversões definidos pelo usuário|O nome gerado para o membro, por exemplo, “op_Addition”.|  
|Construtor de atributos|O nome do membro ao qual o atributo se aplica. Se o atributo é qualquer elemento dentro de um membro (como um parâmetro, um valor de retorno, ou um parâmetro de tipo genérico), esse resultado é o nome do membro associado a esse elemento.|  
|Nenhum membro contentor (por exemplo, nível de assembly ou atributos que são aplicadas aos tipos)|O valor padrão do parâmetro opcional.|  
  
## <a name="see-also"></a>Confira também

- [Atributos (Visual Basic)](../../language-reference/attributes.md)
- [Atributos comuns (Visual Basic)](attributes/common-attributes.md)
- [Parâmetros Opcionais](../language-features/procedures/optional-parameters.md)
- [Conceitos de Programação (Visual Basic)](index.md)
