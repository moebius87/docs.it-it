---
title: -addmodule
ms.date: 03/09/2018
helpviewer_keywords:
- /addmodule compiler option [Visual Basic]
- addmodule compiler option [Visual Basic]
- -addmodule compiler option [Visual Basic]
ms.assetid: fb4b89d4-4926-4f20-868d-427fa28497b2
ms.openlocfilehash: 2de5fe82f1969a2fdb305d45951d7d698252c0c8
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61839218"
---
# <a name="-addmodule"></a>-addmodule
Fa sì che il compilatore renda disponibili per il progetto in compilazione tutte le informazioni sui tipi presenti nei file specificati.  
  
## <a name="syntax"></a>Sintassi  
  
```  
-addmodule:fileList  
```  
  
## <a name="arguments"></a>Argomenti  
 `fileList`  
 Obbligatorio. Elenco delimitato da virgole di file che contengono i metadati, ma non contengono manifesti dell'assembly. I nomi di file contenenti spazi devono essere racchiusi tra virgolette doppie ("").  
  
## <a name="remarks"></a>Note  
 I file elencati in base al `fileList` parametro deve essere creato con il `-target:module` opzione, o con equivalente un'altra del compilatore `-target:module`.  
  
 Tutti i moduli aggiunti con `-addmodule` deve essere nella stessa directory del file di output in fase di esecuzione. Vale a dire, è possibile specificare un modulo in qualsiasi directory in fase di compilazione, ma il modulo deve essere nella directory dell'applicazione in fase di esecuzione. In caso contrario, otterrai un <xref:System.TypeLoadException> errore.  
  
 Se si specificano (in modo implicito o esplicito) eventuali[-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md) opzione diverso da `-target:module` con `-addmodule`, i file passati alla `-addmodule` diventano parte dell'assembly del progetto. Un assembly è necessario per eseguire un file di output che contiene uno o più file aggiunti con `-addmodule`.  
  
 Uso [/reference (Visual Basic)](../../../visual-basic/reference/command-line-compiler/reference.md) per importare metadati da un file contenente un assembly.  
  
> [!NOTE]
>  Il `-addmodule` opzione non è disponibile all'interno dell'ambiente di sviluppo di Visual Studio, è disponibile solo durante la compilazione dalla riga di comando.  
  
## <a name="example"></a>Esempio  
 Il codice seguente crea un modulo.  
  
 [!code-vb[VbVbalrCompiler#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#47)]  
  
 Il codice seguente importa i tipi del modulo.  
  
 [!code-vb[VbVbalrCompiler#48](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#48)]  
  
 Quando si esegue `t1`, restituisce come output `802`.  
  
## <a name="see-also"></a>Vedere anche

- [Compilatore della riga di comando di Visual Basic](../../../visual-basic/reference/command-line-compiler/index.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [-riferimenti (Visual Basic)](../../../visual-basic/reference/command-line-compiler/reference.md)
- [Esempi di righe di comando di compilazione](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
