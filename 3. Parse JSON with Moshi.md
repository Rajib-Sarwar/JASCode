# Retrofit & Moshi Quick Reference

## Purpose
Simplifies HTTP network requests and JSON parsing in Android/Kotlin apps through concise annotations, interfaces, and automated serialization/deserialization.

## Retrofit Call Interface
- **Call<T>** represents a single network request and response.
- Strongly typed, enabling automatic serialization/deserialization.

Example:
```kotlin
@GET("user")
fun getProfile(): Call<User>
```

## Handling Raw Responses
- Use **ResponseBody** for direct raw response handling.
- Essential to close ResponseBody after use to prevent resource leaks.

Example:
```kotlin
@GET("user")
fun getProfile(): Call<ResponseBody>
```

## Sending Data with RequestBody
- Represents the body of HTTP requests (POST, PUT).
- Can be created from strings, files, or byte arrays.

Example:
```kotlin
val body = RequestBody.create(MediaType.parse("application/json"), jsonBody)

@POST("user/register")
fun registerUser(@Body body: RequestBody): Call<ResponseBody>
```

## Making Requests
- **Synchronous (`execute()`):** Blocks current thread; avoid on UI thread.
- **Asynchronous (`enqueue()`):** Recommended; uses callbacks, non-blocking.

Example:
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

## Moshi Integration
- **Serialization**: Converts Kotlin objects to JSON strings.
- **Deserialization**: Converts JSON strings back to Kotlin objects.

### Adding Moshi to Project
```gradle
implementation "com.squareup.moshi:moshi:1.15.1"
ksp "com.squareup.moshi:moshi-kotlin-codegen:1.15.1"
implementation "com.squareup.retrofit2:converter-moshi:2.9.0"
```

### Moshi Annotations
- Generate adapters automatically:
```kotlin
@JsonClass(generateAdapter = true)
data class User(val name: String, val email: String)
```
- Rename JSON fields:
```kotlin
@Json(name = "last_name") val lastName: String
```
- Ignore fields:
```kotlin
@Json(ignore = true) val password: String = "default"
```

## Using Moshi with Retrofit
Configure Retrofit to use Moshi:
```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl(BASE_URL)
    .client(OkHttpClient.Builder().build())
    .addConverterFactory(MoshiConverterFactory.create())
    .build()

val apiService = retrofit.create(ApiService::class.java)
```

Example Retrofit service method:
```kotlin
@POST("user/register")
fun registerUser(@Body body: RegisterBody): Call<Unit>
```

## Error Handling
- Check `response.isSuccessful`.
- Retrieve error details using `response.errorBody()?.string()`.

## Alternative
- **Ktor**: Kotlin-based framework suitable for multiplatform development.

