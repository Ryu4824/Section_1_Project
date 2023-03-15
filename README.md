# 📝 Section_1_Project
## 새롭게 알게된 코드 정리
index_col=0으로 0번째 컬럼을 인덱스로 사용하겠다</br>
`pd.read_csv('https://ds-lecture-data.s3.ap-northeast-2.amazonaws.com/datasets/vgames2.csv',index_col=0)`</br>

keep=False 중복된 항목도 같이 표시</br>
`df_copy[df_copy.duplicated(keep=False)]`</br>

.to_frame() 데이터프레임 형식으로 표시</br>
`df_copy.isnull().sum().to_frame()`</br>

시각화 색상을 Paired로 전부 세팅</br>
`sns.set_palette('Paired')`</br>

'_'는 변수값을 필요로 하지 않을 시 사용 / 0.005를 12번 explode에 넣는다</br>
`explode = [ 0.005 for _ in range(12)]`</br>

범례의 위치와 범례 컬럼의 갯수 지정도 가능</br>
`plt.legend(loc='upper center', bbox_to_anchor=(0.5, 1.1), ncol=4)`</br>

기존의 y축 라벨은 세로의 형태지만 rotation=0으로 가로로 가능</br>
`plt.ylabel('Sales',rotation=0, ha='right',fontsize=14)`</br>

.nlargest(4)큰 값을 가진 4개만 선택 가능</br>
#Platform_sales에서 Sales 합계를 구하고 내림차순으로 정렬하여 상위 4개 Platform 인덱스를 리스트 형태로 대입</br>
`top_platforms = Platform_sales.groupby('Platform')['Sales'].sum().nlargest(4).index.tolist()`</br>

범례의 인덱스로 이름 변경 가능
```
legend = plt.legend()
legend.texts[0].set_text('Total_PS3')
```

Year 컬럼을 인덱스로 사용하고 %Y(년도만) format하고 데이터 타입을 datetime으로 바꿈
```
total_genre_sales = total_genre_sales.set_index('Year')
total_genre_sales.index = pd.to_datetime(total_genre_sales.index, format='%Y')
```
---
## 알고 있었지만 잊은 코드 정리
.isin(), .unique(), .sort_values(ascending = False), .first_valid_index(), pd.melt(data,id_vars,var_name,value_name)

---
## 시각화
### pie (원그래프)
```
explode = [ 0.005 for _ in range(12)]
explode[0] = 0.2

wedgeprops={'width': 0.7, 'edgecolor': 'w', 'linewidth': 3}
plt.pie(na_genre_sales, labels=na_genre_sales.index, autopct='%.1f%%', startangle=90,counterclock=False,explode=explode,wedgeprops=wedgeprops)
```
explode(중심에서 벗어난 정도),wedgeprops(부채꼴 영역의 스타일을 설정)

### barplot (막대그래프 *안겹쳐짐)
`sns.barplot(data = Genre_sales,x='Region',y='Sales',hue='Genre',estimator=sum,ci=0)`
estimator(y축의 값을 계산하는 방법), ci(신뢰구간)
### bar (막대그래프 *겹쳐짐)
```
df_pivot = pd.pivot_table(total_genre_sales, index = pd.Grouper(freq='5Y'), columns = 'Genre', values='Sales',aggfunc='sum')
xname = df_pivot.plot.bar(stacked=True, figsize=(10,7))
```
pd.pivot_table() 함수는 total_genre_sales 데이터프레임에서 index를 pd.Grouper(freq='5Y')로 설정하여,</br>
연도별 데이터를 5년 단위로 그룹화한 후, Genre 열을 컬럼으로 하고 Sales 열을 값으로 하는 피벗 테이블을 생성합니다.</br>

df_pivot.plot.bar()는 df_pivot을 막대 그래프로 시각화합니다. stacked=True로 설정하여,</br>
각 장르의 출고량을 쌓아서 그래프로 나타냅니다. figsize=(10,7)로 설정하여 그래프의 크기를 조절합니다.</br>
```
name = []
for i in range(len(df_pivot)):
  name.append(str(df_pivot.index[i])[:4])

xname.set_xticklabels(name,rotation=0)
```
name 리스트는 df_pivot의 인덱스의 연도를 4자리 숫자로 변환하여 저장합니다.</br>
str(df_pivot.index[i])를 통해 df_pivot의 인덱스를 문자열로 변환한 후, [:4]를 통해 4자리 숫자로 슬라이싱합니다.
### plot (선그래프)
`plt.plot(year_sales1.index, year_sales1, marker='o', label='1980~2003',color='red')`</br>
marker='o'는 데이터 포인트의 마커 스타일을 지정합니다. 'o'는 원 모양의 마커를 지정합니다.
