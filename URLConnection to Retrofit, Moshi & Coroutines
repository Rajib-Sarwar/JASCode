# Gradual Improvement from URLConnection to Retrofit, Moshi & Coroutines

## Initial Approach (URLConnection)
Using HttpURLConnection involves extensive manual setup and error handling:

```kotlin
val url = URL("https://example.com/api/user")
val connection = url.openConnection() as HttpURLConnection
try {
    connection.requestMethod = "POST"
    connection.setRequestProperty("Content-Type", "application/json")
    connection.doOutput = true

    connection.outputStream.use { output ->
        output.write(jsonBody.toByteArray())
    }

    val response = connection.inputStream.bufferedReader().use { it.readText() }
    // Manually parse response JSON
} catch (e: Exception) {
    // Handle errors manually
} finally {
    connection.disconnect()
}
```

## Improvement with Retrofit
Retrofit simplifies HTTP calls by abstracting the boilerplate:

```kotlin
apiService.registerUser(body).enqueue(object : Callback<ResponseBody> {
  override fun onResponse(call: Call<ResponseBody>, response: Response<ResponseBody>) {
    if (response.isSuccessful) {
      val result = response.body()?.string()
      // Handle success
    } else {
      // Handle server error
    }
  }

  override fun onFailure(call: Call<ResponseBody>, error: Throwable) {
    // Handle network failure
  }
})
```

## Further Improvement with Moshi
Moshi automates JSON parsing, reducing manual work:

```kotlin
@JsonClass(generateAdapter = true)
data class RegisterBody(val username: String, val email: String, val password: String)

interface ApiService {
  @POST("user/register")
  fun registerUser(@Body body: RegisterBody): Call<ResponseBody>
}

val retrofit = Retrofit.Builder()
    .baseUrl(BASE_URL)
    .client(OkHttpClient.Builder().build())
    .addConverterFactory(MoshiConverterFactory.create())
    .build()

val apiService = retrofit.create(ApiService::class.java)
```

## Ultimate Improvement with Kotlin Coroutines
Integrating coroutines makes asynchronous calls straightforward and intuitive:

```kotlin
interface ApiService {
  @POST("user/register")
  suspend fun registerUser(@Body body: RegisterBody): Response<Unit>
}

suspend fun registerUser(username: String, email: String, password: String): Result<String> =
  runCatching {
    val response = apiService.registerUser(RegisterBody(username, email, password))
    if (response.isSuccessful) response.message()
    else throw Exception(response.errorBody()?.string())
  }

// Handling response clearly
movieDiaryApi.registerUser(username, email, password)
  .onSuccess { response ->
    // Handle successful registration
  }
  .onFailure { error ->
    // Handle registration error
  }
```

## Benefits at Each Stage
- **URLConnection → Retrofit**: Reduced boilerplate, clearer API structure.
- **Retrofit → Moshi**: Automated JSON parsing, strongly-typed objects.
- **Moshi → Coroutines**: Clean asynchronous code, straightforward error handling.

This progression clearly illustrates how modern tools significantly improve Android app development.

