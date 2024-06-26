# 문제 작성 및 응시 프로그램 만들기

## 사용 툴
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=Python&logoColor=white"> <img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white"> <img src="https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white"> <img src="https://img.shields.io/badge/docker-2496ED?style=for-the-badge&logo=docker&logoColor=white">


<h2>파이썬 파일 개요</h2>

|구분|파일 이름|설명|담당자|링크|
|--|--|--|--|--|
|0|questionnaires_main|메인|-|[questionnaires_main.py](docs/questionnaires_main.py)|
|1|question_setting|문제 입력|노주현|[question_setting.py](docs/question_setting.py)|
|2|question_outputs|시험 응시|김유진| [question_outputs.py](docs/question_outputs.py)|
|3|user_scoring|응시자별 채점 결과|노주현| [question_ouuser_scoringtputs.py](docs/user_scoring.py) |

<br>

## 역할분담
###  문제 입력 : 주현
```
- 문제와 관련된 정보(문제, 답항, 점수, 정답)을 DB에 저장
```

### 시험 응시 : 유진
```
- 문제 리스트 출력
- 사용자의 답 입력
- 사용자 정보와 사용자가 입력한 답 DB에 저장
- 사용자가 입력한 답안지 리스트 출력
```

### 응시자별 채점 결과 : 주현
```
- 응시자별 채점 결과 출력
- 과목 평균 점수 출력
```
<br>

## 접근 방식
### 테이블 구성
- 문제지, 답항, 유저정보, 유저가 입력한 답항
### 유저별 점수 계산 방식 
- 답항별로 점수를 지정
- 오답일 경우 0점, 정답일 경우 해당 문제의 점수를 부여

![image](https://github.com/nohjuhyeon/toy_ERDs/assets/151099184/b4dce6f7-78e4-410c-b26b-f64e64d73cc2)

### ERD 다이어그램
![TOY_ERDs_diagram](https://github.com/nohjuhyeon/toy_ERDs/assets/151099474/89dec672-c1a5-47a6-a56c-618a9b0c16ce)

<br>

## 동작 순서도
### 1. 문제 입력
    1. 만들 문제 수와 문항 수를 입력
    2. 문제 입력
        - 만약에 중복 문제가 있다면 다시 입력
    3. 입력 받은 문제와 해당 문제의 question_id를 question_table에 저장
    4. 문항들을 입력 받아 리스트 작성
        - 리스트에 있는 문항을 또 입력할 경우 다시 입력
    5. 정답 입력
        - 정답이 숫자가 아니거나 문항 수보다 클 경우 다시 입력
    6. 점수 입력
        - 점수가 숫자가 아닐 경우 다시 입력
        - 점수를 score_table에서 확인하여 똑같은 점수가 있을 경우, 해당 점수의 score_id 가져오기
        - 만약에 없을 경우 새로운 score_id 생성
    7. choice_answer_table에 문항별 choice_id와 question_id, score_id, 문항, 문항 번호 저장
    8. 만들 문제 수를 충족시켰을 경우 종료.

### 2. 시험 응시
    1. 응시자 이름 입력
    2. 응시자 이름과 id user_info_table에 저장
    3. 문제와 문제에 해당하는 문항들을 DB에서 가져와서 출력
    4. 응시자가 답 입력
        - 만약에 답이 숫자가 아니거나 문항 수보다 클 경우 다시 입력
    5. 응시자가 입력한 답 user_answer_table에 저장
    6. 모든 문제를 풀었을 경우, 응시자가 입력한 답안지 출력
    7. 다음 응시자가 있는지 확인
        - 다음 응시자가 있을 경우 다시 실행
        - 다음 응시자가 없을 경우 종료.

### 3. 응시자별 점수와 전체 응시자의 평균 출력
    1. DB에서 응시자 이름과 응시자들의 문제별 점수 가져오기
    2. 응시자별 채점 결과 출력
    3. 전체 응시자의 평균 출력

<br>

## 프로젝트 결과

### 느낀점
```
- 노주현
MONGO와 달리 RDB에서는 TABLE간의 동일한 컬럼이 있을 경우, 연결성을 부여할 수 있다.
이 연결성을 활용하여 데이터 베이스를 좀 더 편리하게 사용할 수 있었다.

- 김유진
```
### 프로젝트 진행 중 Episode
```
choice_id가 9보다 10이 더 우선적으로 배치되어  문제를 출력할 때 3번 문항의 순서가 2-3-4-1로 나옴
➜ order by를 통해 choice_number 순으로 정렬될 수 있도록 설정
```

![image](https://github.com/nohjuhyeon/toy_ERDs/assets/151099184/1ccb896d-a328-4512-a2d8-4896b3e01bb1)

### 프로그램 동작 영상 : https://www.youtube.com/watch?v=QAIcF4GyOG4

<br>

## 데이터 베이스 구성 예시

### 예상 DB 구성도 
![TOY_DATABASE구성_YJH](https://github.com/nohjuhyeon/toy_ERDs/assets/151099474/5069d6c9-e9f7-43b6-b000-405e1ecff69f)

## 예상 DB 구성 파일
|구분|테이블 이름|구성 컬럼|링크|
|--|--|--|--|
|1|QUESTION_TABLE|QUESTION_ID, QUESTION|[QUESTION_TABLE](DATABASE/TOY_ERDs_QUESTION_TABLE.csv)|
|2|SCORE_TABLE|SCORE_ID, SCORE|[SCORE_TABLE](DATABASE/TOY_ERDs_SCORE_TABLE.csv)|
|3|CHOICE_ANSWER_TABLE|CHOICE_ID, QUESTION_ID, SCORE_ID, CHOICE, CHOICE_NUMBER|[CHOICE_ANSWER_TABLE](DATABASE/TOY_ERDs_CHOICE_ANSWER_TABLE.csv)|
|4|USER_INFO_TABLE|USER_ID, USER_NAME|[USER_INFO_TABLE](DATABASE/TOY_ERDs_USER_INFO_TABLE.csv)|
|5|USER_ANSWER_TABLE|USER_ANSWER_ID, USER_ID, CHOICE_ID|[USER_ANSWER_TABLE](DATABASE/TOY_ERDs_USER_ANSWER_TABLE.csv)|

<br>

## 기본 세팅 정보
### docker 세팅하기
DOCKER FILE : [DOCKER FILE](dockers/)

### DOCKER 설치 코드
```
# --project-name is docker container name
Docker installation command copied
~$ docker-compose --project-name python__mysql up -d --build
Docker reinstallation command copied
~$ docker-compose --project-name python__mysql build --no-cache
~$ docker-compose --project-name python__mysql up -d
```

#### DATABASE 정보
+ user='cocolabhub',
+ password='cocolabhub',
+ db='python_mysql'


