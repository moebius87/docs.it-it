---
title: -win32res (opzioni del compilatore C#)
ms.date: 07/20/2015
f1_keywords:
- /win32res
helpviewer_keywords:
- win32res compiler option
- /win32res compiler option [C#]
- -win32res compiler option [C#]
- win32res compiler option [C#]
ms.assetid: 3c33f750-6948-4c7e-a27e-bef98f77255b
ms.openlocfilehash: 4026fcbd7dc2ef29c1e7ee01a0f37b3ff471b187
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2019
ms.locfileid: "59322382"
---
# <a name="-win32res-c-compiler-options"></a>-win32res (opzioni del compilatore C#)
L'opzione **-win32res** inserisce una risorsa Win32 nel file di output.  
  
## <a name="syntax"></a>Sintassi  
  
```console  
-win32res:filename  
```  
  
## <a name="arguments"></a>Argomenti  
 `filename`  
 Il file di risorse da aggiungere al file di output.  
  
## <a name="remarks"></a>Osservazioni  
 Un file di risorse Win32 può essere creato con il [compilatore di risorse](../../language-reference/compiler-options/resource-compiler-option.md). Il Compilatore di risorse viene richiamato quando si compila un programma Visual C++. Dal file .rc viene creato un file .res.  
  
 In una risorsa Win32 può essere contenute le informazioni sulla versione o sulla bitmap (icona) che consentono di identificare l'applicazione in Esplora file. Se non si specifica **-win32res**, il compilatore genererà le informazioni sulla versione in base alla versione dell'assembly.  
  
 Vedere [-linkresource](../../../csharp/language-reference/compiler-options/linkresource-compiler-option.md) (per fare riferimento a un file di risorse di .NET Framework) o a [-resource](../../../csharp/language-reference/compiler-options/resource-compiler-option.md) (per associarlo).  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Per impostare l'opzione del compilatore nell'ambiente di sviluppo di Visual Studio  
  
1. Aprire la pagine **Proprietà** del progetto.  
  
2. Fare clic sulla pagina delle proprietà **Applicazione**.  
  
3. Fare clic sul pulsante **File di risorse** e scegliere un file usando la casella combinata.  
  
## <a name="example"></a>Esempio  
 Compilare `in.cs` e associare un file di risorse Win32 `rf.res` per produrre `in.exe`:  
  
```console  
csc -win32res:rf.res in.cs  
```  
  
## <a name="see-also"></a>Vedere anche

- [Opzioni del compilatore C#](../../../csharp/language-reference/compiler-options/index.md)
- [Gestione delle proprietà di progetti e soluzioni](/visualstudio/ide/managing-project-and-solution-properties)
