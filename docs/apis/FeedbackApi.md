# Blackman.Client.Api.FeedbackApi

All URIs are relative to *https://app.useblackman.ai/v1*

| Method | HTTP request | Description |
|--------|--------------|-------------|
| [**SubmitFeedback**](FeedbackApi.md#submitfeedback) | **POST** /v1/feedback | Submit feedback for a completion response |

<a id="submitfeedback"></a>
# **SubmitFeedback**
> SubmitFeedbackResponse SubmitFeedback (SubmitFeedbackRequest submitFeedbackRequest)

Submit feedback for a completion response


### Parameters

| Name | Type | Description | Notes |
|------|------|-------------|-------|
| **submitFeedbackRequest** | [**SubmitFeedbackRequest**](SubmitFeedbackRequest.md) |  |  |

### Return type

[**SubmitFeedbackResponse**](SubmitFeedbackResponse.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | Feedback submitted successfully |  -  |
| **401** | Unauthorized |  -  |
| **500** | Internal server error |  -  |

[[Back to top]](#) [[Back to API list]](../../README.md#documentation-for-api-endpoints) [[Back to Model list]](../../README.md#documentation-for-models) [[Back to README]](../../README.md)

