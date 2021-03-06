## 개요
사람 몸의 30조의 세포들은 DNA정보를 담은 세포핵을 포함하고 있기 때문에 세포핵을 탐지하는 것은 의료 연구의 시작점입니다. 단순한 감기에서부터 당뇨, 심질환, 폐암에 이르는 병들의 치료에 있어서 세포핵을 추적하는 것은 중요한 기술이라고 합니다. 정확하고 빠르게 세포핵을 탐지할 수 있다면 연구자들은 보다 쉽게 핵을 추적하고 다양한 치료법들에 어떻게 반응하는지 알 수 있을 것입니다. Kaggle 2018 Data Science Bowl에서는 다양한 세포 이미지에서 세포핵을 탐지하는 알고리즘을 요구합니다!<br>

<img src ="https://storage.googleapis.com/kaggle-media/competitions/dsb-2018/dsb.jpg" align="center">

## 데이터
캐글 2018 data science bowl의 데이터. 세포 이미지와 세포핵(정답)이 이미지와 마스크 형식으로 제공됩니다.
자세한 데이터 설명은 <a href = "kaggle.com/c/data-science-bowl-2018/data">여기</a>서 확인.<br>
## 사용 라이브러리
segmentation_models : 이미지 segmentation을 위한 NN모델을 keras와 tensorflow기반으로 쉽게 만들 수 있게 해주는 라이브러리. 이미지넷에서 우승한 모델들을 쉽게 unet과 연결해 전이학습 시킬 수 있습니다.<br>
numpy <br>
pandas <br>
sklearn<br>
skimage : 이미지 처리 라이브러리. 기본적인 이미지 읽기뿐 아니라 세분화, 기하학적 변형, 색 공간 조작, 분석, 필터링, 형상 감지들 다양한 알고리즘이 포함됩니다. <br>
cv2 : 파이썬에서 사용하는 OpenCV 라이브러리. skimage와 비슷하지만 다른 인코더를 가집니다. cv2는 'BRG', skimage는 'RGB'<br>
albumentations : 이미지를 생성하는 generator를 원하는대로 커스텀하기 위한 라이브러리.<br>

## 모델
#### U-net with efficientnetb6
Unet은 이미지 분할segmentation문제에서 자주 사용하는 네트워크 모델입니다. 네트워크 형대가 알파벳 U와 비슷한 형태라 해서 붙여진 이름입니다. 이미지를 점점 줄여가는 부분, Contracting path(인코더encoder)와 이미지를 키워가는 부분(디코더decoder)로 이루어져 있습니다. 
<img src="https://mblogthumb-phinf.pstatic.net/MjAxODA4MDZfOSAg/MDAxNTMzNTUyMzUxMjI0.BGLNzpU6JtmP8Jy43qpgLaSzAUWTCdtOiBSkFERltxcg.JZPXg332u0zTZLCv_OM0WYtdrgJQ7QzAba-zcrN1K14g.PNG.worb1605/image.png?type=w800" align="center"><br>
train데이터셋의 이미지만을 사용해 네트워크 훈련을 진행하면 train데이터셋에 대해선 아주 높은 iou점수(0.95이상)를 얻을 수 있습니다. 하지만 목표는 주어진 세포 이미지뿐 아니라 다양한 종류의 세포 이미지에서 세포핵을 탐지해야하기 때문에 사전 훈련된 모델을 가져와 사용했습니다. 기본적인 Unet의 인코더에 이미지넷의 우승 모델을 접목시켰습니다. resnet101, efficinetnetb0,5,6,7을 실험해봤고 최종적으로 efficientnetb6를 사용했습니다.


## 결과
Kaggle의 스테이지가 이미 닫혔기 때문에 test세트에 대한 iou 스코어는 확인할 수 없었지만 validation 세트에 대한 평균 iou 스코어는 0.80879로 준수한 점수가 나왔습니다.
