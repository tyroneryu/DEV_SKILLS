# Git 원격 저장소

- github : git을 이용한 원격 저장소를 제공하는 서비스

## 원격 저장소에 push 하기!
1. 원격 저장소를 생성한다.
2. 생성한 저장소를 로컬에 등록한다.
    - `git remote add 별칭 원격저장소주소`
    - `git remote rm 원격_저장소_이름` : 현재 로컬 저장소에 등록된 원격 저장소를 삭제한다.
3. `git push 별칭 브랜치명` : 원격 저장소에 버전 정보를 push(업로드)한다.

- 로컬 저장소는 원격 저장소와 자동 동기화되지 않는다.(어림도 없는 소리)
    - 항상 pull하는 것을 생활화하자.
    - 추천) pull - 작업 - add -commit -push

- pull vs clone
    - pull : git 저장소가 로컬에 이미 있을 때
    - clone : git 저장소가 로컬에 없을 때
        -clone을 하면 원격 저장소가 그대로 로컬에 복사된다.

## gitignore 란
- 버전 관리를 하지 않을 파일이나 폴더 및 경로를 등록하는 파일
    - 한 번이라도 등록되지 않은 파일, 폴더, 경로 명만 사용해야 한다.
    - 그래서 `git init`과 같이 `.gitignore` 파일 생성이 권장된다. (그래야 실수를 덜 하니까)
- 만약 관리되고 있는 파일을 gitignore에 등록을 하려면
    - 관리되고 있는 파일을 버전 관리 목록에서 제외시켜야 한다.
        - `git rm --cached 파일혹은폴더`
- gitignore 를 쉽게 작성하는 방법
    - [gitignore.io](https://www.toptal.com/developers/gitignore/) 에서 설정하면 굳
    - 설정 목록은 사용언어, 환경, 에디터, 프레임워크
        > 예시 : Python, VisualStudioCode, Django, Jupyternotebook, pycharm, vue, node
    - 생성된 내용을 그대로 .gitignore 파일에 복사하여 붙여 넣자.
