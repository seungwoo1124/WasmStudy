# 시작하는 법

# [[emscripten]] 설치
- 
```bash
git clone https://github.com/emscripten-core/emscripten.git ./emscripten
cd ./emscripten
./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh
```
- 마지막 줄인 source ./emsdk_env.sh는 터미널 실행시마다 실행시켜줘야 인식된다.

# 코드 작성
- *hello.cpp*
- 
```cpp
#include <iostream>
using namespace std;

int main()
{
	cout << "Hello World ~~" << endl;
}
```

# 컴파일
- 
```bash
emcc hello.cpp -o hello.html
```
- 결과물로 hello.html, hello.js가 나오게 됩니다.

# 서버 작동
- 
```bash
python3 -m http.server 8080
```
- 이후 브라우저에 localhost:8080을 입력하여 접속 후 hello.html을 접속한다.
- ![[스크린샷 2023-10-10 13-43-20.png]]




# JavaScript에서 C언어 함수 호출

- *call1.cpp*
- 
```cpp
#include <emscripten/bind.h>

float my_analysis(float a, float b, float c)
{
	return a + b * c;
}

EMSCRIPTEN_BINDINGS(my_module)
{
	emscripten::function("calc1", &my_analysis);
}
```
- EMSCRIPTEN_BINDINGS(my_module)에서 my_analysis 함수를 calc1이라는 이름으로 매핑합니다.
- 이제 JavaScript에서 calc1이라는 이름으로 my_analysis를 호출 할 수 있습니다.


- *call1.html*
- 
```html
<!DOCTYPE html>
<html>
<meta charset="UTF-8">
<head>
	<script>
		var Module = {
			onRuntimeInitialized: function() {
			reqForm.arg1.value = 1.5;
			reqForm.arg2.value = 3.5;
			reqForm.arg3.value = 0.5;
			}
		};

		function getResult() {
			let arg1 = parseFloat(reqForm.arg1.value);
			let arg2 = parseFloat(reqForm.arg2.value);
			let arg3 = parseFloat(reqForm.arg3.value);
			
			let myRet = Module.calc1(arg1, arg2, arg3);
			reqForm.result.value = myRet;
		}
	</script>
	<script src = "call1.js"></script>
	<body>
		<form method id="reqForm" onsubmit="return false;">
			<input type="text" name="arg1" value="" size="3"> +
			<input type="text" name="arg2" value="" size="3"> *
			<input type="text" name="arg3" value="" size="3"> =
			<input type="text" name="result" value="" size="3">
			<input type="button" onclick="getResult()" value="결과">
		</form>
	</body>
</head>
</html>
```

### workflow
1. call.html이 호출 될 때 onRuntimeInitiailized가 실행됩니다.
2. 사용자가 button을 클릭했을 때 js의 getResult()가 실행됩니다.
3. getResult()의 Module.calc1(arg1, arg2, arg3)로 call1.cpp의 my_analysis()를 실행합니다.


- 
```bash
emcc -lembind -o call1.js call1.cpp
```
- ![[스크린샷 2023-10-10 14-07-41.png]]

# C에서 JavaScript 함수 호출하기
- *callAtC.cpp*
- 
```cpp
#include <iostream>
#include <string>
#include <emscripten.h>
#include <emscripten/bind.h>

EM_JS(void, call_js, (const char* subject, int len_subject, const char* msg, int len_msg), {
	jsFunction(UTF8ToString(subject, len_subject),
			   UTF8ToString(msg, len_msg));
});

bool my_callJs()
{
	const std::string subject = "JESUS LOVES YOU";
	const std::string msg     = "예수님은 당신을 사랑합니다.";

	call_js(subject.c_str(), subject.length(), msg.c_str(), msg.length());

	return true;
}

EMSCRIPTEN_BINDINGS(module)
{
	emscripten::function("callJs", &my_callJs);
}
```

- *callAtC.html*
- 
```html
<!DOCTYPE html>
<html>
<meta charset="UTF-8">
<head>
	<script>
		var Module = {
			onRuntimeInitialized: function() {
				Module.callJs();
			}
		}

		function jsFunction(subject, msg) {
			document.getElementById('my_div').innerText = subject;
			document.getElementById('my_title').innerText = msg;
		}
	</script>
	<script src="callAtC.js"></script>
</head>
<body>
	[ 제 목 ]
	<div id="my_div"></div>
	[ 내 용 ]
	<div id="my_title"></div>
</body>
</html>
```


### workflow
1. callAtC.html의 onRuntimeInitialized에서 callAtC.cpp의 my_callJs 실행(callJs라는 이름으로 매핑되어있음)
2. callAtC.cpp의 my_callJs에서 call_js(EM_JS에서 정의)를 실행
3. callAtC.cpp에서 UTF-8형태로 변환하여 js의 jsFunction 호출

- 
```bash
emcc -lembind -o callAtC.js callAtC.cpp
```

- ![[스크린샷 2023-10-10 14-09-09.png]]
