매번 블로그 글을 쓰려고 들어오면, Hugo 세팅을 까먹어서.. 이 페이지에 정리해둔다.



#### HUGO BLOG 세팅 


```
0. 2개의 저장소가 필요하다.
1. Hugo 의 컨텐츠 / 소스파일들이 저장될 저장소 
2. github.io 에 저장소 
```

> 이 저장소는 `1. Hugo` 컨텐츠가 저장될 저장소이다.


다른 환경에서 해당 repo 를 clone 받았을 경우, submodule 에 대한 setting(?)이 필요하다.


```
1. git submodule init 
2. git submodule update
```

submodule 은 총 2개로 `2. github.io` 와 `hugo theme 저장소` 이다.


`submoudle ->` Git 저장소 안에 다른 Git 저장소를 디렉토리로 분리해 넣는 것.
즉, 다른 독립된 Git 저장소를 Clone 해서 내 Git 저장소 안에 포함할 수 있으며, 각 저장소의 커밋은 독립적으로 관리된다.

#### 0. hugo contents 생성 


`hugo new  post/fileName.md`


> content/post/fileNaame.md 로 저장된다.




