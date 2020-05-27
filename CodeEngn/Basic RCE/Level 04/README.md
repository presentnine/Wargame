# Basic RCE L04
## Problem
 * 이 프로그램은 디버거 프로그램을 탐지하는 기능을 갖고 있다. 디버거를 탐지하는 함수의 이름은 무엇인가
## Tool
 * ollydbg
 * pestudio
## Approach
먼저 해당 exe 파일을 직접 실행했을 때와, ollydbg를 통해 실행했을 때 창에서 뜨는 변화는 다음과 같다.  
1)직접 실행  
![screen][jpg1]  
2)ollydbg를 통해 실행  
![screen][jpg2]  
  
pestudio를 통해 해당 파일을 분석하니, 파일 이름 중에 `IsDebuggerPresent`함수를 발견. 해당 function을 검색해 보았다.  
![screen][jpg3]  
* API인 [IsDebuggerPresent] function[API]에 대한 정보
  * `Return value`
    * If the current process is running in the context of a debugger, the return value is `nonzero`.
    * If the current process is not running in the context of a debugger, the return value is `zero`.
* 프로그램 진행 방식  
![screen][jpg4]  
  * `IsDebuggerPresent`함수를 호출, return 값은 `EAX`에 저장.
  * `Test Eax,Eax`를 통해 값이 0인지 아닌지를 비교해 분기
    * 0인 경우 `정상`이 출력
    * 0이 아닌 경우 `디버깅 당함`이 출력
  * 무한 루프로 반복
* 해결
  * `IsDebuggerPresent` 함수를 호출하는 부분을 EAX 값 대입으로 변경
  * `CALL DWORD PTR DS:[<&KERNEL32.IsDebuggerPresent>]` -> `MOV Eax,0`  
  ![screen][jpg5]  
  ![screen][jpg6]
  
  [API]: https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-isdebuggerpresent
  [jpg1]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2004/Basic%20RCE%20L04%201.png
  [jpg2]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2004/Basic%20RCE%20L04%202.png
  [jpg3]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2004/Basic%20RCE%20L04%203.png
  [jpg4]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2004/Basic%20RCE%20L04%204.png
  [jpg5]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2004/Basic%20RCE%20L04%205.png
  [jpg6]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2004/Basic%20RCE%20L04%206.png
