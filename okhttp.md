### @FormUrlEncoded

**@FormUrlEncoded**：它的作用是将请求体中的数据编码为键值对的形式，以便服务器可以正确地解析请求数据。这个注解通常与@POST或@PUT等HTTP方法注解一起使用。



> ```java
> @FormUrlEncoded
> @POST("user/emails")
> Call<ResponseBody> getPostData2(@Field("name") String nameStr, @Field("sex") String sexStr);
> ```
>
> 这段代码是一个使用 Retrofit 进行 POST 请求的示例。其中，使用了注解 `@FormUrlEncoded` 表示请求体的编码格式为表单形式，即 `application/x-www-form-urlencoded`。接着，使用注解 `@POST("user/emails")` 指定了请求的 URL 地址为 `user/emails`。最后，使用注解 `@Field` 分别指定了两个表单字段 `name` 和 `sex` 的值，并将它们作为参数传入方法中。这样，在调用 `getPostData2` 方法时，Retrofit 会自动将这两个字段的值封装到请求体中，并发送给服务器。
>
> 需要注意的是，这里的返回类型是 `Call<ResponseBody>`，表示服务器响应的数据类型为 `ResponseBody`，即原始的响应体。在实际应用中，我们可能需要将响应体转换成其他类型的数据，如 JSON 或 Java 对象。此时，可以使用 Retrofit 提供的转换器来完成相应的转换操作。

### @Query



> ```css
> GET http://example.com/get_data.json?u=<user>&t=<token>
> 这是一种标准的带参数GET请求的格式。接口地址的最后使用问号来连接参数部分，每个参数都
> 是一个使用等号连接的键值对，多个参数之间使用“&”符号进行分隔。那么很显然，在上述地址
> 中，服务器要求我们传入user和token这两个参数的值。对于这种格式的服务器接口，我们可
> 以使用刚才所学的@Path注解的方式来解决，但是这样会有些麻烦，Retrofit针对这种带参数的
> GET请求，专门提供了一种语法支持：
> ```
>
> ```java
> @GET("user")
> Call<ResponseBody> getData2(@Query("id") long idLon, @Query("name") String nameStr);
> ```
>
> 这个方法使用了 Retrofit 库的注解来定义一个 GET 请求，请求的路径是 "user"。其中包含两个参数：idLon 和 nameStr，分别对应着请求中的 "id" 和 "name" 参数。这个方法返回一个 Call 对象，泛型为 ResponseBody，用于处理请求的响应结果。 
>
> 这个方法的作用是获取用户数据，通过传入不同的 id 和 name 参数来获取不同用户的数据。我们可以根据实际需求来修改参数的类型和名称，以满足不同的业务需求。同时，在使用 Retrofit 进行网络请求时，我们还需要注意其他一些方面，比如请求的线程、请求头、请求体等等。



### **Retrofit中@Path和@Query 的区别**

`@Path` 和 `@Query` 都是 Retrofit 中用于构建 URL 的注解，但它们的作用有所不同。

- `@Path`：用于替换 URL 中的占位符，例如：

```java
@GET("users/{id}")
Call<User> getUser(@Path("id") int userId);
```

在上面的例子中，`{id}` 是 URL 中的占位符，其值由 `@Path("id")` 中的参数提供。当调用 `getUser(1)` 时，Retrofit 会将 URL 替换为 `users/1`。

- `@Query`：用于添加查询参数（链接中出现`?`)，用于`GET`例如：

```java
@GET("users")
Call<List<User>> getUsers(@Query("sort") String sortBy, @Query("order") String order);
```

在上面的例子中，`sort` 和 `order` 是查询参数的名称，其值由 `@Query("sort")` 和 `@Query("order")` 中的参数提供。当调用 `getUsers("name", "asc")` 时，Retrofit 会将 URL 替换为 `users?sort=name&order=asc`。

因此，`@Path` 和 `@Query` 的区别在于，前者用于替换 URL 中的占位符，后者用于添加查询参数。

+ `@Field`，功能跟`@Query`一样，区别在于@Field常用于`POST`

  
  
  而Retrofit对所有常用的HTTP请求类型都进行了支持，使用@GET、@POST、@PUT、@PATCH、@DELETE注解，就可以让Retrofit发出相应类型的请求了。
  
  比如服务器提供了如下接口地址：
  
  DELETE http://example.com/data/<id>
  
  这种接口通常意味着要根据id删除一条指定的数据，而我们在Retrofit当中想要发出这种请求就
  
  可以这样写：
  
  ```kotlin
  interface ExampleService {
  
   @DELETE("data/{id}")
  
   fun deleteData(@Path("id") id: String): Call<ResponseBody>
  
  }
  ```
  
  这里使用了@DELETE注解来发出DELETE类型的请求，并使用了@Path注解来动态指定id，这些都很好理解。但是在返回值声明的时候，我们将Call的泛型指定成了ResponseBody，这是什么意思呢？
  
  **由于`POST、PUT 、PATCH、DELETE`这几种请求类型与GET请求不同，它们更多是用于操作服务器上的数据，而不是获取服务器上的数据，所以通常它们对于服务器响应的数据并不关心。这个时候就可以使用ResponseBody，表示Retrofit能够接收任意类型的响应数据，并且不会对响应数据进行解析。**

