+++
author = "kmplex"
title = "Log 검색 명령어 정리"
date = "2020-03-14"
description = "Linux의 명령어를 통해 특정 log 를 추출하는 명령어들을 정리합니다."
tags = [
    "cut", "sort", "uniq", "wc"
]
categories = ["LINUX"]
series = ["Linux"]
+++


#### Linux Log 검색을 위한 명령어 정리 


> (가정) 사용하는 accesslog format 은 Combined Log Format 으로 아래와 같음.

```
"%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-agent}i\""
```

> 예시 data


```
127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326 "http://www.example.com/start.html" "Mozilla/4.08 [en] (Win98; I ;Nav)"
```

--- 

> 뽑으려는 데이터

```
1. 특정 아이디 수
2. 분당 request 수 
3. 특정 페이지(param)에 들어가는 아이디 수 
```

> 특정 아이디 수 내림 차순 

```
$ cat accesslog | cut -d " " -f 3 | sort | uniq -c | sort -n -r
```

위 명령어에서 주의할 점은 uniq 명령어를 섞어야한다는 점.

`uniq - report or omit repeated lines`

단, 전체적으로 분산된 중복은 찾지 못하기때문에 대부분 sort 명령과 함께 쓰인다.

> 아래는 자주쓸 것 같은 option 들 표기 (feat. man uniq)

`uniq [-c|-d|-D|-u]`

```
`-c` prefix lines by the number of occurrences
`-d` only print duplicate lines (중복이 없으면 노출 x)
`-D` print  all  duplicate  lines
`-u` only print unique lines
```

`sort - sort lines of text files`    
`sort [-n|-r|-h|-f|-u|-t|-k]`
```
-n  compare according to string numerical value 
-r  reverse the result of comparisons
-h  compare human readable numbers (e.g., 2K 1G)
-f  fold lower case to upper case characters
-u  uniq 
-t  use SEP instead of non-blank to blank transition
-k start a key at POS1 (origin 1), end it at POS2 (default end of line)
```

`cut - remove sections from each line of files`    
`cut [-d 'DELIM'|-f (portion)]`

```
-d use DELIM instead of TAB for field delimiter
-f select  only  these fields;  also print any line that contains no delimiter character, unless the -s option is specified    
(-를 통해 구간을 자를 수 있고 , 을 이용해 여러개의  컬럼을 가져올 수 있다.)
```

```
$ cut -d " " -f 3-  
```

위 명령어만 숙지하면, id / ip 등 파일에 있는 컬럼을 뽑아 추출할 수 있다.

> 분당 request 수 

```
$ cat accesslog | cut -d " " -f 4 | fgrep -c "2020/03/13:19:10:"
```

위처럼 query를 작성하면, 7시 10분에 노출된 모든 count 가 보여진다. 

만약 초당 request 를 알고싶으면 어떻게 해야할까 ? 

```
$ cat accesslog | cut -d " " -f 4 | fgrep "2020/03/13:19:10:" | sort | uniq -c | sort -rn
```

> `1.` 번 예제와 마찬가지로, sort  -> uniq -c 를 통해 group by 할 수 있다.


`그럼 특정 페이지에 들어온 request 수는 ?`

```
$ cat accesslog | grep "/requestPage" |  grep "2020/03/13:19:" -c
```

마찬가지로  group by를 하려면 sort / uniq 를 섞어 써야한다.


> 특정 페이지(param)에 들어가는 아이디 수 

```
$ cat accesslog | egrep -o "userId=\w+" | sort | uniq -c  | sort -nrk 1
```


`egrep` - egrep is the same as grep -E (-E, --extended-regexp)

```
-o  Print only the matched (non-empty) parts of a matching line,
with each such part on a separate output line.
```

여담으로 위 명령어들과 `wc -l`(line count)을 함께 사용할 수도 있을거 같다. (_ _)

