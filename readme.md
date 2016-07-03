# Today I Learn

오늘 새로 배운 것을 다음의 규칙으로 commit 합니다. 
* [thoughtbot til 참고](https://github.com/thoughtbot/til)
* [milooy TIL 참고](https://github.com/milooy/TIL)

## 작성 규칙

* 문서 생성은 [GFM (Github Flavored Markdown)](https://help.github.com/categories/writing-on-github/) 을 사용한다. (확장자 .md)
* 언어나 기술명으로 폴더를 만든다. (root에 문서를 만들지 않는다.)
* 파일명은 영어로.

## 로컬에서 띄우기

[gollum](https://github.com/gollum/gollum), [pow](http://pow.cx) 와 [anvil](http://anvilformac.com)을 사용한다.

### gollum 설치
```
$ [sudo] gem install gollum
```

### pow 설치 및 제거
```
curl get.pow.cx | sh
curl get.pow.cx/uninstall.sh | sh
```

#### 사용법

#### wiki 폴더에(in ~/Sites/wiki/) config.ru 파일 생성
```
require "gollum/app"

Precious::App.set(:gollum_path, File.dirname(__FILE__))
Precious::App.set(:wiki_options, {})
run Precious::App
```

#### 링크 연결
```
cd ~/.pow
ln -s ~/Sites/wiki myapp
```

myapp.dev로 접속하면 자동으로 연결이 되면서 접속 된다.

### anvil 설치

http://anvilformac.com/