# prjdect_db
-- root 계정 
show databases; -- 전체  DB

drop database if exists shopdb;  -- DB가 존재하면 삭제

create database shopdb;  -- DB생성

use shopdb;  --  shopdb 사용

show tables;  -- DB table조회

CREATE TABLE `shopdb`.`member_tb` (
  `member_id` INT NOT NULL AUTO_INCREMENT,
  `member_name` VARCHAR(45) NOT NULL,
  `member_addr` VARCHAR(100) NULL,
  PRIMARY KEY (`member_id`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE `shopdb`.`project_tb` (
  `project_id` INT NOT NULL AUTO_INCREMENT, -- 제품아이디
  `project_name` VARCHAR(45) NOT NULL, -- 제품명
  `creat_time` DATETIME NULL,  		-- 등록시간
  `company` VARCHAR(45) NOT NULL,	 -- 제조사
  `price` INT NOT NULL,				 -- 가격
  `amount` INT NULL, 				 -- 수량
  PRIMARY KEY (`project_id`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

show tables;  -- DB table조회

desc member_tb;
desc project_tb;

select * from member_tb;
select * from project_tb;

alter table project_tb rename product_tb;  --  테이블 이름 변경

desc product_tb;

alter table product_tb change project_id product_id INT NOT NULL AUTO_INCREMENT;

-- ALTER TABLE 테이블이름 ADD 필드이름 필드타입    -- 새칼럼 추가
-- ALTER TABLE 테이블이름 modify 필드명 새필드타입  -- 타입 변경
-- ALTER TABLE 테이블이름 DROP 필드이름          -- 칼럼 삭제
-- ALTER TABLE 테이블이름 change 필드명 새필드명 새필드타입 --  칼럼명 변경
-- ALTER TABLE 테이블이름 add constraint 기본키명 primary key (필드값)  -- 제약조건 추가
-- ALTER TABLE 이전테이블명 RENAME 새테이블명;    --  테이블명 변경



-- 테이블 생성시 외래키 설정  
-- 외래키가 있는 테이블이 주테이블 
-- CREATE TABLE tb1 (
--   컬럼명2,  
--   FOREIGN KEY(컬럼명2) REFERENCES tb2(칼럼명1) -- 다른테이블 외래키 참조
-- );
-- CREATE TABLE tb2 (
--   컬럼명1,
--   PRIMARY KEY(컬럼명1)
-- );

-- 테이블 생성 후 외래키 설정  
-- ALTER table 테이블이름 add FOREIGN KEY(칼럼명) REFERENCES 참조테이블(참조컬럼);
-- ALTER table tb1 add FOREIGN KEY(칼럼명2) REFERENCES tb2(칼럼명1);
-- 외래키 삭제
-- ALTER TABLE 테이블이름 drop foreign key 외래키명


desc member_tb;
desc product_tb;

-- ALTER TABLE 테이블이름 ADD 필드이름 필드타입    -- 새칼럼 추가

alter table product_tb add member_id int;  -- 외래키 

-- ALTER table 테이블이름 add FOREIGN KEY(칼럼명) REFERENCES 참조테이블(참조컬럼);
alter table product_tb add foreign key(member_id) references member_tb(member_id)  ;

-- ALTER TABLE 테이블이름 drop foreign key 외래키명

alter table product_tb drop foreign key member_id ;

-- CREATE TABLE `member_tb` (
--   `member_id` int NOT NULL AUTO_INCREMENT,
--   `member_name` varchar(45) NOT NULL,
--   `member_addr` varchar(100) DEFAULT NULL,
--   PRIMARY KEY (`member_id`)
-- ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3

-- CREATE TABLE `product_tb` (
--   `product_id` int NOT NULL AUTO_INCREMENT,
--   `product_name` varchar(45) NOT NULL,
--   `create_time` datetime DEFAULT NULL,
--   `company` varchar(45) NOT NULL,
--   `price` int NOT NULL,
--   `amount` int NOT NULL,
--   `member_id` int DEFAULT NULL,
--   PRIMARY KEY (`product_id`),
--   KEY `member_id` (`member_id`),
--   CONSTRAINT `member_id` FOREIGN KEY (`member_id`) REFERENCES `member_tb` (`member_id`)
-- ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3;

insert into member_tb(member_name,member_addr) values('m1','서울');
insert into member_tb(member_name,member_addr) values('m2','서울');
insert into member_tb(member_name,member_addr) values('m3','부산');
insert into member_tb(member_name,member_addr) values('m4','춘천');
insert into member_tb(member_name,member_addr) values('m5','대전');

select * from member_tb;

desc product_tb;
select * from product_tb;
-- 상품 등록 , 등록자 O
INSERT INTO product_tb (product_name, create_time, company, price, amount,member_id) 
VALUES ('A상품', now(), 'A회사', 1000, 1000 ,1);
INSERT INTO product_tb (product_name, create_time, company, price, amount,member_id) 
VALUES ('B상품', now(), 'B회사', 1500, 10000 ,1);
INSERT INTO product_tb (product_name, create_time, company, price, amount,member_id) 
VALUES ('C상품', now(), 'C회사', 5500, 10000 ,2);
INSERT INTO product_tb (product_name, create_time, company, price, amount,member_id) 
VALUES ('D상품', now(), 'D회사', 5500, 10000 ,2);
INSERT INTO product_tb (product_name, create_time, company, price, amount,member_id) 
VALUES ('E상품', now(), 'E회사', 5500, 10000 ,3);
-- 상품 등록 , 등록자 X
INSERT INTO product_tb (product_name, create_time, company, price, amount) 
VALUES ('Cart1상품', now(), 'E회사', 5500, 10000 );
INSERT INTO product_tb (product_name, create_time, company, price, amount) 
VALUES ('Cart2상품', now(), 'C2회사', 5500, 10000 );
INSERT INTO product_tb (product_name, create_time, company, price, amount) 
VALUES ('Cart3상품', now(), 'C3회사', 5500, 10000 );

-- 조회  findAll()  List
select * from product_tb;
-- 상품 등록 아이디 1   findByMemberId -> Optional
select * from product_tb where member_id=1;

select * from member_tb ;
-- 이름이  m1이고 주소가 서울인 member 레코드 조회
-- findByMemberNameAndMemberAddr(String memberName, String member_Addr)
select * from member_tb where member_name='m1' and member_addr='서울';

-- 이름이  m1이거나 주소가 서울인 member 레코드 조회
-- findByMemberNameOrMemberAddr(String memberName, String member_Addr)
select * from member_tb where member_name='m1' or member_addr='서울';

-- member_id 1이상 3이하 member 레코드 조회
select * from member_tb where member_id>=1 and member_id<=3;
select * from member_tb where member_id between 1 and 3;   --   1~3

-- member_name 에 'm1', 'm2'가 포함되어 있는 레코드 
select * from member_tb where member_name in('m1','m2');

-- 검색  like
-- member_name 'm'으로 시작하는 
-- findByMemberNameLike(String memberName)
select * from member_tb where member_name like 'm%';
-- member_name '2'으로 끝나는 
select * from member_tb where member_name like '%2';
-- member_name 'm'으로 포함되어 있는 
-- findByMemberNameContaining(String memberName)
select * from member_tb where member_name like '%m%';

-- 서브쿼리 -> 쿼리 안에 또다른 쿼리 서브쿼리 
select * from member_tb where  member_id>=3 ; -- 아이디가 3 
select * 
from member_tb 
where member_id>= ( select member_id from member_tb where member_name='m3');

select member_id from member_tb where member_name='m3';

-- order by  정렬 ASC, DESC 
select * from product_tb;

-- product_id 내림 -> 최신, 큰 -> 작은 z->a 
select * from product_tb order by product_id desc;
select * from product_tb order by price desc;
select * from product_tb order by create_time desc;

-- limit  ~ 까지 조회    block, page ,  size,
select * from product_tb limit 3;  -- 3
select * from product_tb limit 0 ,2; -- 0~ 2개
select * from product_tb limit 3 ,2; -- 3~ 2개
select * from product_tb limit 5 ,2; -- 5~ 2개
select * from product_tb limit 7 ,2; -- 7~ 2개

-- group by
-- COUNT(필드명) --NULL 값이 아닌 레코드 수
-- SUM(필드명) - 필드명의 합계
-- AVG(필드명) - 각각의 그룹 안에서 필드명의 평균값
-- MAX(필드명) - 최대값
-- MIN(필드명) - 최소값
-- Math 함수 -> Math.min(), Math.max()
-- MOD (분자, 분모) - 분자를 분모로 나눈 나머지
-- 소수
-- CEILING(숫자) - 소수점 올림   ceil (올림)
-- FLOOR(숫자) - 소수점 내림   floor(내림)
-- ROUND(숫자,자릿수) - 숫자를 소수점 이하 자릿수에서 반올림
-- 현재 날짜와 시간 출력
-- NOW()

select * from member_tb;
select member_id from member_tb ;

select * from product_tb;
select member_id from product_tb;
-- member_id 그룹 설정
select member_id from product_tb group by member_id;
-- 등록자별 상품 
select * from product_tb group by member_id;
-- 집합 함수
-- member_id 별 상품 등록 갯수 총합계
select count(*) 갯수 , member_id from product_tb group by member_id;
select count(*) 갯수, sum(price) 합계  , member_id from product_tb group by member_id;

-- group by   그룹  having 조건
select count(*) 갯수, sum(price) 합계  , member_id from product_tb 
group by member_id 
having  member_id is not null ;  -- member_id가 null 아닌 

select count(*) 갯수, sum(price) 합계  , member_id   -- 3
from product_tb   -- 1
group by member_id -- 2
having(합계>5000) ;  -- 합계 5000이상

-- auto_increment 초기화 100~, 조정 가능
alter table member_tb auto_increment=100;

select * from member_tb;

insert into member_tb(member_name,member_addr) values('m6','서울');



-- 조인
-- 두 개 이상의 테이블을 묶어서 하나의 결과 집합으로 만들어 내는 것
-- 서로 다른 테이블에서 데이터를 가져올 때 사용하는 것이 조인(Join)
-- INNER JOIN(내부 조인)
-- INNER JOIN은 양쪽 테이블에 모두 내용이 있는 경우에만 결과가 검색
-- (INNER) JOIN
-- 조인하는 테이블의 ON 절의 조건이 일치하는 결과만 출력
-- 표준 SQL과는 달리 MySQL에서는 JOIN, INNER JOIN, CROSS JOIN이 모두 같은 의미
-- SELECT <열 목록>
-- FROM <기준 테이블>
--     INNER JOIN<참조할 테이블>
--     ON <조인 조건>
-- [WHERE 검색조건]

-- join

-- 상품 테이블을 등록한 상품목록, 등록인 목록 모두 출력
select *   -- 두 테이블 전부 출력
from product_tb as t inner join member_tb as m  -- 상품,회원 inner join
on t.member_id=m.member_id ;  -- 상품등록 아이디가 일치(상품등록한 회원이 있는 목록)
-- 상품 등록 이력 회원 목록
select m.*
from product_tb as t inner join member_tb as m 
on t.member_id=m.member_id ;

select * from member_tb;

-- 상품등록인이 있는  상품  목록
select t.*
from product_tb as t inner join member_tb as m 
on t.member_id=m.member_id ;

-- member_id 1인 사람이 등록한 상품 목록을 출력
select t.*
from product_tb as t inner join member_tb as m 
on t.member_id=m.member_id 
where t.member_id=1;


-- select t.member_id, t.product_name
-- from product_tb as t inner join member_tb as m 
-- on t.member_id=m.member_id ;



![db1](https://user-images.githubusercontent.com/116870677/218368183-66946b5b-b328-4825-ba94-76b9974e49c6.png)


![db2](https://user-images.githubusercontent.com/116870677/218368274-a07f3190-9c77-4d72-a832-b9f329c9a3f6.png)




