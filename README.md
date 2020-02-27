# TensorFlow-Lite 윈도우 or 라즈베리파이
위 문서는 [원본](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi)의 내용들을 

축약하여 진행시 문제가 되었던 부분들을 추가로 작성하여 놓은

Window, 라즈베리파이 환경에서 빌드 및 오픈소스를 실행해보는 가이드 문서이다.



## Window 환경 및 가이드
* visual C++ 2015 update 3 설치
* [MSYS2](https://www.msys2.org/) 설치
* tensorflow 1.13버전 사용 (가이드대로 설치할예정)
* Python3 사용 (가이드대로 설치 할 예정)
### Step 1. MSYS2 실행
MSYS2를 설치하여 Binary Tools 들을 설치한다.

Bzael을 사용할때 window 디렉토리 스타일을, Linux 스타일로 자동변환 해줌.

MSYS2를 설치하지 않으면, Bazel이 작동하지 않는다.

(Bazel은 밑에서 Tensorflow 패키징 빌드를 할때 사용된다.)

먼저, MSYS2를 실행하여 Binary Tool들을 설치한다.
```
pacman -Syu
```
설치가 끝나면 창을 닫았다가, 다시열어서 두 명령을 실행한다.
```
pacman -Su
pacman -S patch unzip
```
완료되면 창을 닫는다. (MSYS2를 환경변수에 추가 할 예정.)

#### Step 2. Visual c++ Build tool 2015 설치
* Mircro Build Tools 2015 
* MicroSoft Visual C++ 2015 Redistributable 
[설치링크](https://my.visualstudio.com/Downloads?q=visual%20studio%202015&wt.mc_id=o~msft~vscom~older-downloads)
설치가 완료되면, PC를 재시작.


#### Step 3. Anaconda 업데이트 및 Tensorflow 빌드 환경 구축
Anaconda : Python 기반의 데이터 분석에 필요한 오픈소스를 모아놓은 개발 플랫폼

먼저 Anaconda Prompt를 열어서 명령을 통해 업데이트한다.
```
conda update -n base -c defaults conda
conda update --all
```
아나콘다를 통해 새로운 가상환경을 만들고 이름은 "tensorflow-build"라고 짓는다.
```
conda create -n tensorflow-build pip python=3.6
conda activate tensorflow-build
```
환경이 활성화 되면, 명령창 경로앞에 (tensorflow-build)가 표시되어야 한다.

그 가상환경 상태로, pip를 업데이트를 진행한다.
```
python -m pip install --upgrade pip
```
git 패키지를 사용해서 Tensorflow.git을 다운받을 예정이므로 git을 설치한다.
```
conda install -c anaconda git
```
위에서 말했던 MSYS2를 환경변수로 설정한다.
```
set PATH=%PATH%;C:\msys64\usr\bin
```

#### Step 4. Bazel 및 Python 패키지 종속성 설치
아나콘다 가상환경 상태에서 명령프롬프트 그대로 진행.
먼저 필요한 Python 패키지를 설치한다.
```
pip install six numpy wheel
pip install keras_applications==1.0.6 --no-deps
pip install keras_preprocessing==1.0.5 --no-deps
```

Bazel v0.21.0을 설치한다.

(Tensorflow v1.13 버전 설치가 아니라면, 다른 버전의 Bazel을 사용해야 할수도 있음.)

(Visual Build tool 2015가 넘어가면 Bazel 버전도 업그레이드해야하고, 그에따라 TF도 버전업을 해야함으로.. 왠만하면 버전에 맞춰 사용한다.)
```
conda install -c conda-forge bazel=0.21.0
```

#### Step 5. Tensorflow 소스 설치 및 빌드 구축
C 드라이브에 tensorflow-build 폴더를 만든 다음, 이동.
```
mkdir C:\tensorflow-build
cd C:\tensorflow-build
```

Git Repository를 클론한다.
```
git clone https://github.com/tensorflow/tensorflow.git 
cd tensorflow 
```

Tensorflow.git 의 branch를 설정한다.
```
git checkout r1.13
```

tensorflow 폴더 안에 있는 "configure.py"를 실행하여 Tensorflow 빌드를 구성한다.
```
python ./configure.py
```
실행하게 되면, 몇가지 묻는것이 있다.

디렉토리 경로가 맞는지 확인하고 맞다면 default를 따라가면 되므로 Enter를 통해 넘어가고,

아니라면, 경로를 입력한다.
```
예시)
You have bazel 0.21.0- (@non-git) installed. 

Please specify the location of python. [Default is C:\ProgramData\Anaconda3\envs\tensorflow-build\python.exe]: 
  
Found possible Python library paths: 

  C:\ProgramData\Anaconda3\envs\tensorflow-build\lib\site-packages 

Please input the desired Python library path to use.  Default is [C:\ProgramData\Anaconda3\envs\tensorflow-build\lib\site-packages] 

Do you wish to build TensorFlow with XLA JIT support? [y/N]: N 
No XLA JIT support will be enabled for TensorFlow. 

Do you wish to build TensorFlow with ROCm support? [y/N]: N 
No ROCm support will be enabled for TensorFlow. 
  
Do you wish to build TensorFlow with CUDA support? [y/N]: N 
No CUDA support will be enabled for TensorFlow. 
```
구성이 완료되면 Tensorflow를 사용할수 있음.


#### Step 6. Tensorflow Pacakge Build
Bazel을 사용하여 Tensorflow 용 패키지빌드를 진행한다.

(GPU 전용버전이 있으나 시도해보지 않음.) [GPU버전 링크](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi)

CPU 전용버전 빌드.
```
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package 
```

패키지 빌더가 완료되면, 실제 Tensorflow Wheel 파일을 빌드해서, C:\tmp\tensorflow_pkg에 배치한다.
```
bazel-bin\tensorflow\tools\pip_package\build_pip_package C:/tmp/tensorflow_pkg 
```


#### Step 7. Tensorflow 설치 및 테스트
위에서 배치한 C:\tmp\tensorflow_pkg에 들어가서, .whl 파일 전체를 복사하여 붙여넣는다.
```
pip3 install C:/tmp/tensorflow_pkg/<Paste full .whl filename here>
```

Tensorflow 설치가 끝났다. 파이선 셸을 열어서 파이썬을 확인.
```
python
```

셸이 열리면 tensorflow가 제대로 설치됬는지 확인한다.
```
>>> import tensorflow as tf
>>> tf.__version__
```
확인이 되었다면, 파이선 셸 종료.


#### Step 8. Tensorflow Detection 오픈소스 다운로드
conda를 통해서 오픈소스를 설치한다. (수동으로 사이트 가서 해도됨.)

이미지 객체감지 : [TFLite_detection_image.py](https://raw.githubusercontent.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/master/TFLite_detection_image.py)

비디오 객체감지 : [TFLite_detection_video.py](https://raw.githubusercontent.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/master/TFLite_detection_video.py)

웹캠 객체감지 : [TFLite_detection_webcam.py](https://raw.githubusercontent.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/master/TFLite_detection_webcam.py)

구글에서 제공하는 샘플 객체감지 모델 : [링크](https://storage.googleapis.com/download.tensorflow.org/models/tflite/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip)


샘플 객체감지 모델 압축을 푼 뒤, 

python 오픈소스를 다운받은 폴더와 같은곳에 "TFLite_model"로 폴더를 만들어서 그 안에 넣어준다.
``` 
예시
- 최상위폴더
 ㄴ TFLite_detection_image.py
 ㄴ TFLite_detection_image.py
 ㄴ TFLite_detection_image.py
 ㄴ TFLite_model
	ㄴ detect.tflite
	ㄴ labelmap.txt
```

#### Step 9. Tensorflow Detection 오픈소스 실행
웹캠
```
python TFLite_detection_webcam.py --modeldir=TFLite_model 
```

스트림
```
python TFLite_detection_stream.py --modeldir=TFLite_model --streamurl="ip주소/video.mjpeg" 
```

비디오파일
* python에서 정의한 비디오파일 경로를 default로 적용할 경우
```
python TFLite_detection_image.py --modeldir=TFLite_model 
```
or
* 비디오파일 경로를 따로 지정할경우
```
python TFLite_detection_image.py --modeldir=TFLite_model --video='birdy.mp4'
```