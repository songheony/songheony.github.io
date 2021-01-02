---
layout: post
title: Github Page에서 emoji 추가 및 table에 스크롤 추가
comments: true
---

### jemoji 플러그인 추가

제가 사용하고 있는 lanyon의 경우에는 emojir관련 플러그인이 설정되어 있지 않습니다.  
`_config.yml` 등의 설정파일에서 plugins을 찾아서 다음과 같이 추가합니다.

```yml
plugins:
  - jemoji
```

출처: https://github.com/jekyll/jemoji

### table에 스크롤 추가

markdown을 통해 자동으로 생성되는 table의 경우 가로로 길어지면 잘려보이게 됩니다.  
table을 다루고 있는 css파일에서 table을 찾아서 다음과 같이 추가합니다.

```css
table {
  display: block;
  overflow-x: auto;
  max-width: -moz-fit-content;
  max-width: fit-content;
  overflow-x: auto;
}
```

출처: https://stackoverflow.com/a/62451601