### @header

有些服务器接口还可能会要求我们在HTTP请求的header中指定参数，比如：

```css
GET http://example.com/get_data.json

User-Agent: okhttp

Cache-Control: max-age=0
```

这些header参数其实就是一个个的键值对，我们可以在Retrofit中直接使用@Headers注解来

对它们进行声明。

```kotlin
interface ExampleService {

 @Headers("User-Agent: okhttp", "Cache-Control: max-age=0")

 @GET("get_data.json")

 fun getData(): Call<Data>

}
```

但是这种写法只能进行静态header声明，如果想要动态指定header的值，则需要使用

`@Header`注解，如下所示：

```kotlin
interface ExampleService {

 @GET("get_data.json")

 fun getData(@Header("User-Agent") userAgent: String,

 @Header("Cache-Control") cacheControl: String): Call<Data>

}
```



#### 单例模式

```java
public static HttpUtil getInstance() {
        if (httpUtil!=null){
            return httpUtil;
        }else {
            return new HttpUtil();
        }
    }
```

> 这段代码是一个单例模式的实现，用于获取 HttpUtil 的唯一实例。如果 httpUtil 已经被实例化过，则直接返回该实例；否则，创建一个新的实例并返回。
>
> 单例模式的作用是保证一个类只有一个实例，并提供全局访问点。在多线程环境下，需要考虑线程安全问题，可以通过加锁等方式来保证线程安全。
>
> 在这段代码中，没有考虑线程安全问题，可能会出现多个线程同时调用 getInstance() 方法，导致创建多个实例的情况。因此，可以考虑在方法上加 synchronized 关键字来保证线程安全。
>
> 在 Java 中，当一个对象没有任何引用指向它时，它就会被垃圾回收器回收。对于单例模式来说，只有当整个应用程序退出时，该单例对象才会被销毁。
>
> 在这段代码中，当 httpUtil 对象被创建时，它会一直存在于内存中，直到整个应用程序退出。如果需要在某个时刻销毁该单例对象，可以提供一个销毁方法，并在该方法中将 httpUtil 置为 null，这样就可以触发垃圾回收器回收该对象了。
>
> “保证一个类只有一个实例，并提供全局访问点”是单例模式的核心思想。
>
> 在应用程序中，有些类只需要一个实例，例如配置文件、线程池、数据库连接池等。如果每次需要使用这些类时都创建一个新的实例，会浪费系统资源并影响程序性能，因此需要使用单例模式来保证只有一个实例存在。
>
> 单例模式通过将类的构造方法私有化，防止外部代码创建新的实例，同时提供一个全局访问点（通常是一个静态方法），使得外部代码可以获取该类的唯一实例。由于该实例是全局唯一的，因此可以在整个应用程序中共享该实例，避免了重复创建对象和数据不一致的问题。
>
> 需要注意的是，单例模式并不是适用于所有的场景，过度使用单例模式可能会导致代码耦合度过高、单例对象生命周期过长等问题，因此需要根据具体场景进行选择和设计。

#### 添加Header

（1）使用注解的方式

添加一个Header参数

```java
public interface UserService {  
    @Headers("Cache-Control: max-age=640000")
    @GET("/tasks")
    Call<List<Task>> getTasks();
}
```

添加多个Header参数

```java
public interface UserService {  
    @Headers({
        "Accept: application/vnd.yourapi.v1.full+json",
        "User-Agent: Your-App-Name"
    })
    @GET("/tasks/{task_id}")
    Call<Task> getTask(@Path("task_id") long taskId);
}
```

（2）使用代码的方式，则需要使用拦截器


```java
OkHttpClient.Builder httpClient = new OkHttpClient.Builder();  
httpClient.addInterceptor(new Interceptor() {  
    @Override
    public Response intercept(Interceptor.Chain chain) throws IOException {
        Request original = chain.request(); 
   Request request = original.newBuilder()
        .header("User-Agent", "Your-App-Name")
        .header("Accept", "application/vnd.yourapi.v1.full+json")
        .method(original.method(), original.body())
        .build();
 
    return chain.proceed(request);
}
    }

OkHttpClient client = httpClient.build();  
Retrofit retrofit = new Retrofit.Builder()  
    .baseUrl(API_BASE_URL)
    .addConverterFactory(GsonConverterFactory.create())
    .client(client)
    .build();


```
（3）使用注解的方式，但是Header参数每次提交的都不同，也就是动态的Header

```java
public interface UserService {  
    @GET("/tasks")
    Call<List<Task>> getTasks(@Header("Content-Range") String contentRange);
}
```

#### 获取Cookie

