---
title: StudyRoom(SRoom) 프로젝트 - 08(Kakao Rest API를 활용한 계정 연동하기 - 사용자 토큰 받아내기)
date: 2019-04-10 16:52:58
tags: OAuth
---

# SRoom 프로젝트

![images](/images/sroom/studyroom.jpg)<br/>

앞서 받아온 코드를 사용하여 사용자의 토큰을 받아와야 합니다.<br/>
<br/>
## Request 요청하기
![kakao](/images/oauth/tok1.png) 위 주소로 POST요청을 해야합니다.<br/>
![kakao](/images/oauth/tok2.png) 위 파라미터 값들을 요청해야 합니다.<br/>
<br/>
## Response 요청하기
![kakao](/images/oauth/tok3.png) 응답은 JSON 객체로 위 값들을 포함합니다.<br/>
```
http://kauth.kakao.com/oauth/token?grant_type=authorization_code(고정값)&client_id=API 주소&redirect_uri=본인이 만든 URI주소&code=로그인시 발급받았던 코드
```
얻어온 코드까지 포함하여 위 주소로 요청할 수 있습니다.<br/>
<br/>
## Controller 요청하기
이제 컨트롤러에서 요청을 하기 위해 클래스를 세팅합니다.
```
package com.sroom.sroom.oauth;

import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.message.BasicNameValuePair;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.ArrayList;
import java.util.List;

public class KakaoAccessToken {

    public static JsonNode getKakaoAccessToken(String code) {

        final String RequestUrl = "https://kauth.kakao.com/oauth/token"; // Host
        final List<NameValuePair> postParams = new ArrayList<NameValuePair>();

        postParams.add(new BasicNameValuePair("grant_type", "authorization_code"));
        postParams.add(new BasicNameValuePair("client_id", "API KEY")); // REST API KEY
        postParams.add(new BasicNameValuePair("redirect_uri", "http://localhost:9000/kakaoLogin")); // 리다이렉트 URI
        postParams.add(new BasicNameValuePair("code", code)); // 로그인 과정중 얻은 code 값

        final HttpClient client = HttpClientBuilder.create().build();
        final HttpPost post = new HttpPost(RequestUrl);

        JsonNode returnNode = null;

        try {
            post.setEntity(new UrlEncodedFormEntity(postParams));

            final HttpResponse response = client.execute(post);
            final int responseCode = response.getStatusLine().getStatusCode();

            System.out.println("\nSending 'POST' request to URL : " + RequestUrl);
            System.out.println("Post parameters : " + postParams);
            System.out.println("Response Code : " + responseCode);

            // JSON 형태 반환값 처리
            ObjectMapper mapper = new ObjectMapper();

            returnNode = mapper.readTree(response.getEntity().getContent());

        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (ClientProtocolException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
        }

        return returnNode;
    }
}
```
REST API는 자신이 발급받은 API를 입력하면 됩니다.
<br/>
![kakao](/images/oauth/tok4.png) 그리고 import하려면  위 라이브러리를 추가해야합니다.<br/>
![kakao](/images/oauth/tok5.png) 그리고 컨트롤러에서 access_Token을 받아옵니다.
```
@GetMapping("/kakaoLogin")
    public void kakaoLogin(@RequestParam("code") String code, @RequestParam("access_token") JsonNode accessToken,
                           RedirectAttributes re, HttpSession session, HttpServletResponse response) throws IOException {
        // code를 나타낸다.
        System.out.println("kakao code : " + code);

        // JsonNode트리 형태로 토큰을 받아온다
        JsonNode jsonToken = KakaoAccessToken.getKakaoAccessToken(code);
        // 여러 json객체 중 access_token을 가져온다
        accessToken = jsonToken.get("access_token");

        System.out.println("access_token : " + accessToken);
    }
```
코드는 위와 같습니다.<br/>
다음엔 발급받은 access_Token으로 사용자의 정보를 가져와 보겠습니다.