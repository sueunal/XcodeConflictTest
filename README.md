<img width="609" alt="스크린샷 2024-08-03 오후 9 37 13" src="https://github.com/user-attachments/assets/952d5a03-71b1-4342-a76e-09b65ce95ca8"># Xcode Conflict Test

# git 사용시 Xcode 충돌나는 상황 정리

## 1. 동시 수정
- 한 파일에 같은 부분을 동시에 수정하고 merge 했을때 발생   

ex) main branch 수정, test0 branch 수정
   
  <img width="342" alt="스크린샷 2024-08-03 오후 4 35 57" src="https://github.com/user-attachments/assets/ff678340-1622-4673-a27f-21517ae91407">

> 이렇게될 경우 git에서는 어떤 branch의 파일이 최신버전인지 알 수 없기때문에 충돌이 발생하고 개발자가 어떤코드를 반영할 것 인지 직접수정 해야한다.

```
<<<< test0 branch에서 수정한 부분
     수정된 내용
==== 여기까지 test0 수정
     수정된 내용
>>>> main 여기까지 메인이 작업한 부분
```
#### 해결 방법
```
>>> test0
Text("테스트 브랜치에서 수정함") <<< 반영할 코드
===
Text("메인에서 수정함")
<<< main
```
- 적용할 코드를 확인하고 >>>>, ====, <<<< 부분을 제거
```
Text("테스트 브랜치에서 수정함") 
```
- 이렇게 확인하고 수동으로 수정하자!
- 팀원들과 규칙을 정해서 git을 사용하도록하자!

## 2. 파일 이동
- Xcode내부에서 파일이 이동되었다면 project.pbxproj 파일이 수정된다.
파일의 위치도 기록하고 있기 때문에 이동되면 위치가 변경되었다고 수정하기 때문

##### 사례
- main branch에서 test0과 test1을 생성
- test0에서 파일 위치를 이동시키고 main에 merge
<img width="254" alt="스크린샷 2024-08-03 오후 9 39 26" src="https://github.com/user-attachments/assets/3f1dc130-c8cc-4859-8532-968f938e55df">

- test1에서 파일을 수정하고 PR를 요청하면 충돌 발생
<img width="609" alt="스크린샷 2024-08-03 오후 9 37 13" src="https://github.com/user-attachments/assets/dcfc8e3d-26bd-4d1b-ae01-267b7b0fb366">
- test1에서는 수정이 없는데 main에서는 수정됨

#### 이유
test0 branch 에서 파일의 위치를 이동하고 merge했기 때문에   
main branch에서 project.pbxproj 파일의 수정이 발생!

하지만 test1에서는 파일의 위치를 변경하지 않았고 작업한 내용을 PR
기존 메인 브랜치에서 가져온 내용은 현재 merge된 main과는 다른 버전이 되어 충돌 발생

#### 해결 방법
1. 파일 자체는 수정이 되지 않고 경로상에서 이동이 되었기 때문에 수정된 파일의 변경 사항을 적용
2. 충돌난 파일을 기존 있었던 위치로 이동하거나 test1 branch에서 수정된 위치도 파일 이동

