---
title: Instrução End
ms.date: 07/20/2015
f1_keywords:
- vb.End
- End
helpviewer_keywords:
- execution [Visual Basic], ending
- files [Visual Basic], closing
- End keyword [Visual Basic], End statements
- programs [Visual Basic], quitting
- code, exiting
- program termination
- End statement [Visual Basic]
- execution [Visual Basic], stopping
ms.assetid: 0e64467c-0f34-4aab-9ddd-43f8b9d55d90
ms.openlocfilehash: fe17a82662c4014069c77f2da76723a051ab9084
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404700"
---
# <a name="end-statement"></a>Instrução End
Finaliza a execução imediatamente.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
End  
```  
  
## <a name="remarks"></a>Comentários  
 Você pode posicionar a `End` instrução em qualquer lugar em um procedimento para forçar o aplicativo inteiro a parar de ser executado. `End`Fecha todos os arquivos abertos com uma `Open` instrução e limpa todas as variáveis do aplicativo. O aplicativo é fechado assim que não há nenhum outro programa contendo referências a seus objetos e nenhum de seus códigos está em execução.  
  
> [!NOTE]
> A `End` instrução interrompe a execução do código abruptamente e não invoca o `Dispose` método ou ou `Finalize` qualquer outro código de Visual Basic. As referências de objeto mantidas por outros programas são invalidadas. Se uma `End` instrução for encontrada dentro de `Try` um `Catch` bloco ou, o controle não passará para o `Finally` bloco correspondente.  
  
 A `Stop` instrução suspende a execução, mas, ao contrário `End` , não fecha nenhum arquivo ou limpa nenhuma variável, a menos que seja encontrado em um arquivo executável compilado (. exe).  
  
 Como `End` o encerra o aplicativo sem participar de recursos que possam estar abertos, você deve tentar fechar corretamente antes de usá-lo. Por exemplo, se seu aplicativo tiver qualquer formulário aberto, você deverá fechá-lo antes que o controle atinja a `End` instrução.  
  
 Você deve usar com `End` moderação e somente quando precisar parar imediatamente. As maneiras normais de encerrar um procedimento (instrução[Return](return-statement.md) e [Exit Statement](exit-statement.md)) não apenas fecham o procedimento de forma limpa, mas também dão ao código de chamada a oportunidade de fechar corretamente. Um aplicativo de console, por exemplo, pode simplesmente `Return` do `Main` procedimento.  
  
> [!IMPORTANT]
> A `End` instrução chama o <xref:System.Environment.Exit%2A> método da <xref:System.Environment> classe no <xref:System> namespace. <xref:System.Environment.Exit%2A>exige que você tenha `UnmanagedCode` permissão. Se você não fizer isso, <xref:System.Security.SecurityException> ocorrerá um erro.  
  
 Quando seguido por uma palavra-chave adicional, a [ \<keyword> instrução End](end-keyword-statement.md) delineia o final da definição do procedimento ou bloco apropriado. Por exemplo, `End Function` encerra a definição de um `Function` procedimento.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `End` instrução para encerrar a execução de código se o usuário solicitá-la.  
  
 [!code-vb[VbVersHelp60Controls#64](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVersHelp60Controls/VB/Form1.vb#64)]  
  
## <a name="smart-device-developer-notes"></a>Notas para desenvolvedores de dispositivos inteligentes  
 Não há suporte para essa instrução.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Security.Permissions.SecurityPermissionFlag>
- [Instrução Stop](stop-statement.md)
- [\<keyword>Instrução End](end-keyword-statement.md)
