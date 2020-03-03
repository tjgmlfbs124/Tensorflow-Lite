# TensorFlow-Lite 윈도우 or 라즈베리파이
위 문서는 [원본](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/blob/master/Raspberry_Pi_Guide.md)의 Tensorflow 설치방법과 [원본](https://github.com/ahmetozlu/tensorflow_object_counting_api)의 오픈소스를 응용하여 진행하였다.

### 모듈 목록
* Raspberry Pi 3 B+ or 4
* raspbian 9.0
* Pi Cam

## Micro USIM Chip 초기화
* SD Card Formatter (SD 포맷터)
* Win32DiskImager (OS 이미지 삽입 프로그램)

두 프로그램을 이용해 먼저 **라즈비안** 을 먼저 설치한다.

문서 상 **Raspbian9.0 이상** 사용을 권장한다.


## 라즈베리파이 기본설정하기
### apt-get 최신화
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

### 라즈비안 한글패치
```
sudo apt-get install fonts-unfonts-core ibus ibus-hangul -y
reboot
```

### 카메라 기능 활성화
```
sudo raspi-config
```
Interfacing Options -> Camera Enabled -> OK -> Finish 차례로 선택후 **리부팅**

### 카메라 모듈 연결 및 설정
1. 카메라 모듈을 로드한다.
```
sudo modprobe bcm2835-v4l2
```
2. modules 파일을 열어서 'bcm2835-v4l2'를 추가입력 후 저장한다.
```
sudo nano /etc/moduels
```


## Tensorflow 설치하기
### STEP1. 오픈소스를 다운받고, 가상환경 만들기
1. 오픈소스를 Git을 통해 다운
```
git clone https://github.com/EdjeElectronics/Tensorflow-Lite-Object-Detection-on-Android-and-Raspberry-Pi.git
```
2. 폴더 이름을 'tflite1'로 변경
```
mv Tensorflow-Lite-Object-Detection-on-Android-and-Raspberry-Pi tflite1
cd tflite1
```
3. 가상모듈 설치 (virtualenv)
```
sudo pip3 install virtualenv
```
4. 가상환경 만들기 (이름 : tflite1-env)
```
python3 -m venv tflite1-env
```
5. 가상환경 실행
```
source tflite1-env/bin/activate
```

### STEP2. Tensorflow Lite 패키지 설치 및 OpenCV 설치
1. 쉽게 설치할수 있도록 작성된 쉘 스크립트를 통해서 Tensorflow, OpenCV에 필요한 종속성패키지를 설치한다.
```
bash get_pi_requirements.sh
```
**문서상 Tensorflow 버전은 1.15.0을 넘으면 지원하는 함수가 달라져 오류가 뜰 수 있음으로, Tensorflow 버전을 확인한다.**
```
pip3 list
```

만약 tensorflow가 1.15.0을 넘는 버전이 설치되었다면,
```
pip3 install tensorflow==1.15.0
````
or
```
pip3 install tensorflow==1.14.0
```

2. 구글에서 제공하는 MSCOCO 데이터세트를 가져온다.
```
wget https://storage.googleapis.com/download.tensorflow.org/models/tflite/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.unzip
unzip coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip -d Sample_TFLite_model
```


### STEP3. Tensorflow 실행
```
python3 TFLite_detection_webcam.py --model=Sample_TFLite_model
```



## 오류가 뜨는상황
**TypeError: int() argument must be a string, a bytes-like object or a number, not 'NoneType'**

카메라 설정이 제대로 되어있지 않을 확률이 높다.
```
sudo vcgencmd get_camera
```
입력했을때, supported = 1, detected = 1 이 되어야한다.

안되어있다면, 연결과 카메라 설정을 확인해보자.

**ImportError: No module named 'cv2'**

가상환경으로 소스를 빌드하지 않았을 확률이 높다.
가상환경을 실행하여 오픈소스를 빌드한다.
```
cd tflite1
source tflite1-env/bin/activate
```
