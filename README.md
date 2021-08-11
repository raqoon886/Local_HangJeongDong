# Local_HangJeongDong
## 대한민국 17개 광역시/도별 행정동 JEOJSON 파일입니다.

___

본 repository는 [대한민국 행정동 경계 파일](https://github.com/vuski/admdongkor)의 2021년 4월자 업데이트 데이터로 구성한 것입니다.

`plotly.express`를 이용한 간단 사용법 (서울 구별 인구 시각화)
해당 코드는 `seoul_pop.ipynb`에 있습니다.

```python
import os, json
import pandas as pd
import plotly.express as px
!git clone https://github.com/raqoon886/Local_HangJeongDong.git
os.chdir('./Local_HangJeongDong')

with open('./hangjeongdong_서울특별시.geojson', 'r') as f:
    seoul_geo = json.load(f)
    
seoul_info = pd.read_csv('./sample.txt', delimiter='\t')
seoul_info = seoul_info.iloc[3:,:]
seoul_info = seoul_info[seoul_info['동']!='소계']
seoul_info['full_name'] = '서울특별시'+' '+seoul_info['자치구']+' '+seoul_info['동']
seoul_info['full_name'] = seoul_info['full_name'].apply(lambda x: x.replace('.','·'))
seoul_info['인구'] = seoul_info['인구'].apply(lambda x: int(''.join(x.split(','))))

fig = px.choropleth_mapbox(seoul_info,
                           geojson=seoul_geo,
                           locations='full_name',
                           color='인구',
                           color_continuous_scale='viridis', featureidkey = 'properties.adm_nm',
                           mapbox_style='carto-positron',
                           zoom=9.5,
                           center = {"lat": 37.563383, "lon": 126.996039},
                           opacity=0.5,
                          )

fig
```
![](https://github.com/raqoon886/Local_HangJeongDong/blob/master/seoul.png?raw=true)
