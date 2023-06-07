---
title: Instrução SyncLock
ms.date: 07/20/2015
f1_keywords:
- vb.SyncLock
- SyncLock
helpviewer_keywords:
- threading [Visual Basic], locks
- SyncLock statement [Visual Basic]
- locks, threads
ms.assetid: 14501703-298f-4d43-b139-c4b6366af176
ms.openlocfilehash: fd932a20af274faf2b56136a94675b28399989b8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404155"
---
# <a name="synclock-statement"></a>Instrução SyncLock
Adquire um bloqueio exclusivo para um bloco de instrução antes de executar o bloco.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
SyncLock lockobject  
    [ block ]  
End SyncLock  
```  
  
## <a name="parts"></a>Partes  
 `lockobject`  
 Obrigatórios. Expressão que é avaliada como uma referência de objeto.  
  
 `block`  
 Opcional. Bloco de instruções que devem ser executadas quando o bloqueio é adquirido.  
  
 `End SyncLock`  
 Finaliza um `SyncLock` bloco.  
  
## <a name="remarks"></a>Comentários  
 A `SyncLock` instrução garante que vários threads não executem o bloco de instrução ao mesmo tempo. `SyncLock`impede que cada thread Insira o bloco até que nenhum outro thread o execute.  
  
 O uso mais comum do `SyncLock` é proteger os dados de serem atualizados por mais de um thread simultaneamente. Se as instruções que manipulam os dados devem ir para conclusão sem interrupção, coloque-as dentro de um `SyncLock` bloco.  
  
 Um bloco de instruções protegido por um bloqueio exclusivo às vezes é chamado de *seção crítica*.  
  
## <a name="rules"></a>Regras  
  
- Ramificação. Não é possível ramificar em um `SyncLock` bloco de fora do bloco.  
  
- Bloquear valor do objeto. O valor de `lockobject` não pode ser `Nothing` . Você deve criar o objeto de bloqueio antes de usá-lo em uma `SyncLock` instrução.  
  
     Não é possível alterar o valor de `lockobject` durante a execução de um `SyncLock` bloco. O mecanismo requer que o objeto de bloqueio permaneça inalterado.  
  
- Você não pode usar o operador [Await](../operators/await-operator.md) em um `SyncLock` bloco.  
  
## <a name="behavior"></a>Comportamento  
  
- Mecanismo. Quando um thread atinge a `SyncLock` instrução, ele avalia a `lockobject` expressão e suspende a execução até que ela adquira um bloqueio exclusivo no objeto retornado pela expressão. Quando outro thread atinge a `SyncLock` instrução, ele não adquire um bloqueio até que o primeiro thread execute a `End SyncLock` instrução.  
  
- Dados protegidos. Se `lockobject` for uma `Shared` variável, o bloqueio exclusivo impedirá que um thread em qualquer instância da classe execute o `SyncLock` bloco enquanto qualquer outro thread estiver executando-o. Isso protege os dados compartilhados entre todas as instâncias.  
  
     Se `lockobject` for uma variável de instância (não `Shared` ), o bloqueio impedirá que um thread em execução na instância atual execute o `SyncLock` bloco ao mesmo tempo que outro thread na mesma instância. Isso protege os dados mantidos pela instância individual.  
  
- Aquisição e versão. Um `SyncLock` bloco se comporta como uma `Try...Finally` construção na qual o `Try` bloco adquire um bloqueio exclusivo `lockobject` e o `Finally` bloco o libera. Por isso, o `SyncLock` bloqueio garante a liberação do bloqueio, independentemente de como você sai do bloco. Isso é verdadeiro mesmo no caso de uma exceção sem tratamento.  
  
- Chamadas à estrutura. O `SyncLock` bloco adquire e libera o bloqueio exclusivo chamando os `Enter` `Exit` métodos e da `Monitor` classe no <xref:System.Threading> namespace.  
  
## <a name="programming-practices"></a>Práticas de programação  
 A `lockobject` expressão sempre deve ser avaliada como um objeto que pertence exclusivamente à sua classe. Você deve declarar uma `Private` variável de objeto para proteger os dados pertencentes à instância atual ou uma `Private Shared` variável de objeto para proteger os dados comuns a todas as instâncias.  
  
 Você não deve usar a `Me` palavra-chave para fornecer um objeto de bloqueio para dados de instância. Se o código externo à sua classe tiver uma referência a uma instância da sua classe, ele poderá usar essa referência como um objeto de bloqueio para um `SyncLock` bloco completamente diferente do seu, protegendo dados diferentes. Dessa forma, sua classe e a outra classe podem impedir que uma da outra execute seus blocos não relacionados `SyncLock` . De maneira semelhante, o bloqueio em uma cadeia de caracteres pode ser problemático, pois qualquer outro código no processo que usa a mesma cadeia de caracteres compartilhará o mesmo bloqueio.  
  
 Você também não deve usar o `Me.GetType` método para fornecer um objeto de bloqueio para dados compartilhados. Isso ocorre porque `GetType` o sempre retorna o mesmo `Type` objeto para um determinado nome de classe. O código externo pode chamar `GetType` em sua classe e obter o mesmo objeto de bloqueio que você está usando. Isso resultaria em duas classes bloqueando-as a partir de seus `SyncLock` blocos.  
  
## <a name="examples"></a>Exemplos  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir mostra uma classe que mantém uma lista simples de mensagens. Ele contém as mensagens em uma matriz e o último elemento usado dessa matriz em uma variável. O `addAnotherMessage` procedimento incrementa o último elemento e armazena a nova mensagem. Essas duas operações são protegidas pelas `SyncLock` `End SyncLock` instruções e, já que o último elemento foi incrementado, a nova mensagem deve ser armazenada antes que qualquer outro thread possa incrementar o último elemento novamente.  
  
 Se a `simpleMessageList` classe compartilhou uma lista de mensagens entre todas as suas instâncias, as variáveis `messagesList` e `messagesLast` serão declaradas como `Shared` . Nesse caso, a variável `messagesLock` também deve ser `Shared` , de modo que haveria um único objeto de bloqueio usado por cada instância.  
  
### <a name="code"></a>Código  
 [!code-vb[VbVbalrThreading#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrThreading/VB/Class1.vb#1)]  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir usa threads e `SyncLock` . Contanto que a `SyncLock` instrução esteja presente, o bloco de instruções é uma seção crítica e `balance` nunca se torna um número negativo. Você pode comentar as `SyncLock` instruções e `End SyncLock` para ver o efeito de sair da `SyncLock` palavra-chave.  
  
### <a name="code"></a>Código  
 [!code-vb[VbVbalrThreading#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrThreading/VB/class2.vb#21)]  
  
### <a name="comments"></a>Comentários  
  
## <a name="see-also"></a>Confira também

- <xref:System.Threading.Monitor?displayProperty=nameWithType>
- <xref:System.Threading.Interlocked?displayProperty=nameWithType>
- [Visão geral dos primitivos de sincronização](../../../standard/threading/overview-of-synchronization-primitives.md)
