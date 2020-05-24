Basic RCE L01
=============
## Problem
* HDD를 CD-Rom으로 인식시키기 위해서는 GetDriveTypeA의 리턴값이 무엇이 되어야 하는가
## Tool
* ollydbg
## Approach  
![screen][jpg1]
* API인 [GetDriveTypeA function][DriveAPI]에 대한 정보
  * `Return value`
    * `DRIVE_FIXED`: `3` //The drive has fixed media; for example, a hard disk drive or flash drive.
    * `DRIVE_CDROM`: `5` //The drive is a CD-ROM drive.  
* 프로그램 진행 방식
  * 파라미터 `c:\`를 넣어 `GetDriveTypeA` 함수를 호출, return 값은 `EAX`에 저장.
  * `EAX`의 `DEC`를 2번 진행, `ESI`의 `INC`를 3번 진행
  * 둘의 값을 비교해 조건 분기 `JE`를 진행
    * 같으면 `40103D`로 점프해 `Ok, I really think that your HD is a CD-ROM! :p`를 메시지 박스로 출력
    * 다르면 점프를 실행하지 않고 `Nah... This is not a CD-ROM Drive!`를 메시지 박스로 출력  
* 해결
  * `GetDriveTypeA`를 호출하고 나면 `EAX`값과 `ESP`값만 변경
  * 함수를 직접 타고 가지 않고 해당 값만 적절히 교체
  * `401055`영역의 `JMP DWORD PTR DS:[<&KERNEL32.GetDriveTypeA>]` -> `MOV AL,5` `RETN 4`  

하지만 FAIL  
이후 다른 사람들의 write-up을 찾아봤더니 이전 버전에선 `ESI`가 exe 시작 시 0의 값을 띈다.  
현재는 `EntryPoint`값이 ESI에 저장(00401000)  

* 다른 방법
  * 조건분기 `JE`를 `JMP`로 바꿔 앞에 조건식을 따지지 않고 `40103D`영역으로 점프
  * `CMP EAX,ESI`를 `CMP EAX,EAX`와 같은 참 값을 띄게 변경
  
[DriveAPI]: https://docs.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-getdrivetypea
[jpg1]: https://github.com/presentnine/Wargame/blob/master/CodeEngn/Basic%20RCE/Level%2001/Basic%20RCE%20L01.png
