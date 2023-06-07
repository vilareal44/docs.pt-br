---
title: O que é obsoleto no .NET Framework
description: Veja como a biblioteca de classes .NET marca Membros como obsoletos. Entenda o atributo ObsoleteAttribute, como lidar com tipos e membros obsoletos e muito mais.
ms.date: 04/02/2019
helpviewer_keywords:
- obsolete [.NET Framework]
- what's obsolete [.NET Framework]
- deprecated [.NET Framework]
ms.assetid: d356a43a-73df-4ae2-a457-b9628074c7cd
ms.openlocfilehash: 188d9184476e58fb679421467cd68e2ea8a8a101
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558862"
---
# <a name="whats-obsolete-in-the-net-framework-class-library"></a>O que está obsoleto na biblioteca de classes do .NET Framework

O .NET muda ao longo do tempo. Cada nova versão adiciona novos tipos e membros de tipo que oferecem uma nova funcionalidade. Tipos existentes e seus membros também mudam com o passar do tempo. Por exemplo, alguns tipos se tornam menos importantes à medida que a tecnologia à qual eles dão suporte é substituída por uma nova tecnologia e alguns métodos são substituídos por métodos mais novos que são superiores de alguma maneira.

.NET Framework e a Common Language Runtime buscam suporte à compatibilidade com versões anteriores (permitindo que os aplicativos desenvolvidos com uma versão do .NET Framework sejam executados na próxima versão do .NET Framework). Isso dificulta a simples remoção de um tipo ou de um membro de tipo. Em vez disso, o .NET indica que um tipo ou um membro de tipo não deve mais ser usado, marcando-o como obsoleto ou preterido. A substituição de um tipo ou membro envolve marcá-lo para que os desenvolvedores estejam cientes de que eles sumirão e tenham tempo para responder a essa remoção. No entanto, o código existente que usa o tipo ou o membro continua a ser executado na nova versão do .NET.

> [!NOTE]
> Os termos *obsoletos* e *preteridos* têm o mesmo significado quando aplicados a tipos e membros do .net.

## <a name="the-obsoleteattribute-attribute"></a>O atributo ObsoleteAttribute

O .NET Framework indica que um tipo ou um membro de tipo está obsoleto marcando-o com o atributo <xref:System.ObsoleteAttribute>. A aplicação do atributo a um tipo ou um membro indica que esse tipo ou membro será removido em qualquer versão futura do .NET Framework sem interromper o código compilado que usa esse membro.

Além de indicar que um tipo ou membro de tipo é obsoleto, <xref:System.ObsoleteAttribute> define como o compilador identifica o código-fonte que inclui esse tipo ou membro. O compilador pode compilar o código, mas emite uma mensagem de aviso, ou pode tratar o uso do tipo ou do membro como um erro. No primeiro caso, o código pode ser compilado com êxito, mas uma mensagem de aviso indica que o tipo ou o membro é obsoleto. No segundo caso, a compilação falha.

Mesmo se a compilação produzir um erro em vez de uma mensagem de aviso, o <xref:System.ObsoleteAttribute> não afetará o comportamento do tempo de execução. Ou seja, os aplicativos que usam o tipo ou o membro e que são compilados com êxito sempre terão êxito. Somente a tentativa de recompilar um aplicativo que use o tipo ou o membro falha.

## <a name="how-to-handle-obsolete-types-and-members"></a>Como lidar com tipos e membros obsoletos

Quando você atualiza e depois recompila o código existente, o uso de um tipo ou de um membro obsoleto que produza um aviso do compilador em seu aplicativo é perfeitamente aceitável. No entanto, você deve examinar a mensagem de aviso do compilador para determinar se você deve alterar o código do aplicativo. Se a mensagem não apontar para uma alternativa adequada, você deverá fazer o seguinte:

- Altere seu código removendo o uso do tipo ou do membro, se possível.

     -ou-

- Examine a documentação dessa área de tecnologia para determinar como responder à substituição.

Você pode optar por não recompilar o código existente em comparação com uma versão posterior do .NET Framework. Em vez disso, você pode especificar a versão do .NET Framework na qual o código existente compilado é executado. Por exemplo, suponhamos que você tenha um aplicativo chamado app1.exe, compilado no .NET Framework 3.5, mas queira que o aplicativo seja executado no .NET Framework 4.5. Isso exige as seguintes etapas:

1. Crie um arquivo de configuração para o executável principal e denomine-o *appName*.exe.config, em que *appName* é o nome do executável do aplicativo. Para o aplicativo chamado *app1.exe* em nosso exemplo, você criaria um arquivo de configuração chamado *app1.exe.config*.

2. Adicione o seguinte ao arquivo de configuração.

    ```xml
    <configuration>
       <startup>
          <supportedRuntime version="v4.0" />
       </startup>
    </configuration>
    ```

Para direcionar uma versão específica do .NET Framework, atribua um dos seguintes valores de cadeia de caracteres ao `version` atributo:

|Versão do .NET Framework|Cadeia de caracteres `version`|
|-|-|
|4.8|v4.0|
|4.7 (incluindo 4.7.1 e 4.7.2)|v4.0|
|4.6 (incluindo 4.6.1 e 4.6.2)|v4.0|
|4.5 (incluindo 4.5.1 e 4.5.2)|v4.0|
|4|v4.0|
|3,5|v2.0.50727|
|2.0|v2.0.50727|
|1,1|v1.1.4322|
|1.0|v1.0.3705|

## <a name="obsolete-apis-for-net-framework-45-and-later-versions"></a>APIs obsoletas para o .NET Framework 4,5 e versões posteriores

- [Tipos obsoletos](obsolete-types.md)
- [Membros obsoletos](obsolete-members.md)

## <a name="obsolete-apis-for-previous-versions"></a>APIs obsoletas para versões anteriores

- [Tipos obsoletos no .NET Framework 4](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ee461503(v=vs.100))
- [Membros obsoletos no .NET Framework 4](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ee471421(v=vs.100))
- [Lista obsoleta do .NET Framework 3.5](https://docs.microsoft.com/previous-versions/cc835481(v=msdn.10))
- [Lista obsoleta do .NET Framework 2.0](https://docs.microsoft.com/previous-versions/aa497286(v=msdn.10))

## <a name="see-also"></a>Confira também

- [\<supportedRuntime> Elementos](../configure-apps/file-schema/startup/supportedruntime-element.md)
