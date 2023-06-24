### ChatGPT APP

#### ①添加权限

```css
<uses-permission android:name="android.permission.INTERNET"/>
```

#### ②创建输入框形状

```css
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle"
    >

<corners android:radius="100dp"/>
<stroke android:width="1dp"/>
    <solid android:color="@color/white"/>
</shape>
```



#### ③修改布局文件

```css
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@id/bottom_layout"
        />
        <TextView
            android:id="@+id/welcome_text"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:padding="5sp"
            android:text="@string/welcome"
            android:gravity="center"
            android:textSize="28sp"
            />
        <RelativeLayout
            android:id="@+id/bottom_layout"
            android:layout_width="match_parent"
            android:layout_height="80dp"
            android:padding="8dp"
            android:layout_alignParentBottom="true"
            >

            <EditText
                android:id="@+id/message_editText"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_centerInParent="true"
                android:background="@drawable/rounded_corner"
                android:hint="@string/hint"
                android:padding="16dp" />
            <ImageButton
                android:id="@+id/send"
                android:layout_width="48dp"
                android:layout_height="48dp"
                android:layout_alignParentEnd="true"
                android:layout_centerInParent="true"
                android:layout_marginStart="10dp"
                android:src="@drawable/ic_baseline_send_24"
                android:background="?attr/selectableItemBackgroundBorderless"
                android:padding="8dp" />

        </RelativeLayout>

</RelativeLayout>
```

#### ④创建消息类

```java
public class Message {
    public static final String SENT_KEY = " sent_key";
    public static final String SENT_BY_BOT=" sent_by_bot";

    String message;
    String sentBy;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public String getSentBy() {
        return sentBy;
    }

    public void setSentBy(String sentBy) {
        this.sentBy = sentBy;
    }

    public Message(String message, String sentBy) {
        this.message = message;
        this.sentBy = sentBy;
    }
}
```

#### ⑤创建适配器

```java
public class MessageAdapter extends RecyclerView.Adapter<MessageAdapter.MyViewHolder>{
    private List<Message> messageList;

    public MessageAdapter(List<Message> messageList) {
        this.messageList = messageList;
    }

    @NonNull
    @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.chat_item,parent,false);
        return new MyViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        Message message = messageList.get(position);
        if(message.getSentBy().equals(Message.SENT_BY_USER)){
            holder.leftChatView.setVisibility(View.GONE);
            holder.rightChatView.setVisibility(View.VISIBLE);
            holder.rightMsgView.setText(message.getMessage());
        }else {
            holder.leftChatView.setVisibility(View.VISIBLE);
            holder.rightChatView.setVisibility(View.GONE);
            holder.leftMsgView.setText(message.getMessage());
        }
    }

    @Override
    public int getItemCount() {
        return messageList.size();
    }

    public static class MyViewHolder extends RecyclerView.ViewHolder{
        LinearLayout leftChatView,rightChatView;
        TextView leftMsgView,rightMsgView;
        public MyViewHolder(View itemView) {
            super(itemView);
            leftChatView = itemView.findViewById(R.id.left_chat_view);
            rightChatView = itemView.findViewById(R.id.right_chat_view);
            leftMsgView = itemView.findViewById(R.id.left_chat_text_view);
            rightMsgView = itemView.findViewById(R.id.right_chat_text_view);
        }
    }
}
```

#### ⑥绑定适配器

```java
public class MainActivity extends AppCompatActivity {
    RecyclerView recyclerView;
    TextView welcomeTextView;
    EditText messageEditView;
    ImageButton sendButton;
    List<Message> messageList;
    MessageAdapter messageAdapter;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        messageList = new ArrayList<>();
        recyclerView=findViewById(R.id.recyclerView);
        welcomeTextView = findViewById(R.id.welcome_text);
        messageEditView=findViewById(R.id.message_editText);
        sendButton=findViewById(R.id.send);

        //设置RecyclerView
        messageAdapter = new MessageAdapter(messageList);
        recyclerView.setAdapter(messageAdapter);
        LinearLayoutManager layoutManager=new LinearLayoutManager(this);
        layoutManager.setStackFromEnd(true);
        recyclerView.setLayoutManager(layoutManager);

        sendButton.setOnClickListener(v -> {
            String message = messageEditView.getText().toString().trim();
            if(message.isEmpty()){
                Toast.makeText(MainActivity.this, "Message cannot be empty", Toast.LENGTH_SHORT).show();
                return;
            }
            Toast.makeText(MainActivity.this, message, Toast.LENGTH_SHORT).show();
            messageEditView.setText("");
            addToChat(message, Message.SENT_BY_USER);
        });


    }
    void addToChat(String message, String sentBy){
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                messageList.add(new Message(message, sentBy));
                messageAdapter.notifyDataSetChanged();
                recyclerView.smoothScrollToPosition(messageAdapter.getItemCount());
            }
        });

    }
}
```

