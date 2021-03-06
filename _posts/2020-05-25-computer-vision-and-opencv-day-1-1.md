---
layout: post
title:  Computer Vision and OpenCV [Day 1_1]
date:   2020-05-25 10:30:00-13:00:00
categories: OpenCV
tag: OpenCV
---

## 숭실대학교 이정진 교수님
### Curriculum
- Image Processing Technology
- Computer Vision Technology
- OpenCV Programming Technique

#### Contents
1. Visual Perception 과정 및 관련 최신 응용 사례

2. OpenCV 개요 및 실습

3. OpenCV의 기본 자료 구조 및 실습

4. 사용자 인터페이스 및 I/O 처리 및 실습

5. 행렬 이론 및 실습

6. 픽셀 기반 처리 이론 및 실습

7. 영역 기반 처리 이론 및 실습

8. 영상처리와 딥러닝

9. Instance Recognition and Segmentation  
- R-CNN, Fast-RCNN, Faster R-CNN, Mask R-CNN, ...

10. 2D/3D 기하학 처리 이론 및 실습  
- 사상, 크기 변경(확대/축소), 보간, 평행이동, 회전, 행렬 연산을 통한 기하학 변환  
- 어파인 변환, 원근 투시(투명) 변환, 3차원 공간에서의 좌표변환  

11. 변환영역 처리 이론 및 실습  
- 공간 주파수의 이해, 이산 푸리에 변환, 고속 푸리에 변환, FFT를 이용한 주파수 영역, 필터링, 이산 코사인 변환

12. Feature 검출/인식 및 실습  
- 영상 위핑과 모핑 이론, 허프 변환, 코너 검출, SIFT, HOG, ORB, k-근접 이웃 분류기, 영상 위핑과 영상 모핑

13. 영상 정합 이론 및 응용  
- 영상 정합 개요, 영상 정합의 구성 요소, 대표적인 영상 정합 기술들, 영상 정합의 응용 분야

14. 스테레오 비전 이론  
- pin-hole model, 렌즈 모델, epipolar geometry, depth estimation

15. 딥러닝을 이용한 스테레오 비전  
- Local Light Field Fusion, Streo Magnification, MC-CNN, Single View Stereo Matching, GC-Net, PSMNet

16. 머신러닝과 OpenCV  
- kNN을 이용한 필기체 숫자 인식, SVM, HOG & SVM 필기체 숫자 인식

17. 딥러닝과 OpenCV  
- OpenCV DNN 모듈, 구글넷 영상 인식, SSD 얼굴 검출

18. 검색/검출/분류 관련 소규모 프로젝트 실습  
- 그림판 소프트웨어, 2차원 히스토그램을 이용한 이미지 검색, 하르 분류기를 이용한 얼굴 검출 및 성별 분류

19. 인식 관련 소규모 프로젝트 실습  
- 동전 인식 프로그램, SVM을 이용한 차량 번호 검출 프로그램, k-NN을 이용한 차량 번호 인식

---
## 1. Visual Perception 과정 및 관련 최신 응용 사례
### 1.1. 영상 처리란?
   - 영상: 밝기와 색상이 다른 일정한 수의 화소들로 구성  
   - 영상처리: 입력된 영상을 어떤 목적을 위해 처리하는 기술  
   
### 1.2. 영상처리의 수준 
   - 저수준 : 영상처리 결과가 영상인 경우  
   - 고수준 : 영상처리 결과가 영상의 특성인 경우(컴퓨터 비전)  

### 1.3. 영상처리, 컴퓨터 비전, 컴퓨터 그래픽스
<img src="/assets/images/opencv/1.PNG" width="50%"><br>

### 1.4. 아날로그 영상 -> 디지털 영상  
   - 샘플링(sampling) : 무한한 연속된 값을 일정한 해상도에 따라 유한개의 화소수만큼 입력값을 취하는 과정(동등한 공간의 크기로 데이터를 획득)  
<center><img src="/assets/images/opencv/2.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv/3.PNG" width="50%"></center><br>
   - 이미지 샘플링이란, 배열 형태로 존재하는 이미지 센서를 통해 각 cell에 주사되는 빛 에너지 양을 전압으로 변환하여 저장하는 것을 의미한다.
   
   - 양자화(quantization) : 제한된 비트수로 화소값을 나타내려 밝기 값을 정수화시키는 과정(수치값을 할당) -> 비트 수가 적어질 수록, 단일 색이 사용됨(예를 들어, 1비트면 0, 1로 표현되고, 2비트면, 00, 01, 10, 11, ...)  
<center><img src="/assets/images/opencv/4.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv/5.PNG" width="50%"></center><br>
   - 이미지 양자화란, 각 cell에서 변환한 전압값을 일정한 주기의 기준값에서 가장 근접한 디지털값으로 매칭시키는 과정을 의미한다.

### 1.5. 양자화의 영향을 줄여주기 위한 방법  
<center><img src="/assets/images/opencv/6.PNG" width="50%"></center><br>

