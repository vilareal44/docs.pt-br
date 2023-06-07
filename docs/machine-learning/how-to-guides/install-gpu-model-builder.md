---
title: Como instalar o suporte de GPU no construtor de modelos
description: Saiba como instalar o suporte a GPU no construtor de modelos
ms.date: 08/18/2020
author: luisquintanilla
ms.author: luquinta
ms.topic: how-to
ms.openlocfilehash: ce629efa4c12a69f87196de35ebfe4331dc0800f
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608561"
---
# <a name="how-to-install-gpu-support-in-model-builder"></a>Como instalar o suporte de GPU no construtor de modelos

Saiba como instalar os drivers de GPU para usar sua GPU com o construtor de modelos.

## <a name="prerequisites"></a>Pré-requisitos

- Extensão do Visual Studio do construtor de modelos. A extensão é incorporada ao Visual Studio a partir da versão 16.6.1.
- [Extensão de suporte do Visual Studio GPU do construtor de modelos](https://marketplace.visualstudio.com/items?itemName=MLNET.ModelBuilderGPU).
- Pelo menos uma GPU compatível com CUDA. Para obter uma lista de GPUs compatíveis, consulte o [Guia do NVIDIA](https://developer.nvidia.com/cuda-gpus).
- Conta de desenvolvedor NVIDIA. Se você não tiver uma, [crie uma conta gratuita](https://developer.nvidia.com/developer-program).

## <a name="install-dependencies"></a>Instalar dependências

1. Instale o [CUDA v 10.0](https://developer.nvidia.com/cuda-10.0-download-archive). Certifique-se de instalar o CUDA v 10.0, e não qualquer outra versão mais recente. Você não pode ter várias versões do CUDA instaladas.
1. Instale o [cuDNN v 7.6.4 para CUDA 10,0](https://developer.nvidia.com/rdp/cudnn-download). Você não pode ter várias versões do cuDNN instaladas. Depois de baixar o arquivo zip cuDNN v 7.6.4 e descompactá-lo, copie `<CUDNN_zip_files_path>\cuda\bin\cudnn64_7.dll` para `<YOUR_DRIVE>\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\bin` .

## <a name="troubleshooting"></a>Solução de problemas

**Como fazer saber qual GPU eu tenho?**

Siga as instruções fornecidas:

1. Clique com o botão direito do mouse em área de trabalho
1. Se você vir "painel de controle NVIDIA" ou "vídeo NVIDIA" na janela pop-up, você terá uma GPU NVIDIA
1. Clique em "painel de controle NVIDIA" ou "vídeo NVIDIA" na janela pop-up
1. Veja "informações sobre a placa gráfica"
1. Você verá o nome da GPU NVIDIA

**Não vejo o painel de controle NVIDIA (ou ele não é aberto), mas sei que tenho uma GPU NVIDIA.**

1. Abra o Gerenciador de Dispositivos
1. Examinar adaptadores de vídeo
1. Instale o [Driver](https://www.nvidia.com/drivers) apropriado para sua GPU.
