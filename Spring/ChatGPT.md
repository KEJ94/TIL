## 1. ChatGPT 연동
__API KEY 발급__  
https://platform.openai.com/settings/organization/billing/overview  
<br/>
__build__
```.gradle
implementation 'com.squareup.okhttp3:okhttp:4.9.3'
implementation 'com.google.code.gson:gson:2.8.8'
```
__ChatGPTClient__
```.java
package com.jiransnc.vada;

import com.google.gson.Gson;
import com.google.gson.JsonArray;
import com.google.gson.JsonObject;
import okhttp3.*;

import java.io.IOException;

public class ChatGPTClient {

    private static final String API_KEY = "";
    private static final String BASE_URL = "https://api.openai.com/v1/chat/completions";

    private final OkHttpClient client;
    private final Gson gson;

    public ChatGPTClient() {
        this.client = new OkHttpClient();
        this.gson = new Gson();
    }

    public String sendMessage(String prompt) throws IOException {
        JsonObject json = new JsonObject();
        json.addProperty("model", "gpt-3.5-turbo");

        JsonArray messages = new JsonArray();
        JsonObject message = new JsonObject();
        message.addProperty("role", "user");
        message.addProperty("content", prompt);
        messages.add(message);

        json.add("messages", messages);
        json.addProperty("max_tokens", 150);

        RequestBody body = RequestBody.create(MediaType.parse("application/json"), gson.toJson(json));

        Request request = new Request.Builder()
                .url(BASE_URL)
                .post(body)
                .addHeader("Authorization", "Bearer " + API_KEY)
                .build();

        String text = "";

        try (Response response = client.newCall(request).execute()) {
            if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);

            String responseBody = response.body().string();
            JsonObject jsonResponse = gson.fromJson(responseBody, JsonObject.class);

            // choices 배열에서 assistant의 content 속성을 가져옴
            JsonArray choices = jsonResponse.getAsJsonArray("choices");
            text += choices.get(0).getAsJsonObject().getAsJsonObject("message").get("content").getAsString();
        }

        return text;
    }

    public static void main(String[] args) {
        ChatGPTClient client = new ChatGPTClient();
        try {
            String response = client.sendMessage("");
            System.out.println(response);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
## 2. Fine-Tuning 수행
__FineTuneDataUploader__
```.java
package com.jiransnc.vada;

import okhttp3.*;

import java.io.File;
import java.io.IOException;

public class FineTuneDataUploader {

    private static final String API_KEY = "";
    private static final String BASE_URL = "https://api.openai.com/v1/files";
    private final OkHttpClient client;

    public FineTuneDataUploader() {
        this.client = new OkHttpClient();
    }

    //public String uploadFile(String filePath) throws IOException {
    public String uploadFile() throws IOException {
        String filePath = "C:\\Users\\kth\\Documents\\GitHub\\fine_tune_data.jsonl";
        File file = new File(filePath);

        RequestBody fileBody = RequestBody.create(MediaType.parse("application/json"), file);
        MultipartBody requestBody = new MultipartBody.Builder()
                .setType(MultipartBody.FORM)
                .addFormDataPart("purpose", "fine-tune")
                .addFormDataPart("file", file.getName(), fileBody)
                .build();

        Request request = new Request.Builder()
                .url(BASE_URL)
                .post(requestBody)
                .addHeader("Authorization", "Bearer " + API_KEY)
                .build();

        try (Response response = client.newCall(request).execute()) {
            if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);

            return response.body().string();
        }
    }

    public static void main(String[] args) {
        FineTuneDataUploader uploader = new FineTuneDataUploader();
        try {
            String response = uploader.uploadFile();
            System.out.println(response);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
```.java
package com.jiransnc.vada;

import com.google.gson.Gson;
import com.google.gson.JsonObject;
import okhttp3.*;

import java.io.IOException;

public class FineTuneRequester {

    private static final String API_KEY = "";
    private static final String BASE_URL = "https://api.openai.com/v1/fine_tuning/jobs";
    private final OkHttpClient client;
    private final Gson gson;

    public FineTuneRequester() {
        this.client = new OkHttpClient();
        this.gson = new Gson();
    }

    public String requestFineTune(String fileId) throws IOException {
        JsonObject json = new JsonObject();
        json.addProperty("training_file", fileId);
        json.addProperty("model", "gpt-3.5-turbo"); // 모델을 명시적으로 지정

        RequestBody body = RequestBody.create(MediaType.parse("application/json"), gson.toJson(json));

        Request request = new Request.Builder()
                .url(BASE_URL)
                .post(body)
                .addHeader("Authorization", "Bearer " + API_KEY)
                .build();

        try (Response response = client.newCall(request).execute()) {
            if (!response.isSuccessful()) throw new IOException("Unexpected code " + response.code() + ": " + response.body().string());

            return response.body().string();
        }
    }

    public static void main(String[] args) {
        FineTuneRequester requester = new FineTuneRequester();
        try {
            // 업로드한 파일의 ID를 여기에 입력합니다.
            String fileId = "file-PC817R6hAslohrqCstCCYyRa";
            String response = requester.requestFineTune(fileId);
            System.out.println(response);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
