# 백기선과 함께하는 라이브 스터디
1주차 과제


`목표`
+ JVM이란 무엇인가
+ 컴파일 하는 방법
+ 실행하는 방법
+ 바이트코드란 무엇인가
+ JIT 컴파일러란 무엇이며 어떻게 동작하는지
+ JVM 구성 요서
+ JDE와 JRE의 차이
  

# JVM이란 무엇인가
JVM(자바 가상 머신)은 자바 응용프로그램이 운영체제에 독립적으로 실행할 수 있게 한다.
![image](https://user-images.githubusercontent.com/24693833/126054735-dd6a017c-3438-4b8d-b133-fa70eca9cc73.png)


자바의 특징 중 하나인 `운영체제에 독립성`은 이 JVM을 통해 이루어진다. 자바 응용프로그램이 OS, 하드웨어가 아니라 JVM으로만 통신하고 JVM은 자바 응용프로그램으로부터 전달받은 명령을 해당 운영체제가 이해할 수 있도록 변환하여 전달한다. 그러므로 자바로 작성된 프로그램은 운영체제에 독립적이지만 JVM은 운영체제에 종속적이다. [JVM 구성](#jvm의-구성-요소)

# 컴파일 하는 방법
자바 컴파일러(avac.exe)를 이용해 .java 파일을 .class(바이트코드)로 변환
```
javac Hello.java
```
생성된 .class(바이트코드)를 인터프리터로 실행
```
java Hello
```
**자바 컴파일 과정**
1. 프로그램이 실행되면 JVM은 OS로부터 프로그램이 필요로 하는 메모리를 할당받는다. 이 메모리는 JVM 내부에서 용도에 따라 여러 영역으로 나누어 관리한다.
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 JVM이 이해할 수 있는 자바 바이트코드(.class)로 변환한다.
3. Class Loader를 통해 class 파일을 JVM으로 로딩한다.
4. 로딩된 class 파일은 Interpreter나 JIT를 통해 OS 기계어로 해석된다.
5. 해석된 바이트코드는 메모리 상(Runtime Data Areas)에 배치되어 Execution Engine에 의해 실행된다(실질적 수행). 

> javac.exe : 자바 컴파일러, 자바소스코드를 바이트코드로 컴파일한다.  
> java.exe: 자바 인터프리터, 컴파일러가 생성한 바이트코드를 해석하고 실행한다.  
> javap.exe: 역어셈블리어, 컴파일된 클래스파일을 원래의 소스로 변환하다.
> javadoc.exe: 자동문서생성기, 소스파일에 있는 주석을(/** */) JAVA API문서처럼 자동 생성한다.  
> jar.exe: 클래스파일과 실행에 관련된 파일을 jar파일(.jar)로 압축하거나 압축해제한다.

![image](https://user-images.githubusercontent.com/24693833/126054722-68cc8047-153f-4953-8832-c0de807301e0.png)

.class파일(바이트코드)는 JVM에 로드되고 실행엔진에 의해 OS가 해석할 수 있는 기계어로 해석되는데 이 때 두가지 방법이 있다. 
+ interpreter
자바 바이트코드를 한줄씩 실행, 여러번 실행하는 환경에선 다소 느림
+ JIT Compiler
인터프리터의 단점을 보완해, 전체 바이트 코드를 컴파일하고 캐시를 사용해서 한번 컴파일하면 다음에는 빠르게 수행된다.

# 실행하는 방법


# 바이트코드란 무엇인가
자바 바이트코드(Java bytecode)란 JVM이 이해할 수 있는 언어로 변환된 자바 소스 코드(.class)를 의미한다. 
C에서는 바이트코드와 비슷하게 어셈블리어가 있다. 

# JIT 컴파일러란 무엇이며 어떻게 동작하는지
ref) https://catch-me-java.tistory.com/11?category=438116  

컴퓨터는 태생 부터 CPU가 직접 해독하고 실행할 수 있게 비트 단위(0,1 이진수)로 쓰인 기계어만을 해석할 수 있다. 따라서 소스코드를 기계어(네이티브어)로 번역하는 방식에 따라 `컴파일러 방식`과 `인터프리터 방식`으로 구분한다.
+ 컴파일러 방식: 프로그램 전체를 한번에 기계어로 번역한다. 대신 기계에 종속적인 실행코드가 생성되며 실행 기계가 달라지면 새로이 컴파일해야 한다. 
+ 인터프리터 방식: 런타임 시 명령어를 한줄씩 해석하여 기계어로 번역한다. 느리다.


자바는 컴파일 방식과 인터프리터 방식 둘 다 사용한다. 바이트코드를 인터프리터로 한줄 한줄 읽어들여야 해 느리다는 단점을 보완하기 위해 JIT가 도입되었다. JIT는 한번 읽어서 기계어로 변경한 소스코드는 캐싱해뒀다가 다시 사용해서 빠르다. 다만 JIT 컴파일러가 컴파일하는 과정은 일반 인터프리터가 바이트코드를 해석하는 것보다 훨씬 오래 걸리므로 한번만 실행하는 경우엔 일반 인터프리터를 사용하는 것이 훨씬 빠르다. 따라서 JIT 컴파일러를 사용하는 JVM은 내부적으로 해당 메서드가 얼마나 자주 수행되는지 체크하고, 일정 정도를 넘을 때만 JIT컴파일을 수행한다.
![image](https://user-images.githubusercontent.com/24693833/126055198-6196fbdd-512a-49d0-a16b-bed31463f586.png)

![image](https://user-images.githubusercontent.com/24693833/126055205-a3680a0c-fb8e-49b3-ae9b-9f4fa33b6903.png)

# JVM의 구성 요소
JVM은 Class Loader, Execution Engine, Interpreter, JIT, Garbage Collector로 구성된다.
![image](https://user-images.githubusercontent.com/24693833/126055392-bb364e17-903d-4258-9dbb-eaf8a65723b2.png)


### Class Loader
JVM 내로 클래스(.class파일)을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈. 실행시애 모든 클래스가 로딩되지 않고 필요한 시점에 클래스를 동적으로 로딩한다. jar 파일 내 저장된 클래스들을 JVM 위에 탑재하고 사용하지 않는 클래스들은 메모리에서 삭제한다. 자바는 동적 코드, 컴파일 타임이 아니라 런타임에 참조한다. 즉, 클래스를 처음으로 참조할 때 해당 클래스를 로드하고 링크하게 되는데 이를 클래스 로드가 한다.

### Interpreter(인터프리터)
자바 컴파일러에 의해 변환된 자바 바이트 코드를 읽고 해석하는 역할을 하는 것인 인터프리터이다.

### Execution Engine(실행 엔진)
로드된 class의 바이트코드를 실행하는 역할이다. class laoder를 통해 jvm 내의 runtime data areas에 배치된 바이트 코드는 execution engine에 의해 실행된다.


### JIT(Just In Time)
인터프리터 방식을 보완하기 위해 도입된 것이 JIT이다. 

### Garbage Collector
ref) https://yaboong.github.io/java/2018/06/09/java-garbage-collection/  
Garbage Collector(GC)는 Heap 메모리 영역에 생성된 객체들 중에 참조되지 않는 객체들을 제거한다. 실제론 garbage를 수집해 제거하는 것이 아니라 garbage가 아닌 것들을 따로 마크하고 그 외의 것들은 모두 지운다. `System.gc()`를 호출하면 명시적으로 가비지 컬렉션을 일으킬 수 있지만 모든 Thread가 중단되기 때문에 코드상에서 호출하는 짓은 하면 안된다.


# JDK와 JRE의 차이
### JRE(Java Runtime Environment)
자바 어플리케이션의 실행환경이다. JRE는 JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리 파일들과 기타 파일들을 가지고 있다. JRE는 JVM의 실행환경을 구현했다고 볼 수 있다.  
![image](https://user-images.githubusercontent.com/24693833/126055471-e1f04ee1-6cb2-4a01-ac90-b55776ff27c6.png)



### JDK(Java Development Kit)
Java Development Kit로 JVM과 자바클래스 라이브러리(Java API)외에 자바를 개발하는데 필요한 프로그램들이 설치된다. JDK는 JRE + 개발을 위해 필요한 도구(javac, java 등)을 포함한다.  
![image](https://user-images.githubusercontent.com/24693833/126055475-73d3c8e3-cb3a-48a3-996f-f32478728fdb.png)
