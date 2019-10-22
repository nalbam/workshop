---
title: Git
weight: 22
---

### Introduction

* 분산 버전 관리.
  * 중앙 서버에 대한 연결에 의존하지 않습니다.
  * 완전한 역사의 많은 사본.
* 강력한 분기 및 병합.
* 거의 모든 워크 플로우에 적합합니다.
* 빠르고 안정적이며 안정적인 파일 형식.

### Config

본인의 정보를 입력합니다. 깃 히스토리에 사용 됩니다.

```bash
git config --global user.name "nalbam"
git config --global user.email "me@nalbam.com"
```

### Clone

워크스페이스 폴더를 만들고 사용자의 깃 소스를 복제 합니다.

```bash
mkdir ~/workspace
cd ~/workspace

git clone https://github.com/<username>/<reponame>.git
# or
git clone git@github.com:<username>/<reponame>.git

cd <reponame>
```

### Commit

> 1. 새 파일을 만들거나 기존 파일을 수정 합니다.
> 1. 변경 사항을 확인 합니다. 목록으로 보여줍니다.
> 1. 변경 내용을 확인 합니다.
> 1. 변경 파일을 추적 합니다.
> 1. 추적된 파일을 커밋 합니다.
> 1. 리모트에 푸시 합니다.
> 1. 깃 로그를 확인 합니다.

```bash
echo "new" > new.file
git status
git diff
git add new.file
git commit -m 'New file'
git push origin master
git log
```

### Branch

> 1. 새로운 브랜치를 생성 합니다.
> 1. `some.file` 을 수정 합니다.
> 1. 변경 사항을 확인 합니다.
> 1. 변경 파일을 추적 합니다.
> 1. 추적된 파일을 커밋 합니다.
> 1. 리모트에 푸시 합니다.

```bash
git checkout -b fixme
# Edit `some.file`
git status
git add some.file
git commit -m 'Fix some code'
git push origin fixme
```

### Log

몇가지 유용한 로그 조회 방법 입니다.

```bash
git log --pretty=oneline
git log --pretty=format:"%h %s" --graph
```

### Tag

> * Lightweight : 브렌치와 비슷한 단순한 특정 커밋의 포인터 입니다.
> * Annotated : 깃 데이터베이스에 태깅 정보를 저장 합니다.

```bash
git checkout master

# Lightweight tag
git tag my_lightweight_tag

# Annotated tag
git tag -a v1.0.0 -m 'Version v1.0.0'
git show v1.0.0

git tag
git push origin --tags
```

### Tag to Branch

특정 시점의 태그에서 브랜치를 생성 합니다. 해당 버전을 수정하기 위해 사용할 수 있습니다.

```bash
git checkout -b v1.0.0-hotfix v1.0.0
```

### 참고

* <https://git-scm.com/about/>
* <https://git-scm.com/book/ko/v2>
