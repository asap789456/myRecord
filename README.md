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
##### 에디터를 사용하기 위해 #edit인 textarea 태그를 입력합니다.
```javascript
// 에디터 선언
<body>
	<div class="contain">
		<textarea id="edit">
		</textarea>
	</div>
</body>

```
##### 버튼들을 설정합니다.
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

##### 파일을 업로드할 때  Ajax Url 을 설정합니다.
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
##### 파일 업로드 시 실행되는 url입니다.
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
			throws ServletException, IOException {}
```
##### 업로드를 허용할 확장자를 설정합니다.
```java
String[] allowedExts = new String[] { "txt", "html", "pdf", "doc", ".octet-stream" };
```

##### 업로드 된 파일을 보여주는 url입니다.
```java
@WebServlet("/files/*")
public class FileServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) {}
```
##### 파일을 다운로드 형태로 보기 위해 ContentType을 설정합니다.
```java
response.setContentType("application/download;UTF-8");
```









# Api Store 를 이용한 카카오 알림톡 발송

- 개발환경
  - 언어
    - JavaScript, Java
  - 개발도구
    - Eclipse
  - Tomcat 8.5
  - JDK 1.8



> ## 1. Api Store
##### 알림톡 템플릿을 신규 등록 & 관리할 수 있습니다.
![image](https://user-images.githubusercontent.com/28374739/186803835-f5cc2c0a-4ef7-47cf-b48e-47693757ed0c.png)


##### 템플릿 내용을 설정하고 등록합니다.
![image](https://user-images.githubusercontent.com/28374739/186803935-c32d1712-85ce-4801-beb8-cad97087fd92.png)


> ## 2. 알림톡 발송 처리
##### 알림톡 발신번호 등록 & 인증을 합니다.
```java
// http://localhost:8080/kkotel/발신번호/인증방법/
@RequestMapping(value = "/kkotel/{tel}/{type}/{pincode}", method = RequestMethod.GET)
@ResponseBody
public void tel(@PathVariable("tel") String tel, @PathVariable("type") String type, @PathVariable("pincode") String pincode) {
	String url = "http://api.apistore.co.kr/kko/2/sendnumber/save/" + client_id; // client_id는 Api Store 아이디 입니다.
}
```


##### 알림톡을 발송합니다.
```java
// http://localhost:8080/kkomsg/mycsnm/name/name/odno/csnm/dam_nm/phoneNumber
@RequestMapping(value = "/kkomsg/{mycsnm}/{name}/{msg}/{odno}/{csnm}/{dam_nm}/{phoneNumber}", method = RequestMethod.GET)
@ResponseBody
public void response(@PathVariable("mycsnm") String mycsnm, @PathVariable("name") String name, @PathVariable("msg") String msg
		   , @PathVariable("odno") String odno, @PathVariable("csnm") String csnm, @PathVariable("dam_nm") String dam_nm
		   , @PathVariable("phoneNumber") String phone_number) {
	String url = "http://api.apistore.co.kr/kko/1/msg/" + client_id; // client_id는 Api Store 아이디 입니다.
}
```
##### 알림톡 발송 후 처리코드를 결과로 받습니다.
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
