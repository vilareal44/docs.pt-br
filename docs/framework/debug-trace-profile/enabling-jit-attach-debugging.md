---
title: Habilitando a depuração por anexação JIT
description: Habilite a depuração de anexação JIT (just-in-time) para anexar um depurador a um processo quando você encontrar erros. Ele pode ser disparado por determinados métodos ou funções.
ms.date: 03/30/2017
helpviewer_keywords:
- JIT-attach debugging
- debugging [.NET Framework], JIT-attach debugging
ms.assetid: f91fc5f7-de5a-4f23-b6ac-f450e63c662e
ms.openlocfilehash: d1190c51a9cc6b5322ec832e0d35bc01dc855b12
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85416038"
---
# <a name="enabling-jit-attach-debugging"></a>Habilitando a depuração por anexação JIT
A depuração de anexação JIT é a expressão usada para descrever a anexação de um depurador a um processo quando você encontra erros ou ela pode ser disparada por funções ou métodos específicos.  
  
 A depuração de anexação JIT é usada nas seguintes condições de falha:  
  
- Exceções sem tratamento (no código nativo e gerenciado).  
  
- Método <xref:System.Environment.FailFast%2A?displayProperty=nameWithType> ou função [RaiseFailFastException](/windows/win32/api/errhandlingapi/nf-errhandlingapi-raisefailfastexception) (família Windows 7).  
  
- Erros fatais de runtime.  
  
 A depuração de anexação JIT também é disparada por chamadas às seguintes funções e métodos:  
  
- Método <xref:System.Diagnostics.Debugger.Launch%2A?displayProperty=nameWithType>.  
  
- Método <xref:System.Diagnostics.Debugger.Break%2A?displayProperty=nameWithType>.  
  
- Função [DebugBreak](/windows/win32/api/debugapi/nf-debugapi-debugbreak) (Win32).  
  
 Antes do .NET Framework 4, o .NET Framework forneceu chaves de registro separadas para controlar o comportamento de depuradores nativos e gerenciados. A partir do .NET Framework 4, o controle é consolidado em uma única chave do registro: HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows NT\Current Version\AeDebug. Os valores que podem ser definidos para essa chave determinam se um depurador é invocado e, nesse caso, se ele é invocado com uma caixa de diálogo que exige a interação do usuário. Para obter informações sobre como definir essa chave do registro, consulte [Configurando a depuração automática](/windows/win32/debug/configuring-automatic-debugging).  
  
## <a name="see-also"></a>Veja também

- [Depuração, rastreamento e criação de perfil](index.md)
- [Facilitar a depuração de uma imagem](making-an-image-easier-to-debug.md)
