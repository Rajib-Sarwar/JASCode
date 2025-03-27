### Retrofit Overview
Retrofit is a popular, type-safe HTTP client for Android and Java, simplifying the creation of network requests.

### Purpose of Retrofit
- Simplifies network requests and response parsing.
- Provides clear, concise code via annotations and interfaces.
- Automates boilerplate tasks (serialization/deserialization, threading).

### Retrofit vs. HttpUrlConnection

| Feature                 | Retrofit                             | HttpUrlConnection                 |
|-------------------------|--------------------------------------|-----------------------------------|
| **Concise API Definition** | Interfaces with annotated methods.  | Manual request/response handling. |
| **Type Safety**         | Compile-time verification.           | Manual, error-prone conversions.  |
| **Serialization**       | Automated via libraries (Moshi, etc.). | Manual parsing required.          |
| **Threading**           | Built-in support via `enqueue()` or `execute()`. | Manual thread handling required. |

### Essential Retrofit Components

- **Base URL**: Common part of API endpoint URLs.
- **OkHttp Client**: Handles actual network operations.
- **ApiService Interface**: Defines API endpoints using annotations.

### Retrofit Implementation Steps

1. **Dependency Setup**
```gradle
implementation "com.squareup.retrofit2:retrofit:2.9.0"
```

2. **Create API Interface** (`MovieDiaryApiService` example)
```kotlin
interface MovieDiaryApiService {
  @POST("user/register") fun registerUser()
  @POST("user/login") fun loginUser()
  @GET("user") fun getProfile()
  @GET("movies") fun getMovies()
  @POST("movies") fun makeEntry()
}
```

3. **Retrofit Configuration**

Create a configuration file (`RetrofitConfig.kt`):

```kotlin
private const val BASE_URL = "https://http-api-93211a10efe2.herokuapp.com/"

private fun buildClient(): OkHttpClient = OkHttpClient.Builder().build()

private fun buildRetrofit(): Retrofit = Retrofit.Builder()
    .baseUrl(BASE_URL)
    .client(buildClient())
    .build()

fun buildMovieDiaryService(): MovieDiaryApiService =
    buildRetrofit().create(MovieDiaryApiService::class.java)
```

### Alternative to Retrofit
- **Ktor**: A Kotlin framework by JetBrains suitable for multiplatform apps. Useful when Retrofit is not ideal (e.g., Kotlin Multiplatform).

