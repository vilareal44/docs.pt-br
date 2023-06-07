---
title: Como controlar um MediaElement (executar, pausar, parar, volume e velocidade)
description: Controle a reprodução de mídia no WPF (Windows Presentation Foundation). Iniciar, parar, pausar, ignorar e ajustar o volume e a velocidade.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- playback of media [WPF], controlling
- controlling playback of media [WPF]
- multimedia [WPF], controlling playback of media
- media [WPF], controlling playback of
ms.assetid: 6885a730-e054-4c16-8c1e-ffe17b1f7c32
ms.openlocfilehash: da846299eaa1e36b6e5caab08bc1f861e9f9c46d
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853608"
---
# <a name="how-to-control-a-mediaelement-play-pause-stop-volume-and-speed"></a>Como controlar um MediaElement (executar, pausar, parar, volume e velocidade)
O exemplo a seguir mostra como controlar a reprodução de mídia usando um <xref:System.Windows.Controls.MediaElement> . O exemplo cria um player de mídia simples que permite reproduzir, pausar, parar, retroceder e avançar a mídia, bem como ajustar a taxa de velocidade e o volume.  
  
## <a name="example"></a>Exemplo  
 O código a seguir cria a interface do usuário.  
  
> [!NOTE]
> A <xref:System.Windows.Controls.MediaElement.LoadedBehavior%2A> propriedade de <xref:System.Windows.Controls.MediaElement> deve ser definida como para poder `Manual` parar, pausar e reproduzir a mídia interativamente.  
  
 [!code-xaml[MediaGallery_snip#MediaElementExampleWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MediaGallery_snip/VB/MediaElementExample.xaml#mediaelementexamplewholepage)]  
  
## <a name="example"></a>Exemplo  
 O código a seguir implementa a funcionalidade dos controles de interface do usuário de exemplo. Os <xref:System.Windows.Controls.MediaElement.Play%2A> <xref:System.Windows.Controls.MediaElement.Pause%2A> métodos, e <xref:System.Windows.Controls.MediaElement.Stop%2A> são usados para reproduzir, pausar e parar a mídia respectivamente. Alterar a <xref:System.Windows.Controls.MediaElement.Position%2A> Propriedade do <xref:System.Windows.Controls.MediaElement> permite que você pule na mídia. Por fim, <xref:System.Windows.Controls.MediaElement.Volume%2A> as <xref:System.Windows.Controls.MediaElement.SpeedRatio%2A> Propriedades e são usadas para ajustar o volume e a velocidade de reprodução da mídia.  
  
 [!code-csharp[MediaGallery_snip#CodeBehindMediaElementExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/MediaGallery_snip/CSharp/MediaElementExample.xaml.cs#codebehindmediaelementexamplewholepage)]
 [!code-vb[MediaGallery_snip#CodeBehindMediaElementExampleWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MediaGallery_snip/VB/MediaElementExample.xaml.vb#codebehindmediaelementexamplewholepage)]  
  
## <a name="see-also"></a>Confira também

- [Controlar um MediaElement usando um Storyboard](how-to-control-a-mediaelement-by-using-a-storyboard.md)
