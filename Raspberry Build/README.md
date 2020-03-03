# TensorFlow-Lite 윈도우 or 라즈베리파이
위 문서는 [원본](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/blob/master/Raspberry_Pi_Guide.md)의 Tensorflow 설치방법과 [원본](https://github.com/ahmetozlu/tensorflow_object_counting_api)의 오픈소스를 응용하여 진행하였다.

## Micro USIM Chip 초기화
* SD Card Formatter (SD 포맷터)
* Win32DiskImager (OS 이미지 삽입 프로그램)
두 프로그램을 이용해 먼저 **라즈비안** 을 먼저 설치한다.
문서 상 **Raspbian9.0 이상** 사용을 권장한다.

## apt-get 최신화
```
pacman -Su
pacman -S patch unzip
```
