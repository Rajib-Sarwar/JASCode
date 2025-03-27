# Retrofit Quick Reference

## Purpose
Simplifies HTTP network requests in Android/Java apps by reducing boilerplate code through annotations and interfaces.

## Retrofit Call Interface
- **Call<T>** represents a single network request and response.
- Strongly typed, enabling automatic serialization/deserialization.
- Example:
  ```kotlin
  @GET("user")
  fun getProfile(): Call<User>
  ```

## Handling Raw Responses
- Use **ResponseBody** for direct raw response handling.
- Essential to close ResponseBody after use to prevent resource leaks.
- Example:
  ```kotlin
  @GET("user")
  fun getProfile(): Call<ResponseBody>
  ```

## Sending Data with RequestBody
- Represents the body of HTTP requests (POST, PUT).
- Can be created from strings, files, or byte arrays.
- Example:
  ```kotlin
  val body = RequestBody.create(MediaType.parse("application/json"), jsonBody)

  @POST("user/register")
  fun registerUser(@Body body: RequestBody): Call<ResponseBody>
  ```

## Making Requests
- **Synchronous (`execute()`):** Blocks current thread; avoid on UI thread.
- **Asynchronous (`enqueue()`):** Recommended; uses callbacks, non-blocking.

### Example:
```kotlin
apiService.registerUser(body).enqueue(object : Callback<ResponseBody> {
  override fun onResponse(call: Call<ResponseBody>, response: Response<ResponseBody>) {
    val responseString = response.body()?.string()
    // handle success or error
  }

  override fun onFailure(call: Call<ResponseBody>, error: Throwable) {
    // handle network or request error
  }
})
```

## JSON Parsing
- Manually parse JSON responses into Kotlin objects when not using a converter.
- Example:
  ```kotlin
  val json = JSONObject(response.body()?.string())
  val token = json.getString("token")
  ```

## Error Handling
- Always check `response.isSuccessful` and handle errors appropriately.
- Extract error details with `response.errorBody()?.string()`.

## Alternative
- **Ktor**: Kotlin-based framework suitable for multiplatform development.

