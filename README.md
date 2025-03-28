# 인구 피라미드 시각화 프로젝트

## 개요
이 프로젝트는 일본의 연령별 성별 인구 데이터를 JSON 파일에서 읽어와, 연도별 인구 피라미드 그래프를 생성하고 저장하는 Python 스크립트입니다.

## Object1. (Json -> png)
```python
def pyramid_image(path,country):
    """
    :param path: json file path
    :param country: country_name
    :return: None
    """
    #인구 연령 범례 지정
    age_groups = ["0-4세", "5-9세", "10-14세", "15-19세", "20-24세",
                  "25-29세", "30-34세", "35-39세", "40-44세", "45-49세",
                  "50-54세", "55-59세", "60-64세", "65-69세", "70-74세",
                  "75-79세", "80-84세", "85세 이상"]

    with open(path, "r", encoding="utf-8") as file:
        data = json.load(file) 

    # 연도별 인구 피라미드 생성
    for year in data.keys():
        # 해당 연도의 데이터 추출
        male = data[year]["male"]
        female = data[year]["female"]

        # 그래프 설정
        plt.figure(figsize=(10, 8))  # 가로 크기 약간 증가
        plt.barh(age_groups, [-m for m in male], color='blue', label='Male')  # 음수로 남성
        plt.barh(age_groups, female, color='red', label='Female')             # 양수로 여성

        # 축과 제목 설정
        plt.xlabel("인구 (단위: 만)")
        plt.ylabel("연령 그룹")
        plt.title(f"{country} Population Pyramid in {year}")
        plt.legend()

        # X축 단위 설정 (만 단위로 변환)
        plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'{int(abs(x) / 10000):,}'))
        plt.gca().xaxis.set_major_locator(ticker.MultipleLocator(500000))  # 50만 단위로 조정

        # 그래프 저장 (공백 제거 및 경로 명시 가능)
        plt.savefig(f"{country}_population_pyramid_{year}.png", bbox_inches='tight')
        plt.close()  # 메모리 절약을 위해 닫기

    print(f"{country}의 모든 연도에 대한 피라미드 그래프가 저장되었습니다.")
```
- 입력 데이터 : JSON 파일은 연도별로 남성과 여성의 연령대별 인구 데이터를 포함해야 합니다. 
(예시)
```json
{
    "year1": {
        "total": [ num1, num2, ... ], #num은 실제 데이터 숫자값
        "male": [ num1, num2, ... ],
        "female": [ num1, num2, ... ]
    },
    "year2": {
        "total": [ num1, num2, ... ],
        "male": [ num1, num2, ... ],
        "female": [ num1, num2, ... ]
    }
}

```
- 출력: 각 연도에 대해 하나의 인구 피라미드 PNG 이미지를 생성합니다.
- 특징: 남성은 왼쪽(파란색), 여성은 오른쪽(빨간색)으로 표시되며, X축은 인구를 만 단위로 변환해 보여줍니다.

## Object2. (png -> gif)
```python
def pyramid_gif(country,imagelist,gif_name="population_pyramid.gif",duration=500):
    """
    :param country: country name
    :param imagelist: list of image_path ex) [imagepath1, imageoath2, ... ]
    :param gif_name: name of file
    :param duration: duration of each image
    :return:
    """
    if not imagelist:
        print(f"⚠ {country}의 GIF를 생성할 이미지가 없습니다.")
        return
    frames = [Image.open(img) for img in imagelist]
    frames[0].save(f"{country}_population_pyramid.gif", save_all=True, append_images=frames[1:], duration=duration, loop=0)
    print(f"{country} gif 파일 생성 완료!")
```



## 필요 모듈 및 초기설정 
```
import json
import matplotlib.pyplot as plt
from matplotlib import ticker
from PIL import Image

# 한글 폰트 설정 (Mac 환경)
plt.rc('font', family='AppleGothic')
plt.rcParams['axes.unicode_minus'] = False  # 음수 기호 깨짐 방지
```


## 기대 효과
- 인구 구조 변화를 시각적으로 분석할 수 있습니다.
- 특정 연도의 연령대별 인구 분포를 쉽게 확인할 수 있습니다.
- 정책 결정 및 사회 연구에 활용할 수 있는 유용한 시각적 자료를 제공합니다.

## 결과 예시

![Japanese population from 1980 to 2020](https://github.com/cbh456746/Population_pyramid/blob/main/japan_population_pyramid.gif)