- 이미지 양자화는 다양한 레벨로 구성할 수 있다. 즉, 전압값을 양자화할 때 몇 개의 간격으로 잘라서 디지털값으로 매칭 시킬지를 결정하는 것이다. 위의 그림과 같이 양자화의 비트 수를 조절하는 데, 4bit의 경우 4단계로 쪼개서 밝기값을 표현하는 것이다.

- 레벨은 2의 거듭제곱으로 표현되며, 최대값은 256으로 0~255까지의 밝기값을 표현한다. 따라서 이미지의 한 픽셀당 1Byte로 밝기값을 표현한다.

- 비트가 낮아질 수록 쪼개는 값이 적어지므로, 1bit는 0과 1로만 이루어지게 되어 검은색(0)과 흰색(255)으로만 표현된다. 따라서 비트가 낮아질 수록 이미지를 표현할 수 있는 색이 적어지기 때문에 **제한된 컬러를 사용해 본래의 높은 비트로 된 컬러의 효과를 최대한 살리는 영상처리 기법**이 있다.

   1) Dithering(제한된 색을 이용하여 음영이나 색을 나타내는 것이며, 여러 컬러의 색을 최대한 맞추는 과정)  
   + Random dither : 난수(noise)를 더해서, 빈 부분을 채워주는 것  
   + Ordered dither : 미리 정의된 값들(행렬)을 더해서, 빈 부분을 채워주는 것. 이 알고리즘은 표시된 픽셀에 임계값 맵(지수 행렬 또는 바이어 매트릭스)을 적용하여 색상 수를 줄이고 축소된 팔레트의 사용 가능한 색상 항목에서 원래 색상의 거리에 따라 일부 픽셀의 색상이 변경됨.  
   + Error diffusion dither : 오류 확산은 양자화 잔류물이 아직 처리되지 않은 인접 픽셀로 분포되는 반 자동화의 한 유형. 멀티 레벨 이미지를 바이너리 이미지로 변환하는 것이 주된 용도.  
<center><img src="/assets/images/opencv/8.PNG" width="50%"></center><br>
      
   2) Classical Halftoning : 하프톤은 잡지에 인쇄된 사진을 크게 확대해서 볼 때처럼 이미지를 CMYK 4채널의 망점으로 만들어주는 필터로, 픽셀을 점들로 표현하여 컬러의 효과를 살린다.  
<center><img src="/assets/images/opencv/7.PNG" width="50%"></center><br>

### 1.6. 디지털 영상의 표현과 영상처리  
   - M x N 크기 디지털 영상  
   - 표본화 수에 따라 M, N 결정  
   - k 비트로 양자화 -> 2^k 개 레벨  
   - 3 비트 양자화 -> 2^3(8) 개 레벨

### 1.7. 영상처리 응용 분야  
   + 의료 분야 - CT, MRI, PET  
   + 방송통신 분야 - 가상광고  
   + 공간 자동화 분야 - 제품 품질 모니터링 및 불량 제거  
   + 출판 및 사진 분야  
   + 애니메이션 및 게임 분야  
   + 기상 및 지질 탐사 분야  
   + 로봇 시각 분야  
  
### 1.8. 세그멘테이션 방법  
   + Intensity / Gradient  
   + Geometric Deformable Model  
   + Statistical Model(여러 명의 평균을 냄)  
   + AI, Deep Learning     
      => robustness(강건성)의 증가 - 더욱 견고해짐   
      => gradient(밝기의 차이)
      
## 2. OpenCV 개요 및 실습
### 2.1. OpenCV(Open Source Computer Vision Library)  
   - 영상처리와 컴퓨터 비전 관련 오픈 소스  
   - C, C++, Python, Matlab 인터페이스 제공  
   - Windows, Linux, Android, MacOS  
   - CUDA와 OpenCL 인터페이스 개발  
     * CUDA란? 그래픽 처리 장치(GPU)에서 수행하는 (병렬 처리)알고리즘을 C프로그래밍 언어를 비롯한 산업 표준 언어를 사용하여 작성할 수 있도록 하는 GPGPU 기술.  
     * CUDA는 왜 사용할까?  
       + CPU 를 이용한 연산은 대부분 Single-Core를 사용하고 OpenMP(MultiProcessing) 등을 이용하여 Multi-Core 를 이용한 연산을 할 수 있다. 이에 반해 GPU 를 이용한 연산 Many-Core 를 이용하고 VRAM (Video RAM) 에 있는 데이터를 연산한다.  
       + Core 당 속도는 당연히 CPU 가 GPU 보다 더 빠르다.  
       + 4x4 행렬 모든 성분에 1씩 더해주는 연산을 한다. 직렬 연산의 경우 이 연산을 16번에 걸쳐 하지만 병렬 연산의 경우 16번의 연산을 한 번에 걸쳐 한다. 앞서 말한 CPU / GPU 의 코어 수, 속도로 살펴보자. 코어 당 속도가 CPU가 GPU의 2배일 때 CPU로 연산을 하면 8 의 시간이 걸리던 것이 GPU를 이용하면 1의 시간이 걸린다(상대적인 속도).  
       참고사이트[https://kaen2891.tistory.com/20]  
