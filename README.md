# Froala Editor

- 개발환경
  - 언어
    - JavaScript, Java
  - 개발도구
    - Eclipse
  - Tomcat 8.5
  - JDK 1.8



![ezgif com-gif-maker](https://user-images.githubusercontent.com/28374739/186798409-8284cc9b-05db-40a7-909b-4ebb14d47469.gif)

> ## 에디터 화면
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



> ## 파일 업로드 처리
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
