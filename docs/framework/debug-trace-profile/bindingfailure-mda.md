---
title: MDA bindingFailure
description: Leia sobre o MDA (Assistente de depuração gerenciada) bindingFailure, que é ativado quando um assembly não é carregado no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- binding failure
- binding, failures
- MDAs (managed debugging assistants), binding failures
- assemblies [.NET Framework], binding failures
- managed debugging assistants (MDAs), binding failures
- BindingFailure MDA
ms.assetid: 26ada5af-175c-4576-931a-9f07fa1723e9
ms.openlocfilehash: 98c7947c7e5d2a1f0af8c26744d3b292ed8cb4c4
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415622"
---
# <a name="bindingfailure-mda"></a>MDA bindingFailure

O MDA (assistente para depuração gerenciada) `bindingFailure` é ativado quando um assembly falha ao ser carregado.

## <a name="symptoms"></a>Sintomas

O código tentou carregar um assembly usando uma referência estática ou um dos métodos de carregador, como <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> ou <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>. O assembly não foi carregado e uma exceção <xref:System.IO.FileNotFoundException> ou <xref:System.IO.FileLoadException> é gerada.

## <a name="cause"></a>Causa

Uma falha de associação ocorre quando o runtime não pode carregar um assembly. Uma falha de associação pode ser o resultado de uma das seguintes situações:

- O CLR (Common Language Runtime) não pode localizar o assembly solicitado. Há muitos motivos para que isso ocorra, como o assembly não está sendo instalado ou o aplicativo não está sendo configurado corretamente para localizar o assembly.

- Um cenário comum de problema é a passagem de um tipo para outro domínio do aplicativo, que exige que o CLR carregue o assembly que contém esse tipo no outro domínio do aplicativo. Talvez não seja possível que o runtime carregue o assembly se o domínio do aplicativo estiver configurado de maneira diferente do domínio do aplicativo original. Por exemplo, os dois domínios do aplicativo podem ter valores de propriedade <xref:System.AppDomain.BaseDirectory%2A> diferentes.

- O assembly solicitado está corrompido ou não é um assembly.

- O código que está tentando carregar o assembly não tem as permissões corretas de segurança de acesso do código para carregar assemblies.

- As credenciais do usuário não fornecem as permissões necessárias para ler o arquivo.

## <a name="resolution"></a>Resolução

A primeira etapa é determinar por que o CLR não pôde ser associado ao assembly solicitado. Há muitas razões pelas quais o runtime pode não ter encontrado ou não pôde carregar o assembly solicitado, como os cenários listados na seção Causa. As seguintes ações são recomendadas para eliminar a causa da falha de associação:

- Determine a causa usando os dados fornecidos pelo MDA `bindingFailure`:

  - Execute o [Fuslogvw.exe (Visualizador de Log de Associação do Assembly)](../tools/fuslogvw-exe-assembly-binding-log-viewer.md) para ler os logs de erro produzidos pelo associador do assembly.

  - Determine se o assembly está no local solicitado. No caso dos métodos <xref:System.Reflection.Assembly.LoadFrom%2A> e <xref:System.Reflection.Assembly.LoadFile%2A>, o local solicitado pode ser determinado com facilidade. No caso do método <xref:System.Reflection.Assembly.Load%2A>, que é associado usando a identidade do assembly, você deve procurar assemblies que correspondem à identidade no caminho de investigação da propriedade <xref:System.AppDomain.BaseDirectory%2A> do domínio do aplicativo e no cache de assembly global.

- Resolva a causa conforme a determinação anterior. As possíveis opções de resolução são as seguintes:

  - Instale o assembly solicitado no cache de assembly global e chame o método <xref:System.Reflection.Assembly.Load%2A> para carregar o assembly por identidade.

  - Copie o assembly solicitado para o diretório do aplicativo e chame o método <xref:System.Reflection.Assembly.Load%2A> para carregar o assembly por identidade.

  - Reconfigure o domínio do aplicativo no qual ocorreu a falha de associação para incluir o caminho do assembly alterando a propriedade <xref:System.AppDomain.BaseDirectory%2A> ou adicionando caminhos de investigação particulares.

  - Altere a lista de controle de acesso do arquivo para permitir que o usuário conectado leia o arquivo.

## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime

Esse MDA não tem efeito sobre o CLR. Ele relata apenas os dados sobre falhas de associação.

## <a name="output"></a>Saída

O MDA relata o assembly que falhou ao ser carregado, incluindo o caminho solicitado e/ou nome de exibição, o contexto de associação, o domínio do aplicativo no qual a carga foi solicitada e o motivo da falha.

O nome de exibição ou o caminho solicitado pode ficar em branco se esses dados não estavam disponíveis para o CLR. Se a chamada que falhou era para o método <xref:System.Reflection.Assembly.Load%2A>, é provável que o runtime não pôde determinar o nome de exibição do assembly.

## <a name="configuration"></a>Configuração

```xml
<mdaConfig>
  <assistants>
    <bindingFailure />
  </assistants>
</mdaConfig>
```

## <a name="example"></a>Exemplo

O seguinte exemplo de código demonstra uma situação que pode ativar esse MDA:

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Reflection;
namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // This call attempts to load a nonexistent assembly.
            // The call will throw a System.IO.FileNotFound exception
            // and cause the activation of the bindingFailure MDA
            // if it is registered.
            Assembly.Load("NonExistentAssembly");
        }
    }
}
```

## <a name="see-also"></a>Veja também

- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
