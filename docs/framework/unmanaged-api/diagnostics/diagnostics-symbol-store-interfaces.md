---
title: Interfaces de armazenamento de símbolo de diagnóstico
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], debugging
- diagnostics symbol store interfaces [.NET Framework]
- interfaces [.NET Framework], diagnostics symbol store
- unmanaged interfaces [.NET Framework], diagnostics symbol store
- debugging interfaces [.NET Framework]
- interfaces [.NET Framework debugging]
ms.assetid: f96987d5-e6a5-478b-ac5e-302e16545cce
ms.openlocfilehash: 34eee8c05e1c356d4c431245c6837bd2b3a89b32
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504466"
---
# <a name="diagnostics-symbol-store-interfaces"></a>Interfaces de armazenamento de símbolo de diagnóstico
Este tópico descreve as interfaces não gerenciadas que permitem que um compilador gere informações de símbolo para uso por um depurador.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Interface IBindingDisplay](ibindingdisplay-interface.md)  
 Fornece métodos que exibem informações de associação atuais sobre o aplicativo em execução.  
  
 [Interface IDebugAutoAttach](idebugautoattach-interface.md)  
 Define a interface para uma anexação automática do depurador invocado pelo servidor.  
  
 [Interface INotifyConnection2](inotifyconnection2-interface.md)  
 Declara métodos para registrar e cancelar o registro de uma fonte de notificação de conexão.  
  
 [Interface INotifySink2](inotifysink2-interface.md)  
 Declara métodos para notificação de coletor.  
  
 [Interface INotifySource2](inotifysource2-interface.md)  
 Declara um método para definir filtros de notificação.  
  
 [Interface ISymENCUnmanagedMethod](isymencunmanagedmethod-interface.md)  
 Fornece informações para o recurso Editar e continuar.  
  
 [Interface ISymUnmanagedAsyncMethod](isymunmanagedasyncmethod-interface.md)  
 Essa interface é o complemento de leitura para a [interface ISymUnmanagedAsyncMethodPropertiesWriter](isymunmanagedasyncmethodpropertieswriter-interface.md).  
  
 [Interface ISymUnmanagedAsyncMethodPropertiesWriter](isymunmanagedasyncmethodpropertieswriter-interface.md)  
 Permite a definição de informações opcionais de método Async por símbolo de método. Deve ser usado com um método aberto (ou seja, entre chamadas para o [método OpenMethod](isymunmanagedwriter-openmethod-method.md)e o [método CloseMethod](isymunmanagedwriter-closemethod-method.md)).  
  
 [Interface ISymUnmanagedBinder](isymunmanagedbinder-interface.md)  
 Representa um fichário de símbolo para código não gerenciado.  
  
 [Interface ISymUnmanagedBinder2](isymunmanagedbinder2-interface.md)  
 Representa um fichário de símbolo para código não gerenciado e estende a `ISymUnmanagedBinder` interface.  
  
 [Interface ISymUnmanagedBinder3](isymunmanagedbinder3-interface.md)  
 Representa um fichário de símbolo para código não gerenciado e estende a `ISymUnmanagedBinder` interface.  
  
 [Interface ISymUnmanagedConstant](isymunmanagedconstant-interface.md)  
 Fornece acesso a constantes não gerenciadas.  
  
 [Interface ISymUnmanagedDispose](isymunmanageddispose-interface.md)  
 Descartes de recursos não gerenciados.  
  
 [Interface ISymUnmanagedDocument](isymunmanageddocument-interface.md)  
 Representa um documento referenciado por um repositório de símbolos.  
  
 [Interface ISymUnmanagedDocumentWriter](isymunmanageddocumentwriter-interface.md)  
 Fornece métodos para gravar em um documento referenciado por um repositório de símbolos.  
  
 [Interface ISymUnmanagedENCUpdate](isymunmanagedencupdate-interface.md)  
 Fornece métodos para o recurso Editar e continuar.  
  
 [Interface ISymUnmanagedMethod](isymunmanagedmethod-interface.md)  
 Representa um método dentro do repositório de símbolos.  
  
 [Interface ISymUnmanagedNamespace](isymunmanagednamespace-interface.md)  
 Representa um namespace.  
  
 [Interface ISymUnmanagedReader](isymunmanagedreader-interface.md)  
 Representa um leitor de símbolo que fornece acesso a documentos, métodos e variáveis em um repositório de símbolos.  
  
 [Interface ISymUnmanagedReader2](isymunmanagedreader2-interface.md)  
 Obtém um método de leitor de símbolos dado um token de método e um número de versão de edição e cópia.  
  
 [Interface ISymUnmanagedReaderSymbolSearchInfo](isymunmanagedreadersymbolsearchinfo-interface.md)  
 Fornece métodos que recebem informações de pesquisa de símbolo.  
  
 [Interface ISymUnmanagedScope](isymunmanagedscope-interface.md)  
 Representa um escopo léxico dentro de um método.  
  
 [Interface ISymUnmanagedScope2](isymunmanagedscope2-interface.md)  
 Representa um escopo léxico dentro de um método e estende a `ISymUnmanagedScope` interface com métodos que obtêm informações sobre constantes definidas no escopo.  
  
 [Interface ISymUnmanagedSourceServerModule](isymunmanagedsourceservermodule-interface.md)  
 Fornece dados do servidor de origem para um módulo.  
  
 [Interface ISymUnmanagedSymbolSearchInfo](isymunmanagedsymbolsearchinfo-interface.md)  
 Fornece métodos que obtêm informações sobre o caminho de pesquisa.  
  
 [Interface ISymUnmanagedVariable](isymunmanagedvariable-interface.md)  
 Representa uma variável, como um parâmetro, uma variável local ou um campo.  
  
 [Interface ISymUnmanagedWriter](isymunmanagedwriter-interface.md)  
 Representa um gravador de símbolo e fornece métodos para definir documentos, pontos de sequência, escopos léxicos e variáveis.  
  
 [Interface ISymUnmanagedWriter2](isymunmanagedwriter2-interface.md)  
 Representa um gravador de símbolo e fornece métodos para definir documentos, pontos de sequência, escopos léxicos e variáveis. Estende a `ISymUnmanagedWriter` interface.  
  
 [Interface ISymUnmanagedWriter3](isymunmanagedwriter3-interface.md)  
 Representa um gravador de símbolo e fornece métodos para definir documentos, pontos de sequência, escopos léxicos e variáveis. Estende a `ISymUnmanagedWriter` interface.  
  
 [Interface ISymUnmanagedWriter4](isymunmanagedwriter4-interface.md)  
 Interface ISymUnmanagedWriter4.  
  
 [Interface ISymUnmanagedWriter5](isymunmanagedwriter5-interface.md)  
 Interface ISymUnmanagedWriter5.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Enumerações de armazenamento de símbolo de diagnóstico](diagnostics-symbol-store-enumerations.md)  
  
 [Estruturas de armazenamento de símbolo de diagnóstico](diagnostics-symbol-store-structures.md)  
  
 [Depuração](../debugging/index.md)
