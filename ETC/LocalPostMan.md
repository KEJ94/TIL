```.java
public static void main(String[] args){
	try {
		// SSL 인증서 무시
		TrustManager[] trustAllCerts = new TrustManager[] {
			new X509TrustManager() {
				public java.security.cert.X509Certificate[] getAcceptedIssuers() {
					return null;
				}
				public void checkClientTrusted(X509Certificate[] certs, String authType) {
				}
				public void checkServerTrusted(X509Certificate[] certs, String authType) {
				}
			}
		};
		
		// SSL 컨텍스트 설정
		SSLContext sc = SSLContext.getInstance("SSL");
		sc.init(null, trustAllCerts, new SecureRandom());
		
		// HTTPS 연결 설정
		HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());

		// 요청할 URL 설정
		String url = "https://192.168.3.30/api/get_asset";

		// URL 객체 생성
		URL obj = new URL(url);

		// HttpURLConnection 객체 생성
		HttpURLConnection con = (HttpURLConnection) obj.openConnection();

		// HTTP 메소드 설정 (GET 방식)
		con.setRequestMethod("POST");

		// 응답 코드 확인 (200이면 성공)
		int responseCode = con.getResponseCode();

		if (responseCode == HttpURLConnection.HTTP_OK) {
			// 응답 내용 읽기
			BufferedReader in = new BufferedReader(new InputStreamReader(con.getInputStream()));
			String inputLine;
			StringBuilder response = new StringBuilder();

			while ((inputLine = in.readLine()) != null) {
				response.append(inputLine);
			}
			in.close();

			// 응답 내용 출력
			System.out.println(response.toString());
		} else {
			System.out.println("HTTP 요청 실패: " + responseCode);
		}
	} catch (Exception e) {
		e.printStackTrace();
	}
}
}
```
