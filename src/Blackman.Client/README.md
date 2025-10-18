# Created with Openapi Generator

<a id="cli"></a>
## Creating the library
Create a config.yaml file similar to what is below, then run the following powershell command to generate the library `java -jar "<path>/openapi-generator/modules/openapi-generator-cli/target/openapi-generator-cli.jar" generate -c config.yaml`

```yaml
generatorName: csharp
inputSpec: openapi.json
outputDir: out

# https://openapi-generator.tech/docs/generators/csharp
additionalProperties:
  packageGuid: '{44535332-98F6-4E79-BDF4-47196B169F62}'

# https://openapi-generator.tech/docs/integrations/#github-integration
# gitHost:
# gitUserId:
# gitRepoId:

# https://openapi-generator.tech/docs/globals
# globalProperties:

# https://openapi-generator.tech/docs/customization/#inline-schema-naming
# inlineSchemaOptions:

# https://openapi-generator.tech/docs/customization/#name-mapping
# modelNameMappings:
# nameMappings:

# https://openapi-generator.tech/docs/customization/#openapi-normalizer
# openapiNormalizer:

# templateDir: https://openapi-generator.tech/docs/templating/#modifying-templates

# releaseNote:
```

<a id="usage"></a>
## Using the library in your project

```cs
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Blackman.Client.Api;
using Blackman.Client.Client;
using Blackman.Client.Model;
using Org.OpenAPITools.Extensions;

namespace YourProject
{
    public class Program
    {
        public static async Task Main(string[] args)
        {
            var host = CreateHostBuilder(args).Build();
            var api = host.Services.GetRequiredService<ICompletionsApi>();
            ICompletionsApiResponse apiResponse = await api.CompletionsAsync("todo");
            CompletionResponse? model = apiResponse.Ok();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) => Host.CreateDefaultBuilder(args)
          .ConfigureApi((context, services, options) =>
          {
              // The type of token here depends on the api security specifications
              // Available token types are ApiKeyToken, BasicToken, BearerToken, HttpSigningToken, and OAuthToken.
              BearerToken token = new("<your token>");
              options.AddTokens(token);

              // optionally choose the method the tokens will be provided with, default is RateLimitProvider
              options.UseProvider<RateLimitProvider<BearerToken>, BearerToken>();

              options.ConfigureJsonOptions((jsonOptions) =>
              {
                  // your custom converters if any
              });

              options.AddApiHttpClients(client =>
              {
                  // client configuration
              }, builder =>
              {
                  builder
                      .AddRetryPolicy(2)
                      .AddTimeoutPolicy(TimeSpan.FromSeconds(5))
                      .AddCircuitBreakerPolicy(10, TimeSpan.FromSeconds(30));
                      // add whatever middleware you prefer
                  }
              );
          });
    }
}
```
<a id="questions"></a>
## Questions

- What about HttpRequest failures and retries?
  Configure Polly in the IHttpClientBuilder
- How are tokens used?
  Tokens are provided by a TokenProvider class. The default is RateLimitProvider which will perform client side rate limiting.
  Other providers can be used with the UseProvider method.
- Does an HttpRequest throw an error when the server response is not Ok?
  It depends how you made the request. If the return type is ApiResponse<T> no error will be thrown, though the Content property will be null.
  StatusCode and ReasonPhrase will contain information about the error.
  If the return type is T, then it will throw. If the return type is TOrDefault, it will return null.
- How do I validate requests and process responses?
  Use the provided On and After partial methods in the api classes.

## Api Information
- appName: Blackman AI API
- appVersion: 0.1.0
- appDescription: A transparent AI API proxy that optimizes token usage to reduce costs.  ## Authentication  Blackman AI supports two authentication methods:  ### 1. API Key (Recommended for integrations)  Use the API key created from your dashboard:  &#x60;&#x60;&#x60;bash curl -X POST https://ap.useblackman.ai/v1/completions \\   -H \&quot;Authorization: Bearer sk_your_api_key_here\&quot; \\   -H \&quot;Content-Type: application/json\&quot; \\   -d &#39;{\&quot;provider\&quot;: \&quot;OpenAI\&quot;, \&quot;model\&quot;: \&quot;gpt-4\&quot;, \&quot;messages\&quot;: [{\&quot;role\&quot;: \&quot;user\&quot;, \&quot;content\&quot;: \&quot;Hello!\&quot;}]}&#39; &#x60;&#x60;&#x60;  ### 2. JWT Token (For web UI)  Obtain a JWT token by logging in:  &#x60;&#x60;&#x60;bash curl -X POST https://ap.useblackman.ai/v1/auth/login \\   -H \&quot;Content-Type: application/json\&quot; \\   -d &#39;{\&quot;email\&quot;: \&quot;user@example.com\&quot;, \&quot;password\&quot;: \&quot;yourpassword\&quot;}&#39; &#x60;&#x60;&#x60;  Then use the token:  &#x60;&#x60;&#x60;bash curl -X POST https://ap.useblackman.ai/v1/completions \\   -H \&quot;Authorization: Bearer your_jwt_token\&quot; \\   -H \&quot;Content-Type: application/json\&quot; \\   -d &#39;{...}&#39; &#x60;&#x60;&#x60;  ### Provider API Keys (Optional)  You can optionally provide your own LLM provider API key via the &#x60;X-Provider-Api-Key&#x60; header, or store it in your account settings.  ## Client SDKs  Auto-generated SDKs are available for 10 languages:  - **TypeScript**: [View Docs](/v1/sdks/typescript) - **Python**: [View Docs](/v1/sdks/python) - **Go**: [View Docs](/v1/sdks/go) - **Java**: [View Docs](/v1/sdks/java) - **Ruby**: [View Docs](/v1/sdks/ruby) - **PHP**: [View Docs](/v1/sdks/php) - **C#**: [View Docs](/v1/sdks/csharp) - **Rust**: [View Docs](/v1/sdks/rust) - **Swift**: [View Docs](/v1/sdks/swift) - **Kotlin**: [View Docs](/v1/sdks/kotlin)  All SDKs are generated from this OpenAPI spec using [openapi-generator](https://openapi-generator.tech).  ## Quick Start  &#x60;&#x60;&#x60;python # Python example with API key import blackman_client from blackman_client import CompletionRequest  configuration &#x3D; blackman_client.Configuration(     host&#x3D;\&quot;http://localhost:8080\&quot;,     access_token&#x3D;\&quot;sk_your_api_key_here\&quot;  # Your Blackman API key )  with blackman_client.ApiClient(configuration) as api_client:     api &#x3D; blackman_client.CompletionsApi(api_client)     response &#x3D; api.completions(         CompletionRequest(             provider&#x3D;\&quot;OpenAI\&quot;,             model&#x3D;\&quot;gpt-4o\&quot;,             messages&#x3D;[{\&quot;role\&quot;: \&quot;user\&quot;, \&quot;content\&quot;: \&quot;Hello!\&quot;}]         )     ) &#x60;&#x60;&#x60;

## Build
This C# SDK is automatically generated by the [OpenAPI Generator](https://openapi-generator.tech) project.

- SDK version: 0.0.5
- Generator version: 7.14.0
- Build package: org.openapitools.codegen.languages.CSharpClientCodegen
