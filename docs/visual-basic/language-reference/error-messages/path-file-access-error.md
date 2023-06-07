---
title: Erro de acesso ao demarcador/arquivo
ms.date: 07/20/2015
f1_keywords:
- vbrID75
ms.assetid: 6ce3a161-7316-46bd-a785-0d50e5414020
ms.openlocfilehash: dfe96cd6eaa673438849fe8f799d46fa2617bfdd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387251"
---
# <a name="pathfile-access-error"></a>Erro de acesso ao demarcador/arquivo
Durante uma operação de acesso a arquivo ou de acesso a disco, o sistema operacional não pôde estabelecer uma conexão entre o caminho e o nome do arquivo.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Verifique se a especificação do arquivo está formatada corretamente. Um nome de arquivo pode conter um caminho totalmente qualificado (absoluto) ou relativo. Um caminho totalmente qualificado começa com o nome da unidade (se o caminho estiver em outra unidade) e listará o caminho explícito da raiz para o arquivo. Qualquer caminho que não esteja totalmente qualificado é relativo à unidade e ao diretório atuais.  
  
2. Certifique-se de que você não tentou salvar um arquivo que substitua um arquivo somente leitura existente. Se esse for o caso, altere o atributo somente leitura do arquivo de destino ou salve o arquivo com um nome de arquivo diferente.  
  
3. Certifique-se de que você não tentou abrir um arquivo somente leitura em sequência `Output` ou no `Append` modo. Se esse for o caso, abra o arquivo no `Input` modo ou altere o atributo somente leitura do arquivo.  
  
4. Certifique-se de que você não tentou alterar um projeto de Visual Basic dentro de um banco de dados ou documento.  
  
## <a name="see-also"></a>Confira também

- [Tipos de erro](../../programming-guide/language-features/error-types.md)
