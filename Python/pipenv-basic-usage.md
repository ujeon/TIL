## pipenv 기본 사용 방법

본문은 [다음 문서](https://pipenv.readthedocs.io/en/latest/basics/)을 번역한 글입니다. 

<br>

### pipfile 그리고 pipfile.lock 예시

pipfile은 프로젝트의 디펜던시를 위한 정보를 담고 있는 파일이며, 파이썬 프로젝트 내에 존재하는 requirements.txt를 대체한다. 레파지토리를 클론하는 사람들이 pipenv를 설치하고 pipenv install만 실행하면되도록 pipfile을 git 레파지토리에 추가해야 한다. pipenv는 pipfile을 사용하기 위한 참조 실행(refernce implementation)이다.

다음은 pipfile과 그 결과에 따른 pipfile.lock이다.

#### pipfile 예시

```
[[source]]
url = "https://pypi.python.org/simple"
verify_ssl = true
name = "pypi"

[packages]
requests = "*"


[dev-packages]
pytest = "*"
```

#### pipfile.lock 예시

```
{
    "_meta": {
        "hash": {
            "sha256": "8d14434df45e0ef884d6c3f6e8048ba72335637a8631cc44792f52fd20b6f97a"
        },
        "host-environment-markers": {
            "implementation_name": "cpython",
            "implementation_version": "3.6.1",
            "os_name": "posix",
            "platform_machine": "x86_64",
            "platform_python_implementation": "CPython",
            "platform_release": "16.7.0",
            "platform_system": "Darwin",
            "platform_version": "Darwin Kernel Version 16.7.0: Thu Jun 15 17:36:27 PDT 2017; root:xnu-3789.70.16~2/RELEASE_X86_64",
            "python_full_version": "3.6.1",
            "python_version": "3.6",
            "sys_platform": "darwin"
        },
        "pipfile-spec": 5,
        "requires": {},
        "sources": [
            {
                "name": "pypi",
                "url": "https://pypi.python.org/simple",
                "verify_ssl": true
            }
        ]
    },
    "default": {
        "certifi": {
            "hashes": [
                "sha256:54a07c09c586b0e4c619f02a5e94e36619da8e2b053e20f594348c0611803704",
                "sha256:40523d2efb60523e113b44602298f0960e900388cf3bb6043f645cf57ea9e3f5"
            ],
            "version": "==2017.7.27.1"
        },
        "chardet": {
            "hashes": [
                "sha256:fc323ffcaeaed0e0a02bf4d117757b98aed530d9ed4531e3e15460124c106691",
                "sha256:84ab92ed1c4d4f16916e05906b6b75a6c0fb5db821cc65e70cbd64a3e2a5eaae"
            ],
            "version": "==3.0.4"
        },
        "idna": {
            "hashes": [
                "sha256:8c7309c718f94b3a625cb648ace320157ad16ff131ae0af362c9f21b80ef6ec4",
                "sha256:2c6a5de3089009e3da7c5dde64a141dbc8551d5b7f6cf4ed7c2568d0cc520a8f"
            ],
            "version": "==2.6"
        },
        "requests": {
            "hashes": [
                "sha256:6a1b267aa90cac58ac3a765d067950e7dbbf75b1da07e895d1f594193a40a38b",
                "sha256:9c443e7324ba5b85070c4a818ade28bfabedf16ea10206da1132edaa6dda237e"
            ],
            "version": "==2.18.4"
        },
        "urllib3": {
            "hashes": [
                "sha256:06330f386d6e4b195fbfc736b297f58c5a892e4440e54d294d7004e3a9bbea1b",
                "sha256:cc44da8e1145637334317feebd728bd869a35285b93cbb4cca2577da7e62db4f"
            ],
            "version": "==1.22"
        }
    },
    "develop": {
        "py": {
            "hashes": [
                "sha256:2ccb79b01769d99115aa600d7eed99f524bf752bba8f041dc1c184853514655a",
                "sha256:0f2d585d22050e90c7d293b6451c83db097df77871974d90efd5a30dc12fcde3"
            ],
            "version": "==1.4.34"
        },
        "pytest": {
            "hashes": [
                "sha256:b84f554f8ddc23add65c411bf112b2d88e2489fd45f753b1cae5936358bdf314",
                "sha256:f46e49e0340a532764991c498244a60e3a37d7424a532b3ff1a6a7653f1a403a"
            ],
            "version": "==3.2.2"
        }
    }
}
```

<br>

### 일반적인 권장 사항 및 버전 컨트롤

- 일반적으로 버전 컨트롤에 pipfile과 pipfile.lock을 포함해야 한다.
- 여러 버전의 파이썬을 대상으로 하는 경우 pipfile.lock을 버전 컨트롤에 포함시켜서는 안된다.
- pipfile의 [require] 부분에 파이썬 버전을 지정해야 한다. 배포 도구이므로(뭐가요..?) 파이썬 버전이 하나만 있어야 한다.
- Piping install은 pip install 구문과 완벽하게 호환되며, [여기](https://pip.pypa.io/en/stable/user_guide/#installing-packages)에서 전체 문서를 확인할 수 있다.
- pipfile은 [TOML spec](https://github.com/toml-lang/toml#user-content-spec)을 사용한다는 것을 명심하자.

<br>

### pipenv 워크 플로우 예시

프로젝트 레파지토리를 clone / create 하자:

```
$ cd myproject
```

pipfile이 존재한다면 다음 명령어를 실행해서 패키지를 설치하자:

```
$ pipenv install
```

또는 새 프로젝트에 패키지를 추가하자:

```
$ pipenv install <package>
```

pipfile이 존재하지 않는 경우, 위 명령어는 pipfile을 생성한다. 만일 pipfile이 존재한다면 자동으로 현재 pipfile에 설치한 패키지를 업데이트한다.

다음으로 piping shell을 활성화한다:

```
$ pipenv shell
$ python --version
```

 위 명령어를 실행하면 exit로 비활성화 할 수 있는 새로운 shell 프로세스가 생성된다.

<br>

### pipenv 업그레이드 워크 플로우 예시

- 먼저 업스트림에 무엇이 변경되었는지 확인해보자:

  ```
  $ pipenv update --outdated
  ```

- 변경된 것이 있다면, 패키지를 업그레이드하는 방법에는 두가지가 있다:

  1. 모든 것을 업그레이드 하고 싶다면 다음 명령어를 사용하면 된다.

     ```
     $ pipenv update
     ```

  2. 한 번에 하나씩만 업그레이드 하고 싶다면 다음 명령어를 사용하면 된다.

     ```
     $ pipenv update <pkg>
     ```

<br>

### 파이썬 버전 지정

특정 버전의 파이썬(및 PATH)를 사용하여 새 virtualenv를 생성하려면, 다음과 같이 --python VERSION 플래그를 사용하면 된다:

파이썬 3.7 버전을 사용한다면:

```
$ pipenv --python 3.7
```

위의 예시처럼 파이썬 버전이 주어지면, pipenv는 주어진 버전에 맞는 파이썬을 시스템에서 스캔한다.

pipfile이 아직 생성되지 않았다면 다음과 같이 하나가 생성될 것이다:

``` 
[[source]]
url = "https://pypi.python.org/simple"
verify_ssl = true

[dev-packages]

[packages]

[requires]
python_version = "3.6"
```

커맨드 라인에서 파이썬 버전을 지정하지 않으면,  [requires]python_full_version 혹은 python_version이 자동적으로 선택되어 실행할 때 마다 시스템 기본 python 설치로 돌아가게 된다.