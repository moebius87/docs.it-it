---
title: Configurazione automatica di IPv6
ms.date: 03/30/2017
ms.assetid: 581c1d21-1013-43a3-bf3e-2d9ead62b79c
ms.openlocfilehash: 2c7f77bbdebe093b5e3bc706653ed203082d9c3f
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64647329"
---
# <a name="ipv6-auto-configuration"></a>Configurazione automatica di IPv6
Un obiettivo importante per IPv6 consiste nel supportare la modalità Plug and Play per i nodi. Questo significa che deve essere possibile collegare un nodo in una rete IPv6 perché il nodo venga automaticamente configurato senza alcun intervento manuale.  
  
## <a name="type-of-auto-configuration"></a>Tipo di configurazione automatica  
 IPv6 supporta i tipi di configurazione automatica seguenti:  
  
- **Configurazione automatica con stato**. Questo tipo di configurazione richiede un certo livello di intervento manuale, perché è necessario un server DHCPv6 (Dynamic Host Configuration Protocol for IPv6) per l'installazione e l'amministrazione dei nodi. Il server DHCPv6 mantiene un elenco di nodi a cui fornisce informazioni di configurazione. Mantiene inoltre le informazioni sullo stato in modo da determinare per quanto tempo ogni indirizzo è in uso e quando può essere disponibile per la riassegnazione.  
  
- **Configurazione automatica senza stato**. Questo tipo di configurazione è adatto per piccole organizzazioni e singoli utenti. In questo caso, ogni host determina il proprio indirizzo dal contenuto degli annunci router ricevuti. Usando lo standard IEEE EUI-64 per definire la parte dell'ID di rete dell'indirizzo, l'host presuppone in modo ragionevole l'univocità dell'indirizzo host nel collegamento.  
  
 Indipendentemente dal modo in cui viene determinato l'indirizzo, il nodo deve verificare che il proprio possibile indirizzo sia univoco per il collegamento locale. A questo scopo, viene inviato un messaggio Neighbor Solicitation al possibile indirizzo. Se il nodo riceve una risposta, sa che l'indirizzo è già in uso e deve determinarne un altro.  
  
## <a name="ipv6-mobility"></a>Mobilità di IPv6  
 La grande diffusione di dispositivi mobili ha introdotto un nuovo requisito: un dispositivo deve essere in grado di cambiare arbitrariamente posizione nella rete Internet IPv6, mantenendo comunque le connessioni esistenti. Per fornire questa funzionalità, a un nodo mobile viene assegnato un indirizzo di posizione iniziale a cui può essere sempre raggiunto. Quando il nodo mobile si trova nella posizione iniziale, si connette al collegamento di questa posizione e usa il proprio indirizzo di posizione iniziale. Quando il nodo mobile si trova in un'altra posizione, un home agent, che è in genere un router, inoltra i messaggi tra il nodo mobile e i nodi con cui sta comunicando.  
  
## <a name="see-also"></a>Vedere anche

- [Protocollo IPv6](../../../docs/framework/network-programming/internet-protocol-version-6.md)
- [Socket](../../../docs/framework/network-programming/sockets.md)
