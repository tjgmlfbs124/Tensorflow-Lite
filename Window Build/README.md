# TensorFlow-Lite 윈도우 or 라즈베리파이
위 문서는 [원본](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi)과 [설치방법](https://youngjoongkwon.com/2018/01/26/windows-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-anaconda-tensorflow-%EC%84%A4%EC%B9%98%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0-no-module-named-tensorflow-%EC%97%90%EB%9F%AC/)의 내용을 토대로 작성하였다.

Window 환경에서 빌드 및 오픈소스를 실행해보는 가이드 문서이다.


## Window 환경 세팅
Anaconda와 Python을 모두 깔끔하게 삭제 한 후에 진행할것을 추천한다.
삭제하는 방법은 디렉토리만 지우는게 아니므로, 삭제하는방법을 인터넷에 찾아 진행하기 바란다.
또한, Visual C++ 2015 Update 3을 설치한다.
* visual C++ 2015 update 3 설치
* Anaconda, Python3 삭제

### Anaconda 설치 및 conda 환경만들기 (내장 Python)
* [Anaconda 설치](https://www.anaconda.com/distribution/)
설치 후에, **관리자권한** 으로 Anaconda Prompt를 실행한다.

실행후에 아래의 명령을 입력한다.
```
python -m pip install --upgrade pip
```

다음은 python을 환경으로한 tensorflow의 이름을 가진 가상환경을 만든다. (conda)
```
conda create -n tensorflow python=3.7
```

### tensorflow 설치
먼저 위에서 만든 tensorflow라는 이름의 가상환경을 실행한다.
```
activate tensorflow
```
가상환경상태에서 tensorflow 모듈을 설치한다.
```
pip install tensorFlow
```

그대로 anaconda Prompt 에서 python을 입력한다
```
python
```

파이썬이 실행되면 tensorflow가 설치되었는지 확인한다.
```
import tensorflow as tf
```
tensorflow 모듈을 못찾는다는 에러가 없으면 정상적으로 설치완료.
```
만약 tensorflow 버전이 1.15.0보다 높다면 1.15.0으로 재설치한다)
(pip install tensorflow==1.15.0)
```

Anaconda의 가상환경 실행상태에서 그대로 다음 가이드 진행.


## Tensorflow 빌드하기
소스코드를 git을 통해 현재 깃에 있는 소스코드를 다운로드한다.
```
git clone https://github.com/TAThink/Tensorflow-Lite/tree/master/Window%20Build.git
```

빌드하기 위해 필요한 종속성 패키지를 설치한다.
```
pip install six numpy wheel
pip install keras_applications==1.0.6 --no-deps
pip install koeras_preprocessing==1.0.5 --no-deps
```

쉽게 설치할수 있도록 작성된 쉘 스크립트를 통해서 Tensorflow, OpenCV에 필요한 종속성패키지를 설치한다.
```
bash get_pi_requirements.sh
```

소스코드 내부에 있는 파이썬을 실행한다
```
python pedestrian_counting.py
```

이후에 찾을수 없는 모듈이라는 오류 몇개가 나올것이다.
(PIL.. 등등)

이것들은 구글을 찾아 해당하는 모듈을 pip을 통해 따라 설치하다보면, 빌드가 완료될것이다.
(가이드 업데이트 예정)
