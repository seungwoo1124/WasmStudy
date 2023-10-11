LLVM (Low Level Virtual Machine)은 컴파일러의 기반 구조입니다. 프로그램을 컴파일 타임, 링크 타입, 런타임 상황에서 프로그램의 작성 언어에 상관없이 최적화를 쉽게 구현할 수 있도록 구성되어 있습니다.

LLVM은 원래는 저급 가상 기계(low-level virtual machine)의 약자를 가리켰지만, LLVM이 성장하고 다양한 목적을 가지게 되면서 현재는 그 이름을 약자로서 사용하는 것이 아니라 그냥 *프로젝트의 이름*으로서 사용하고 있습니다.

LLVM으로 언어에 가상 기계를 생성, 가상 기계가 언어에 독립적인 최적화를 실행합니다. LLVM은 언어와 구조로부터 독립적이며, 언어 모듈과 시스템을 위한 코드 생성부의 사이에 위치합니다. LLVM은 컴파일 과정 동안 최적화와 함께 [[JIT]]를 정적 컴파일러로 사용, 개발의 각종 단계에서 사용할 수 있는 많은 부분을 가지고 있습니다. LLVM은 전통적인 GCC 시스템에서 그랬듯이 코드를 정적으로 컴파일 할 수도 있고, 자바처럼 JIT를 이용하여 기계어(machine code)로 한번 더 컴파일되는 중간 형식으로 코드를 컴파일 할 수도 있습니다. 

<출처 : https://ko.wikipedia.org/wiki/LLVM>

LLVM 프로젝트는 moduler / 재사용 가능한 컴파일러 (reusable) / 툴체인 기술의 집합체 입니다.
주요 역할은 코드를 intermediate representation(IR)로 변환하는 역할을 합니다.
LLVM의 부분으로 **프론트엔드,  백엔드, 미들엔드**부분이 있습니다.

### 프론트엔드
- C, C++, ObjectC, Swift, Python, Ruby와 같은 고급 언어를 읽고 파싱합니다.
- 파싱이 되면 이 언어들이 *IR (Intermediate Representation)* 이 됩니다.

### 백 엔드
- 백엔드에서는 이 IR을 가지고 최적화를 하고 최종적으로 타겟 머신에 맞는 기계어로 만듭니다.
- ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZbUmK%2FbtqO6f8YlZo%2FkWMuzfKIJeXKLnwNOok4k1%2Fimg.jpg)
- IR 은 어셈블리어와 비슷한 저급 프로그래밍 언어입니다.