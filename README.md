# Blackman AI C# SDK

Official C# client for [Blackman AI](https://www.useblackman.ai) - The AI API proxy that optimizes token usage to reduce costs.

## Features

- 🚀 Drop-in replacement for OpenAI, Anthropic, and other LLM APIs
- 💰 Automatic token optimization (save 20-40% on costs)
- 📊 Built-in analytics and cost tracking
- 🔒 Enterprise-grade security with SSO support
- ⚡ Low latency overhead (<50ms)
- 🎯 Semantic caching for repeated queries
- ⚙️ Async/await support
- 🎯 Strongly typed models

## Installation

### .NET CLI

```bash
dotnet add package Blackman.Client
```

### Package Manager Console

```powershell
Install-Package Blackman.Client
```

### PackageReference

```xml
<PackageReference Include="Blackman.Client" Version="0.0.9" />
```

## Quick Start

```csharp
using Blackman.Client.Api;
using Blackman.Client.Client;
using Blackman.Client.Model;

// Configure client
var config = new Configuration
{
    BasePath = "https://app.useblackman.ai",
    AccessToken = "sk_your_blackman_api_key"
};

var api = new CompletionsApi(config);

// Create completion request
var request = new CompletionRequest
{
    Provider = "OpenAI",
    Model = "gpt-4o",
    Messages = new List<Message>
    {
        new Message
        {
            Role = "user",
            Content = "Explain quantum computing in simple terms"
        }
    }
};

try
{
    // Send request
    var response = await api.CompletionsAsync(request);
    Console.WriteLine(response.Choices[0].Message.Content);
    Console.WriteLine($"Tokens used: {response.Usage.TotalTokens}");
}
catch (ApiException e)
{
    Console.WriteLine($"Error: {e.Message}");
}
```

## Authentication

Get your API key from the [Blackman AI Dashboard](https://app.useblackman.ai/settings/api-keys).

```csharp
var config = new Configuration
{
    BasePath = "https://app.useblackman.ai",
    AccessToken = "sk_your_blackman_api_key"
};
```

## Framework Integration

### ASP.NET Core

Configure dependency injection in `Program.cs`:

```csharp
using Blackman.Client.Api;
using Blackman.Client.Client;

var builder = WebApplication.CreateBuilder(args);

// Add Blackman client
builder.Services.AddSingleton(sp =>
{
    var config = new Configuration
    {
        BasePath = builder.Configuration["Blackman:Host"] ?? "https://app.useblackman.ai",
        AccessToken = builder.Configuration["Blackman:ApiKey"]
    };
    return new CompletionsApi(config);
});

builder.Services.AddControllers();

var app = builder.Build();

app.MapControllers();
app.Run();
```

Configure in `appsettings.json`:

```json
{
  "Blackman": {
    "Host": "https://app.useblackman.ai",
    "ApiKey": "sk_your_blackman_api_key"
  }
}
```

Use in a controller:

```csharp
using Blackman.Client.Api;
using Blackman.Client.Model;
using Microsoft.AspNetCore.Mvc;

namespace MyApi.Controllers;

[ApiController]
[Route("api/[controller]")]
public class ChatController : ControllerBase
{
    private readonly CompletionsApi _completionsApi;

    public ChatController(CompletionsApi completionsApi)
    {
        _completionsApi = completionsApi;
    }

    [HttpPost]
    public async Task<IActionResult> Chat([FromBody] ChatRequest request)
    {
        var completionRequest = new CompletionRequest
        {
            Provider = "OpenAI",
            Model = "gpt-4o",
            Messages = new List<Message>
            {
                new Message
                {
                    Role = "user",
                    Content = request.Message
                }
            }
        };

        try
        {
            var response = await _completionsApi.CompletionsAsync(completionRequest);
            return Ok(new
            {
                Response = response.Choices[0].Message.Content,
                Tokens = response.Usage.TotalTokens
            });
        }
        catch (Exception ex)
        {
            return StatusCode(500, new { Error = ex.Message });
        }
    }
}

public record ChatRequest(string Message);
```

### Minimal API

```csharp
using Blackman.Client.Api;
using Blackman.Client.Client;
using Blackman.Client.Model;

var builder = WebApplication.CreateBuilder(args);

var config = new Configuration
{
    BasePath = "https://app.useblackman.ai",
    AccessToken = builder.Configuration["Blackman:ApiKey"]
};

var api = new CompletionsApi(config);

var app = builder.Build();

app.MapPost("/chat", async (ChatRequest request) =>
{
    var completionRequest = new CompletionRequest
    {
        Provider = "OpenAI",
        Model = "gpt-4o",
        Messages = new List<Message>
        {
            new Message { Role = "user", Content = request.Message }
        }
    };

    var response = await api.CompletionsAsync(completionRequest);
    return Results.Ok(new
    {
        Response = response.Choices[0].Message.Content,
        Tokens = response.Usage.TotalTokens
    });
});

app.Run();

record ChatRequest(string Message);
```

## Advanced Usage

### Custom HTTP Client

```csharp
var httpClient = new HttpClient
{
    Timeout = TimeSpan.FromSeconds(60)
};

var config = new Configuration
{
    BasePath = "https://app.useblackman.ai",
    AccessToken = "sk_your_blackman_api_key"
};

var api = new CompletionsApi(httpClient, config);
```

### Error Handling

```csharp
using Blackman.Client.Client;

try
{
    var response = await api.CompletionsAsync(request);
    Console.WriteLine(response.Choices[0].Message.Content);
}
catch (ApiException ex)
{
    Console.WriteLine($"HTTP Status Code: {ex.ErrorCode}");
    Console.WriteLine($"Error Content: {ex.ErrorContent}");
    Console.WriteLine($"Headers: {string.Join(", ", ex.Headers)}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

### Cancellation Tokens

```csharp
var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30));

try
{
    var response = await api.CompletionsAsync(request, cancellationToken: cts.Token);
    Console.WriteLine(response.Choices[0].Message.Content);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Request timeout");
}
```

### Retry Logic with Polly

```csharp
using Polly;
using Polly.Retry;

var retryPolicy = Policy
    .Handle<ApiException>()
    .WaitAndRetryAsync(3, retryAttempt =>
        TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));

var response = await retryPolicy.ExecuteAsync(async () =>
    await api.CompletionsAsync(request));
```

### Parallel Requests

```csharp
var messages = new[] { "Message 1", "Message 2", "Message 3" };

var tasks = messages.Select(msg =>
{
    var request = new CompletionRequest
    {
        Provider = "OpenAI",
        Model = "gpt-4o",
        Messages = new List<Message>
        {
            new Message { Role = "user", Content = msg }
        }
    };
    return api.CompletionsAsync(request);
});

var responses = await Task.WhenAll(tasks);

foreach (var response in responses)
{
    Console.WriteLine(response.Choices[0].Message.Content);
}
```

## Documentation

- [Full API Reference](https://app.useblackman.ai/docs)
- [Getting Started Guide](https://app.useblackman.ai/docs/getting-started)
- [C# Examples](https://github.com/blackman-ai/csharp-sdk/tree/main/examples)

## Requirements

- .NET 8.0 or higher

## Support

- 📧 Email: [support@blackman.ai](mailto:support@blackman.ai)
- 💬 Discord: [Join our community](https://discord.gg/blackman-ai)
- 🐛 Issues: [GitHub Issues](https://github.com/blackman-ai/csharp-sdk/issues)

## License

MIT © Blackman AI
