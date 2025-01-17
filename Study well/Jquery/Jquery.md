### config

```

1. 라이브러리 다운 방식
	 jquery.com 다운 

2. CDN 방식
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.4/jquery.min.js"></script>


3. 적용
	 jquery를 사용하려는 모든 jsp 페이지마다 CDN 방식으로 적용

```


### 문법

* 간단 이해
* 제이쿼리는 오로지 함수로 되어있다고 보면 이해하기 쉬움
* 선택자도 (), 함수명 이후에도 ().

```

$("선택자").jquey함수명()

- 선택자
	document.querySelectorAll("css 선택자")
	jquery의 선택자는css 선택자와 동일 
  문자열 형식
	
- $("#a").action()
- const v = "#a";
	$(v).action();
  $("#abc"+1).action()

```


### 이벤트

- Event 이름은 JS와 동일

js -> const a = document.getElementById("a")
	    a.addEventListener("click", callback function)


jquery
	1. $("선택자").이벤트명(callback function)
			$("#a").click(callback function)
	2. $("선택자").on("이벤트명", callback function)

	3. 하나의 선택자에 여러개의 이벤트 등록
		 $("선택자").on({
				이벤트명1:callback function,
				이벤트명2:callback function,
				...
				이벤트명2:callback function
		})	

	4. 이벤트 위임(이벤트 전달)
		$("부모선택자").on("이벤트명", "자식선택자", callback function)



### get

```

Element
<tagname attribute1="" attribute2="">contents</tagname>

1. Text Node 반환
		$("선택자").text();
	 js - innerText

2. HTML Node 반환
	 $("선택자").html();
	 js - innerHTML	

3. Attribute 반환
	 js - 선택자.속성명, 선택자.getAttribute()

	 $("선택자").attr("속성명")
	 - id, title, data-*** 등 상태가 변해도 값이 변하지 않는 속성

	 $("선택자").prop("속성명")
	 - selected, checked 등 상태가 변할 때 값도 변하는 속성

4. input 태그의 value 반환
	 js - 선택자.value
	 $("선택자").val()
	 

```

### set

```

Element
<tagname attribute1="" attribute2="">contents</tagname>

1. Text node
	 $("선택자").text("값")

2. HTML node
	 $("선택자").html("html code")

3. Attribute
	 $("선택자").attr("속성명", "값")
	 $("선택자").prop("속성명", "값")

4. input 태그의 value
	 $("선택자").val("값")	

```

### 반복문

```

1. for(let i=0;i<..;i++){}
2. for in
3. for of
   foreach(function(v, i, items))

4. jquery 반복문
	 - each()
	 $("선택자").each(function(index, item){
				-- index = index번호
				-- item  = 배열에서 꺼내온 Element
		})

	let ar = [1,2,3]
	let ar2 = [3,4,5]
	$.each(배열명, (function(index, item){
				-- index = index번호
				-- item  = 배열에서 꺼내온 Element
	})

	- break, countiue가 없음
	  break    -> return false;
    countinue -> return true;


```

### 탐색(Traversing) 선택자

```

1. Ancestros(부모)
	- 자기를 기준으로 부모 또는 그 이상
	1) parent()     : 자기 기준으로 바로 윗 부모
	2) parents()    : 자기 기준으로 부모들
	3) parents("선택자") : 부모들 중에서 해당 선택자가 있는 부모만 선택
	4) parentsUtil("선택자") : 부모들 중에서 해당 선택자까지 모든 부모들

2. Decendants(자식)
	- 자기를 기준으로 자식들 
	1) children()    : 자기 기준으로 직계 자손
	2) children("선택자") : 자기 기준으로 직계 자손중 선택자만 선택
	3) find("선택자")     : 자기 기준으로 후손 들 중 선택자만 선택

3. Sibling(형제)
	https://www.w3schools.com/jquery/jquery_traversing_siblings.asp
    1)siblings()
    2)next()
    3)nextAll()
    4)nextUntil()
    5)prev()
    6)prevAll()
    7)prevUntil()

```


### ajax

```

- Jquery 문법을 사용한 Ajax
- 기본원리는 JS의 Ajax와 같음
- Jquery ajax는 함수가 많음


1. Get 요청
$.get("URL?param1=값&param2=값2", callback function)
$.get("URL?param1=값&param2=값2", function(응답 Data를 받는 변수명){})
	- 변수명은 개발자 마음대로 선언

2. Post 요청
$.post("URL", {param1:값1, param2:값2,...}, callback function)

3. 통합 요청 -->제일 많이 씀
let ar = [2, 4, 5]
$.ajax({
	type:"메서드 형식", //"GET", "POST"
	url :"URL",
	traditional:true, //배열을 전송할 때 사용, true
	data:{
		ar: ar,
		param1:값1,
		param2:값2,
		...
		paramN:값n       // 마지막일 경우 , 제거 
	},
	success : callback function,
	error   : callback function
})

```


### formData 객체로 이미지 전송하기

* 아래는 섬머노트API로 진행했음

```

$('#contents').summernote({
		height:400,
		callbacks:{
			onImageUpload:function(files){
				alert('이미지 업로드');
			//	$('#summernote').summernote('insertNode', imageNode);
				// 이미지를 서버로 전송하고
				// 응답으로 이미지 경로와 파일명을 받아서
				// img태그를 만들어서 src속성에 이미지 경로를 넣는 것


				let formData = new FormData(); 
				// <form></form>이 만들어진거임
				// 자바스크립트로 객체만들기

				formData.append("files",files[0]); 
				// <input type="file" name="files">추가하는 거
 
				$.ajax({
					type: 'post',
					 url: 'setContentsImg',
					 data: formData, //객체 자체만 넣으면되서 중괄호 없어도됨
					 enctype: 'multipart/form-data',
					 cache: false,
					 contentType: false,
					 processData: false,
					 success:function(result){
						$('#contents').summernote('insertImage', result.trim());

					 },
					 error:function(){
						console.log('error');

					 }
				});
				// 이걸로 끝이 아님
				// 업로드한 이미지는 여러 상황(내용 수정, 글 작성하다 이동 등등)
			    // 에 따라 어떻게 지울 것인가? 결정해야 됨
                 

			}
		}
	});

```

2. 휴지통 버튼 눌렀을 때 폴더에서도 지워지게 하기.

 - 1단계 src값을 통째로 보낸다
 - 2단계 서비스에서 잘라서 처리
 - 3단계 후처리

```

		onMediaDelete:function(files){
				let path = $(files[0]).attr("src") // /resources/upload/notice/파일명

				$.ajax({
					type:'post',
					 url:'./setContentsImgDelete', 
					 data:{
						path:path
					 },
					success:function(result){ㅐ
						console.log(result);
					},
					error:function(){
						console.log("에러 발생");
					}
				})

			}


```

