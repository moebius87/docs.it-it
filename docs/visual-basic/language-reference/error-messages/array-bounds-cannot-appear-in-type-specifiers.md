---
title: I limiti di matrice non possono trovarsi negli identificatori di tipo
ms.date: 07/20/2015
f1_keywords:
- vbc30638
- bc30638
helpviewer_keywords:
- BC30638
ms.assetid: 93b654f4-70fa-4a48-baed-ffae42075550
ms.openlocfilehash: 50e1cd0e41da467a9e816c8e5d64d09a36923d65
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64665742"
---
# <a name="array-bounds-cannot-appear-in-type-specifiers"></a>I limiti di matrice non possono trovarsi negli identificatori di tipo
Le dimensioni di matrice non possono essere dichiarate come parte di un identificatore di tipo di dati.  
  
 **ID errore:** BC30638  
  
## <a name="to-correct-this-error"></a>Per correggere l'errore  
  
- Specificare le dimensioni della matrice subito dopo il nome della variabile anziché inserire la dimensione della matrice dopo il tipo, come illustrato nell'esempio seguente.  
  
    ```  
    Dim Array(8) As Integer   
    ```  
  
- Definire una matrice e inizializzarlo con il numero desiderato di elementi, come illustrato nell'esempio seguente.  
  
    ```  
    Dim Array2() As Integer = New Integer(8) {}  
    ```  
  
## <a name="see-also"></a>Vedere anche

- [Matrici](../../../visual-basic/programming-guide/language-features/arrays/index.md)
