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





> ```java
>  @GET("user")
>  Call<ResponseBody> getData2(@Query("id") long idLon, @Query("name") String nameStr);
> ```
>
> 这个方法使用了 Retrofit 库的注解来定义一个 GET 请求，请求的路径是 "user"。其中包含两个参数：idLon 和 nameStr，分别对应着请求中的 "id" 和 "name" 参数。这个方法返回一个 Call 对象，泛型为 ResponseBody，用于处理请求的响应结果。 
>
> 这个方法的作用是获取用户数据，通过传入不同的 id 和 name 参数来获取不同用户的数据。我们可以根据实际需求来修改参数的类型和名称，以满足不同的业务需求。同时，在使用 Retrofit 进行网络请求时，我们还需要注意其他一些方面，比如请求的线程、请求头、请求体等等。



+ **Retrofit中@Path和@Query 的区别**

`@Path` 和 `@Query` 都是 Retrofit 中用于构建 URL 的注解，但它们的作用有所不同。

- `@Path`：用于替换 URL 中的占位符，例如：

```java
@GET("users/{id}")
Call<User> getUser(@Path("id") int userId);
```

在上面的例子中，`{id}` 是 URL 中的占位符，其值由 `@Path("id")` 中的参数提供。当调用 `getUser(1)` 时，Retrofit 会将 URL 替换为 `users/1`。

- `@Query`：用于添加查询参数，例如：

```java
@GET("users")
Call<List<User>> getUsers(@Query("sort") String sortBy, @Query("order") String order);
```

在上面的例子中，`sort` 和 `order` 是查询参数的名称，其值由 `@Query("sort")` 和 `@Query("order")` 中的参数提供。当调用 `getUsers("name", "asc")` 时，Retrofit 会将 URL 替换为 `users?sort=name&order=asc`。

因此，`@Path` 和 `@Query` 的区别在于，前者用于替换 URL 中的占位符，后者用于添加查询参数。



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