#### **⑦添加`OkHttp`依赖**

```css
    implementation 'com.squareup.okhttp3:okhttp:4.10.0'
    implementation 'com.squareup.okhttp3:logging-interceptor:4.10.0'
```

#### ⑧修改`MainActivity`中的代码

```java
public class MainActivity extends AppCompatActivity {
    RecyclerView recyclerView;
    TextView welcomeTextView;
    EditText messageEditView;
    ImageButton sendButton;
    List<Message> messageList;
    MessageAdapter messageAdapter;

    public static final MediaType JSON
            = MediaType.get("application/json; charset=utf-8");

    OkHttpClient client = new OkHttpClient();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        messageList = new ArrayList<>();
        recyclerView=findViewById(R.id.recyclerView);
        welcomeTextView = findViewById(R.id.welcome_text);
        messageEditView=findViewById(R.id.message_editText);
        sendButton=findViewById(R.id.send);

        //设置RecyclerView
        messageAdapter = new MessageAdapter(messageList);
        recyclerView.setAdapter(messageAdapter);
        LinearLayoutManager layoutManager=new LinearLayoutManager(this);
        layoutManager.setStackFromEnd(true);
        recyclerView.setLayoutManager(layoutManager);

        //发送数据
        sendButton.setOnClickListener(v -> {
            String message = messageEditView.getText().toString().trim();
            if(message.isEmpty()){
                Toast.makeText(MainActivity.this, "Message cannot be empty", Toast.LENGTH_SHORT).show();
                return;
            }
            messageEditView.setText("");
            addToChat(message, Message.SENT_BY_USER);
            callApi(message);
            welcomeTextView.setVisibility(View.GONE);

        });


    }
    //将数据添加到适配器的数据列表里面
    void addToChat(String message, String sentBy){
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                messageList.add(new Message(message, sentBy));
                messageAdapter.notifyDataSetChanged();
                recyclerView.smoothScrollToPosition(messageAdapter.getItemCount());
            }
        });

    }

    //添加请求到的数据到适配器中的数据
    void addResponse(String response){
        addToChat(response,Message.SENT_BY_BOT);

    }

    //通过API发送请求，并接受返回数据，进行解析
    void callApi(String question) {



        JSONObject jsonObject=new JSONObject();
        try {
            jsonObject.put("model","text-davinci-003");
            jsonObject.put("prompt",question);
            jsonObject.put("max_tokens",4000);
            jsonObject.put("temperature",0);

        } catch (JSONException e) {
            e.printStackTrace();
        }

        RequestBody body=RequestBody.create(jsonObject.toString(),JSON);
        Request request=new Request.Builder()
                .url("https://api.openai.com/v1/completions")
                .header("Authorization", "Bearer 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")
                .post(body)
                .build();
            client.newCall(request).enqueue(new Callback() {
                @Override
                public void onFailure(@NonNull Call call, @NonNull IOException e) {
                    addResponse("error"+e.getMessage());
                }

                @Override
                public void onResponse(@NonNull Call call, @NonNull Response response) throws IOException {
                    JSONObject jsonObject= null;
                    try {
                        jsonObject = new JSONObject(response.body().string());
                        JSONArray jsonArray=jsonObject.getJSONArray("choices");
                        String result=jsonArray.getJSONObject(0).getString("text");
                        Log.e("6666666666666",result);
                        addResponse(result.trim());
                    } catch (JSONException e) {
                        e.printStackTrace();
                    }
                }
            });

    }
}
```

<span style="background-color:#a2e043;"><b>运行结果：</b></span>



<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306222047784.gif" alt="Screenrecording_20230622_204638" style="zoom:25%;" />

#### ⑨由于网络的原因，使用国内镜像API网站访问

