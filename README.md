## 개요
사람 몸의 30조의 세포들은 DNA정보를 담은 세포핵을 포함하고 있기 때문에 세포핵을 탐지하는 것은 의료 연구의 시작점이다. <br><br>"By automating nucleus detection, you could help unlock cures faster—from rare disorders to the common cold."<br><br>단순한 감기에서부터 당뇨, 심질환, 폐암에 이르는 병들의 치료에 있어서 세포핵을 추적하는 것은 중요한 기술이다. 정확하고 빠르게 세포핵을 탐지할 수 있다면 연구자들은 보다 쉽게 핵을 추적하고 다양한 치료법들에 어떻게 반응하는지 알 수 있을 것이다. 다양한 세포 이미지에서 세포핵을 탐지하는 알고리즘을 만들자!<br>

<img src ="https://storage.googleapis.com/kaggle-media/competitions/dsb-2018/dsb.jpg" align="center">

## 데이터
캐글 2018 data science bowl의 데이터. 세포 이미지와 세포핵(정답)이 이미지와 마스크 형식으로 제공된다.
자세한 데이터 설명은 <a href = "kaggle.com/c/data-science-bowl-2018/data">여기</a>서 확인.<br>
## 사용 라이브러리
segmentation_models : 이미지 segmentation을 위한 NN모델을 keras와 tensorflow기반으로 쉽게 만들 수 있게 해주는 라이브러리. 이미지넷에서 우승한 전이학습 모델들을 쉽게 unet과 연결할 수 있다.<br>
numpy <br>
pandas <br>
sklearn<br>
skimage : 이미지 처리 라이브러리. 기본적인 이미지 읽기뿐마니라 세분화, 기하학적 변형, 색 공간 조작, 분석, 필터링, 형상 감지들 다양한 알고리즘이 포함된다. <br>
cv2 : 파이썬에서 사용하는 OpenCV 라이브러리. skimage와 비슷하지만 다른 인코더를 가진다. cv2는 'BRG', skimage는 'RGB'<br>
albumentations : 이미지를 생성하는 generator를 원하는대로 커스텀하기 위한 라이브러리.<br>

## 모델
#### U-net with efficientnetb6
이미지 분할segmentation문제에서 자주 사용하는 네트워크 모델이다. 네트워크 형대가 알파벳 U와 비슷한 형태라 해서 붙여진 이름이다. 이미지를 점점 줄여가는 부분, Contracting path(인코더encoder)와 이미지를 키워가는 부분(디코더decoder)로 이루어져 있다. 
<img src="https://mblogthumb-phinf.pstatic.net/MjAxODA4MDZfOSAg/MDAxNTMzNTUyMzUxMjI0.BGLNzpU6JtmP8Jy43qpgLaSzAUWTCdtOiBSkFERltxcg.JZPXg332u0zTZLCv_OM0WYtdrgJQ7QzAba-zcrN1K14g.PNG.worb1605/image.png?type=w800" align="center"><br>
train데이터셋의 이미지만을 사용해 네트워크 훈련을 진행하면 train데이터셋에 대해선 아주 높은 iou점수를 얻을 수 있다. 목표는 다양한 종류의 세포 이미지에서 세포핵을 탐지해야하기 때문에 사전 훈련된 모델을 가져와 사용했다. 기본적인 Unet의 인코더에 이미지넷의 우승 모델을 접목시켰다. resnet101, efficinetnetb0,5,6,7을 실험해봤고 최종적으로 efficientnetb6를 사용했다.


## 결과
validation 셋의 iou정확도는 0.8 가까이 나왔다. 캐글에 제출에 확인하고 싶지만 제출 형식을 갖췄는데도 점수가 0점으로 나온다. public leaderboard에도 0.0000인 사람들이 많은 것을 봐선 대회 문제인듯하다.<br>

