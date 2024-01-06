private class ProxyOpenAIHandler : HttpClientHandler
{
    protected override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        if (request.RequestUri != null && request.RequestUri.Host.Equals("api.openai.com", StringComparison.OrdinalIgnoreCase))
        {
            // your proxy url
            request.RequestUri = new Uri($"https://openai.yourproxy.com/{request.RequestUri.PathAndQuery}");
        }
        return base.SendAsync(request, cancellationToken);
    }
}


var kernel = Kernel.Builder
        .WithOpenAIChatCompletionService("<model-id>", "<api-key>", httpClient: new HttpClient(new ProxyOpenAIHandler()))
        .Build();