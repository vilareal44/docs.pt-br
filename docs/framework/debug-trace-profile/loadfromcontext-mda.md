---
title: MDA loadFromContext
description: Entenda o MDA (Assistente de depuração gerenciada) do loadFromContext no .NET, que será ativado se um assembly for carregado no contexto LoadFrom.
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), LoadFrom context
- managed debugging assistants (MDAs), LoadFrom context
- LoadFrom context
- LoadFromContext MDA
ms.assetid: a9b14db1-d3a9-4150-a767-dcf3aea0071a
ms.openlocfilehash: 8d55268f2b2106dde4e488a6f0271fd3b17349da
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051643"
---
# <a name="loadfromcontext-mda"></a>MDA loadFromContext
O MDA (Assistente de Depuração Gerenciado) de `loadFromContext` é ativado se um assembly é carregado no contexto `LoadFrom`. Essa situação pode ocorrer como resultado da chamar <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> ou outros métodos semelhantes.  
  
## <a name="symptoms"></a>Sintomas  
 O uso de alguns métodos de carregador pode resultar em assemblies que estão sendo carregados no contexto `LoadFrom`. O uso desse contexto pode resultar em comportamento inesperado para serialização, conversão e resolução de dependência. Em geral, é recomendável que os assemblies sejam carregados no contexto `Load` para evitar esses problemas. É difícil determinar qual contexto de um assembly foi carregado sem esse MDA.  
  
## <a name="cause"></a>Causa  
 Em geral, um assembly foi carregado para o contexto de `LoadFrom` se ele foi carregado de um caminho fora do contexto `Load`, tal como o cache de assembly global ou a propriedade <xref:System.AppDomainSetup.ApplicationBase%2A?displayProperty=nameWithType>.  
  
## <a name="resolution"></a>Resolução  
 Configure os aplicativos de modo que as chamadas <xref:System.Reflection.Assembly.LoadFrom%2A> não sejam mais necessárias. Você pode usar as seguintes técnicas para fazer isso:  
  
- Instale assemblies no cache de assembly global.  
  
- Coloque os assemblies no diretório <xref:System.AppDomainSetup.ApplicationBase%2A> para o <xref:System.AppDomain>. No caso do domínio padrão, o diretório <xref:System.AppDomainSetup.ApplicationBase%2A> é aquele que contém o arquivo executável que iniciou o processo. Isso também poderá exigir a criação de um novo <xref:System.AppDomain>, se não for conveniente mover o assembly.  
  
- Adicione um caminho de sondagem para o arquivo (.config) de configuração do aplicativo ou para domínios de aplicativo secundários se assemblies dependentes estão em diretórios filhos em relação ao executável.  
  
 Em cada caso, o código pode ser alterado para usar o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 O MDA não tem nenhum efeito no CLR. Informa o contexto que foi usado como resultado de uma solicitação de carregamento.  
  
## <a name="output"></a>Saída  
 O MDA informa que o assembly foi carregado para o contexto `LoadFrom`. Especifica o nome simples do assembly e o caminho. Também sugere atenuantes para evitar o uso do contexto `LoadFrom`.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <loadFromContext />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>Exemplo  
 O seguinte exemplo de código demonstra uma situação que pode ativar esse MDA:  
  
```csharp
using System.Reflection;  
namespace ConsoleApplication1  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            // The following call caused the LoadFrom context to be used  
            // because the assembly is loaded using LoadFrom and the path is
            // located outside of the Load context probing path.
            Assembly.LoadFrom(@"C:\Text\Test.dll");  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
