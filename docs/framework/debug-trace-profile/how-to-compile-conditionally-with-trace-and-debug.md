---
title: Como compilar condicionalmente com Trace e Debug
description: Saiba como compilar condicionalmente com os atributos condicionais TRACE e DEBUG ao compilar um aplicativo .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- trace compiler options
- trace statements
- compiling source code, trace statements
- tracing [.NET Framework], enabling or disabling
- tracing [.NET Framework], compiling conditionally
- TRACE directive
- conditional compilation, tracing code
ms.assetid: 56d051c3-012c-42c1-9a58-7270edc624aa
ms.openlocfilehash: 8758b793866ec0317f91d636476d33bd001ddd78
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051214"
---
# <a name="how-to-compile-conditionally-with-trace-and-debug"></a>Como compilar condicionalmente com Trace e Debug
Enquanto você estiver depurando um aplicativo durante o desenvolvimento, a saída de rastreamento e de depuração é enviada para a janela de Saída no Visual Studio. No entanto, para incluir recursos de rastreamento em um aplicativo implantado, compile os aplicativos instrumentados com a diretiva do compilador **TRACE** habilitada. Isso permite que o código de rastreamento seja compilado na versão de lançamento do aplicativo. Se você não habilitar a diretiva **TRACE**, todo o código de rastreamento será ignorado durante a compilação e não será incluído no código executável que será implantado.  
  
 Os métodos de rastreamento e de depuração têm atributos condicionais associados. Por exemplo, se o atributo condicional do rastreamento for **true**, todas as instruções de rastreamento serão incluídas em um assembly (um arquivo .exe ou .dll compilado); se o atributo condicional de **Trace** for **false**, as instruções de rastreamento não serão incluídas.  
  
 É possível ativar o atributo condicional **Trace** ou **Debug** em um build, ambos ou nenhum. Portanto, há quatro tipos de build: **Debug**, **Trace**, ambos ou nenhum. Alguns builds de versão para implantação de produção podem não conter nenhum dos dois; a maioria dos builds de depuração contém ambos.  
  
 É possível especificar as configurações do compilador para o aplicativo de várias maneiras:  
  
- As páginas de propriedades  
  
- A linha de comando  
  
- **#CONST** (para o Visual Basic) e **#define** (para o C#)  
  
### <a name="to-change-compile-settings-from-the-property-pages-dialog-box"></a>Para alterar as configurações de compilação na caixa de diálogo das páginas de propriedades  
  
1. Clique com o botão direito do mouse no nó do projeto no **Gerenciador de Soluções**.  
  
2. Escolha **Propriedades** no menu de atalho.  
  
    - No Visual Basic, clique na guia **Compilar** no painel esquerdo da página de propriedades e, em seguida, clique no botão **Opções Avançadas de Compilação** para exibir a caixa de diálogo **Configurações Avançadas do Compilador**. Marque as caixas de seleção para as configurações do compilador que você deseja habilitar. Desmarque as caixas de seleção das configurações que você deseja desabilitar.  
  
    - No C#, clique na guia **Compilar** no painel esquerdo da página de propriedades e, em seguida, marque as caixas de seleção para as configurações do compilador que você deseja habilitar. Desmarque as caixas de seleção das configurações que você deseja desabilitar.  
  
### <a name="to-compile-instrumented-code-using-the-command-line"></a>Para compilar um código instrumentado usando a linha de comando  
  
1. Defina uma opção de compilador condicional na linha de comando. O compilador incluirá o código de rastreamento ou de depuração no executável.  
  
     Por exemplo, a seguinte instrução do compilador inserida na linha de comando incluirá o código de rastreamento em um executável compilado:  
  
     Para Visual Basic: **vbc -r:System.dll-d:TRACE = True-d:debug = false MyApplication. vb**  
  
     Para C#: **csc -r:System.dll-d:Trace-d:debug = FALSE MyApplication.cs**  
  
    > [!TIP]
    > Para compilar mais de um arquivo de aplicativo, deixe um espaço em branco entre os nomes de arquivo, por exemplo, **MyApplication1.vb MyApplication2.vb MyApplication3.vb** ou **MyApplication1.cs MyApplication2.cs MyApplication3.cs**.  
  
     O significado das diretivas de compilação condicional usadas nos exemplos acima é o seguinte:  
  
    |Diretiva|Significado|  
    |---------------|-------------|  
    |`vbc`|compilador do Visual Basic|  
    |`csc`|Compilador C#|  
    |`-r:`|Referencia um assembly externo (EXE ou DLL)|  
    |`-d:`|Define um símbolo de compilação condicional|  
  
    > [!NOTE]
    > É necessário escrever TRACE ou DEBUG com letras maiúsculas. Para obter mais informações sobre os comandos de compilação condicional, insira `vbc /?` (para o Visual Basic) ou `csc /?` (para o C#) no prompt de comando. Para obter mais informações, consulte [Compilando por meio da linha de comando](../../csharp/language-reference/compiler-options/how-to-set-environment-variables-for-the-visual-studio-command-line.md) (C#) ou [Invocando o compilador de linha de comando](../../visual-basic/reference/command-line-compiler/how-to-invoke-the-command-line-compiler.md) (Visual Basic).  
  
### <a name="to-perform-conditional-compilation-using-const-or-define"></a>Para executar a compilação condicional usando #CONST ou #define  
  
1. Digite a instrução apropriada para a linguagem de programação na parte superior do arquivo de código-fonte.  
  
    |Linguagem|de|Result|  
    |--------------|---------------|------------|  
    |**Visual Basic**|**#CONST TRACE = true**|Habilita o rastreamento|  
    ||**#CONST TRACE = false**|Desabilita o rastreamento|  
    ||**#CONST DEBUG = true**|Habilita a depuração|  
    ||**#CONST DEBUG = false**|Desabilita a depuração|  
    |**C#**|**#define TRACE**|Habilita o rastreamento|  
    ||**#undef TRACE**|Desabilita o rastreamento|  
    ||**#define DEBUG**|Habilita a depuração|  
    ||**#undef DEBUG**|Desabilita a depuração|  
  
### <a name="to-disable-tracing-or-debugging"></a>Para desabilitar o rastreamento ou a depuração  
  
Exclua a diretiva do compilador do código-fonte.  
  
\- ou –  
  
Comente a diretiva do compilador.  
  
> [!NOTE]
> Quando você estiver pronto para compilar, escolha **Compilar** no menu **Compilar** ou use o método de linha de comando, mas sem digitar o **d:** para definir símbolos de compilação condicional.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento e instrumentação de aplicativos](tracing-and-instrumenting-applications.md)
- [Como criar, inicializar e configurar opções de rastreamento](how-to-create-initialize-and-configure-trace-switches.md)
- [Opções de rastreamento](trace-switches.md)
- [Ouvintes de rastreamento](trace-listeners.md)
- [Como adicionar instruções de rastreamento ao código de um aplicativo](how-to-add-trace-statements-to-application-code.md)
- [Como configurar variáveis de ambiente para a linha de comando do Visual Studio](../../csharp/language-reference/compiler-options/how-to-set-environment-variables-for-the-visual-studio-command-line.md)
- [Como: Invocar o compilador de linha de comando](../../visual-basic/reference/command-line-compiler/how-to-invoke-the-command-line-compiler.md)
