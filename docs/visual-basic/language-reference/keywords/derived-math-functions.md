---
title: Funções Matemáticas Derivadas
ms.date: 07/20/2015
helpviewer_keywords:
- arithmetic operations, derived math functions
- cosecant function
- arcsecant function
- arccotangent function
- functions [Visual Basic], derived math functions
- inverse functions
- math functions, derived
- derived math functions
- cotangent function
- angles
- secant function
- trigonometric functions
- logarithms
- arccosecant function
- hyperbolic functions
- arcsine function
- degrees
- arccosine function
ms.assetid: 63e449d8-9444-44fb-8db1-6d9cf346e2aa
ms.openlocfilehash: 611f3d8faf2148b8a983467d9ace4fd6c18b30e6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84373869"
---
# <a name="derived-math-functions-visual-basic"></a>Funções matemáticas derivadas (Visual Basic)
A tabela a seguir mostra funções matemáticas não intrínsecas que podem ser derivadas das funções matemáticas intrínsecas do <xref:System.Math?displayProperty=nameWithType> objeto. Você pode acessar as funções matemáticas intrínsecas adicionando `Imports System.Math` ao seu arquivo ou projeto.  
  
|Função|Equivalentes derivados|  
|--------------|-------------------------|  
|Secante (s (x))|1/cos (x)|  
|Cossecante (CSC (x))|1/sin (x)|  
|Cotangente (ctan (x))|1/tan (x)|  
|Seno inverso (Asen (x))|ATAN (x/sqrt (-x * x + 1))|  
|Cosseno inverso (ACOS (x))|ATAN (-x/sqrt (-x * x + 1)) + 2 \* ATAN (1)|  
|Secante inversa (ASEC (x))|2 * ATAN (1) – ATAN (sinal (x)/sqrt (x \* x – 1))|  
|Cossecante inversa (acsc (x))|ATAN (sinal (x)/sqrt (x * x – 1))|  
|Cotangente inversa (ACOT (x))|2 * ATAN (1)-ATAN (x)|  
|Seno hiperbólico (sinh (x))|(Exp (x) – exp (-x))/2|  
|Cosseno hiperbólico (cosh (x))|(Exp (x) + exp (-x))/2|  
|Tangente hiperbólica (TANH (x))|(Exp (x) – exp (-x))/(exp (x) + exp (-x))|  
|Secante Hiperbólica (sech (x))|2/(exp (x) + exp (-x))|  
|Cossecante hiperbólica (csch (x))|2/(exp (x) – exp (-x))|  
|Cotangente hiperbólica (coth (x))|(Exp (x) + exp (-x))/(exp (x) – exp (-x))|  
|Seno hiperbólico inverso (Asinh (x))|Log (x + sqrt (x * x + 1))|  
|Cosseno hiperbólico inverso (ACOSH (x))|Log (x + sqrt (x * x – 1))|  
|Tangente hiperbólica inversa (ATANH (x))|Log ((1 + x)/(1 – x))/2|  
|Secante hiperbólica inversa (AsecH (x))|Log ((sqrt (-x * x + 1) + 1)/x)|  
|Cossecante hiperbólica inversa (Acsch (x))|Log ((sinal (x) * sqrt (x \* x + 1) + 1)/x)|  
|Cotangente hiperbólica inversa (Acoth (x))|Log ((x + 1)/(x – 1))/2|  
  
## <a name="see-also"></a>Confira também

- [Funções matemáticas](../functions/math-functions.md)
