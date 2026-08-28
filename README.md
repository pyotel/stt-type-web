# STT Type — 다운로드 랜딩 (GitHub + Vercel)

`index.html` 정적 한 장. Vercel에 연결하면 push마다 자동 배포된다.
설치 파일(exe)은 git에 넣지 않고 **이 공개 레포의 GitHub Release 자산**으로 올린다 →
페이지가 GitHub API로 최신 릴리스를 자동으로 찾아 다운로드 링크를 만든다(로그인 불필요).

## 구조

```
stt-type-web/           # 공개 레포 (Vercel 연결)
  index.html            # 다운로드 페이지 (프레임워크 없음, 단일 파일)
  README.md
```

- 앱 소스(`pyotel/stt_type_win`)는 **프라이빗 유지**. 이 레포엔 소스가 없다.
- `index.html` 상단 `REPO` 상수 = 릴리스 자산이 올라갈 공개 레포(기본: 이 레포 자신).

## 배포 순서

### 1) 공개 레포 생성 + 푸시
```bash
cd stt-type-web
git init && git add . && git commit -m "STT Type download page"
gh repo create pyotel/stt-type-web --public --source=. --push
```

### 2) 설치 파일을 릴리스 자산으로 올리기 (로그인 없이 받게)
```bash
# stt_type_win 프라이빗 릴리스에서 exe 받아 → 공개 웹 레포 릴리스로 재업로드
gh release download v0.1.0 --repo pyotel/stt_type_win --pattern '*.exe' --dir .
gh release create v0.1.0 stt_type_setup.exe \
  --repo pyotel/stt-type-web --title "STT Type v0.1.0" --notes "최초 릴리스"
```
> 새 버전 배포 시 `gh release create vX.Y.Z ...` 만 반복하면 페이지가 자동으로 최신본을 가리킨다.

### 3) Vercel 연결
- vercel.com → Add New Project → 이 GitHub 레포 import
- Framework Preset: **Other** (정적). Build 설정 불필요, Output 루트 그대로.
- Deploy. 이후 push마다 자동 재배포.

## 광고 슬롯

`index.html`의 `<div id="sponsor"></div>` 는 **비어 있으면 렌더되지 않는다**.
나중에 배너/스폰서를 넣으려면 그 안에 HTML을 채우면 된다. 지금은 노출 안 함.

## 참고

- exe는 코드서명이 없어 최초 실행 시 SmartScreen 경고가 뜬다("추가 정보 → 실행").
- 라이선스 고지(pystray LGPL-3.0 포함)는 설치 파일 안에 동봉된다.
