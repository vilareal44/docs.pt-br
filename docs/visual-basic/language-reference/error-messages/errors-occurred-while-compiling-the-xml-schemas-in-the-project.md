---
title: Ocorreram erros na compilação dos esquemas XML no projeto
ms.date: 07/20/2015
f1_keywords:
- bc36810
- vbc36810
helpviewer_keywords:
- BC36810
ms.assetid: 9323b5d2-ba14-4e49-91f1-9ad647162144
ms.openlocfilehash: 919c6873ba63addb776d756a58c44a3fe3f0ec3d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409616"
---
# <a name="errors-occurred-while-compiling-the-xml-schemas-in-the-project"></a>Ocorreram erros na compilação dos esquemas XML no projeto
Ocorreram erros ao compilar os esquemas XML no projeto. Por isso, o XML IntelliSense não está disponível.  
  
 Há um erro em um esquema de definição de esquema XML (XSD) incluído no projeto. Esse erro ocorre quando você adiciona um arquivo de esquema XSD (. xsd) que está em conflito com o esquema XSD existente definido para o projeto.  
  
 **ID do erro:** BC36810  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Clique duas vezes no aviso na janela **lista de erros** . Visual Basic levará você para o local no arquivo XSD que é a origem do aviso. Corrija o erro no esquema XSD.  
  
- Verifique se todos os arquivos de esquema XSD (. xsd) necessários estão incluídos no projeto. Talvez seja necessário clicar em **Mostrar todos os arquivos** no menu do **projeto** para ver seus arquivos. xsd no **Gerenciador de soluções**. Clique com o botão direito do mouse em um arquivo. xsd e clique em **incluir no projeto** para incluir o arquivo em seu projeto.  
  
- Se você estiver usando o assistente de XML para esquema, esse erro poderá ocorrer se você inferir esquemas mais de uma vez da mesma origem. Nesse caso, você pode remover os arquivos de esquema XSD existentes do projeto, adicionar um novo XML ao modelo de item de esquema e fornecer o assistente de XML para esquema com todas as fontes XML aplicáveis para seu projeto.  
  
- Se nenhum erro for identificado no seu esquema XSD, o compilador XML poderá não ter informações suficientes para fornecer uma mensagem de erro detalhada. Você poderá obter informações de erro mais detalhadas se garantir que os namespaces XML para os arquivos. xsd incluídos em seu projeto correspondam aos namespaces XML identificados para o esquema XML definido no Visual Studio.  
  
## <a name="see-also"></a>Confira também

- [Janela de Lista de Erros](/visualstudio/ide/reference/error-list-window)
- [XML](../../programming-guide/language-features/xml/index.md)
