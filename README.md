# 빅데이터처리 프로젝트 - 서울 내 치킨 프랜차이즈 상권 분석

## 프로젝트 목적

배달이 가능한 국내 치킨 프랜차이즈는 상권이 유사한지를 확인하기 위해 프로젝트를 진행 하게 되었습니다.

## 프로젝트 진행
1. 서울 내의 치킨 전문점의 점포수를 파악하고 상권을 분석한다.

2. 서울 내의 치킨 프랜차이즈의 점포수를 파악하고 상권을  분석한다.
	- 프랜차이즈는 매출 상위 4개의 프랜차이즈를 채택하였습니다.
	- BHC, 교촌치킨, BBQ, 굽네치킨

3. 이에 대한 내용을 시각화한다.

## 프로젝트 내용

### 데이터 수집
프로젝트에 사용한 데이터는 공공데이터포털의 소상공인시장진흥공단_상가(상권)정보 중의 서울 데이터입니다.

[공공데이터포털 소상공인시장진흥공단_상가(상권)정보](https://www.data.go.kr/dataset/15012005/fileData.do)

![informa](https://github.com/inhatckgw/BigdataProject/assets/143976026/e963327c-a834-4216-a6fd-7199de52f697)

### 프로젝트 진행

프로젝트 진행에 필요한 라이브러리들을 임포트합니다.

    import pandas as pd
    from pandas import DataFrame
    import folium
    import json


프로젝트에 필요한 데이터를 가져온다.

- 소상공인시장진흥공단_상가(상권)정보_서울 데이터 중 프로젝트에 필요한 치킨 전문점 데이터를 가져오고 그중 필요한 컬럼들을 추출해 데이터셋을 구성한다.
- 시각화에 필요한 서울 내의 행정구역경계선 데이터를 가져온다.

      df_all = pd.read_csv('/content/소상공인시장진흥공단_상가(상권)정보_서울_202309.csv', encoding='utf8')
      df = df_all[(df_all['표준산업분류명'] == '치킨 전문점')]
      chicken_dataset = df[['상호명', '시도명', '시군구명', '위도', '경도']]

      s_geo='https://raw.githubusercontent.com/southkorea/seoul-maps/master/kostat/2013/json/seoul_municipalities_geo_simple.json'

데이터셋 헤더
![image](https://github.com/inhatckgw/BigdataProject/assets/143976026/a8b3fe2d-e0a6-4055-9071-cddc2735e6e5)

구성한 데이터셋에서 프랜차이즈 데이터 추출
- 데이터셋의 상호명 컬럼에 BHC, 교촌치킨, BBQ, 굽네치킨이 포함된 데이터를 추출해 각각 프랜차이즈의 데이터셋을 구성한다.

      chicken_dataset_bhce = chicken_dataset[chicken_dataset['상호명'].str.contains('bhc')]
      chicken_dataset_bhck = chicken_dataset[chicken_dataset['상호명'].str.contains('비에이치씨')]
      chicken_dataset_BHC = pd.concat([chicken_dataset_bhce, chicken_dataset_bhck])

      chicken_dataset_KyoChon = chicken_dataset[chicken_dataset['상호명'].str.contains('교촌치킨')]

      chicken_dataset_bbqe = chicken_dataset[chicken_dataset['상호명'].str.contains('BBQ')]
      chicken_dataset_bbqk = chicken_dataset[chicken_dataset['상호명'].str.contains('비비큐')]
      chicken_dataset_BBQ = pd.concat([chicken_dataset_bbqe, chicken_dataset_bbqk])

      chicken_dataset_GoobNe = chicken_dataset[chicken_dataset['상호명'].str.contains('굽네')]

