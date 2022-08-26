# Froala Editor 개발과정


- 목록1
  - 목록 1-1
    - 목록 1-1-1


![image](https://user-images.githubusercontent.com/28374739/186792448-c1c177ae-9223-4649-b388-00371cbcc906.png)

> This is a first blockqute.
>	> This is a second blockqute.
>	>	> This is a third blockqute.

### 에디터를 사용하기 위해 id가 edit인 textarea 태그를 입력합니다.
```javascript
// 에디터 선언
<body>
	<div class="contain">
		<textarea id="edit">
		</textarea>
	</div>
</body>

```
# 사용할 버튼들을 설정합니다.
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

