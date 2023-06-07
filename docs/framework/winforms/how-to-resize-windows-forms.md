---
title: Redimensionar formulário
description: Saiba como redimensionar a altura e a largura do formulário definindo um novo valor para a propriedade tamanho ou ajuste as propriedades de altura ou largura individualmente.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- resizing Windows Forms
- Windows Forms, resizing
ms.assetid: 5d9dd47e-e68c-48c9-a0a3-a9ff34ba009d
ms.openlocfilehash: 0d6383e4d29d9407d3da97bf8b94761f06d99748
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903266"
---
# <a name="how-to-resize-windows-forms"></a>Como: redimensionar o Windows Forms

Você pode especificar o tamanho do seu Windows Forms de várias maneiras. Você pode alterar a altura e a largura do formulário programaticamente definindo um novo valor para a <xref:System.Windows.Forms.Form.Size%2A> propriedade ou ajustar as <xref:System.Windows.Forms.Control.Height%2A> <xref:System.Windows.Forms.Control.Width%2A> Propriedades ou individualmente. Se você estiver usando o Visual Studio, poderá alterar o tamanho usando o Designer de Formulários do Windows. Consulte também [Como redimensionar Windows Forms usando o Designer](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/37k2zkwx(v=vs.100)).

## <a name="resize-a-form-programmatically"></a>Redimensionar um formulário programaticamente

Defina o tamanho de um formulário em tempo de execução definindo a <xref:System.Windows.Forms.Form.Size%2A> Propriedade do formulário.

O exemplo de código a seguir mostra o tamanho do formulário definido como 100 x 100 pixels.

```vb
Form1.Size = New System.Drawing.Size(100, 100)
```

```csharp
Form1.Size = new System.Drawing.Size(100, 100);
```

```cpp
Form1->Size = System::Drawing::Size(100, 100);
```

## <a name="change-form-width-and-height-programmatically"></a>Alterar a largura e a altura do formulário programaticamente

Depois que o <xref:System.Windows.Forms.Form.Size%2A> for definido, altere a altura ou a largura do formulário usando <xref:System.Windows.Forms.Control.Width%2A> as <xref:System.Windows.Forms.Control.Height%2A> Propriedades ou.

O exemplo de código a seguir mostra a largura do formulário definida para 300 pixels da borda esquerda do formulário, enquanto a altura permanece constante.

```vb
Form1.Width = 300
```

```csharp
Form1.Width = 300;
```

```cpp
Form1->Width = 300;
```

-ou-

Altere <xref:System.Drawing.Size.Width%2A> ou <xref:System.Drawing.Size.Height%2A> definindo a <xref:System.Windows.Forms.Form.Size%2A> propriedade.

No entanto, como mostra o exemplo de código a seguir, essa abordagem é mais complicada do que apenas definir <xref:System.Windows.Forms.Control.Width%2A> ou <xref:System.Windows.Forms.Control.Height%2A> Propriedades.

```vb
Form1.Size = New Size(300, Form1.Size.Height)
```

```csharp
Form1.Size = new Size(300, Form1.Size.Height);
```

```cpp
Form1->Size = System::Drawing::Size(300, Form1->Size.Height);
```

## <a name="change-form-size-by-increments-programmatically"></a>Alterar o tamanho do formulário por incrementos programaticamente

Para incrementar o tamanho do formulário, defina as <xref:System.Drawing.Size.Width%2A> <xref:System.Drawing.Size.Height%2A> Propriedades e.

O exemplo de código a seguir mostra a largura do formulário definida para 200 pixels mais larga do que a configuração atual.

```vb
Form1.Width += 200
```

```csharp
Form1.Width += 200;
```

```cpp
Form1->Width += 200;
```

> [!CAUTION]
> Sempre use a <xref:System.Drawing.Size.Height%2A> <xref:System.Drawing.Size.Width%2A> propriedade ou para alterar uma dimensão de um formulário, a menos que você esteja definindo dimensões de altura e largura ao mesmo tempo definindo a <xref:System.Windows.Forms.Form.Size%2A> propriedade como uma nova <xref:System.Drawing.Size> estrutura. A <xref:System.Windows.Forms.Form.Size%2A> propriedade retorna uma <xref:System.Drawing.Size> estrutura, que é um tipo de valor. Não é possível atribuir um novo valor para a propriedade de um tipo de valor. Portanto, o código a seguir não será compilado.

```vb
' NOTE: CODE WILL NOT COMPILE
Dim f As New Form()
f.Size.Width += 100
```

```csharp
// NOTE: CODE WILL NOT COMPILE
Form f = new Form();
f.Size.Width += 100;
```

```cpp
// NOTE: CODE WILL NOT COMPILE
Form^ f = gcnew Form();
f->Size->X += 100;
```

## <a name="see-also"></a>Veja também

- [Introdução com Windows Forms](getting-started-with-windows-forms.md)
- [Aprimorando aplicativos do Windows Forms](./advanced/index.md)
