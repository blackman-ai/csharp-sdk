# Blackman.Client.Api.CompletionsApi

All URIs are relative to *https://app.useblackman.ai/v1*

| Method | HTTP request | Description |
|--------|--------------|-------------|
| [**Completions**](CompletionsApi.md#completions) | **POST** /v1/completions |  |

<a id="completions"></a>
# **Completions**
> CompletionResponse Completions (CompletionRequest completionRequest, string xProviderApiKey = null)




### Parameters

| Name | Type | Description | Notes |
|------|------|-------------|-------|
| **completionRequest** | [**CompletionRequest**](CompletionRequest.md) |  |  |
| **xProviderApiKey** | **string** | Optional provider API key to override stored or system keys | [optional]  |

### Return type

[**CompletionResponse**](CompletionResponse.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | AI completion response |  -  |
| **401** | Unauthorized - missing or invalid authentication token |  -  |
| **402** | Payment required - no active subscription |  -  |
| **500** | Provider error |  -  |
| **503** | Service unavailable - provider API key not configured |  -  |

[[Back to top]](#) [[Back to API list]](../../README.md#documentation-for-api-endpoints) [[Back to Model list]](../../README.md#documentation-for-models) [[Back to README]](../../README.md)

