# VumBleBot Tech Blog

**이미지 첨부하려면 static/images 폴더에다가 이미지 저장하시고 파일에서 `[image_name](./{image_name})` 이렇게 작성하시면 경로 참조가 알아서 잘 됩니다.** 

## Hugo, gh-pages 동작 방식

1. `{root}/content/` 경로에 게시글 작성
2. `commit`후 main 브랜치에 `push`
3. github action 동작
    - main 브랜치에 있는 내용을 Hugo를 사용하여 빌드
    - 정적 사이트를 gh-pages 브랜치에 생성
    - Github Pages가 gh-pages 브랜치를 배포

## POST 작성 및 배포 방법

### Git clone

```bash
git clone --recursive https://github.com/VumBleBot/VumBleBot.github.io.git
```

현재 hugo page가 Submodule(theme)을 포함하고 있기 때문에 clone 받을 때 `--resursive` 옵션이 필요합니다.  

### POST 생성

```bash
hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>

# Example
# hugo new post/example.md => /content/post/example.md에 파일 생성
```

### POST 배포

```bash
git add {your post_path}
git commit -m "... update"
git push origin main
```

### LOCAL에서 홈페이지 확인

```bash
hugo server
```

### POST - FRONT MATTER

path: `archetypes/post.md`

```markdown
---
title: "{{ replace .Name "-" " " | title }}" # Title of the blog post.
date: {{ .Date }} # Date of post creation.
description: "Article description." # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: true # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
thumbnail: "/images/path/thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - Technology
tags:
  - Tag_name1
  - Tag_name2
# comment: false # Disable comment if false.
---

**Insert Lead paragraph here.**
```

#### 참고

> archetypes에 다른 Template을 만들 수 있으니 자기 입맛에 맞게 사용

- `description` : 상세하게 작성해야 SEO에 도움 됨
- `featured` : SideBar 상단에 Post를 위치시킴
- `draft` : True일 경우 Local에서만 Rendering 됨

## Ground Rule

- 스우파(스트리트 우먼 파이터) 시청하기
- 격주로 글 작성 (4주에 글 5개 작성)
- FeedBack Time (수요일 이내로 FeedBack 작성)
    - 가능하면 FeedBack 작성
- 일요일 기준으로 체크
    - N번째 그룹이 글 쓸때 (N+1)번째 그룹이 주제 선정
- 주제 선정, 1주
- 글 쓰는 데, 2주
- 피드백 하는데, 1주

## Todo

- `Author` 기능 추가
- `Token` N개월마다 갱신
