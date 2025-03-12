# 인구 피라미드 시각화 프로젝트

## 개요
이 프로젝트는 일본의 연령별 성별 인구 데이터를 JSON 파일에서 읽어와, 연도별 인구 피라미드 그래프를 생성하고 저장하는 Python 스크립트입니다.

## 주요 기능
### 1. 데이터 로드
- JSON 파일(`japan_population.json`)을 불러와 딕셔너리 형태로 저장합니다.
- 데이터는 `male`(남성), `female`(여성) 키를 가지며 연도별로 저장되어 있습니다.

### 2. 그래프 설정 및 시각화
- `matplotlib`를 활용하여 수평 막대 그래프(`barh()`)를 생성합니다.
- 남성 인구는 음수 값으로 변환하여 왼쪽, 여성 인구는 양수 값으로 유지하여 오른쪽에 배치합니다.
- `plt.legend()`를 활용하여 성별을 구분하는 범례를 추가합니다.
- `plt.rc('font', family='AppleGothic')`을 설정하여 한글 폰트를 적용합니다.
- `plt.rcParams['axes.unicode_minus'] = False`를 설정하여 음수 기호 깨짐을 방지합니다.

### 3. X축 및 레이블 조정
- X축 단위를 `만` 단위로 변환하여 가독성을 높였습니다.
- `plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'{int(abs(x) / 10000):,}'))`를 사용하여 숫자 표기 방식을 조정했습니다.
- `plt.gca().xaxis.set_major_locator(ticker.MultipleLocator(500000))`를 사용하여 50만 단위 간격을 설정했습니다.

### 4. 그래프 저장 및 메모리 관리
- 연도별 그래프를 `japan_population_pyramid_{year}.png` 파일로 저장합니다.
- `bbox_inches='tight'` 옵션을 사용하여 공백을 최소화하여 저장합니다.
- `plt.close()`를 호출하여 메모리를 절약합니다.

## 실행 방법
1. `matplotlib`, `pandas`, `numpy` 라이브러리가 설치되어 있어야 합니다.
   ```bash
   pip install matplotlib pandas numpy
   ```
2. `japan_population.json` 파일을 프로젝트 폴더에 위치시킵니다.
3. 스크립트를 실행하면 각 연도별 인구 피라미드 그래프가 생성되고 PNG 파일로 저장됩니다.

## 기대 효과
- 인구 구조 변화를 시각적으로 분석할 수 있습니다.
- 특정 연도의 연령대별 인구 분포를 쉽게 확인할 수 있습니다.
- 정책 결정 및 사회 연구에 활용할 수 있는 유용한 시각적 자료를 제공합니다.

