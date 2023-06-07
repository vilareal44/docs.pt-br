---
title: Como usar a invocação de plataforma para reproduzir um arquivo WAV – guia de programação C#
description: Este exemplo de código C# ilustra como usar os serviços de invocação de plataforma para reproduzir um arquivo de som WAV no sistema operacional Windows.
ms.date: 07/20/2015
helpviewer_keywords:
- platform invoke, sound files
- interoperability [C#], playing WAV files using pinvoke
- wav files
- .wav files
ms.assetid: f7f62f53-e026-4c40-b221-3a26adb0c2c5
ms.openlocfilehash: b30cb08e2dcde0eb85e8d88a690ae24bf7ae7f22
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302978"
---
# <a name="how-to-use-platform-invoke-to-play-a-wav-file-c-programming-guide"></a>Como usar a invocação de plataforma para reproduzir um arquivo WAV (guia de programação C#)

O exemplo de código C# a seguir ilustra como usar os serviços de invocação de plataforma para reproduzir um arquivo de som WAV no sistema operacional Windows.

## <a name="example"></a>Exemplo

Esse código de exemplo usa <xref:System.Runtime.InteropServices.DllImportAttribute> para importar o ponto de entrada de método `PlaySound` da `winmm.dll` como `Form1 PlaySound()`. O exemplo tem um Windows Form simples com um botão. Ao clicar no botão, abre-se uma caixa de diálogo padrão <xref:System.Windows.Forms.OpenFileDialog> do Windows para que você possa abrir o arquivo para reprodução. Quando um arquivo Wave é selecionado, ele é reproduzido usando o `PlaySound()` método da biblioteca de *winmm.dll* . Para obter mais informações sobre esse método, consulte [Usando a função PlaySound com arquivos de áudio Waveform](https://docs.microsoft.com/windows/desktop/multimedia/using-playsound-to-play-waveform-audio-files). Procure e selecione um arquivo que tenha uma extensão .wav e, em seguida, clique em **Abrir** para reproduzir o arquivo wave usando a invocação de plataforma. Uma caixa de texto exibe o caminho completo do arquivo selecionado.

A caixa de diálogo **Abrir Arquivos** é filtrada por meio das configurações de filtro para mostrar somente os arquivos que têm uma extensão .wav:

[!code-csharp[csProgGuideInterop#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInterop/CS/WinSound.cs#5)]

[!code-csharp[csProgGuideInterop#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInterop/CS/WinSound.cs#3)]

## <a name="compiling-the-code"></a>Compilando o código

1. Crie um novo projeto de aplicativo Windows Forms em C# no Visual Studio e nomeie-o **WinSound**.

2. Copie o código acima e cole-o sobre o conteúdo do arquivo *Form1.cs* .

3. Copie o código a seguir e cole-o no arquivo *Form1.designer.cs* , no `InitializeComponent()` método, após qualquer código existente.

     [!code-csharp[csProgGuideInterop#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInterop/CS/WinSound.cs#4)]

4. Compile e execute o código.

## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Visão geral sobre interoperabilidade](interoperability-overview.md)
- [Um olhar detalhado sobre invocação de plataforma](../../../framework/interop/consuming-unmanaged-dll-functions.md#a-closer-look-at-platform-invoke)
- [Marshaling de dados com invocação de plataforma](../../../framework/interop/marshaling-data-with-platform-invoke.md)
