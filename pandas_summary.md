# BOAZ 지원용 Pandas 문법 정리
import pandas as pd
import numpy as np

# 1. 데이터 조회
df[df['age'] == 25]  # age가 25인 행만 선택
df.sort_values(by='score', ascending=False)  # score의 값을 내림차순(ascending이 True면 오름차순,False면 내림차순)
df.describe()[['age','score']]  # 특정 컬럼 통계요약(작성한 코드처럼 여러열을 뽑아낼수있음)

# 2. 결측치 처리
df.isnull().sum()#NULL의 갯수
df['sales'].fillna(0, inplace=True)#sales의 na값을 0으로 바꾸고, inplace=True는 데이터셋의 바로 적용
df['region'].fillna('미정', inplace=True)#region의 na값을 미정으로 바꿈
len(df.dropna())#na값이없는 결측치가 없는 행의 갯수

# 3. 그룹화
df.groupby('지점')['매출'].sum()#지점별 매출의 합
df.groupby('제품')['매출'].mean().reset_index()#제품별 매출의 평균, .reset_index()있으면 그룹형태로 되어있는걸 데이터프레임으로 다시 바꿔줌
df.loc[df.groupby('제품')['매출'].idxmax()]#제품별 매출의 최댓값 ,idxmax는 최댓값의 인덱스를 가져오고 loc로 실제 행을 가져옴

# 4. 병합
merged = pd.merge(sales, target, on='지점', how='outer')#sql과 비슷함, sales와 target를 기본키 지점을 통해 합집합
merged['달성률'] = merged['매출'] / merged['목표'] * 100
merged['목표'] = merged['목표'].fillna('목표없음')#목표에서 na값이 있다면 목표없음으로 변환

# 5. 조건문 처리
df['결과'] = np.where(df['매출'] >= 100, '성공', '실패')#sql의 where(조건) 조건문이 나오고 나선 충족한다면 성공, 아니라면 실패
df[df['결과'] == '성공']

# 6. 날짜 처리
df['주문일자'] = pd.to_datetime(df['주문일자'])#주문일자(문자열)를 datetime으로 변환
df['요일'] = df['주문일자'].dt.day_name(locale='ko_KR')#요일이름을 한국말로 추출
df[df['요일'] == '일요일']

# 7. 중복 제거 및 정렬
df.drop_duplicates()#중복행 제거하고 남은 행만 남김
df.sort_values(by='매출', ascending=False)

# 8. 피벗 테이블
pd.pivot_table(df, values='매출', index='지점', columns='제품', aggfunc='sum')#지점과 제품의 매출합을 테이블로

# 9. apply 함수
df['결과'] = df['매출'].apply(lambda x: '성공' if x >= 100 else '실패')#각각의 행에 조건을 걸어줌(for문을 쓰지않아도 알아서 반복)

# 10. 데이터프레임 결합
df_total = pd.concat([df_jan, df_feb], axis=0)#1월과 2월 리스트를 미리 설정하면 이렇게 결합 가능 axis=0이면 세로로 결합 1이면 가로로

