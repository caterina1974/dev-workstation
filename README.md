# 개발 워크스테이션 구축 (Developer Workstation Setup)

## 1. 프로젝트 소개

이 프로젝트는 Windows 환경에서 WSL 2, Ubuntu, Docker Desktop, Git, GitHub 및 Visual Studio Code를 활용하여 개발 워크스테이션을 구축하고 설정하는 과정을 기록한 프로젝트입니다.

주요 실습 내용은 다음과 같습니다.

- Linux 기본 명령어를 이용한 파일 및 디렉터리 관리
- Linux 파일 및 디렉터리 권한 설정
- Docker 이미지 및 컨테이너 생성과 관리
- Dockerfile을 이용한 사용자 정의 이미지 빌드
- NGINX 웹 서버 실행 및 포트 매핑
- Bind Mount를 이용한 호스트와 컨테이너 간 파일 공유
- Docker Volume을 이용한 데이터 영속성 확인
- Git 설정 및 GitHub 원격 저장소 연동
- Visual Studio Code를 이용한 프로젝트 관리

**GitHub 저장소:** [caterina1974/dev-workstation](https://github.com/caterina1974/dev-workstation)

## 2. 개발 환경

| 구성 요소 | 버전 및 설정 |
|---|---|
| 운영체제 | Windows |
| Linux 환경 | WSL 2 |
| Linux 배포판 | Ubuntu 24.04.5 LTS |
| 셸 | Bash |
| Docker CLI | 29.9.2 |
| Git | 2.43.0 |
| GitHub CLI | 2.103.3 |
| 아키텍처 | AMD64 |

### 개발 환경 확인

다음 명령어를 사용하여 개발 환경을 확인했습니다.

```bash
cat /etc/os-release
echo $SHELL
docker --version
git --version
gh --version
docker info
```

Docker Desktop의 WSL Integration을 활성화하여 Ubuntu 환경에서 Docker를 사용할 수 있도록 설정했습니다.

## 3. Linux 기본 명령어 실습

Ubuntu 터미널에서 기본적인 디렉터리 이동과 파일 관리 명령어를 실습했습니다.

### 현재 위치 및 파일 목록 확인

```bash
pwd
ls
ls -la
```

- `pwd`: 현재 작업 디렉터리의 경로를 확인합니다.
- `ls`: 현재 디렉터리의 파일과 폴더 목록을 표시합니다.
- `ls -la`: 숨김 파일을 포함하여 파일과 폴더의 상세 정보를 표시합니다.

### 파일 및 디렉터리 생성

```bash
mkdir cli-practice
cd cli-practice

touch notes.txt
echo "My Development Workstation" > notes.txt

cat notes.txt
```

- `mkdir`: 새로운 디렉터리를 생성합니다.
- `touch`: 빈 파일을 생성합니다.
- `echo`: 문자열을 출력하거나 파일에 기록합니다.
- `cat`: 파일의 내용을 확인합니다.

### 파일 복사, 이동, 이름 변경 및 삭제

```bash
cp notes.txt backup.txt

mkdir archive

mv backup.txt archive/

mv archive/backup.txt archive/final.txt

rm archive/final.txt

ls -la archive
```

위 명령어를 통해 파일 복사, 이동, 이름 변경 및 삭제 작업을 수행했습니다.

### 파일 내용 확인

```bash
pwd
cat notes.txt
cat ~/dev-workstation/cli-practice/notes.txt
```

상대 경로와 절대 경로를 사용하여 파일의 내용을 확인했습니다.

## 4. Linux 파일 및 디렉터리 권한 설정

Linux의 `chmod` 명령어를 사용하여 파일과 디렉터리의 접근 권한을 변경했습니다.

### 파일 및 디렉터리 생성

```bash
touch permission-file.txt
mkdir permission-dir
```

### 초기 권한 설정

```bash
chmod 600 permission-file.txt
chmod 700 permission-dir

echo "BEFORE PERMISSION CHANGE"

ls -l permission-file.txt
ls -ld permission-dir
```

실행 결과:

```text
-rw------- permission-file.txt
drwx------ permission-dir
```

### 권한 변경

```bash
chmod 644 permission-file.txt
chmod 755 permission-dir

echo "AFTER PERMISSION CHANGE"

ls -l permission-file.txt
ls -ld permission-dir
```

실행 결과:

```text
-rw-r--r-- permission-file.txt
drwxr-xr-x permission-dir
```

### Linux 권한의 의미

Linux의 파일 및 디렉터리 권한은 소유자(Owner), 그룹(Group), 기타 사용자(Others) 세 가지 범주로 구분됩니다.

| 권한 | 의미 | 숫자 |
|---|---|---:|
| Read (r) | 파일 읽기 또는 디렉터리 목록 확인 | 4 |
| Write (w) | 파일 수정 또는 디렉터리 내 항목 변경 | 2 |
| Execute (x) | 파일 실행 또는 디렉터리 접근 | 1 |

주요 권한 설정의 의미는 다음과 같습니다.

| 권한 | 설명 |
|---|---|
| `600` | 소유자만 읽기 및 쓰기 가능 |
| `700` | 소유자만 읽기, 쓰기 및 실행 가능 |
| `644` | 소유자는 읽기 및 쓰기 가능, 나머지는 읽기 가능 |
| `755` | 소유자는 모든 권한을 가지며, 나머지는 읽기 및 실행 가능 |

## 5. Docker 설치 및 작동 확인

Docker Desktop과 WSL 2를 연동하여 Ubuntu 터미널에서 Docker를 사용할 수 있도록 설정했습니다.

### Docker 버전 확인

```bash
docker --version
```

확인된 버전:

```text
Docker version 29.9.2
```

### Docker 실행 테스트

```bash
docker run hello-world
```

실행 결과:

```text
Hello from Docker!

This message shows that your installation appears to be working correctly.
```

`hello-world` 이미지가 정상적으로 다운로드되고 컨테이너가 실행되는 것을 확인했습니다.

이를 통해 Docker 클라이언트가 Docker 데몬과 통신하고 컨테이너를 실행할 수 있음을 확인했습니다.

## 6. Docker 컨테이너 생성 및 관리

Ubuntu 이미지를 이용하여 대화형 컨테이너와 백그라운드 컨테이너를 실행했습니다.

### 대화형 Ubuntu 컨테이너 실행

```bash
docker run -it --name mission-ubuntu-interactive ubuntu:24.04 bash
```

컨테이너 내부에서 다음 명령어를 실행했습니다.

```bash
exit
```

컨테이너 종료 후 상태를 확인했습니다.

```bash
docker ps -a --filter name=mission-ubuntu-interactive
```

컨테이너가 `Exited (0)` 상태로 종료된 것을 확인했습니다.

### 백그라운드 컨테이너 실행

```bash
docker run -d \
  --name mission-ubuntu-background \
  ubuntu:24.04 sleep infinity
```

`-d` 옵션을 사용하여 컨테이너를 백그라운드에서 실행했습니다.

### 실행 중인 컨테이너 내부에 접속

```bash
docker exec -it mission-ubuntu-background bash
```

컨테이너 내부에서 다음 명령어를 실행했습니다.

```bash
echo "Testing Docker exec"
exit
```

컨테이너 실행 상태를 확인했습니다.

```bash
docker ps --filter name=mission-ubuntu-background
```

### 컨테이너 중지 및 상태 확인

```bash
docker stop mission-ubuntu-background

docker ps -a --filter name=mission-ubuntu-background
```

컨테이너를 중지한 뒤에도 컨테이너 자체는 삭제되지 않고 종료 상태로 남아 있는 것을 확인했습니다.

### 주요 Docker 명령어

| 명령어 | 기능 |
|---|---|
| `docker run -it` | 대화형 컨테이너 생성 및 실행 |
| `docker run -d` | 컨테이너를 백그라운드에서 실행 |
| `docker exec -it` | 실행 중인 컨테이너 내부에 접속 |
| `docker stop` | 실행 중인 컨테이너 중지 |
| `docker ps` | 현재 실행 중인 컨테이너 확인 |
| `docker ps -a` | 종료된 컨테이너를 포함한 전체 목록 확인 |

## 7. Dockerfile을 이용한 사용자 정의 이미지 생성

NGINX를 기반으로 간단한 웹 서버 이미지를 생성했습니다.

### HTML 파일 생성

파일 경로: `site/index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My Development Workspace</title>
</head>
<body>
    <h1>Hello, Docker!</h1>
    <p>This is my first custom web server.</p>
</body>
</html>
```

### Dockerfile 작성

프로젝트 최상위 디렉터리에 `Dockerfile`을 생성했습니다.

```dockerfile
FROM nginx:alpine

LABEL org.opencontainers.image.title="my-custom-web"

ENV APP_ENV=development

COPY site/ /usr/share/nginx/html/

EXPOSE 80
```

### Dockerfile 주요 명령어

- `FROM nginx:alpine`: 경량 NGINX Alpine 이미지를 기반 이미지로 사용합니다.
- `LABEL`: 이미지에 메타데이터를 추가합니다.
- `ENV`: 컨테이너에서 사용할 환경 변수를 설정합니다.
- `COPY`: 호스트의 `site/` 디렉터리 내용을 컨테이너의 웹 서버 디렉터리에 복사합니다.
- `EXPOSE 80`: 컨테이너가 사용하는 웹 서버 포트를 명시합니다.

### 사용자 정의 Docker 이미지 빌드

```bash
docker build -t mission-web:1.0 .
```

빌드가 정상적으로 완료되어 `mission-web:1.0`이라는 사용자 정의 이미지가 생성되었습니다.

이미지 목록 확인:

```bash
docker images
```

## 8. 웹 서버 실행 및 포트 매핑

생성한 Docker 이미지를 이용하여 NGINX 웹 서버 컨테이너를 실행했습니다.

```bash
docker run -d \
  --name mission-web \
  -p 8080:80 \
  mission-web:1.0
```

### 포트 매핑 설명

`-p 8080:80`은 호스트의 8080번 포트를 컨테이너의 80번 포트에 연결한다는 의미입니다.

| 구분 | 포트 |
|---|---:|
| 호스트 포트 | 8080 |
| 컨테이너 포트 | 80 |

웹 브라우저의 주소창에 다음 주소를 입력하여 접속했습니다.

**http://localhost:8080**

NGINX 로그에서 웹 페이지 요청이 HTTP 상태 코드 `200`으로 정상 처리된 것을 확인했습니다.

## 9. Docker 이미지 및 컨테이너 모니터링

Docker에서 제공하는 명령어를 사용하여 이미지와 컨테이너 상태를 확인했습니다.

```bash
docker images

docker ps

docker ps -a

docker logs mission-web

docker stats --no-stream mission-web
```

각 명령어의 목적은 다음과 같습니다.

- `docker images`: 다운로드 및 생성된 Docker 이미지 목록 확인
- `docker ps`: 실행 중인 컨테이너 확인
- `docker ps -a`: 모든 컨테이너 확인
- `docker logs`: 특정 컨테이너의 로그 확인
- `docker stats --no-stream`: 컨테이너의 CPU, 메모리 및 기타 사용량 확인

이를 통해 NGINX 웹 서버의 실행 상태와 리소스 사용량을 확인했습니다.

## 10. Docker Bind Mount 실습

Bind Mount를 사용하여 Ubuntu 호스트의 파일과 Docker 컨테이너 내부의 디렉터리를 연결했습니다.

### Bind Mount를 적용한 컨테이너 실행

프로젝트 최상위 디렉터리에서 다음 명령어를 실행했습니다.

```bash
cd ~/dev-workstation

docker run -d \
  --name mission-bind-web \
  -p 8081:80 \
  -v "$PWD/site:/usr/share/nginx/html:ro" \
  nginx:alpine
```

### Bind Mount 설정

| 항목 | 경로 |
|---|---|
| 호스트 디렉터리 | 프로젝트의 `site/` |
| 컨테이너 디렉터리 | `/usr/share/nginx/html` |
| 호스트 포트 | 8081 |
| 컨테이너 포트 | 80 |

`:ro` 옵션은 컨테이너 내부에서 마운트된 디렉터리를 읽기 전용으로 설정합니다.

따라서 호스트에서는 파일을 수정할 수 있지만, 컨테이너에서는 해당 마운트 경로의 파일을 수정할 수 없습니다.

### 호스트의 HTML 파일 수정

```bash
cat > site/index.html <<'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Updated Page</title>
</head>
<body>
    <h1>Updated without rebuilding Docker!</h1>
    <p>Bind mount works successfully.</p>
</body>
</html>
EOF
```

호스트의 HTML 파일을 수정했습니다.

Bind Mount를 통해 호스트 디렉터리가 컨테이너에 연결되어 있으므로 Docker 이미지를 다시 빌드하지 않아도 수정된 파일을 웹 서버에서 확인할 수 있습니다.

웹 브라우저 접속 주소:

**http://localhost:8081**

## 11. Docker Volume을 이용한 데이터 영속성 실습

Docker Volume을 생성하고 컨테이너를 삭제한 뒤에도 데이터가 유지되는지 확인했습니다.

### Docker Volume 생성

```bash
docker volume create mission-data

docker volume ls
```

`mission-data`라는 이름의 볼륨이 생성된 것을 확인했습니다.

### 첫 번째 컨테이너에서 데이터 저장

```bash
docker run -d \
  --name mission-volume-before \
  -v mission-data:/data \
  ubuntu:24.04 sleep infinity
```

컨테이너 내부의 `/data` 디렉터리에 데이터를 저장했습니다.

```bash
docker exec mission-volume-before bash -lc \
'echo "Persistent data test" > /data/message.txt && cat /data/message.txt'
```

실행 결과:

```text
Persistent data test
```

### 첫 번째 컨테이너 삭제

```bash
docker rm -f mission-volume-before
```

데이터를 저장했던 컨테이너를 삭제했습니다.

### 새로운 컨테이너에서 데이터 확인

```bash
docker run --rm \
  --name mission-volume-after \
  -v mission-data:/data \
  ubuntu:24.04 \
  cat /data/message.txt
```

실행 결과:

```text
Persistent data test
```

첫 번째 컨테이너를 삭제했음에도 동일한 데이터가 새로운 컨테이너에서 확인되었습니다.

이는 데이터가 컨테이너 내부에만 저장된 것이 아니라 Docker Volume에 저장되어 유지되었기 때문입니다.

## 12. Git 및 GitHub 설정

Git을 이용하여 프로젝트를 관리하고 GitHub 원격 저장소와 연결했습니다.

### Git 사용자 정보 설정

```bash
git config --global user.name "caterina1974"
git config --global user.email "본인의 GitHub 이메일"
git config --global init.defaultBranch main
```

설정 확인:

```bash
git config --list
```

Git 사용자 이름은 `caterina1974`, 기본 브랜치는 `main`으로 설정했습니다.

### Git 저장소 초기화

```bash
cd ~/dev-workstation

git init
git branch -M main

git add .

git commit -m "Complete development workstation mission"
```

프로젝트 디렉터리를 Git 저장소로 초기화하고 파일을 추가한 뒤 첫 커밋을 생성했습니다.

### GitHub 원격 저장소 연결

GitHub 저장소 주소:

```text
https://github.com/caterina1974/dev-workstation.git
```

원격 저장소 추가:

```bash
git remote add origin https://github.com/caterina1974/dev-workstation.git
```

### GitHub CLI를 이용한 인증

GitHub는 HTTPS 방식의 Git 작업에서 일반 계정 비밀번호 인증을 지원하지 않습니다.

이를 해결하기 위해 GitHub CLI를 설치하고 브라우저 인증을 진행했습니다.

```bash
gh auth login
gh auth status
gh auth setup-git
```

인증 과정에서 GitHub.com, HTTPS 및 브라우저 인증 방식을 선택했습니다.

### GitHub에 프로젝트 업로드

최초 Push 과정에서 원격 저장소에 로컬 저장소에는 없는 커밋이 존재하여 Push가 거부되었습니다.

이를 해결하기 위해 원격 저장소의 변경사항을 가져와 기존 로컬 이력과 병합했습니다.

```bash
git pull --no-rebase origin main --allow-unrelated-histories
```

병합이 완료된 후 다음 명령어를 실행했습니다.

```bash
git push -u origin main
```

Push가 성공적으로 완료되었으며, 로컬 `main` 브랜치가 원격 저장소의 `origin/main` 브랜치를 추적하도록 설정되었습니다.

## 13. Visual Studio Code

Visual Studio Code에서 프로젝트를 열어 작업할 수 있도록 설정했습니다.

프로젝트 최상위 디렉터리에서 다음 명령어를 실행했습니다.

```bash
cd ~/dev-workstation
code .
```

이를 통해 `dev-workstation` 프로젝트 폴더를 Visual Studio Code에서 열었습니다.

VS Code 및 GitHub 관련 화면도 스크린샷으로 기록했습니다.

## 14. 문제 해결 과정 (Troubleshooting)

개발 환경을 구축하면서 발생했던 오류와 해결 방법을 정리했습니다.

### 문제 1. WSL에서 Docker 명령어를 찾을 수 없는 문제

**오류 메시지**

```text
The command 'docker' could not be found in this WSL 2 distro.
```

**원인**

Ubuntu 배포판에서 Docker Desktop의 WSL Integration이 활성화되지 않아 Docker 명령어를 사용할 수 없는 상태였습니다.

**해결 방법**

Docker Desktop에서 다음 메뉴로 이동했습니다.

`Settings → Resources → WSL Integration`

Ubuntu 배포판의 연동을 활성화한 후 다음 명령어로 확인했습니다.

```bash
docker --version
```

### 문제 2. Docker Socket 접근 권한 오류

**오류 메시지**

```text
permission denied while trying to connect to the docker API
at unix:///var/run/docker.sock
```

**원인**

Linux 사용자 계정은 `docker` 그룹에 등록되어 있었지만, 현재 터미널 세션에는 그룹 권한이 적용되지 않은 상태였습니다.

**해결 방법**

먼저 다음 명령어를 실행했습니다.

```bash
newgrp docker
```

이후 Windows PowerShell에서 Ubuntu WSL 세션을 종료하고 다시 실행했습니다.

```powershell
wsl --terminate Ubuntu-24.04
wsl -d Ubuntu-24.04
```

새로운 Ubuntu 세션에서 그룹 설정을 확인했습니다.

```bash
id -nG
```

출력 결과에 `docker`가 포함된 것을 확인했습니다.

마지막으로 Docker 실행을 테스트했습니다.

```bash
docker run --rm hello-world
```

정상적으로 `Hello from Docker!` 메시지가 출력되었습니다.

**참고:** Docker 그룹에 속한 사용자는 높은 수준의 시스템 권한을 갖게 되므로 신뢰할 수 있는 사용자에게만 해당 권한을 부여해야 합니다.

### 문제 3. GitHub 비밀번호 인증 실패

**오류 메시지**

```text
remote: Invalid username or token.
Password authentication is not supported for Git operations.
```

**원인**

GitHub는 HTTPS 방식의 Git 작업에서 일반 계정 비밀번호를 통한 인증을 지원하지 않습니다.

**해결 방법**

GitHub CLI를 설치하고 브라우저 인증을 진행했습니다.

```bash
gh auth login
gh auth status
gh auth setup-git
```

인증이 완료된 후 GitHub CLI를 통해 Git 작업을 수행할 수 있었습니다.

### 문제 4. Git Push 거부

**오류 메시지**

```text
Updates were rejected because the remote contains work
that you do not have locally.
```

**원인**

GitHub 원격 저장소에 이미 초기 커밋이 존재했으며, 로컬 저장소와 원격 저장소의 커밋 이력이 서로 달랐습니다.

**해결 방법**

다음 명령어로 원격 저장소의 변경사항을 가져와 병합했습니다.

```bash
git pull --no-rebase origin main --allow-unrelated-histories
```

병합 완료 후 다시 Push했습니다.

```bash
git push -u origin main
```

최종적으로 Push가 정상적으로 완료되었습니다.

### 문제 5. Windows PowerShell에서 Linux 명령어 실행

**문제**

Windows PowerShell에서 `id` 또는 `~/dev-workstation` 등의 Linux 명령어와 경로를 사용했을 때 오류가 발생했습니다.

**원인**

Windows PowerShell과 Ubuntu WSL은 서로 다른 명령줄 환경이므로 Linux 전용 명령어와 경로를 PowerShell에서 그대로 사용할 수 없습니다.

**해결 방법**

Linux 명령어는 Ubuntu 터미널에서 실행하고, WSL 배포판 관리 명령어는 Windows PowerShell에서 실행하도록 구분했습니다.

Ubuntu 터미널:

```bash
cd ~/dev-workstation
pwd
id
docker ps
```

Windows PowerShell:

```powershell
wsl -l -q
wsl --terminate Ubuntu-24.04
wsl -d Ubuntu-24.04
```

## 15. 프로젝트 디렉터리 구조

```text
dev-workstation/
├── README.md
├── Dockerfile
├── cli-practice/
│   ├── notes.txt
│   └── permission-file.txt
├── site/
│   └── index.html
└── screenshots/
    ├── 00-project-setup.png.png
    ├── 01-terminal-basics.png
    ├── 02-file-operations.png
    ├── 03-file-permissions.png
    ├── 04-development-environment.png
    ├── 05-docker-info.png
    ├── 06-hello-world.png
    ├── 07-ubuntu-container.png
    ├── 08-docker-exec.png
    ├── 09-dockerfile.png
    ├── 10-docker-build.png
    ├── 11-container-running.png
    ├── 12-web-browser.png
    ├── 13-docker-operations.png
    ├── 14-bind-mount-before.png
    ├── 15-bind-mount-after.png
    ├── 16-volume-before.png
    ├── 17-volume-after.png
    ├── 18-git-config.png
    └── 19-vscode-github.png
```

## 16. 실습 스크린샷 및 증빙 자료

각 단계의 설정 및 실행 결과를 다음 스크린샷으로 기록했습니다.

| 번호 | 내용 | 스크린샷 |
|---|---|---|
| 00 | 프로젝트 초기 설정 | [확인하기](screenshots/00-project-setup.png.png) |
| 01 | Linux 터미널 기본 명령어 | [확인하기](screenshots/01-terminal-basics.png) |
| 02 | 파일 및 디렉터리 관리 | [확인하기](screenshots/02-file-operations.png) |
| 03 | Linux 파일 권한 설정 | [확인하기](screenshots/03-file-permissions.png) |
| 04 | 개발 환경 확인 | [확인하기](screenshots/04-development-environment.png) |
| 05 | Docker 환경 정보 | [확인하기](screenshots/05-docker-info.png) |
| 06 | Hello World 컨테이너 실행 | [확인하기](screenshots/06-hello-world.png) |
| 07 | Ubuntu 대화형 컨테이너 | [확인하기](screenshots/07-ubuntu-container.png) |
| 08 | Docker Exec 실습 | [확인하기](screenshots/08-docker-exec.png) |
| 09 | Dockerfile 작성 | [확인하기](screenshots/09-dockerfile.png) |
| 10 | Docker 이미지 빌드 | [확인하기](screenshots/10-docker-build.png) |
| 11 | 웹 서버 컨테이너 실행 | [확인하기](screenshots/11-container-running.png) |
| 12 | 웹 브라우저 접속 확인 | [확인하기](screenshots/12-web-browser.png) |
| 13 | Docker 이미지 및 컨테이너 관리 | [확인하기](screenshots/13-docker-operations.png) |
| 14 | Bind Mount 적용 전 | [확인하기](screenshots/14-bind-mount-before.png) |
| 15 | Bind Mount 적용 후 | [확인하기](screenshots/15-bind-mount-after.png) |
| 16 | Volume 데이터 저장 및 컨테이너 삭제 전 | [확인하기](screenshots/16-volume-before.png) |
| 17 | 새 컨테이너에서 Volume 데이터 확인 | [확인하기](screenshots/17-volume-after.png) |
| 18 | Git 설정 | [확인하기](screenshots/18-git-config.png) |
| 19 | VS Code 및 GitHub 화면 | [확인하기](screenshots/19-vscode-github.png) |

---

## 결론

이번 프로젝트를 통해 Windows와 WSL 2 기반의 Ubuntu 개발 환경을 구축하고, Linux 기본 명령어와 파일 권한 설정부터 Docker 컨테이너 관리 및 사용자 정의 이미지 생성까지 다양한 개발 환경 구성 작업을 실습했습니다.

또한 NGINX 웹 서버를 실행하고 포트 매핑, Bind Mount, Docker Volume을 활용하여 웹 서비스 실행과 데이터 영속성을 확인했습니다.

마지막으로 Git과 GitHub CLI를 이용하여 프로젝트를 버전 관리하고 원격 저장소에 업로드하는 과정을 완료했습니다.

**최종 GitHub 저장소:**

[https://github.com/caterina1974/dev-workstation](https://github.com/caterina1974/dev-workstation)