<p align="center">
    <img width="200px;" src="https://raw.githubusercontent.com/woowacourse/atdd-subway-admin-frontend/master/images/main_logo.png"/>
</p>
<p align="center">
  <img alt="npm" src="https://img.shields.io/badge/npm-%3E%3D%205.5.0-blue">
  <img alt="node" src="https://img.shields.io/badge/node-%3E%3D%209.3.0-blue">
  <a href="https://edu.nextstep.camp/c/R89PYi5H" alt="nextstep atdd">
    <img alt="Website" src="https://img.shields.io/website?url=https%3A%2F%2Fedu.nextstep.camp%2Fc%2FR89PYi5H">
  </a>
  <img alt="GitHub" src="https://img.shields.io/github/license/next-step/atdd-subway-service">
</p>

<br>

# 인프라공방 샘플 서비스 - 지하철 노선도

<br>

## 🚀 Getting Started

### Install
#### npm 설치
```
cd frontend
npm install
```
> `frontend` 디렉토리에서 수행해야 합니다.

### Usage
#### webpack server 구동
```
npm run dev
```
#### application 구동
```
./gradlew clean build
```
<br>

## 미션

* 미션 진행 후에 아래 질문의 답을 작성하여 PR을 보내주세요.


### 1단계 - 쿼리 최적화

1. 인덱스 설정을 추가하지 않고 아래 요구사항에 대해 200ms 이하(M1의 경우 2s)로 반환하도록 쿼리를 작성하세요.

- 활동중인(Active) 부서의 현재 부서관리자 중 연봉 상위 5위안에 드는 사람들이 최근에 각 지역별로 언제 퇴실했는지 조회해보세요. (사원번호, 이름, 연봉, 직급명, 지역, 입출입구분, 입출입시간)


```sql
select 
	t1.id as 사원번호, 
	t1.last_name as 이름, 
	t1.annual_income as 연봉, 
	t1.position_name as 직급명, 
	r.time as 입출입시간, 
	r.region as 지역, 
	r.record_symbol as 입출입구분
from (
	select e.id, e.last_name, p.position_name, s.annual_income
	from tuning.department d 
		inner join tuning.manager m on m.department_id = d.id and m.end_date = '9999-01-01'
		inner join tuning.employee e on e.id = m.employee_id
		inner join tuning.position p on p.id = e.id and p.position_name = 'manager' and p.end_date = '9999-01-01'
		inner join tuning.salary s on s.id = e.id and s.end_date = '9999-01-01'
	where d.note = 'active'
	order by s.annual_income desc
	limit 5
) t1 inner join tuning.record r on r.employee_id = t1.id and r.record_symbol = 'O'
order by t1.id;
```

![sql_explain](/images/sql_explain.png)


![sql_visual_explain](/images/sql_visual_explain.png)


---

### 2단계 - 인덱스 설계

1. 인덱스 적용해보기 실습을 진행해본 과정을 공유해주세요

---

### 추가 미션

1. 페이징 쿼리를 적용한 API endpoint를 알려주세요
