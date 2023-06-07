---
title: Como descartar um recurso do sistema
ms.date: 07/20/2015
helpviewer_keywords:
- Using statement [Visual Basic], disposing of system resources
- Visual Basic code, control flow
- system resources, disposing of
- resources [Visual Basic], disposing of system
- system resources
- Using statement [Visual Basic], Using...End Using
- Using block
ms.assetid: 8be2b239-8090-419b-8e7e-bcaa75b0ecc8
ms.openlocfilehash: dd15c6746628f45b072d46eea40051ed9afb7921
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403492"
---
# <a name="how-to-dispose-of-a-system-resource-visual-basic"></a>Como descartar um recurso do sistema (Visual Basic)
Você pode usar um `Using` bloco para garantir que o sistema descarte um recurso quando o código sair do bloco. Isso será útil se você estiver usando um recurso do sistema que consome uma grande quantidade de memória ou que outros componentes também queiram usar.  
  
### <a name="to-dispose-of-a-database-connection-when-your-code-is-finished-with-it"></a>Para descartar uma conexão de banco de dados quando seu código for concluído com ele  
  
1. Certifique-se de incluir a [instrução Imports apropriada (namespace e tipo do .net)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md) para a conexão de banco de dados no início do seu arquivo de origem (nesse caso, <xref:System.Data.SqlClient> ).  
  
2. Crie um `Using` bloco com as `Using` `End Using` instruções e. Dentro do bloco, coloque o código que lida com a conexão de banco de dados.  
  
3. Declare a conexão e crie uma instância dela como parte da `Using` instrução.  
  
    ```vb  
    ' Insert the following line at the beginning of your source file.  
    Imports System.Data.SqlClient  
    Public Sub AccessSql(ByVal s As String)  
        Using sqc As New System.Data.SqlClient.SqlConnection(s)  
            MsgBox("Connected with string """ & sqc.ConnectionString & """")  
        End Using  
    End Sub  
    ```  
  
     O sistema descarta o recurso, independentemente de como você sai do bloco, incluindo o caso de uma exceção sem tratamento.  
  
     Observe que você não pode acessar `sqc` de fora do `Using` bloco, porque seu escopo é limitado ao bloco.  
  
     Você pode usar essa mesma técnica em um recurso do sistema, como um identificador de arquivo ou um wrapper COM. Você usará um `Using` bloco quando quiser ter certeza de deixar o recurso disponível para outros componentes depois de ter saído do `Using` bloco.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Data.SqlClient.SqlConnection>
- [Fluxo de controle](index.md)
- [Estruturas de Decisão](decision-structures.md)
- [Estruturas de Loop](loop-structures.md)
- [Outras Estruturas de Controle](other-control-structures.md)
- [Estruturas de Controle Aninhadas](nested-control-structures.md)
- [Instrução using](../../../language-reference/statements/using-statement.md)
