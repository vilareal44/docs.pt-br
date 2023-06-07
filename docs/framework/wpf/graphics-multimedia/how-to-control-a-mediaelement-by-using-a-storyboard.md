---
title: Como controlar um MediaElement usando um storyboard
description: Controle a reprodução de mídia usando um storyboard no WPF (Windows Presentation Foundation). Considere este exemplo para criar um player de mídia simples.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- multimedia [WPF], controlling playback of media with Storyboards
- controlling playback of media [WPF], with Storyboards
- Storyboards [WPF], controlling playback of media with
- media [WPF], controlling playback with Storyboards
- playback of media [WPF], controlling with Storyboards
ms.assetid: 6128ca77-b826-4e36-b968-6f237157c543
ms.openlocfilehash: 5a5e41b9a28211495fd3374c1a51a655dd867bca
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853733"
---
# <a name="how-to-control-a-mediaelement-by-using-a-storyboard"></a>Como controlar um MediaElement usando um storyboard
Este exemplo mostra como controlar um <xref:System.Windows.Controls.MediaElement> usando um <xref:System.Windows.Media.MediaTimeline> em um <xref:System.Windows.Media.Animation.Storyboard> .  
  
## <a name="example"></a>Exemplo  
 Quando você usa um <xref:System.Windows.Media.MediaTimeline> em a <xref:System.Windows.Media.Animation.Storyboard> para controlar o tempo de um <xref:System.Windows.Controls.MediaElement> , a funcionalidade é idêntica à funcionalidade de outros <xref:System.Windows.Media.Animation.Timeline> objetos, como animações. Por exemplo, um <xref:System.Windows.Media.MediaTimeline> usa <xref:System.Windows.Media.Animation.Timeline> Propriedades como a <xref:System.Windows.Media.Animation.Timeline.BeginTime%2A> propriedade para especificar quando iniciar um <xref:System.Windows.Controls.MediaElement> (iniciar reprodução de mídia). Ele também usa a <xref:System.Windows.Media.Animation.Timeline.Duration%2A> propriedade para especificar por quanto tempo o <xref:System.Windows.Controls.MediaElement> está ativo (duração da reprodução de mídia). Para obter mais informações sobre como usar <xref:System.Windows.Media.Animation.Timeline> objetos com um <xref:System.Windows.Media.Animation.Storyboard> , consulte [visão geral de storyboards](storyboards-overview.md).  
  
 Este exemplo mostra como criar um player de mídia simples que usa um <xref:System.Windows.Media.MediaTimeline> para controlar a reprodução. O player de mídia inclui botões de reprodução, retomada, reinício e parada. O Player também tem um <xref:System.Windows.Controls.Slider> controle que atua como uma barra de progresso.  
  
 O exemplo a seguir cria o [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] para o player de mídia.  
  
 [!code-xaml[MediaGallery_snip#MediaTimelineExampleWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MediaGallery_snip/VB/MediaTimelineExample.xaml#mediatimelineexamplewholepage)]  
  
 O exemplo a seguir cria a funcionalidade para a barra de progresso.  
  
 [!code-csharp[MediaGallery_snip#CodeBehindMediaTimelineExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/MediaGallery_snip/CSharp/MediaTimelineExample.xaml.cs#codebehindmediatimelineexamplewholepage)]
 [!code-vb[MediaGallery_snip#CodeBehindMediaTimelineExampleWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/MediaGallery_snip/VB/MediaTimelineExample.xaml.vb#codebehindmediatimelineexamplewholepage)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.MediaElement>
- <xref:System.Windows.Media.MediaTimeline>
- <xref:System.Windows.Media.Animation.Storyboard>
- [Controlar um MediaElement (reproduzir, pausar, parar, volume e velocidade)](how-to-control-a-mediaelement-play-pause-stop-volume-and-speed.md)
- [Visão geral de storyboards](storyboards-overview.md)
- [Visão geral das animações de quadro-chave](key-frame-animations-overview.md)
- [Visão geral da animação](animation-overview.md)
- [Tópicos explicativos](audio-and-video-how-to-topics.md)
- [Gráficos e multimídia](index.md)
