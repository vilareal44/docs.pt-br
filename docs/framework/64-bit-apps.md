---
title: Aplicativos de 64 bits
description: Obtenha informações sobre como configurar aplicativos em um sistema operacional Windows de 64 bits, como um aplicativo nativo de 64 bits ou em WOW64 (Windows 32-bit no Windows de 64 bits).
ms.date: 03/30/2017
helpviewer_keywords:
- applications [C++], 64-bit
- 64-bit applications [C++]
- 64-bit programming [C++]
ms.assetid: fd4026bc-2c3d-4b27-86dc-ec5e96018181
ms.openlocfilehash: 4589d7a070a477dcb229fbaea686f6c6ff7d7e08
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989989"
---
# <a name="64-bit-applications"></a>Aplicativos de 64 bits
Ao compilar um aplicativo, você pode especificar se ele deve ser executado em um sistema operacional Windows de 64 bits como um aplicativo nativo ou no WOW64 (Windows de 32 bits em Windows de 64 bits). O WOW64 é um ambiente de compatibilidade que permite que um aplicativo de 32 bits seja executado em um sistema de 64 bits. O WOW64 está incluído em todas as versões de 64 bits do sistema operacional Windows.  
  
## <a name="running-32-bit-vs-64-bit-applications-on-windows"></a>Executando aplicativos de 32 bits versus 64 bits no Windows  
 Todos os aplicativos criados no .NET Framework 1.0 ou 1.1 são tratados como aplicativos de 32 bits em um sistema operacional de 64 bits e são sempre executados no WOW64 e no CLR (Common Language Runtime) de 32 bits. Os aplicativos de 32 bits criados no .NET Framework 4 ou em versões posteriores também são executados no WOW64 em sistemas de 64 bits.  
  
 O Visual Studio instala a versão de 32 bits do CLR em um computador x86, e a versão de 32 bits e a versão de 64 bits apropriada do CLR em um computador Windows de 64 bits. (Como o Visual Studio é um aplicativo de 32 bits, quando ele é instalado em um sistema de 64 bits, ele é executado no WOW64.)  
  
> [!NOTE]
> Em razão do projeto da emulação do x86 e do subsistema do WOW64 para a família de processadores Itanium, os aplicativos são restritos à execução em um processador. Esses fatores reduzem o desempenho e escalabilidade de aplicativos do .NET Framework de 32 bits executados em sistemas baseados em Itanium. É recomendável usar o .NET Framework 4, que inclui suporte nativo a sistemas baseados em Itanium de 64 bits, para aumento de desempenho e escalabilidade.  
  
 Por padrão, ao executar um aplicativo gerenciado de 64 bits em um sistema operacional Windows de 64 bits, você pode criar um objeto que não ultrapasse 2 GB. No entanto, no .NET Framework 4.5, você pode aumentar esse limite.  Para obter mais informações, consulte o [ \<gcAllowVeryLargeObjects> elemento](./configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md).  
  
 Muitos assemblies são executados de modo idêntico no CLR de 32 bits e no CLR de 64 bits. No entanto, alguns programas podem se comportar de maneira diferente, dependendo do CLR, se eles contiverem um ou mais dos seguintes itens:  
  
- Estruturas que contêm membros que alteram o tamanho de acordo com a plataforma (por exemplo, qualquer tipo de ponteiro).  
  
- Aritmética de ponteiro que inclui tamanhos constantes.  
  
- As declarações COM ou de invocação de plataforma incorreta que usam `Int32` para identificadores em vez de `IntPtr`.  
  
- Código que converte `IntPtr` em `Int32`.  
  
 Para obter mais informações sobre como portar um aplicativo de 32 bits para execução no CLR de 64 bits, consulte [Migrando um código gerenciado de 32 bits para 64 bits](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973190(v=msdn.10)).  
  
## <a name="general-64-bit-programming-information"></a>Informações de Programação em 64 Bits em Geral  
 Para obter informações gerais sobre a programação de 64 bits, confira os seguintes documentos:  
  
- Na documentação do SDK do Windows, confira [Programming Guide for 64-bit Windows](/windows/win32/winprog64/programming-guide-for-64-bit-windows) (Guia de programação para Windows de 64 bits).  
  
- Para saber mais sobre o suporte do Visual Studio para criar aplicativos de 64 bits, confira [Visual Studio IDE 64-Bit Support](/visualstudio/ide/visual-studio-ide-64-bit-support) (Suporte ao IDE do Visual Studio de 64 bits).  
  
## <a name="compiler-support-for-creating-64-bit-applications"></a>Suporte do Compilador para Criar Aplicativos de 64 Bits  
 Por padrão, quando você usa o .NET Framework para criar um aplicativo em um computador de 32 bits ou 64 bits, o aplicativo será executado em um computador de 64 bits como um aplicativo nativo (isto é, não no WOW64). A tabela a seguir lista os documentos que explicam como usar compiladores do Visual Studio para criar aplicativos de 64 bits que serão executados como nativos, no WOW64, ou em ambos.  
  
|Compilador|Opção de compilador|  
|--------------|---------------------|  
|Visual Basic|[-plataforma (Visual Basic)](../visual-basic/reference/command-line-compiler/platform.md)|  
|Visual C#|[-plataforma (opções do compilador C#)](../csharp/language-reference/compiler-options/platform-compiler-option.md)|  
|Visual C++|Você pode criar aplicativos MSIL (Microsoft Intermediate Language) independentes de plataforma usando **/clr:safe**. Para obter mais informações, consulte [-CLR (compilação em tempo de execução de linguagem comum)](/cpp/build/reference/clr-common-language-runtime-compilation).<br /><br /> O Visual C++ inclui um compilador separado para cada sistema operacional de 64 bits. Para saber mais sobre como usar o Visual C++ para criar aplicativos nativos que são executados em um sistema operacional Windows de 64 bits, confira [Programação de 64 bits](/cpp/build/configuring-programs-for-64-bit-visual-cpp).|  
  
## <a name="determining-the-status-of-an-exe-file-or-dll-file"></a>Determinando o Status de um Arquivo .exe ou .dll  
 Para determinar se um arquivo .exe ou .dll deve ser executado apenas em uma plataforma específica ou no WOW64, use [CorFlags.exe (Ferramenta de Conversão CorFlags)](./tools/corflags-exe-corflags-conversion-tool.md) sem opções. Você também pode usar CorFlags.exe para alterar o status da plataforma de um arquivo .exe ou .dll. O cabeçalho CLR de um assembly do Visual Studio tem o número de versão principal do runtime definido como 2 e o número de versão secundário do runtime definido como 5. Os aplicativos que têm a versão secundária do runtime definida como 0 são tratados como aplicativos herdados e são sempre executados no WOW64.  
  
 Para consultar programaticamente um .exe ou .dll para saber se ele deve ser executado somente em uma plataforma específica ou no WOW64, use o método <xref:System.Reflection.Module.GetPEKind%2A?displayProperty=nameWithType>.
