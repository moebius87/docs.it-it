---
title: -nowarn (opzioni del compilatore C#)
ms.date: 07/20/2015
f1_keywords:
- /nowarn
helpviewer_keywords:
- nowarn compiler option [C#]
- /nowarn compiler option [C#]
- -nowarn compiler option [C#]
ms.assetid: 6dcbc5e8-ae67-4566-9df3-f63cfdd9c4e4
ms.openlocfilehash: b455a2f719e7350c51cf4a1f095d4669529d0e5e
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64592802"
---
# <a name="-nowarn-c-compiler-options"></a>-nowarn (opzioni del compilatore C#)
L'opzione **-nowarn** impedisce al compilatore di visualizzare uno o più avvisi. Separare più numeri di avviso con una virgola.  
  
## <a name="syntax"></a>Sintassi  
  
```console  
-nowarn:number1[,number2,...]  
```  
  
## <a name="arguments"></a>Argomenti  
 `number1`, `number2`  
 Il numero o i numeri degli avvisi che il compilatore non deve visualizzare.  
  
## <a name="remarks"></a>Osservazioni  
 Specificare solo la parte numerica dell'identificatore dell'avviso. Ad esempio, per eliminare l'avviso CS0028 è possibile specificare `-nowarn:28`.  
  
 Il compilatore ignorerà automaticamente i numeri di avviso passati a `-nowarn` validi nelle versioni precedenti ma rimossi dal compilatore. Ad esempio, CS0679 era valido nel compilatore in Visual Studio .NET 2002 ma è stato rimosso successivamente.  
  
 Gli avvisi seguenti non possono essere eliminati dall'opzione `-nowarn`:  
  
- Avviso del compilatore (livello 1) CS2002  
  
- Avviso del compilatore (livello 1) CS2023  
  
- Avviso del compilatore (livello 1) CS2029  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Per impostare l'opzione del compilatore nell'ambiente di sviluppo di Visual Studio  
  
1. Aprire la pagina **Proprietà** del progetto. Per informazioni dettagliate, vedere [Pagina Compilazione, Creazione progetti (C#)](/visualstudio/ide/reference/build-page-project-designer-csharp).  
  
2. Fare clic sulla pagina delle proprietà **Compilazione**.  
  
3. Modificare la proprietà **Non visualizzare avvisi**.  
  
 Per informazioni su come impostare questa opzione del compilatore a livello di codice, vedere <xref:VSLangProj80.ProjectProperties3.DelaySign%2A>.  
  
## <a name="see-also"></a>Vedere anche

- [Opzioni del compilatore C#](../../../csharp/language-reference/compiler-options/index.md)
- [Gestione delle proprietà di progetti e soluzioni](/visualstudio/ide/managing-project-and-solution-properties)
- [Errori del compilatore C#](../../../csharp/language-reference/compiler-messages/index.md)
