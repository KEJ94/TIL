## Java ChatGPT Open AI 연동 예제

__build.gradle__
```.gradle
implementation 'com.squareup.okhttp3:okhttp:4.9.3'
implementation 'com.google.code.gson:gson:2.8.8'
```
__ChatGPTClient.java__
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
