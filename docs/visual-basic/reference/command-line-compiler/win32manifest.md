---
title: -win32manifest
ms.date: 03/13/2018
helpviewer_keywords:
- /win32manifest compiler option [Visual Basic]
- win32manifest compiler option [Visual Basic]
- -win32manifest compiler option [Visual Basic]
ms.assetid: 9e3191b4-90db-41c8-966a-28036fd20005
ms.openlocfilehash: 6f77649365f8ca7b163cd55854aa9960d88f2984
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414253"
---
# <a name="-win32manifest-visual-basic"></a>-win32manifest (Visual Basic)
Identifica um arquivo de manifesto do aplicativo Win32 definido pelo usuário para ser inserido no arquivo executável portátil (PE) de um projeto.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-win32manifest: fileName  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Termo|Definição|  
|---|---|  
|`fileName`|O caminho do arquivo de manifesto personalizado.|  
  
## <a name="remarks"></a>Comentários  
 Por padrão, o compilador Visual Basic insere um manifesto do aplicativo que especifica um nível de execução solicitado de asInvoker. Ele cria o manifesto na mesma pasta em que o arquivo executável é compilado, normalmente a pasta bin\Debug ou bin\Release quando você usa o Visual Studio. Se você quiser fornecer um manifesto personalizado, por exemplo, para especificar um nível de execução solicitado de highestAvailable ou requireAdministrator, use essa opção para especificar o nome do arquivo.  
  
> [!NOTE]
> Essa opção e a opção [-Win32Resource](win32resource.md) são mutuamente exclusivas. Se você tentar usar ambas as opções na mesma linha de comando, receberá um erro de compilação.  
  
 Um aplicativo que não tem nenhum manifesto do aplicativo que especifica que um nível de execução solicitado estará sujeito à virtualização de arquivos/Registro sob o recurso de Controle de Conta de Usuário no Windows Vista. Para saber mais sobre virtualização, confira [Implantação do ClickOnce no Windows Vista](/visualstudio/deployment/clickonce-deployment-on-windows-vista).  
  
 Seu aplicativo estará sujeito à virtualização se qualquer uma das seguintes condições for verdadeira:  
  
1. Você usa a `-nowin32manifest` opção e não fornece um manifesto em uma etapa de compilação posterior ou como parte de um arquivo de recurso do Windows (. res) usando a `-win32resource` opção.  
  
2. Você fornece um manifesto personalizado que não especifica um nível de execução solicitado.  
  
 O Visual Studio cria um arquivo .manifest padrão e o armazena nos diretórios de depuração e liberação juntamente com o arquivo executável. Você pode exibir ou editar o arquivo app. manifest padrão clicando em **exibir configurações de UAC** na guia **aplicativo** no designer de projeto. Para obter mais informações, consulte [Página de aplicativo, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic).  
  
 Você pode fornecer o manifesto do aplicativo como uma etapa de pós-compilação personalizada ou como parte de um arquivo de recurso do Win32 usando a `-nowin32manifest` opção. Use essa mesma opção se quiser que o aplicativo seja sujeito à virtualização de arquivo ou Registro no Windows Vista. Isso impedirá que o compilador crie e incorpore um manifesto padrão no arquivo PE.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra o manifesto padrão que o Visual Basic compilador insere em um PE.  
  
> [!NOTE]
> O compilador insere um nome de aplicativo padrão MyApplication. app no XML do manifesto. Essa é uma solução alternativa para habilitar os aplicativos para serem executados no Windows Server 2003 Service Pack 3.  
  
```xml  
<?xml version="1.0" encoding="utf-8" standalone="yes"?>  
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">  
  <assemblyIdentity version="1.0.0.0" name="MyApplication.app"/>  
  <trustInfo xmlns="urn:schemas-microsoft-com:asm.v2">  
    <security>  
      <requestedPrivileges xmlns="urn:schemas-microsoft-com:asm.v3">  
        <requestedExecutionLevel level="asInvoker"/>  
      </requestedPrivileges>  
    </security>  
  </trustInfo>  
</assembly>  
```  
  
## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [-nowin32manifest (Visual Basic)](nowin32manifest.md)
