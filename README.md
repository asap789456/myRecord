# Froala Editor 개발과정


- 개발환경
  - 언어
    - JavaScript, Java
  - 개발도구
    - Eclipse
  - Tomcat 8.5  



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

##### 파일을 첨부할 때  Ajax Url 을 설정합니다.
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
##### 에디터를 사용하기 위해 #edit인 textarea 태그를 입력합니다.
```java
// 에디터 선언
@WebServlet(name = "FileUploadServlet", urlPatterns = { "/upload_file" })
```
