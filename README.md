# SimpleBank
#spring boot #restapi


```SQL

-- 1. 데이터베이스 생성
 create database SB;
-- 2. 계정생성
  -- create user msa2022;
-- 3. 테이블 삭제 
drop table sb.CIA1000;
drop table sb.ACA0100;
drop table sb.RPD0100;  
drop table sb.RPA0100;
drop table sb.RPB1000;
drop table sb.RPD1000;
drop table sb.RPD1010;

-- 4. 테이블생성

/* 고객기본- 회원가입할 때 생성- 정보계*/
CREATE TABLE SB.CIA1000(
  CUST_ID       VARCHAR(50)   NOT NULL comment '고객ID'
, CUST_NM       VARCHAR(50)            comment '고객명'
, HPNO          VARCHAR(11)            comment '휴대전화번호'
, EMAIL         VARCHAR(50)            comment '이메일주소'
, TR_ID         VARCHAR(8)             comment '거래ID'
, OPRT_TR_DTM   VARCHAR(16)            comment '조작거래일시'
, PRIMARY KEY (CUST_ID)
);

/*계좌번호 채번, SEQL를 사용*/
CREATE TABLE SB.ACA0100(
  ACNO          INT(10)        NOT NULL AUTO_INCREMENT PRIMARY KEY
, CUST_ID       VARCHAR(50)   NOT NULL comment '고객ID'
, TR_ID         VARCHAR(8)             comment '거래ID'
, OPRT_TR_DTM   VARCHAR(16)            comment '조작거래일시'
);

/*1.채번 - 계좌개설시 생성, 거래발생시 갱신*/
CREATE TABLE SB.RPD0100(
  ACNO         VARCHAR(11)   NOT NULL comment '계좌번호'
, LAST_TR_NO    INT(8)       NOT NULL comment '최종거래번호'
, LAST_TR_DT    VARCHAR(8)   NOT NULL comment '최종거래일자'
, TR_ID         VARCHAR(8)            comment '거래ID'
, OPRT_TR_DTM  VARCHAR(16)            comment '조작거래일시'
, PRIMARY KEY (ACNO)
);


/*2.적요코드 - 공통코드*/
CREATE TABLE SB.RPA0100(
  SYNS_CD       VARCHAR(3) NOT NULL comment '적요코드'
, SYNS_NM       VARCHAR(20)         comment '적요명'
, TR_TP_DCD     VARCHAR(2)          comment '거래유형구분코드'
, CNCL_SYNS_NM  VARCHAR(20)         comment '취소적요명'
, TR_ID         VARCHAR(8)          comment '거래ID'
, OPRT_TR_DTM   VARCHAR(16)         comment '조작거래일시'
, PRIMARY KEY (SYNS_CD)
);

/*3.예수금잔고 - 계좌개설시 생성, 거래발생시 갱신*/
CREATE TABLE SB.RPB1000(
  ACNO          VARCHAR(11)  NOT NULL comment '계좌번호'
, DACA          BIGINT(18)            comment '예수금'
, CUST_ID       VARCHAR(50)           comment '고객ID'
, TR_ID         VARCHAR(8)            comment '거래ID'
, OPRT_TR_DTM   VARCHAR(16)           comment '조작거래일시'
, PRIMARY KEY  (ACNO)
);

/*4.거래내역 - 거래발생시 갱신*/
CREATE TABLE SB.RPD1000(
  TR_DT        VARCHAR(8) NOT NULL  comment '거래일자'
, ACNO         VARCHAR(11) NOT NULL comment '계좌번호'
, TR_NO        INT(8) NOT NULL      comment '거래번호'
, SYNS_CD      VARCHAR(3)           comment '적요코드'
, TR_TP_DCD    VARCHAR(2)           comment '거래유형구분코드'
, TR_AMT       BIGINT(18)           comment '거래금액'
, BF_DACA      BIGINT(18)           comment '이전예수금'
, AF_DACA      BIGINT(18)           comment '이후예수금'
, CNCL_YN      VARCHAR(1) NOT NULL  comment '취소여부'
, STRT_TR_NO   INT                  comment '시작거래번호'
, ORGN_TR_NO   INT                  comment '원거래번호'
, CLNT_NM      VARCHAR(50)          comment '의뢰인명'
, REAL_TR_DTM  DATE                 comment '실제거래일시'
, TR_ID         VARCHAR(8)          comment '거래ID'
, OPRT_TR_DTM  VARCHAR(16)          comment '조작거래일시'
, PRIMARY KEY (TR_DT,ACNO,TR_NO)
);

/*5.연계거래내역 -  당행/타행 거래발생시 갱신*/
CREATE TABLE SB.RPD1010(
  TR_DT            VARCHAR(11) NOT NULL comment '거래일자'
, ACNO             VARCHAR(11) NOT NULL comment '계좌번호'
, TR_NO            INT(8)     NOT NULL  comment '거래번호'
, REL_TR_DT        VARCHAR(8)           comment '연계거래일자'
, OPNT_ACNO        VARCHAR(11)          comment '상대계좌번호'
, OPNT_TR_NO       INT(8)               comment '상대거래번호'
, REL_COM_ACTNO    VARCHAR(20)          comment '연계기관계좌번호'
, OPNT_ACT_NM      VARCHAR(50)          comment '상대계좌명'
, REL_COM_CD       VARCHAR(10)          comment '연계기관코드'
, TR_ID            VARCHAR(8)           comment '거래ID'
, OPRT_TR_DTM      VARCHAR(16)          comment '조작거래일시'
, PRIMARY KEY (TR_DT,ACNO,TR_NO)
);

INSERT INTO SB.RPA0100 ( SYNS_CD, SYNS_NM , TR_TP_DCD , CNCL_SYNS_NM , OPRT_TR_DTM) VALUES ('001', '입금', '01', '입금취소', DATE_FORMAT(SYSDATE(),'%Y%m%d%H%i%s'));
INSERT INTO SB.RPA0100 ( SYNS_CD, SYNS_NM , TR_TP_DCD , CNCL_SYNS_NM , OPRT_TR_DTM) VALUES ('002', '출금', '02', '출금취소', DATE_FORMAT(SYSDATE(),'%Y%m%d%H%i%s'));
INSERT INTO SB.RPA0100 ( SYNS_CD, SYNS_NM , TR_TP_DCD , CNCL_SYNS_NM , OPRT_TR_DTM) VALUES ('003', '대체입금', '01', '대체입금취소', DATE_FORMAT(SYSDATE(),'%Y%m%d%H%i%s'));
INSERT INTO SB.RPA0100 ( SYNS_CD, SYNS_NM , TR_TP_DCD , CNCL_SYNS_NM , OPRT_TR_DTM) VALUES ('004', '대체출금', '02', '대체출금취소', DATE_FORMAT(SYSDATE(),'%Y%m%d%H%i%s'));

/*데이터베이스 사용*/
USE SB;
```
