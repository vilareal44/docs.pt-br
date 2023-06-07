---
title: Eventos ETW de método
description: Consulte eventos de ETW que coletam informações específicas para métodos, como eventos de método CLR, marcador de método CLR ou eventos detalhados de método CLR e MethodJittingStarted.
ms.date: 03/30/2017
helpviewer_keywords:
- ETW, method events (CLR)
- method events [.NET Framework]
ms.assetid: 167a4459-bb6e-476c-9046-7920880f2bb5
ms.openlocfilehash: f48867a0aef417ad0b19a15d78e0c0f01a7c30a1
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474313"
---
# <a name="method-etw-events"></a>Eventos ETW de método

 Esses eventos coletam informações que são específicas para métodos. A carga desses eventos é necessária para resolução de símbolos. Além disso, esses eventos fornecem informações úteis, como o número de vezes que um método foi chamado.

Todos os eventos de método têm um nível de "Informativo (4)". Todos os eventos detalhados do método têm um nível de "Detalhado (5)".

Todos os eventos de método são gerados pela palavra-chave `JITKeyword` (0x10) ou a palavra-chave `NGenKeyword` (0x20) no provedor de runtime ou então `JitRundownKeyword` (0x10) ou `NGENRundownKeyword` (0x20) com o provedor de encerramento.

## <a name="clr-method-events"></a>Eventos de método do CLR

A tabela a seguir mostra a palavra-chave e o nível. Para obter mais informações, consulte [palavras-chave e níveis do ETW do CLR](clr-etw-keywords-and-levels.md).

|Palavra-chave para acionar o evento|Nível|
|-----------------------------------|-----------|
|Provedor de runtime `JITKeyword` (0x10)|Informativo (4)|
|Provedor de runtime `NGenKeyword` (0x20)|Informativo (4)|
|Provedor de encerramento `JitRundownKeyword` (0x10)|Informativo (4)|
|Provedor de encerramento `NGENRundownKeyword` (0x20)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Descrição|
|-----------|--------------|-----------------|
|`MethodLoad_V1`|136|Gerado quando um método é carregado Just-In-Time (carregado via JIT) ou uma imagem NGEN é carregada. Métodos dinâmicos e genéricos não usam esta versão para carregamentos de método. Os auxiliares JIT nunca usam esta versão.|
|`MethodUnLoad_V1`|137|Gerado quando um módulo é descarregado ou um domínio do aplicativo é destruído. Métodos dinâmicos nunca usam essa versão para os descarregamentos de método.|
|`MethodDCStart_V1`|137|Enumera métodos durante um encerramento inicial.|
|`MethodDCEnd_V1`|138|Enumera métodos durante um encerramento final.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|MethodID|win:UInt64|Identificador exclusivo de um método. Para métodos auxiliares JIT, isso é definido para o endereço inicial do método.|
|ModuleID|win:UInt64|Identificador do módulo ao qual esse método pertence (0 para auxiliares JIT).|
|MethodStartAddress|win:UInt64|Endereço inicial do método.|
|MethodSize|win:UInt32|Tamanho do método.|
|MethodToken|win:UInt32|0 para métodos dinâmicos e auxiliares JIT.|
|MethodFlags|win:UInt32|0x1: método dinâmico.<br /><br /> 0x2: método genérico.<br /><br /> 0x4: método de código com compilação JIT (de outro modo, código de imagem nativa NGEN).<br /><br /> 0x8: método auxiliar.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="clr-method-marker-events"></a>Eventos de marcador de método do CLR

