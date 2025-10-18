# Blackman.Client.Api.SemanticCacheApi

All URIs are relative to *https://app.useblackman.ai/v1*

| Method | HTTP request | Description |
|--------|--------------|-------------|
| [**GetConfig**](SemanticCacheApi.md#getconfig) | **GET** /v1/semantic-cache/config | Get semantic cache configuration for the current account |
| [**GetStats**](SemanticCacheApi.md#getstats) | **GET** /v1/semantic-cache/stats | Get semantic cache statistics including hit rate and savings |
| [**InvalidateAll**](SemanticCacheApi.md#invalidateall) | **DELETE** /v1/semantic-cache/invalidate | Invalidate all cache entries for the current account |
| [**UpdateConfig**](SemanticCacheApi.md#updateconfig) | **PUT** /v1/semantic-cache/config | Update semantic cache configuration for the current account |

<a id="getconfig"></a>
# **GetConfig**
> SemanticCacheConfig GetConfig ()

Get semantic cache configuration for the current account


### Parameters
This endpoint does not need any parameter.
### Return type

[**SemanticCacheConfig**](SemanticCacheConfig.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | Cache configuration retrieved successfully |  -  |
| **401** | Unauthorized |  -  |
| **500** | Internal server error |  -  |

[[Back to top]](#) [[Back to API list]](../../README.md#documentation-for-api-endpoints) [[Back to Model list]](../../README.md#documentation-for-models) [[Back to README]](../../README.md)

<a id="getstats"></a>
# **GetStats**
> SemanticCacheStats GetStats ()

Get semantic cache statistics including hit rate and savings


### Parameters
This endpoint does not need any parameter.
### Return type

[**SemanticCacheStats**](SemanticCacheStats.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | Cache statistics retrieved successfully |  -  |
| **401** | Unauthorized |  -  |
| **500** | Internal server error |  -  |

[[Back to top]](#) [[Back to API list]](../../README.md#documentation-for-api-endpoints) [[Back to Model list]](../../README.md#documentation-for-models) [[Back to README]](../../README.md)

<a id="invalidateall"></a>
# **InvalidateAll**
> InvalidateResponse InvalidateAll ()

Invalidate all cache entries for the current account


### Parameters
This endpoint does not need any parameter.
### Return type

[**InvalidateResponse**](InvalidateResponse.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | All cache entries invalidated successfully |  -  |
| **401** | Unauthorized |  -  |
| **500** | Internal server error |  -  |
| **503** | Semantic cache not configured |  -  |

[[Back to top]](#) [[Back to API list]](../../README.md#documentation-for-api-endpoints) [[Back to Model list]](../../README.md#documentation-for-models) [[Back to README]](../../README.md)

<a id="updateconfig"></a>
# **UpdateConfig**
> void UpdateConfig (SemanticCacheConfig semanticCacheConfig)

Update semantic cache configuration for the current account


### Parameters

| Name | Type | Description | Notes |
|------|------|-------------|-------|
| **semanticCacheConfig** | [**SemanticCacheConfig**](SemanticCacheConfig.md) |  |  |

### Return type

void (empty response body)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: Not defined


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **204** | Cache configuration updated successfully |  -  |
| **400** | Invalid configuration values |  -  |
| **401** | Unauthorized |  -  |
| **500** | Internal server error |  -  |

[[Back to top]](#) [[Back to API list]](../../README.md#documentation-for-api-endpoints) [[Back to Model list]](../../README.md#documentation-for-models) [[Back to README]](../../README.md)

