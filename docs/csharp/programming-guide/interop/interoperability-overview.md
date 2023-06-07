---
title: Visão geral sobre interoperabilidade – Guia de Programação em C#
description: Saiba mais sobre a interoperabilidade entre C# e código não gerenciado, incluindo invocação de plataforma, interoperabilidade de C++, expondo componentes COM para C# e expondo C# para COM.
ms.date: 07/20/2015
helpviewer_keywords:
- COM interop
- C# language, interoperability
- C++ Interop
- interoperability, about interoperability
- platform invoke
ms.assetid: c025b2e0-2357-4c27-8461-118f0090aeff
ms.openlocfilehash: 6b1dec96dfb3fc354c614983ed1dafab66c5b007
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302952"
---
# <a name="interoperability-overview-c-programming-guide"></a>Visão geral sobre interoperabilidade (Guia de Programação em C#)
O tópico descreve métodos para permitir a interoperabilidade entre código gerenciado e código não gerenciado do C#.  
  
## <a name="platform-invoke"></a>Invocação de plataforma  
 A *invocação de plataforma* é um serviço que permite ao código gerenciado chamar funções não gerenciadas que são implementadas em DLLs (bibliotecas de vínculo dinâmico), como aquelas na API do Microsoft Windows. Ela localiza e invoca uma função exportada e realiza marshaling dos argumentos (inteiros, cadeias de caracteres, matrizes, estruturas e assim por diante) além do limite de interoperação, conforme necessário.  
  
Para obter mais informações, consulte [consumindo funções de dll não gerenciadas](../../../framework/interop/consuming-unmanaged-dll-functions.md) e [como usar a invocação de plataforma para reproduzir um arquivo WAV](./how-to-use-platform-invoke-to-play-a-wave-file.md).
  
> [!NOTE]
> O [CLR](../../../standard/clr.md) (Common Language Runtime) gerencia o acesso aos recursos do sistema. Chamar código não gerenciado que esteja fora do CLR ignora esse mecanismo de segurança e, portanto, apresenta um risco de segurança. Por exemplo, o código não gerenciado pode chamar recursos diretamente em código não gerenciado, ignorando os mecanismos de segurança do CLR. Para obter mais informações, confira [Segurança no .NET](../../../standard/security/index.md).  
  
## <a name="c-interop"></a>Interoperabilidade C++  
 Você pode usar a interoperabilidade C++, também conhecida como que ele simplesmente funciona (IJW), para encapsular uma classe C++ nativa para que ela possa ser consumida pelo código que é criado em C# ou outra linguagem .NET. Para fazer isso, você deve escrever código C++ para encapsular um componente nativo DLL ou COM. Ao contrário de outras linguagens .NET, Visual C++ tem suporte de interoperabilidade que permite que o código gerenciado e não gerenciado esteja localizado no mesmo aplicativo e mesmo no mesmo arquivo. Então, você compila o código C++ usando a opção do compilador **/clr** para produzir um assembly gerenciado. Finalmente, você adiciona uma referência ao assembly no seu projeto do C# e usa os objetos encapsulados, assim como usaria outras classes gerenciadas.  
  
## <a name="exposing-com-components-to-c"></a>Expondo componentes COM ao C\#
 Você pode consumir um componente COM de um projeto do C#. As etapas gerais são as seguintes:  
  
1. Localize um componente COM para usar e registre-o. Use regsvr32.exe para registrar ou cancelar o registro de uma DLL do COM.  
  
2. Adicione ao projeto uma referência ao componente COM ou à biblioteca de tipo.  
  
     Quando você adiciona a referência, o Visual Studio usa a [Tlbimp.exe (tipo de importador de biblioteca de tipos)](../../../framework/tools/tlbimp-exe-type-library-importer.md), que usa uma biblioteca de tipos como entrada para gerar um assembly de interoperabilidade .net. O assembly, também chamado de RCW (Runtime Callable Wrapper), contém classes gerenciadas e interfaces que encapsulam as classes COM e as interfaces que estão na biblioteca de tipos. O Visual Studio adiciona ao projeto uma referência ao assembly gerado.  
  
3. Crie uma instância de uma classe que esteja definida no RCW. Isso, por sua vez, criará uma instância do objeto COM.  
  
4. Use o objeto da mesma maneira que usa outros objetos gerenciados. Quando o objeto é recuperado pela coleta de lixo, a instância do objeto COM também é liberada da memória.  
  
 Para obter mais informações, consulte [Expondo componentes COM para o .NET Framework](../../../framework/interop/exposing-com-components.md).  
  
## <a name="exposing-c-to-com"></a>Expondo o C# para o COM  
 Os clientes COM podem consumir tipos do C# que foram expostos corretamente. As etapas básicas para expor os tipos do C# são as seguintes:  
  
1. Adicione atributos de interoperabilidade no projeto do C#.  
  
     Você pode tornar visível um assembly COM ao modificar as propriedades do projeto do Visual C#. Para obter mais informações, consulte [Caixa de diálogo de informações do assembly](/visualstudio/ide/reference/assembly-information-dialog-box).  
  
2. Gere e registre uma biblioteca de tipos COM para ser usada pelo COM.  
  
     Você pode modificar as propriedades do projeto do Visual C# para registrar automaticamente o assembly do C# para a interoperabilidade COM. O Visual Studio usa a [Regasm.exe (ferramenta Assembly Registration)](../../../framework/tools/regasm-exe-assembly-registration-tool.md), com a opção de linha de comando `/tlb`, que usa um assembly gerenciado como entrada para gerar uma biblioteca de tipos. Esta biblioteca de tipos descreve os tipos `public` no assembly e adiciona as entradas do Registro para que os clientes COM possam criar classes gerenciadas.  
  
 Para obter mais informações, consulte [Expondo componentes do .NET Framework para o COM](../../../framework/interop/exposing-dotnet-components-to-com.md) e [Classe COM de exemplo](./example-com-class.md).  
  
## <a name="see-also"></a>Veja também

- [Melhorando o desempenho de interoperabilidade](https://docs.microsoft.com/previous-versions/msp-n-p/ff647812%28v=pandp.10%29)
- [Introdução à interoperabilidade entre COM e .NET](/office/client-developer/outlook/pia/introduction-to-interoperability-between-com-and-net)
- [Introdução à interoperabilidade COM em Visual Basic](../../../visual-basic/programming-guide/com-interop/introduction-to-com-interop.md)
- [Marshaling entre código gerenciado e não gerenciado](../../../framework/interop/interop-marshaling.md)
- [Interoperação com código não gerenciado](../../../framework/interop/index.md)
- [Guia de programação C#](../index.md)
