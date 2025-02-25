---
title: Esplorazione dei tentativi di chiamate HTTP personalizzate con backoff esponenziale
description: Informazioni su come implementare da zero i tentativi di chiamata HTTP con backoff esponenziale per gestire possibili scenari di errore HTTP.
ms.date: 10/16/2018
ms.openlocfilehash: 2b40b73a014faa87d4eb42192c72b5a9b6c8d4fa
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65644734"
---
# <a name="explore-custom-http-call-retries-with-exponential-backoff"></a>Esplorazione dei tentativi di chiamate HTTP personalizzate con backoff esponenziale

Per creare microservizi resilienti, è necessario gestire i possibili scenari di errore HTTP. Un modo di gestire tali errori, ma non consigliabile, è quello di creare la propria implementazione di tentativi con il backoff esponenziale.

**Nota importante:** questa sezione illustra come creare il proprio codice personalizzato per implementare i tentativi di chiamata HTTP. Tuttavia è consigliabile non eseguire questa operazione da soli, ma avvalersi di un meccanismo potente e affidabile e più semplice da usare come `HttpClientFactory` con Polly, disponibile a partire da .NET Core 2.1. Questi approcci consigliati sono illustrati nelle sezioni successive.

Per iniziare, è possibile implementare codice personalizzato con una classe di utilità per il backoff esponenziale, come in [RetryWithExponentialBackoff.cs](https://gist.github.com/CESARDELATORRE/6d7f647b29e55fdc219ee1fd2babb260), e anche di codice come quello riportato di seguito.

```csharp
public sealed class RetryWithExponentialBackoff
{
    private readonly int maxRetries, delayMilliseconds, maxDelayMilliseconds;

    public RetryWithExponentialBackoff(int maxRetries = 50,
        int delayMilliseconds = 200,
        int maxDelayMilliseconds = 2000)
    {
        this.maxRetries = maxRetries;
        this.delayMilliseconds = delayMilliseconds;
        this.maxDelayMilliseconds = maxDelayMilliseconds;
    }

    public async Task RunAsync(Func<Task> func)
    {
        ExponentialBackoff backoff = new ExponentialBackoff(this.maxRetries,
            this.delayMilliseconds,
            this.maxDelayMilliseconds);
        retry:
        try
        {
            await func();
        }
        catch (Exception ex) when (ex is TimeoutException ||
            ex is System.Net.Http.HttpRequestException)
        {
            Debug.WriteLine("Exception raised is: " +
                ex.GetType().ToString() +
                " –Message: " + ex.Message +
                " -- Inner Message: " +
                ex.InnerException.Message);
            await backoff.Delay();
            goto retry;
        }
    }
}

public struct ExponentialBackoff
{
    private readonly int m_maxRetries, m_delayMilliseconds, m_maxDelayMilliseconds;
    private int m_retries, m_pow;

    public ExponentialBackoff(int maxRetries, int delayMilliseconds,
        int maxDelayMilliseconds)
    {
        m_maxRetries = maxRetries;
        m_delayMilliseconds = delayMilliseconds;
        m_maxDelayMilliseconds = maxDelayMilliseconds;
        m_retries = 0;
        m_pow = 1;
    }

    public Task Delay()
    {
        if (m_retries == m_maxRetries)
        {
            throw new TimeoutException("Max retry attempts exceeded.");
        }
        ++m_retries;
        if (m_retries < 31)
        {
            m_pow = m_pow << 1; // m_pow = Pow(2, m_retries - 1)
        }
        int delay = Math.Min(m_delayMilliseconds * (m_pow - 1) / 2,
            m_maxDelayMilliseconds);
        return Task.Delay(delay);
    }
}
```

Usando il codice in un'applicazione client C\# (un altro microservizio client dell'API Web, un'applicazione MVC ASP.NET o anche un'applicazione Xamarin C\#) è semplice. L'esempio seguente spiega come procedere usando la classe HttpClient.

```csharp
public async Task<Catalog> GetCatalogItems(int page,int take, int? brand, int? type)
{
    _apiClient = new HttpClient();
    var itemsQs = $"items?pageIndex={page}&pageSize={take}";
    var filterQs = "";
    var catalogUrl = $"{_remoteServiceBaseUrl}items{filterQs}?pageIndex={page}&pageSize={take}";
    var dataString = "";
    //
    // Using HttpClient with Retry and Exponential Backoff
    //
    var retry = new RetryWithExponentialBackoff();
    await retry.RunAsync(async () =>
    {
        // work with HttpClient call
        dataString = await _apiClient.GetStringAsync(catalogUrl);
    });
    return JsonConvert.DeserializeObject<Catalog>(dataString);
}
```

Ricordare che questo codice è adatto solo come modello di verifica. Le sezioni successive illustrano come usare approcci più sofisticati, anche se più semplici, tramite HttpClientFactory. HttpClientFactory è disponibile a partire dalla versione di .NET Core 2.1, con le librerie di resilienza collaudate come Polly.

>[!div class="step-by-step"]
>[Precedente](implement-resilient-entity-framework-core-sql-connections.md)
>[Successivo](use-httpclientfactory-to-implement-resilient-http-requests.md)