> 网站：[OpenAI API 代理 (openai-proxy.com)](https://www.openai-proxy.com/)



```java
public class MainActivity extends AppCompatActivity {
    RecyclerView recyclerView;
    TextView welcomeTextView;
    EditText messageEditView;
    EditText api_input;
    Button add_apiKey;
    ImageButton sendButton;
    List<Message> messageList;
    MessageAdapter messageAdapter;

    public static final MediaType JSON
            = MediaType.get("application/json; charset=utf-8");

    OkHttpClient client = new OkHttpClient();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Connector.getDatabase();
        messageList = new ArrayList<>();
        recyclerView=findViewById(R.id.recyclerView);
        welcomeTextView = findViewById(R.id.welcome_text);
        messageEditView=findViewById(R.id.message_editText);
        sendButton=findViewById(R.id.send);
        add_apiKey=findViewById(R.id.add_apiKey);
        api_input=findViewById(R.id.api_input);
        //设置RecyclerView
        messageAdapter = new MessageAdapter(messageList);
        recyclerView.setAdapter(messageAdapter);
        LinearLayoutManager layoutManager=new LinearLayoutManager(this);
        layoutManager.setStackFromEnd(true);
        recyclerView.setLayoutManager(layoutManager);
        add_apiKey.setOnClickListener(v -> {
            String apiKey = api_input.getText().toString().trim();
            if(apiKey.isEmpty()){
                Toast.makeText(MainActivity.this, "API Key cannot be empty", Toast.LENGTH_SHORT).show();
                return;
            }
            api_input.setText("");
            LitePal.deleteAll(ApiKey.class,"id > ?","0");
            ApiKey apiKey1=new ApiKey();
            apiKey1.setApi(apiKey);
            apiKey1.save();

        });
        //发送数据
        sendButton.setOnClickListener(v -> {
            String message = messageEditView.getText().toString().trim();
            if(message.isEmpty()){
                Toast.makeText(MainActivity.this, "Message cannot be empty", Toast.LENGTH_SHORT).show();
                return;
            }
            messageEditView.setText("");
            addToChat(message, Message.SENT_BY_USER);
            callApi(message);
            welcomeTextView.setVisibility(View.GONE);

        });


    }
    //将数据添加到适配器的数据列表里面
    void addToChat(String message, String sentBy){
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                messageList.add(new Message(message, sentBy));
                messageAdapter.notifyDataSetChanged();
                recyclerView.smoothScrollToPosition(messageAdapter.getItemCount());
            }
        });

    }

    //添加请求到的数据到适配器中的数据
    void addResponse(String response){
        addToChat(response,Message.SENT_BY_BOT);

    }

    //通过API发送请求，并接受返回数据，进行解析
    void callApi(String question) {



        JSONObject jsonObject=new JSONObject();
        try {
            jsonObject.put("model", "gpt-3.5-turbo");
            JSONArray messages = new JSONArray();
            JSONObject msg = new JSONObject();
            msg.put("role", "user");
            msg.put("content", question);
            messages.put(msg);
            jsonObject.put("messages", messages);
        } catch (JSONException e) {
            e.printStackTrace();
        }
        String api="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";
        List<ApiKey> apiKeys=LitePal.findAll(ApiKey.class);
        if(apiKeys.size()!=0) {
            ApiKey apiKey = LitePal.findFirst(ApiKey.class);
             api=apiKey.getApi();
        }
        RequestBody body=RequestBody.create(jsonObject.toString(),JSON);
        Request request=new Request.Builder()
                .url("https://api.openai-proxy.com/v1/chat/completions")
                .post(body)
                .addHeader("Authorization", "Bearer "+api)
                .addHeader("Content-Type", "application/json")
                .build();
            client.newCall(request).enqueue(new Callback() {
                @Override
                public void onFailure(@NonNull Call call, @NonNull IOException e) {
                    addResponse("error"+e.getMessage());
                }

                @Override
                public void onResponse(@NonNull Call call, @NonNull Response response) throws IOException {
                    JSONObject jsonObject= null;
                    try {
                        jsonObject = new JSONObject(response.body().string());
                        Log.e("6666666666666",jsonObject.toString().trim());
                        JSONArray jsonArray=jsonObject.getJSONArray("choices");
                        JSONObject jsonObject1=jsonArray.getJSONObject(0).getJSONObject("message");
                        String content=jsonObject1.getString("content");
                        Log.e("6666666666666",content);
                        addResponse(content.trim());
                    } catch (JSONException e) {
                        e.printStackTrace();
                    }
                }
            });

    }
}
```

<span style="background-color:#a2e043;"><b>运行结果：</b></span>·

<img src="https://voyager0587.oss-cn-guangzhou.aliyuncs.com/%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/202306222203821.gif" alt="Screenrecording_20230622_220059" style="zoom:25%;" />
