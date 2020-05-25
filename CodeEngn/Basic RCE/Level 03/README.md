# Basic RCE L03
## Problem
* 비주얼베이직에서 스트링 비교함수 이름은?
## Tool
* ollydbg
## Approach
![screen][jpg1]  
* 프로그램 진행 방식
  * 프로그램을 열면 먼저 하나의 메시지 박스가 나오고, 그다음 코드를 적는 창이 나타난다.
  * 문자열을 입력받아 미리 지정된 문자열과 비교하여, 해당 두 문자열을 비교하는 함수 `vbaStrCmp`를 호출
    + 틀리면 `Error ! Das Passwort ist falsch !`가 담긴 메시지 박스가 나오고
    + 맞으면 `Danke, das Passwort ist richtig !`가 담긴 메시지 박스가 나온다
* 해결
  * ollydbg의 기능 중 사용된 문자열들을 한 번에 보여주는 `All referenced Strings` 을 사용해, 맞을 때 나오는 메시지 박스의 글을 따라간다.  
  ![screen][jpg2]  
  * 해당 문자열에서 조그만 위로 올라가면 `2G83G35Hs2`의 문자열과 다른 인자를 매개변수로 하는 `vbaStrCmp`함수를 볼 수 있다.
  * 해당 함수에 `Break Point`를 걸고 프로그램을 실행하면 결과에 따른 메시지 박스가 튀어나오지 않는다. (입력 문자열 "1234")
  * 스택에도 내가 적은 문자열과 특정 문자열이 적혀있는 것을 확인가능  
  ![screen][jpg3]  
  * 정답인 문자열은 `2G83G35Hs2`  
  ![screen][jpg4]
  
  [jpg1]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2003/Basic%20RCE%20L03%201.png
  [jpg2]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2003/Basic%20RCE%20L03%202.png
  [jpg3]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2003/Basic%20RCE%20L03%203.png
  [jpg4]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2003/Basic%20RCE%20L03%204.png
