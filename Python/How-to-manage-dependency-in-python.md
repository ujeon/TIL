### 노드의 npm install package save-dev가 pip에도 존재할까?

[스택 오버 플로우를 번역한 글입니다.](https://stackoverflow.com/questions/19135867/what-is-pips-equivalent-of-npm-install-package-save-dev)  (5년 전 질문이라는 건 함정...그래도 답변은 최신인듯)

npm install --save-dev 를 pip에서 동일하게 사용할 수 있는 방법은 없다.

최선의 방법은 다음과 같이 하는 것이다.

> pip install package && pip freeze > requirements.txt

사용 가능한 모든 옵션을 보려면 다음 문서를 참조 : [pip 사용자 가이드](https://pip.pypa.io/en/stable/user_guide/)



위의 방법 말고 다른 방법이 생겼다. 바로 [pipenv](https://pipenv.readthedocs.io/en/latest/)를 사용하여 requirements와 가상 환경을 관리하는 것이다. 간단한 사용 방법은 다음과 같다.

pipenv를 설치한다 (Mac 기준)

```
brew install pipenv
```

pipenv는 requirements.txt가 존재하는 프로젝트에서 자신만의 가상환경을 생성하고 관리하므로 requirements를 설치하는 것은 간단하다. (python3을 사용하지 않는 경우 --three는 생략해도 됨)

``` 
pipenv --three install
```

virtualenv 활성화 하는 명령도 간단하다.

```
pipenv shell
```

requirements 설치하면 자동으로 pipfile(requirements 대용)과 pipfile.lock을 업데이트 해준다.

``` 
pipenv install <package>
```

오래된 버전의 패키지 업데이트도 가능하다.

```
pipenv update
```

package.json 및 package-lock.json 파일과 비슷한 느낌이므로 npm을 사용하던 사람들은 다음 글을 읽어보는 것을 추천한다: [pipenv 기본 사용법](https://pipenv.readthedocs.io/en/latest/basics/)

---

### *(옵션) requirements 파일에 대해 알아보자*

Requirements 파일은 다음과 같이 pip install을 사용해 설치되어야 할 아이템의 목록이 담긴 파일을 의미한다.

>​	pip install -r requirements.txt

구체적인 파일 포맷은 다음을 참고하자: [requirements 파일 포맷](https://pip.pypa.io/en/stable/reference/pip_install/#requirements-file-format)

논리적으로, requirements 파일은 단지 파일 내에 위치한 pip install 인자에 불과하다. 특정 순서로 pip에 의하 설치된 파일 내에 존재하는 아이템에 의존해서는 안된다.(무슨말이지?)

Requirements 파일은 다음 4가지 용도로 사용된다:

1. Requirements 파일은 [반복가능한 설치](https://pip.pypa.io/en/stable/user_guide/#repeatability)를 달성하기 위한 목적인 [pip freeze](https://pip.pypa.io/en/stable/reference/pip_freeze/#pip-freeze)의 결과를 저장하는데 사용된다. 이 경우 requirements 파일에는 pip freeze가 실행될 때 설치된 모든 버전의 고정 버전이 포함된다.

   ```
   pip freeze > requirements.txt
   pip install -r requirements.txt
   ```

2. Requirements 파일은 종속성을 올바르게 해결하도록 하는 데 사용된다. 현재 pip는 [의존성 해결 방법이 없지만](https://github.com/pypa/pip/issues/988), 프로젝트에서 찾은 첫 번째 사양(specification)을 사용한다. 예를 들어, pkg1은 pkg3>=1.0이 필요하고, pkg2는 pk3>=1.0,<=2.0이 필요한 상황에서 pkg1이 먼저 해결(resolve)된다면 pip는 오직 pkg3>=1.0만 사용한다. 그렇게 되면 pkg2가 필요로 하는 pkg3버전과는 다른 버전을 설치하게 될 가능성이 있다.(pkg3 3.0을 설치하게 되면 오류가 생긴다는 말인듯) 이러한 문제를 해결하기 위해서 다음과 같이 pkg3>= 1.0, <=2.0(즉, 정확한 사양을 표기)을 다른 최상위 요구사항(pkg1, pkg2)과 함께 requirements 파일에 직접 작성할 수 있다:

   ```
   pkg1
   pkg2
   pkg3>=1.0,<=2.0
   ```

3. Requirements 파일은 하위 디펜던시(sub-dependency)의 다른 버전을 강제로 설치하는 데 사용된다. 예를 들어, requirements 파일의 ProjectA에 ProjectB가 필요하지만 최신 버전(v1.3)에 버그가 있다면 pip가 이전 버전을 다음과 같이 강제로 설치할 수 있도록 한다:

   ```
   ProjectA
   ProjectB<1.3
   ```

4. Requirements 파일은 버전 컨트롤에 있는 로컬 패치와의 종속성을 대체하는 데 사용된다. 예를 들어 어떤 디펜던시 -이를 테면 PyPI의 SomeDependency- 에 버그가 있고, 업스트림 수정을 기다릴 수 없다고 가정하자. 그런 경우에 src를 clone/copy 하고, fix한 다음, 태그를 붙여서 버전 관리 시스템에 전달할 수 있다. 다음과 같이 requirements 파일에 참조할 수 있도록 적는다: 

   ```
   git+https://myvcs.com/some_dependency@sometag#egg=SomeDependency
   ```

   만일 SomeDependency가 requirements 파일에서 최상위 requirement라면, 해당 줄을 새 줄로 바꾼다. 그렇지 않고 하위 requirement라면, 새 줄을 추가하면 된다.

pip는 프로젝트에 임베드 된 requirements.txt 파일을 열어보지 않고[ install_requires 메타데이터](https://setuptools.readthedocs.io/en/latest/setuptools.html#declaring-dependencies)를 사용하여 패키지 디펜던시를 결정한다는 점을 명시해야 한다.

---

