Basic RCE L02
==============
## Problem
* 패스워드로 인증하는 실행파일이 손상되어 실행이 안되는 문제가 생겼다. 패스워드가 무엇인지 분석하시오  
## Tool
* HXD
* pestudio
## Approach  
![non-execute][jpg1]
* 애초에 실행부터 불가 -> `정적 파일 분석`으로 방향 변경  

![HXD_screen][jpg2]
* 헥스 에디터로 파일을 열어 보았을 때 `DOS Header`의 시그니처 `MZ`만 있고 `NT Header`의 시그니처 `PE`는 보이지 않는다.   
파일 손상인 것  

![pestudio][jpg3]
* pestudio로 열어봐도 마찬가지  
  
* 해결
![string][jpg4]
  * 파일의 ASICII 값으로 변환하여 문자쪽들을 보다보면 문자열들이 모여있는 곳을 확인할 수 있다.
  * 해당 문자열들 중에서 `패스워드`를 추측 가능하다.
  * 추측값: `JK3FJZh`
  
  [jpg1]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2002/Basic%20RCE%20L02%201.png
  [jpg2]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2002/Basic%20RCE%20L02%202.png
  [jpg3]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2002/Basic%20RCE%20L02%203.png
  [jpg4]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2002/Basic%20RCE%20L02%204.png
