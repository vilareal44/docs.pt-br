---
title: Assemblies de coleção para geração de tipos dinâmicos
description: Introdução aos assemblies de coleção para geração dinâmica de tipos no .NET. Leia sobre as restrições e tempos de vida do assembly de coleção.
ms.date: 08/29/2017
helpviewer_keywords:
- reflection, dynamic assembly
- assemblies, collectible
- collectible assemblies, retrieving
ms.openlocfilehash: 4981b93dbd49a6da96740bebed0f2ed7b89036c8
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475119"
---
# <a name="collectible-assemblies-for-dynamic-type-generation"></a>Assemblies de coleção para geração de tipos dinâmicos

*Assemblies de coleção* são assemblies dinâmicos que podem ser descarregados sem descarregar o domínio do aplicativo no qual eles foram criados. Toda a memória gerenciada e não gerenciada usada por um assembly de coleção e os tipos que ele contém pode ser recuperada. Informações como o nome do assembly são removidas das tabelas internas.

Para habilitar o descarregamento, use o sinalizador <xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndCollect?displayProperty=nameWithType> ao criar um assembly dinâmico. O assembly é transitório (ou seja, ele não pode ser salvo) e está sujeito às limitações descritas na seção [Restrições de assemblies de coleção](#restrictions-on-collectible-assemblies). O CLR (Common Language Runtime) descarrega um assembly de coleção automaticamente quando todos os objetos associados ao assembly são liberados. Em todos os outros aspectos, os assemblies de coleção são criados e usados da mesma maneira de outros assemblies dinâmicos.

## <a name="lifetime-of-collectible-assemblies"></a>Tempo de vida dos assemblies de coleção

O tempo de vida de um assembly de coleção é controlado pela existência de referências aos tipos que ele contém e aos objetos que são criados com base nesses tipos. O Common Language Runtime não descarregará um assembly contanto que uma ou mais das seguintes condições exista (`T` é qualquer tipo definido no assembly):

- Uma instância de `T`.

- Uma instância de uma matriz de `T`.

- Uma instância de um tipo genérico que tem `T` como um de seus argumentos de tipo. Isso inclui coleções genéricas de `T`, mesmo se a coleção estiver vazia.

- Uma instância de <xref:System.Type> ou <xref:System.Reflection.Emit.TypeBuilder> que representa `T`.

   > [!IMPORTANT]
   > É necessário liberar todos os objetos que representam partes do assembly. O <xref:System.Reflection.Emit.ModuleBuilder> que define `T` mantém uma referência ao <xref:System.Reflection.Emit.TypeBuilder> e o objeto <xref:System.Reflection.Emit.AssemblyBuilder> mantém uma referência ao <xref:System.Reflection.Emit.ModuleBuilder>; portanto, as referências a esses objetos devem ser liberadas. Até mesmo a existência de um <xref:System.Reflection.Emit.LocalBuilder> ou um <xref:System.Reflection.Emit.ILGenerator> usado na construção de `T` impede o descarregamento.

- Uma referência estática a `T` por outro tipo definido de forma dinâmica `T1` que ainda pode ser acessado pelo código em execução. Por exemplo, `T1` pode derivar de `T` ou `T` pode ser o tipo de um parâmetro em um método de `T1`.

- Uma **ByRef** a um campo estático que pertence a `T`.

- Um <xref:System.RuntimeTypeHandle>, <xref:System.RuntimeFieldHandle> ou <xref:System.RuntimeMethodHandle> que se refere a `T` ou a um componente de `T`.

- Uma instância de qualquer objeto de reflexão que poderá ser usada indireta ou diretamente para acessar o objeto <xref:System.Type> que representa `T`. Por exemplo, o objeto <xref:System.Type> de `T` pode ser obtido de um tipo de matriz cujo tipo de elemento é `T` ou de um tipo genérico que tem `T` como um argumento de tipo.

- Um método `M` na pilha de chamadas de qualquer thread, em que `M` é um método de `T` ou um método de nível de módulo definido no assembly.

- Um representante de um método estático definido em um módulo do assembly.

Se houver um único item dessa lista para apenas um tipo ou um método no assembly, o runtime não poderá descarregar o assembly.

> [!NOTE]
> Na verdade, o runtime não descarrega o assembly até que os finalizadores tenham sido executados para todos os itens da lista.

Para fins de acompanhamento do tempo de vida, um tipo genérico construído, como `List<int>` (no C#) ou `List(Of Integer)` (no Visual Basic), que é criado e usado na geração de um assembly de coleção é considerado como tendo sido definido no assembly que contém a definição de tipo genérico ou em um assembly que contém a definição de um de seus argumentos de tipo. O assembly exato usado é um detalhe de implementação e está sujeito a alterações.

## <a name="restrictions-on-collectible-assemblies"></a>Restrições de assemblies de coleção

As seguintes restrições se aplicam a assemblies de coleção:

- **Referências estáticas** Tipos em um assembly dinâmico comum não podem ter referências estáticas a tipos que são definidos em um assembly de coleção. Por exemplo, se você definir um tipo comum herdado de um tipo em um assembly de coleção, uma exceção <xref:System.NotSupportedException> será gerada. Um tipo em um assembly de coleção pode ter referências estáticas a um tipo em outro assembly de coleção, mas isso aumenta o tempo de vida do assembly referenciado para o tempo de vida do assembly de referência.

- **Interoperabilidade com** Nenhuma interface COM pode ser definida em um assembly de coleção e nenhuma instância de tipos dentro de um assembly de coleção pode ser convertida em objetos COM. Um tipo em um assembly de coleção não pode servir como um CCW (COM Callable Wrapper) ou RCW (Runtime Callable Wrapper). No entanto, os tipos em assemblies de coleção podem usar objetos que implementam interfaces COM.

- **Invocação de plataforma** Os métodos que têm o <xref:System.Runtime.InteropServices.DllImportAttribute> atributo não serão compilados quando forem declarados em um assembly de coleção. A instrução <xref:System.Reflection.Emit.OpCodes.Calli?displayProperty=nameWithType> não pode ser usada na implementação de um tipo em um assembly de coleção e esses tipos não podem ter marshaling para um código não gerenciado. No entanto, você pode chamar um código nativo por meio de um ponto de entrada declarado em um assembly de não coleção.

- **Marshaling** Objetos (em particular, delegados) que são definidos em assemblies de coleção não podem ser empacotados. Essa é uma restrição de todos os tipos emitidos transitórios.

- **Carregamento de assembly** A emissão de reflexão é o único mecanismo com suporte para carregar assemblies de coleção. Assemblies carregados por meio de qualquer outra forma de carregamento do assembly não podem ser descarregados.

- **Objetos associados ao contexto** Não há suporte para variáveis de contexto estático. Tipos em um assembly de coleção não podem estender <xref:System.ContextBoundObject>. No entanto, um código em assemblies de coleção podem usar objetos associados a contexto definidos em outro lugar.

- **Dados estáticos de thread** Não há suporte para variáveis de thread estático.

## <a name="see-also"></a>Consulte também

- [Emissão de métodos e assemblies dinâmicos](emitting-dynamic-methods-and-assemblies.md)
