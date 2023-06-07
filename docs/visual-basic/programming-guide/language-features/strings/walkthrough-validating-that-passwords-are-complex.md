---
title: Validando a complexidade de senhas
ms.date: 07/20/2015
helpviewer_keywords:
- String data type [Visual Basic], validation
ms.assetid: 5d9a918f-6c1f-41a3-a019-b5c2b8ce0381
ms.openlocfilehash: 7b2d6a81f5dc88688a469b96d56a098a2b45c59f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363678"
---
# <a name="walkthrough-validating-that-passwords-are-complex-visual-basic"></a>Instruções passo a passo: validando senhas complexas (Visual Basic)
Esse método verifica algumas características de senha forte e atualiza um parâmetro de cadeia de caracteres com informações sobre quais verificações a senha falha.  
  
 As senhas podem ser usadas em um sistema seguro para autorizar um usuário. No entanto, as senhas devem ser difíceis para que usuários não autorizados adivinhem. Os invasores podem usar um programa de *ataque de dicionário* , que itera por todas as palavras em um dicionário (ou vários dicionários em diferentes idiomas) e testa se alguma das palavras funciona como uma senha de usuário. Senhas fracas, como "Yankees" ou "Mustang", podem ser adivinhadas rapidamente. Senhas mais fortes, como "? Você " L1N3vaFiNdMeyeP@sSWerd !", tem muito menos probabilidade de ser adivinhado. Um sistema protegido por senha deve garantir que os usuários escolham senhas fortes.  
  
 Uma senha forte é complexa (contendo uma mistura de letras maiúsculas, letras minúsculas, números e caracteres especiais) e não é uma palavra. Este exemplo demonstra como verificar a complexidade.  
  
## <a name="example"></a>Exemplo  
  
### <a name="code"></a>Código  
 [!code-vb[VbVbcnRegEx#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnRegEx/VB/Class1.vb#1)]  
  
## <a name="compile-the-code"></a>Compilar o código  
 Chame esse método passando a cadeia de caracteres que contém essa senha.  
  
 Este exemplo requer:  
  
- Acesso aos membros do namespace <xref:System.Text.RegularExpressions>. Adicione uma instrução `Imports` se você não está qualificando totalmente os nomes de membros em seu código. Para obter mais informações, consulte [Instrução Imports (tipo e namespace .NET)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md).  
  
## <a name="security"></a>Segurança  
 Se estiver movendo a senha em uma rede, você precisará usar um método seguro para transferir dados. Para obter mais informações, consulte [ASP.NET Web Application Security](https://docs.microsoft.com/previous-versions/aspnet/330a99hc(v=vs.100)).
  
 Você pode melhorar a precisão da `ValidatePassword` função adicionando verificações de complexidade adicionais:  
  
- Compare a senha e suas subcadeias de caracteres com o nome do usuário, o identificador de usuário e um dicionário definido pelo aplicativo. Além disso, trate caracteres visualmente semelhantes como equivalentes ao executar as comparações. Por exemplo, trate as letras "l" e "e" como equivalente aos numerais "1" e "3".  
  
- Se houver apenas um caractere maiúsculo, verifique se ele não é o primeiro caractere da senha.  
  
- Verifique se os dois últimos caracteres da senha são caracteres de letra.  
  
- Não permita senhas em que todos os símbolos sejam inseridos da linha superior do teclado.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Text.RegularExpressions.Regex>
- [Segurança de aplicativo Web ASP .NET](https://docs.microsoft.com/previous-versions/aspnet/330a99hc(v=vs.100))
