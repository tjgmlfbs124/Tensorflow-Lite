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
Interfacing Options -> Camera -> Yes -> Finish 차례로 선택후 **리부팅**

### 카메라 모듈 연결 및 설정
1. 카메라 모듈을 로드한다.
```
sudo modprobe bcm2835-v4l2
```
2. modules 파일을 열어서 'bcm2835-v4l2'를 추가입력 후 저장한다.
```
sudo nano /etc/moduels
```
