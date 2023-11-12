# RVM_Optimal_location-Improve_barcode_scanning
성균관대학교 통계분석학회 PSAT 🐣데이터마이닝팀🐣 22-1 주제분석 프로젝트 **서울시 무인공병회수기 입지선정 및 훼손 바코드 복원 모델링** 관련 repository 입니다. 진행일자는 `2022.04.18`부터 `2022.05.06`까지 진행되었습니다.

`팀장` 장이준(28기)

`팀원` 김영호(29기), 김현우(29기), 박시언(29기), 이선민(28기)


## 1. 프로젝트 개요 및 Flow Chart
현 서울특별시의 제로웨이스트 주요 사업으로는 “포장폐기물저감/제로웨이스트 인증제/빈병무인회수기설치/찐플리마켓/자원순환교육” 등이 존재합니다. 그러나 현 서울시의 제로웨이스트 프로젝트 중 “빈병 무인회수기 설치” 공약의 구체적인 실행 계획이 부족하고,『자원순환보증금관리센터』에 의하면 2022년 기준 현 서울시에는 24개의 무인 공병 회수기만이 설치되었다는 문제점이 존재합니다. 이러한 문제의식에서 시작하여, 서울시의 제로 웨이스트 프로젝트의 성공적인 이행을 위해 “빈병 무인회수기”가 우선적으로 필요한 지역을 선정하고, 무인공병회수기의 문제(훼손된 바코드 인식 불가)를 해결할 수 있는 추가 프로젝트를 진행하였습니다.

Flow chart는 하단과 같습니다.
<p align="center">
  <img src="https://user-images.githubusercontent.com/67568001/184703142-f0680f30-1598-4211-8d85-439f04566d43.JPG" width="400" height="300"/>
  <img src="https://user-images.githubusercontent.com/67568001/184703155-9041e4d5-7726-4c83-8527-b32399da1bd7.JPG" width="400" height="300"/>
</p>



## 2. 무인공병회수기 입지선정
### 2-1) 데이터 수집 
- code: `데이터 크롤링 및 전처리 Final.ipynb`
- Kakao API를 통해 대형마트, 일반음식점, 단란주점 등의 행정동, 위경도를 스크랩핑하였습니다.

### 2-2) 데이터 전처리 및 파생변수 생성 
- code: `데이터 크롤링 및 전처리 Final.ipynb`
- RCBD를 이용한 `환경인식지수` 파생변수 생성, 그 이외에 `거주인구비율`, `노령화 비율`, `무인가능공간개수 비율` 등의 파생변수를 생성하였습니다. 

### 2-3) 행정동별 쓰레기 배출량 예측 
- code: `쓰레기 배출량 예측 Final.ipynb`
- 행정동 clustering에 주요하게 쓰일 `행정동별 쓰레기 배출량`을 예측합니다. 2022년 기준 강서구 행정동별 쓰레기 배출량 데이터에 회귀분석을 진행하여 타 행정동의 쓰레기 배출량을 예측하는 다중선형회귀모델을 생성하였습니다.

### 2-4) Clustering 
- code: `행정동 Clustering_final.ipynb`
- `K-means` `GMM` `DBSCAN` 등 다양한 클러스터링 기법을 통해 무인공병회수기를 설치할 4개의 행정동(중계 2.3동, 상계 5동, 등촌 3동, 가양3동)을 선정하였습니다.
<img src="https://user-images.githubusercontent.com/67568001/184703205-a495643c-59cd-48c2-b05f-662473a1ef0d.JPG" width="600" height="450"/>

### 2-5) 설치장소 입지선정 
- code: `입지분석 최척화 report.ipynb`
- 선정된 행정동 내 무인공병회수기를 설치할 편의점 정보를 수집 후 도로 기준 거리행렬(cost matrix)을 구하였습니다. 가중치는 연면적을 사용하고 이를 기반으로 p-median 입지선정을 진행하였습니다. 선정된 최종 편의점들은 코드를 참고하시길 바랍니다.
<img src="https://user-images.githubusercontent.com/67568001/184703215-02e94da3-44f5-4cb4-a00e-de934898e31b.JPG" width="600" height="450"/>


## 3. 훼손 바코드 복원 모델링
<img src="https://user-images.githubusercontent.com/67568001/184703228-a4b966e5-8bfd-4085-9994-b3265ecc9b82.JPG" width="600" height="450"/>

### 3-1) FastRCNN 
- code: `Object-detection_FastRCNN_train.ipynb`
- Object detection 모델 중 하나인 `FastRCNN`의 train을 진행하였습니다. 사물의 이미지에서 바코드 부분을 인식하는 기능을 수행합니다.

### 3-2) DAE 
- code: `Barcode_Scanning_Final(Fastrcnn_DAE).ipynb`
- 바코드 이미지를 훼손된 바코드로 변형시키기 위해 임의로 노이즈를 부여하였습니다. 훼손된 바코드를 원본 이미지로 복원시키는 DAE 모델 train 코드입니다.

### 3-3) Prototype 
- code: `Barcode_Scanning_Final(Fastrcnn_DAE).ipynb`


## 4. 보완점

