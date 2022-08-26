# Froala Editor

- 개발환경
  - 언어
    - JavaScript, Java
  - 개발도구
    - Eclipse
  - Tomcat 8.5
  - JDK 1.8



![ezgif com-gif-maker](https://user-images.githubusercontent.com/28374739/186798409-8284cc9b-05db-40a7-909b-4ebb14d47469.gif)

> ## 1. 에디터 화면
#### 에디터를 사용하기 위해 #edit인 textarea 태그를 선언합니다.
```javascript
// 에디터 선언
<body>
	<div class="contain">
		<textarea id="edit">
		</textarea>
	</div>
</body>

```
#### 버튼들을 설정합니다.
```javascript
<script>
  (function () {
    // 에디터 버튼 세팅
    new FroalaEditor('#edit', {
      toolbarButtons:[
        ['undo', 'redo', 'remove'
        ,'bold', 'italic', 'underline', 'strikeThrough', 'subscript', 'superscript'
        ,'fontFamily', 'fontSize', 'textColor', 'backgroundColor', 'inlineClass', 'inlineStyle', 'clearFormatting'
        ,'alignLeft', 'alignCenter', 'alignRight', 'alignJustify', 'formatOLSimple', 'formatOL', 'formatUL', 'paragraphFormat', 'paragraphStyle', 'lineHeight'
        ,'outdent', 'indent', 'quote'],
        ['insertLink', 'insertImage', 'insertTable', 'emoticons', 'fontAwesome', 'specialCharacters', 'embedly', 'insertFile', 'insertHR', 'fullscreen', 'print'
        ,'getPDF', 'spellChecker', 'selectAll', 'help'],
        [html],
        []
        ],
        key: '라이센스 key'
    })
 })()
</script>
```

#### 파일을 업로드할 때  Ajax Url 을 설정합니다.
> 이미지
```javascript
fileUploadURL: '/upload_file',
fileUploadParams: {
	id: 'my_editor',
	변수:'${변수}'
}
```
> 파일
```javascript
fileUploadURL: '/upload_image',
fileUploadParams: {
	id: 'my_editor',
	변수:'${변수}'
}
```



> ## 2. 파일 업로드 처리
>> https://froala.com/wysiwyg-editor/docs/server/java/file-upload/ 참고
#### 파일 업로드 시 실행되는 url입니다.
```java
@WebServlet(name = "FileUploadServlet", urlPatterns = { "/upload_file" })
@MultipartConfig
public class FileUpload extends HttpServlet {

	private static final long serialVersionUID = 1L;

	public FileUpload() {
		super();
	}
	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse
	 *      response)
	 */
	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
			...
			}
```
#### 업로드를 허용할 확장자를 설정합니다.
```java
String[] allowedExts = new String[] { "txt", "html", "pdf", "doc", ".octet-stream" };
```

#### 업로드 된 파일을 보여주는 url입니다.
```java
@WebServlet("/files/*")
public class FileServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) {
	...
	}
```
#### 파일을 다운로드 형태로 보기 위해 ContentType을 설정합니다.
```java
response.setContentType("application/download;UTF-8");
```









# Api Store 를 이용한 카카오 알림톡 발송

- 개발환경
  - 언어
    - Java
  - 개발도구
    - Eclipse
  - Tomcat 8.5
  - JDK 1.8



> ## 1. Api Store
#### 알림톡 템플릿을 신규 등록 & 관리할 수 있습니다.
![image](https://user-images.githubusercontent.com/28374739/186803835-f5cc2c0a-4ef7-47cf-b48e-47693757ed0c.png)


#### 템플릿 내용을 설정하고 등록합니다.
![image](https://user-images.githubusercontent.com/28374739/186803935-c32d1712-85ce-4801-beb8-cad97087fd92.png)


> ## 2. 알림톡 발송 처리
#### 알림톡 발신번호 등록 & 인증을 합니다.
```java
// http://localhost:8080/kkotel/발신번호/인증방법/
@RequestMapping(value = "/kkotel/{tel}/{type}/{pincode}", method = RequestMethod.GET)
@ResponseBody
public void tel(@PathVariable("tel") String tel, @PathVariable("type") String type, @PathVariable("pincode") String pincode) {
	String url = "http://api.apistore.co.kr/kko/2/sendnumber/save/" + client_id; // client_id는 Api Store 아이디 입니다.
	...
}
```


#### 알림톡을 발송합니다.
```java
// http://localhost:8080/kkomsg/mycsnm/name/name/odno/csnm/dam_nm/phoneNumber
@RequestMapping(value = "/kkomsg/{mycsnm}/{name}/{msg}/{odno}/{csnm}/{dam_nm}/{phoneNumber}", method = RequestMethod.GET)
@ResponseBody
public void response(@PathVariable("mycsnm") String mycsnm, @PathVariable("name") String name, @PathVariable("msg") String msg
		   , @PathVariable("odno") String odno, @PathVariable("csnm") String csnm, @PathVariable("dam_nm") String dam_nm
		   , @PathVariable("phoneNumber") String phone_number) {
	String url = "http://api.apistore.co.kr/kko/1/msg/" + client_id; // client_id는 Api Store 아이디 입니다.
	...
}
```
#### 알림톡 발송 후 처리결과에 따른 처리코드가 리턴됩니다.
```java
JSONParser jsonParse = new JSONParser();
JSONObject obj = (JSONObject) jsonParse.parse(response.getBody().toString());
Object result_code = obj.get("result_code");
Object cmid = obj.get("cmid");

String result = "";
if (result_code.equals("100")) {
	result = "User Error";
} else if (result_code.equals("200")) {
	result = "성공";
} else if(result_code.equals("300")) {
	result = "파라메터 에러";
} else if(result_code.equals("400")) {
	result = "알수없는 에러";
} else if(result_code.equals("500")) {
	result = "발신번호 사전 등록제에 의한 미등록 차단(4. 발신번호 인증/등록 기능 필수)";
} else if(result_code.equals("600")) {
	result = "충전요금 부족";
} else if(result_code.equals("700")) {
	result = "템플릿코드 사전 승인제에 의한 미승인 차단";
} else if(result_code.equals("800")) {
	result = "템플릿코드 에러";
} else if(result_code.equals("900")) {
	result = "프로파일이 존재하지 않음";
}
```










# 네이버 금융 환율 데이터 크롤링

- 개발환경
  - 언어
    - Java, Python
  - 개발도구
    - Eclipse, PyCharm
  - Tomcat 8.5
  - JDK 1.8

> ## 1. 자바
#### 매일 8시 50분마다 데이터를 가져올 수 있도록 스케줄링을 설정합니다. 
```java
@Scheduled(cron = "0 50 08 * * *") // 매일 8시 50분
public void startCurrencyRate() throws IOException {
	List<ExRateVO> countries = service2.getCurCdList(); // DB에서 국가 정보를 조회합니다.
	for (ExRateVO coutry : countries) { // 국가별로 데이터를 가져옵니다.
		String cCode = coutry.getCur_cd();
		String cName = coutry.getCur_nm();
		getCurrencyRate(cCode, cName, usdRate);
	}
}
```

#### https://finance.naver.com/marketindex/ url을 사용하여 데이터를 조회합니다.
##### 1. Jsoup 을 사용해서 해당 url 을 연결합니다.
##### 2. html 코드 중에 class 명이 'tbl_exchange today' 인 table 태그를 찾아 조회합니다.
##### 3. table 안에 tr 태그의 개수만큼 for문을 실행합니다.
```java
public String getCurrencyRate(String cc, String cn, String usdRate) { // cc : 국가코드, cn : 국가명, usdRate : 미국환율
	try {
		doc = Jsoup.connect(URL).get();
	} catch (IOException e) {
		e.printStackTrace();
	}
	
	Elements elem = doc.select("table[class=\"tbl_exchange today\"]");
	for (Element e1 : elem.select("tr")) {
	...
	}
}
```

#### 데이터를 오브젝트에 담아서 DB에 데이터를 저장합니다.
```java
service2.insertExRate(exVO2)
```



> ## 2. 파이썬
#### 국가 개수만큼 for문을 실행하여 데이터를 가져옵니다.
```python
for i in range(0, iLen): 
    tempList = getRate(sCode[i][0], sCode[i][1], sUsdRate[1]) # 국가코드, 국가 영문명, 미환율

    if f_isNull(tempList) == 1:
	n = n+1 # 국가개수
	tempList.append(today)# 오늘일자 추가
	savaDataList.append(tuple(tempList))
f_dbConnect(savaDataList, n)
```

#### https://finance.naver.com/marketindex/ url을 사용하여 데이터를 조회합니다.
##### 1. BeautifulSoup 을 사용해서 해당 url 을 연결합니다.
##### 2. html 코드 중에 class 명이 'tbl_exchange' 인 table 태그를 찾아 조회합니다.
##### 3. table 안에 tr 태그의 개수만큼 for문을 실행합니다.
```python
# 환율 가져오기
def getRate(sCode, sName, sRate):
    try:
        webpage = urlopen(url)
        soup = BeautifulSoup(webpage, 'html.parser')  # 해당 url 태그를 가져옴
        table = soup.select_one('.tbl_exchange')
        contents = table.select('tbody > tr')
	
        for content in contents:
		...
```
#### DB 에 데이터를 저장합니다.
```python
# db 연결
def f_dbConnect(sData, i):
    conn = pymssql.connect(**params)  # 서버 연결
    iCursor = conn.cursor()
    isql = "INSERT INTO ..." # DB 저장
    iCursor.execute(isql, today)
    conn.commit()  # insert 실행
    conn.close()  # 서버 종료
```

#### 저장된 데이터를 확인할 수 있습니다.
![image](https://user-images.githubusercontent.com/28374739/186810948-725de47a-c1b1-446e-88ae-e7ebfc510109.png)















# 아임포트를 이용한 결제 시스템

- 개발환경
  - 언어
    - JavaScript, Java
  - 개발도구
    - Eclipse
  - Tomcat 8.5
  - JDK 1.8

![image](https://user-images.githubusercontent.com/28374739/186850532-662f1bc0-1c2c-4709-b81e-df8f85c3bf76.png)


> ## 1. 에디터 화면
#### 결제모듈을 사용하기 위해 IMP를 선언합니다.
##### 1. request_pay 안에 결제정보를 입력합니다.
##### 2. 결제가 성공했을 때 ./payments/complete Ajax 가 실행됩니다.
##### 3. 결제가 실패했을 때 ./payments/fail Ajax 가 실행됩니다.
```javascript
var IMP = window.IMP; // 생략 가능
IMP.init("가맹점식별코드");//아임포트 관리자 콘솔에서 확인한 가맹점 식별코드 입력

IMP.request_pay({
	pg : 'PG사 코드값',	//값 형식: [PG사 코드값] 또는 [PG사 코드값].[PG사 상점아이디]
	pay_method : 'card',
	merchant_uid: 'merchant_' + new Date().getTime(), //가맹점에서 생성/관리하는 고유 주문번호
	name : '상품1',//결제창에서 보여질 이름
	amount : 100,//결제가격(신용카드의 최소 주문금액은 500원, 500원 미만 금액이면 오류로 결제실패)
	buyer_email : 'test@naver.com',
	buyer_name : '사용자1',
	buyer_tel : '010-1234-5678',
	buyer_addr : '서울특별시 강남구 삼성동',
	buyer_postcode : '123-456'
}, function(rsp) {
	if (rsp.success) {
		//결제 성공 시 로직
		$.ajax({
			url:"./payments/complete",
			method: "post",
			data: {
				imp_uid: rsp.imp_uid,
				merchant_uid: rsp.merchant_uid
			},
			dataType: "json"
		}).done(function (data) {
			alert(data.msg);
		});
	} else {
		//결제 실패 시 로직
		$.ajax({
			url:"./payments/fail",
			method: "post",
			data: {
				imp_uid: rsp.imp_uid,
				merchant_uid: rsp.merchant_uid
			},
			dataType: "json"
		}).complete(function () {
			alert("결제에 실패하셨습니다. " + rsp.error_msg);
		});				
	}
});
```

> ## 2. 결제 처리
#### 결제 정보를 조회하기 위해서 access token 을 발급받아야 합니다.
##### 1. https://api.iamport.kr/users/getToken 을 POST 방식으로 접근하여 access_token을 발급받습니다.
```java
@ResponseBody
public String getToken() throws UnsupportedEncodingException {
	String ApiUrl = "https://api.iamport.kr/users/getToken";
	String imp_key = URLEncoder.encode(api_key, "UTF-8");
	String imp_secret = URLEncoder.encode(api_secret, "UTF-8");
	String _token = "";// 토큰저장

	HashMap<Object, Object> hash = new HashMap<Object, Object>();
	hash.put("imp_key", imp_key);
	hash.put("imp_secret", imp_secret);

	JSONObject json = new JSONObject(hash);

	try {
		String requestString = "";
		URL url = new URL(ApiUrl);
		HttpURLConnection connection = (HttpURLConnection) url.openConnection();

		connection.setDoOutput(true);
		connection.setInstanceFollowRedirects(false);
		connection.setRequestMethod("POST");
		connection.setRequestProperty("Content-Type", "application/json");

		OutputStream os = connection.getOutputStream();
		os.write(json.toString().getBytes());
		connection.connect();

		StringBuilder sb = new StringBuilder();

		if (connection.getResponseCode() == HttpURLConnection.HTTP_OK) {
			BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream(), "utf-8"));
			String line = null;

			while ((line = br.readLine()) != null) {
				sb.append(line + "\n");
			}

			br.close();
			requestString = sb.toString();
		}
		os.flush();
		connection.disconnect();

		JSONParser jsonParser = new JSONParser();
		JSONObject jsonObj = (JSONObject) jsonParser.parse(requestString);
		if ((Long) jsonObj.get("code") == 0) {
			JSONObject result = (JSONObject) jsonObj.get("response");
			//System.out.println("토큰생성 ==>> " + result.get("access_token"));
			_token = (String) result.get("access_token");
		}

	} catch (Exception e) {
		e.printStackTrace();
		_token = "";
	}
	return _token;
}
```