Esses eventos são acionados apenas no provedor de encerramento. Eles significam o final de enumeração de método durante o encerramento inicial ou final. (Ou seja, eles são gerados quando a palavra-chave `NGENRundownKeyword`, `JitRundownKeyword`, `LoaderRundownKeyword` ou `AppDomainResourceManagementRundownKeyword` está habilitada.)

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Nível|
|-----------------------------------|-----------|
|Provedor de encerramento `AppDomainResourceManagementRundownKeyword` (0x800)|Informativo (4)|
|Provedor de encerramento `JitRundownKeyword` (0x10)|Informativo (4)|
|Provedor de encerramento `NGENRundownKeyword` (0x20)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Descrição|
|-----------|--------------|----------------|
|`DCStartInit_V1`|147|Enviado antes do início da enumeração durante um encerramento inicial.|
|`DCStartComplete_V1`|145|Enviado antes do término da enumeração durante um encerramento inicial.|
|`DCEndInit_V1`|148|Enviado antes do início da enumeração durante um encerramento final.|
|`DCEndComplete_V1`|146|Enviado antes do término da enumeração durante um encerramento final.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="clr-method-verbose-events"></a>Eventos detalhados de método do CLR

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Nível|
|-----------------------------------|-----------|
|Provedor de runtime `JITKeyword` (0x10)|Detalhado (5)|
|Provedor de runtime `NGenKeyword` (0x20)|Detalhado (5)|
|Provedor de encerramento `JitRundownKeyword` (0x10)|Detalhado (5)|
|Provedor de encerramento `NGENRundownKeyword` (0x20)|Detalhado (5)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Descrição|
|-----------|--------------|-----------------|
|`MethodLoadVerbose_V1`|143|Gerado quando um método é carregado via JIT ou uma imagem NGEN é carregada. Métodos dinâmicos e genéricos sempre usam esta versão para carregamentos de método. Os auxiliares JIT sempre usam esta versão.|
|`MethodUnLoadVerbose_V1`|144|Gerado quando um método dinâmico é destruído, um módulo é descarregado ou um domínio do aplicativo é destruído. Métodos dinâmicos sempre usam essa versão para os descarregamentos de método.|
|`MethodDCStartVerbose_V1`|141|Enumera métodos durante um encerramento inicial.|
|`MethodDCEndVerbose_V1`|142|Enumera métodos durante um encerramento final.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|MethodID|win:UInt64|Identificador exclusivo do método. Para métodos auxiliares JIT, defina-o para o endereço inicial do método.|
|ModuleID|win:UInt64|Identificador do módulo ao qual esse método pertence (0 para auxiliares JIT).|
|MethodStartAddress|win:UInt64|O endereço inicial.|
|MethodSize|win:UInt32|O comprimento do método.|
|MethodToken|win:UInt32|0 para métodos dinâmicos e auxiliares JIT.|
|MethodFlags|win:UInt32|0x1: método dinâmico.<br /><br /> 0x2: método genérico.<br /><br /> 0x4: método com compilação JIT (de outro modo, gerado pelo NGen.exe)<br /><br /> 0x8: método auxiliar.|
|MethodNameSpace|win:UnicodeString|O nome completo do namespace associado ao método.|
|MethodName|win:UnicodeString|O nome de classe completo associado ao método.|
|MethodSignature|win:UnicodeString|Assinatura do método (lista separada por vírgulas de nomes de tipo).|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="methodjittingstarted-event"></a>Evento MethodJittingStarted

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Nível|
|-----------------------------------|-----------|
|Provedor de runtime `JITKeyword` (0x10)|Detalhado (5)|
|Provedor de runtime `NGenKeyword` (0x20)|Detalhado (5)|
|Provedor de encerramento `JitRundownKeyword` (0x10)|Detalhado (5)|
|Provedor de encerramento `NGENRundownKeyword` (0x20)|Detalhado (5)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Descrição|
|-----------|--------------|-----------------|
|`MethodJittingStarted`|145|Gerado quando um método está sendo compilado por JIT.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|MethodID|win:UInt64|Identificador exclusivo do método.|
|ModuleID|win:UInt64|Identificador do módulo ao qual esse método pertence.|
|MethodToken|win:UInt32|0 para métodos dinâmicos e auxiliares JIT.|
|MethodILSize|win:UInt32|O tamanho do MSIL (Microsoft Intermediate Language) para o método que está sendo compilado por JIT.|
|MethodNameSpace|win:UnicodeString|O nome de classe completo associado ao método.|
|MethodName|win:UnicodeString|O nome do método.|
|MethodSignature|win:UnicodeString|Assinatura do método (lista separada por vírgulas de nomes de tipo).|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="see-also"></a>Consulte também

- [Eventos ETW no CLR](clr-etw-events.md)
