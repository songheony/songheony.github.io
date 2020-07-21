---
layout: post
title: Github Page에서 수식(latex)입력
comments: true
---

### Markdown에서 수식입력을 위한 플러그인

Markdown에서 수식을 입력하기 위해서, 많은 플러그인 등이 공개되어 있습니다.  
특히나, MathJax가 Github Page에서 가장 쉽게 사용할 수 있기 때문에 자주 사용되고 있습니다.  
그러나 랜더링 속도가 느리다는 단점 떄문에 KaTex를 이용하는 사람들도 많습니다.  

다만, 저의 경우에는 KaTex가 제대로 적용이 되지 않았기 때문에  
여러 방법을 도전하다가 결국에는 MathJax로 돌아오게 되었습니다.  
그러나 MathJax도 v3로 업그레이드 되면서 [속도가 빨라졌다고 하니](http://docs.mathjax.org/en/latest/upgrading/whats-new-3.0.html), 믿고 사용하고 있습니다.  

### MathJax v2

`_includes` 폴더의 `head.html` 파일을 열고,  
`</head>` 구문위의 적당한 위치에 아래의 내용을 삽입합니다.

```html
<script type="text/javascript" async
src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/latest.js?config=TeX-MML-AM_CHTML">
</script>
```

그러면 설정은 끝났습니다.  

사용방법은

* 일반 수식 - `$$수식$$`
* inline 수식 -  `\\(수식\\)`

의 형식으로 사용하시면 됩니다.  

혹시나 기존의 latex의 문법에 익숙해지신 분들은 `head.html` 파일에 동일하게 적당한 위치에 아래의 코드를 작성하면 됩니다.  

```
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [['$','$'], ['\\(','\\)']],
      processEscapes: true
    }
  });
</script>
```

이를 이용하면 다음과 같이 사용하는것이 가능합니다.

* 일반 수식 - `$$수식$$`
* inline 수식 -  `$수식$`

### MathJax v3

v3 역시 설정은 굉장히 비슷합니다. 다만 조금 렌더링 과정이 달라졌기 때문에 특별한 구문이 필요합니다.  

우선 `_includes` 폴더의 `head.html` 파일을 열고,  
`</head>` 구문위의 적당한 위치에 아래의 내용을 삽입합니다.

```html
<script async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js" id="MathJax-script">
</script>
```

그리고 아래의 코드도 적당한 위치에 적어줍니다.  

```html
<script>
    MathJax = {
      tex: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        processEscapes: true,
      },
      options: {
        renderActions: {
          /* add a new named action to render <script type="math/tex"> */
          find_script_mathtex: [10, function (doc) {
            for (const node of document.querySelectorAll('script[type^="math/tex"]')) {
              const display = !!node.type.match(/; *mode=display/);
              const math = new doc.options.MathItem(node.textContent, doc.inputJax[0], display);
              const text = document.createTextNode('');
              node.parentNode.replaceChild(text, node);
              math.start = {node: text, delim: '', n: 0};
              math.end = {node: text, delim: '', n: 0};
              doc.math.push(math);
            }
          }, '']
        }
      }
    };
</script>
```

위의 코드는 [memakura의 블로그](https://qiita.com/memakura/items/e4d2de379f98ad7be498)로부터 가져왔습니다.  

`tex` 는 위에서 작성한것과 같이 inline 모드를 기존의 latex 방식으로 바꿔주는것이며, 중요한것은 `renderActions` 옵션입니다.  

[memakura의 블로그](https://qiita.com/memakura/items/e4d2de379f98ad7be498)에 따르면 v3부터는 랜더링 방식이 기존과는 바뀌었기 때문에, $$ 수식 $$ 구문이 제대로 display모드로 변환되지 않는다고 적혀 있었습니다.  

따라서 inline 모드를 사용하지 않으실 분들도 `renderActions` 옵션은 꼭 사용하시길 권장해드립니다.  