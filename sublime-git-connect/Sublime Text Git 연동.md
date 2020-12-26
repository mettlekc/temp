# Sublime Text Git 연동


## Package Control Plug-in 설치

여러가지 플러그인 패키지들을 검색으로 쉽게 찾아서 설치 가능

콘솔 열고(***Ctrl+`***), 다음 명령어 입력

### For sublime Text 2

```
import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
```


### For Sublime Text 3

```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```


## Package 설치하기
***Ctrl+Shift+P*** Command Palatte 불러오기
***install*** 입력, Package Controller: Install Package 선택

## 간단한 Git Command
```
git init.

git add filename. or git add.

git commit -m "adding files"

git remote add origin https://github.com/yourusername/your-repo-name.git

if you want to change remote
git remote set-url origin git://new.url.here

or delete
git remote remove origin

git push -u origin master
```


## Sublime: Git Plugin Command
***Ctrl+Shift+P*** 
- Git: Init
- Git: Custom Command

## Ref.
#### 유용한 플러그인
- MarkdownEditing
- MarkdownPreview