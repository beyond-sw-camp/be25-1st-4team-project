<div align="center">
	
<img
    src="https://readme-typing-svg.demolab.com?font=Noto+Sans+KR&weight=900&size=56&duration=2200&pause=800&color=FFFFFF&center=true&vCenter=true&width=900&height=120&lines=IT%EC%9E%90%EC%82%B0%EC%9E%A5%EB%B6%80"
    alt="IT자산장부 타이핑"
  />
	
## 사내 IT 자산 대여·반납·검수·통합관리 프로세스 DB 설계

<img src="./이미지/로고_포스터/한문아이콘.png?v=2" width="320" alt="대표 아이콘" />
<br />
<sub><b>대표 아이콘</b></sub>

<br /><br />

<img src="./이미지/로고_포스터/DB설계포스터.png?v=4" width="620" alt="프로젝트 포스터" />
<br />
<sub><b>프로젝트 포스터</b></sub>

<br /><br />

<img src="https://img.shields.io/badge/DB%20Modeling-ERD%2FDDL%2FTrigger%2FProcedure-0f172a?style=for-the-badge" alt="DB Modeling" />
<img src="https://img.shields.io/badge/Asset%20Lifecycle-Assign%20%7C%20Return%20%7C%20Inspection-0f172a?style=for-the-badge" alt="Asset Lifecycle" />

</div>

---

## 👀목차
1. [👥팀원 소개](#팀원-소개)
2. [📚프로젝트 개요](#프로젝트-개요)
3. [🎯서비스 목표](#서비스-목표)
4. [📅WBS](#wbs)
5. [📄프로젝트 기획서](#프로젝트-기획서)
6. [🎬프로젝트 시나리오](#프로젝트-시나리오)
7. [📘요구사항 명세서](#요구사항-명세서)
8. [🧩유스케이스 다이어그램](#유스케이스-다이어그램)
9. [🧱데이터 설계](#데이터-설계)
10. [📊ERD](#erd)
11. [🗃️테이블 명세서](#테이블-명세서)
12. [💾SQL 산출물(DDL/프로시저·트리거)](#sql-산출물ddl프로시저트리거)
13. [🧪테스트 진행 과정](#테스트-진행-과정)
14. [🧭향후 확장 방향](#향후-확장-방향)
15. [📝회고록](#회고록)

---

<a id="팀원-소개"></a>
## 👥팀원 소개

<div align="center">

<table>
  <tr>
    <td align="center" valign="top">
      <img src="./이미지/동물%20이미지/siba.png" width="120" alt="신민수" /><br />
      <b>팀장:신민수</b><br />
      <a href="https://github.com/ZonezIpex">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

 <td align="center" valign="top">
      <img src="./이미지/동물%20이미지/raven.png" width="120" alt="김승욱" /><br />
      <b>김승욱</b><br />
      <a href="https://github.com/KIM-SEUNG-WOOK">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="./이미지/동물%20이미지/squirrel.png" width="120" alt="김지연" /><br />
      <b>김지연</b><br />
      <a href="https://github.com/wldusdus63">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="./이미지/동물%20이미지/rabbit.png" width="120" alt="모희주" /><br />
      <b>모희주</b><br />
      <a href="https://github.com/heejudy">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

   <td align="center" valign="top">
      <img src="./이미지/동물%20이미지/mouse.png" width="120" alt="박지인" /><br />
      <b>박지인</b><br />
      <a href="https://github.com/mondayziin">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="./이미지/동물%20이미지/cat.png" width="120" alt="윤준상" /><br />
      <b>윤준상</b><br />
      <a href="https://github.com/wnstkd704">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>
  </tr>
</table>

</div>

---

<a id="프로젝트-개요"></a>
## 📚프로젝트 개요

사내 IT 자산(노트북, 모니터, 태블릿, 모바일 단말 등)은 프로젝트 투입/부서 이동/외주·협력사 투입 등으로 배정과 회수가 반복됩니다.  
하지만 대여·반납 기록이 메신저/엑셀/담당자 개인 기록으로 분산되어 다음을 한 번에 추적하기 어렵습니다.

- 누가 어떤 자산을 언제부터 사용(할당/출고) 중인지
- 반납 검수 결과(수리 필요/불필요)와 증빙(사진/파일/메모)
- 반납예정일(due_date) 경과 시 미반납 대상 및 반납관리(요구/경고/제재) 발생 이력

본 프로젝트는 **자산 등록 → 할당/출고 → 반납요청 → 회수 → 검수(수리 여부 판정) → 가용 복귀/수리 처리** 흐름을 DB 모델로 고정하고,  
**due_date 경과를 트리거로 반납관리 이벤트를 생성**해 요구/경고/제재를 시스템 로그로 남겨 누락과 지연을 줄입니다.(외부 메신저/메일 실연동 제외)

> **목표(구조적 오류 예방)**
- 상태 역전 방지: 검수 없이 AVAILABLE 복귀 금지, 수리 이력 없이 UNDER_REPAIR 전환 금지
- 근거 누락 방지: 요구/경고 로그 없이 제재 상승 금지(정책 예외는 예외 플래그/근거 로그로 고정)

### 🧾참고자료(링크)

<details>
<summary><b>참고자료 펼치기 / 접기</b></summary>

- 플래텀(2025): IT 자산관리 솔루션 ‘셀리즈’, ITAM 기능 확장(QR 기반 대여/반납 등)  
  https://platum.kr/archives/252140  
  ![](./이미지/참고자료/플래텀.png)

- Sellease(셀리즈) 공식: QR코드 기반 임직원 자산 대여 프로세스(스캔 1회 대여/반납, 실시간 현황 등)  
  https://landing.sellease.io/ko/post/revolutionizing-asset-management-with-sellease-qr-code-based-employee-asset-rental  
  ![](./이미지/참고자료/셀리즈.png)

- IT 자산 관리 절차의 정의 및 구현(Definition and implementation of procedures for IT assets managing)  
  https://doi.org/10.17261/Pressacademia.2017.478  
  ![](./이미지/참고자료/논문.png)

</details>

---

<a id="서비스-목표"></a>
## 🎯서비스 목표

- 자산 라이프사이클: 등록 → 할당/출고 → 반납요청 → 회수/검수 → (수리) → 가용 복귀 흐름을 상태코드 전이로 강제
- 미반납 관리 이력화: due_date 경과를 트리거로 요구/경고/제재 이벤트 로그를 생성하고, 사용자 누적 스냅샷(overdue_count/warning_count/restriction_level)을 함께 관리
- 운영 리스크 감소: 분실/중복 지급/재고 불일치 및 검수 기준 불일치(담당자 주관 판단)를 이력·상태 전이 규칙으로 최소화
- 근거 기반 이력 추적 구조: 검수 증빙(사진/파일/메모), 정책값, 이벤트 발생 시점값을 로그에 고정 저장해 요구/경고 없이 제재 상승 등 근거 단절을 방지

---

<a id="wbs"></a>
## 📅WBS 
🔗 [WBS](https://docs.google.com/spreadsheets/d/1kv5N2GoCfgB2f_pzPkxqvyVfNcI-7SqO/edit?usp=sharing&ouid=102208872170708224187&rtpof=true&sd=true)

<div align="center">

![](./이미지/프로젝트_구성/WBS.png)

</div>
<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>MS Excel</kbd> <kbd>📌 Output</kbd> <kbd>WBS</kbd></sub>
</div>



---

<a id="프로젝트-기획서"></a>
## 📄프로젝트 기획서

🔗 [기획서](./파일/PDF파일/기획서.pdf)

<div align="center">
<img src="./이미지/프로젝트_구성/기획서이미지.png" width="460" alt="기획서" />
</div>

<br>
<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>PDF</kbd> <kbd>📌 Output</kbd> <kbd>Planning Doc</kbd></sub>
</div>

---

<a id="프로젝트-시나리오"></a>
## 🎬프로젝트 시나리오

🔗 [시나리오 다이어그램](https://drive.google.com/file/d/1k9W1oRKgEGQD95bliI3AYVrtp-0PbUSs/view?usp=sharing)

<div align="center">

![](./이미지/프로젝트_구성/프로젝트%20시나리오.png?v=2)

</div>

<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>Draw.io</kbd> <kbd>📌 Output</kbd> <kbd>Scenario</kbd></sub>
</div>



---

<a id="요구사항-명세서"></a>
## 📘요구사항 명세서

🔗 [요구사항 명세서](https://docs.google.com/spreadsheets/d/1qAGAu5frAmMS7LulqDKW_5eYSSz0MkBg/edit?usp=sharing&ouid=102208872170708224187&rtpof=true&sd=true)

<div align="center">

![](./이미지/프로젝트_구성/요구사항명세서.png)

</div>

<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>MS Excel</kbd> <kbd>📌 Output</kbd> <kbd>Requirements</kbd></sub>
</div>

---

<a id="유스케이스-다이어그램"></a>
## 🧩유스케이스 다이어그램

🔗 [유스케이스 다이어그램](https://drive.google.com/file/d/1nNf_P46JZsie_RNl2GgsCoZslGiGDDay/view?usp=sharing)

<div align="center">

![](./이미지/프로젝트_구성/유스케이스%20다이어그램.png?v=2)

</div>

<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>Draw.io</kbd> <kbd>📌 Output</kbd> <kbd>Use Case</kbd></sub>
</div>

---

<a id="데이터-설계"></a>
## 🧱데이터 설계

🔗 [데이터설계](./파일/PDF파일/데이터설계.pdf)

<div align="center">
<img src="./이미지/프로젝트_구성/데이터설계.png" width="460" alt="데이터설계" />
</div>

<br>
<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>PDF</kbd> <kbd>📌 Output</kbd> <kbd>Planning Doc</kbd></sub>
</div>

---

<a id="erd"></a>
## 📊ERD

🔗 [ERD](https://www.erdcloud.com/d/oqzg5Q52Naw23Rkyc)

<div align="center">

![](./이미지/프로젝트_구성/ERD.png?v=1)

</div>

<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>ERDCloud</kbd> <kbd>📌 Output</kbd> <kbd>ERD</kbd></sub>
</div>

---

<a id="테이블-명세서"></a>
## 🗃️테이블 명세서

🔗 [테이블 명세서](https://docs.google.com/spreadsheets/d/1Kq-G8fCzPooRyTlAKVj3nLO7C5vEEy4sBO_PF6zfc2E/edit?usp=sharing)

<div align="center">

![](./이미지/프로젝트_구성/테이블명세서.png)

</div>

<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>EXEL, spreadsheet</kbd> <kbd>📌 Output</kbd> <kbd>테이블명세서</kbd></sub>
</div>
  


---
<!-- DDL 코드 이미지 -->
<a id="sql-산출물ddl프로시저트리거"></a>
## 💾SQL 산출물

<details>
<summary><b>산출물 이미지로 보기</b></summary>

<br>

### 운영 프로세스 (반납/검수/수리/대여)

<div>
	
#### 반납 요청 <br>
<a href="./이미지/sql산출물/반납요청.png"></a>
  <img src="./이미지/sql산출물/반납요청.png?v=1" width="600" />

#### 반납 검수 <br>
<a href="./이미지/sql산출물/반납검수.png"></a>
  <img src="./이미지/sql산출물/반납검수.png?v=1" width="600" />

#### 반납_경고규칙 <br>
<a href="./이미지/sql산출물/반납_경고규칙.png"></a>
  <img src="./이미지/sql산출물/반납_경고규칙.png?v=1" width="600" />

#### 검수 결과 <br>
<a href="./이미지/sql산출물/검수결과.png"></a>
  <img src="./이미지/sql산출물/검수결과.png?v=1" width="600" />

#### 검수 증빙 <br>
<a href="./이미지/sql산출물/검수증빙.png"></a>
  <img src="./이미지/sql산출물/검수증빙.png?v=1" width="600" />

#### 수리 <br>
<a href="./이미지/sql산출물/수리.png"></a>
  <img src="./이미지/sql산출물/수리.png?v=1" width="600" />

#### 대여_배정계약 <br>
<a href="./이미지/sql산출물/대여_배정계약.png"></a>
  <img src="./이미지/sql산출물/대여_배정계약.png?v=1" width="600" />

</div>

<br>

## 

<div>
	
### 자산·재고 데이터 (자산/입출고) 

#### 자산 <br>
<a href="./이미지/sql산출물/자산.png"><a/>
  <img src="./이미지/sql산출물/자산.png?v=1" width="600" /> 

#### 자산 분류 <br>
<a href="./이미지/sql산출물/자산분류.png"></a>
  <img src="./이미지/sql산출물/자산분류.png?v=1" width="600" />

#### 자산 상태 이력 <br>
<a href="./이미지/sql산출물/자산상태이력.png"></a>
  <img src="./이미지/sql산출물/자산상태이력.png?v=1" width="600" />

#### 입고 이력 <br>
<a href="./이미지/sql산출물/입고이력.png"></a>
  <img src="./이미지/sql산출물/입고이력.png?v=1" width="600" />

#### 출고 이력 <br>
<a href="./이미지/sql산출물/출고이력.png"></a>
  <img src="./이미지/sql산출물/출고이력.png?v=1" width="600" />

</div>

<br>

## 

<div>
	
### 조직·사용자 (권한/대상 관리)

#### 부서<br>
<a href="./이미지/sql산출물/부서.png"></a>
  <img src="./이미지/sql산출물/부서.png?v=1" width="600" />

#### 직원 <br>
<a href="./이미지/sql산출물/직원.png"></a>
  <img src="./이미지/sql산출물/직원.png?v=1" width="600" />

#### 사용자_누적 상태 <br>
<a href="./이미지/sql산출물/사용자_누적상태.png"></a>
  <img src="./이미지/sql산출물/사용자_누적상태.png?v=1" width="600" />

</div>

<br />

## 

<!-- ================= D ================= -->
<div>
	
### 시스템 운영 (알림/정책 로그) 

#### 알림 <br>
<a href="./이미지/sql산출물/알림.png"></a>
  <img src="./이미지/sql산출물/알림.png?v=1" width="600" />

#### 정책_이벤트 로그 <br>
<a href="./이미지/sql산출물/정책_이벤트로그.png"></a>
  <img src="./이미지/sql산출물/정책_이벤트로그.png?v=1" width="600" />

</div>

<br />
</details>



<!-- DDL 코드 -->
<details>

<summary><b>DDL</b></summary>

<br>

<div>
	
### 운영 프로세스 (반납/검수/수리/대여)

#### 반납 요청 테이블 
```SQL
CREATE OR REPLACE TABLE `return_requests`(
	`return_request_id` VARCHAR(20) PRIMARY KEY,
	`assignment_id` VARCHAR(20) UNIQUE ,
	`employee_id` VARCHAR(20) UNIQUE ,
	`request_status` VARCHAR(5) ,
	`requested_at` DATE,
	`request_reason` VARCHAR(50),
	FOREIGN KEY (`assignment_id`) REFERENCES `assignments`(`assignment_id`),
	FOREIGN KEY (`employee_id`) REFERENCES `employees`(`employee_id`)
);
```

#### 반납 검수 테이블 
```SQL
CREATE OR REPLACE TABLE `inspection` (
	`inspection_id` VARCHAR(20) PRIMARY KEY,
	`return_request_id` VARCHAR(20) NOT NULL ,
	`employee_id` VARCHAR(20) NOT NULL ,
	`inspection_result_code` VARCHAR(5) NOT NULL ,
	`inspection_result` VARCHAR(5),
	`repair_required` VARCHAR(5) NOT NULL,
	`inspected_at` DATE,
	FOREIGN KEY (`return_request_id`) REFERENCES `return_requests`(`return_request_id`),
	FOREIGN KEY (`employee_id`) REFERENCES `employees`(`employee_id`),
	FOREIGN KEY (`inspection_result_code`) REFERENCES `inspection_results`(`inspection_result_code`)
);
```

#### 반납_경고규칙 테이블 
```SQL
CREATE OR REPLACE TABLE `policy_rules`(
	`policy_id` VARCHAR(20) PRIMARY KEY UNIQUE,
	`assignment_id` VARCHAR(20) NOT NULL UNIQUE ,
	`policy_name` VARCHAR(10),
	`warning_limit` INT,
	`restriction_step` VARCHAR(5),
	FOREIGN KEY (`assignment_id`) REFERENCES `assignments`(`assignment_id`)
);
```

#### 검수 결과 테이블 
```SQL
CREATE OR REPLACE TABLE `inspection_results` (
	`inspection_result_code` VARCHAR(5) PRIMARY KEY,
	`description` VARCHAR(50)
);
```

#### 검수 증빙 테이블 
```SQL
CREATE OR REPLACE TABLE `inspection_evidences`(
	`evidence_id` VARCHAR(20) NOT NULL PRIMARY KEY UNIQUE,
	`inspection_id` VARCHAR(20) NOT NULL UNIQUE,
	`uploaded_at` DATE,
	FOREIGN KEY (`inspection_id`) REFERENCES `inspection`(`inspection_id`)
);
```

#### 수리 테이블 
```SQL
CREATE OR REPLACE TABLE `repairs`(
	`repair_id` VARCHAR(20) PRIMARY KEY,
	`evidence_id` VARCHAR(20) UNIQUE ,
	`repair_status` VARCHAR(5),
	`repair_start_date` DATE,
	`repair_end_date` DATE,
	FOREIGN KEY (`evidence_id`) REFERENCES `inspection_evidences`(`evidence_id`)
);
```

#### 대여/배정 테이블 
```SQL
CREATE OR REPLACE TABLE `assignments` (
	`assignment_id` VARCHAR(20) PRIMARY KEY,
	`asset_id` VARCHAR(20) UNIQUE NOT NULL,
	`created_by` VARCHAR(10) NOT NULL,
	`employee_id` VARCHAR(20) NOT NULL,
	`policy_id` VARCHAR(20) NOT NULL,
	`start_date` DATE,
	`due_date` DATE,
	FOREIGN KEY (`asset_id`) REFERENCES `assets`(`asset_id`),
	FOREIGN KEY (`created_by`) REFERENCES `employees`(`employee_id`),
	FOREIGN KEY (`employee_id`) REFERENCES `employees`(`employee_id`),
	FOREIGN KEY (`policy_id`) REFERENCES `policy_rules`(`policy_id`)
);
```

</div>

<br>

## 

<div> 
	
### 자산·재고 데이터 (자산/상태/입출고)

#### 자산 테이블 
```SQL
CREATE OR REPLACE TABLE `assets` (
	`asset_id` VARCHAR(20) PRIMARY KEY,
	`category_id` VARCHAR(30) NOT NULL UNIQUE,
	`serial_no` INT NOT NULL,
	`model_name` VARCHAR(50),
	FOREIGN KEY (`category_id`) REFERENCES `asset_categories`(`category_id`)
);
```

#### 자산 분류 테이블 
```SQL
CREATE OR REPLACE TABLE `asset_categories`(
	`category_id` VARCHAR(30) PRIMARY KEY,
	`category_name` VARCHAR(10) NOT NULL
);
```

#### 자산 상태 이력 테이블 
```SQL
CREATE OR REPLACE TABLE `asset_status_history` (
	`history_id` VARCHAR(20) PRIMARY KEY UNIQUE,
	`asset_id` VARCHAR(20) NOT NULL UNIQUE ,
	`from_status` VARCHAR(20),
	`to_status` VARCHAR(20) NOT NULL,
	`changed_at` DATE,
	FOREIGN KEY (`asset_id`) REFERENCES `assets`(`asset_id`)
);
```

#### 입고 이력 테이블 
```SQL
CREATE OR REPLACE TABLE `checkin_logs` (
	`checkin_id` VARCHAR(20) PRIMARY KEY UNIQUE,
	`assignment_id` VARCHAR(20) NOT NULL UNIQUE ,
	`employee_id` VARCHAR(20) NOT NULL ,
	`before_due_date` DATE, 
	`after_due_date` DATE,
	`checkin_at` DATE,
	`change_reason` VARCHAR(30),
	FOREIGN KEY (`assignment_id`) REFERENCES `assignments`(`assignment_id`),
	FOREIGN KEY (`employee_id`) REFERENCES `employees`(`employee_id`)
);
```

#### 출고 이력 테이블 
```SQL
CREATE OR REPLACE TABLE `checkout_logs`(
	`checkout_id` VARCHAR(20) PRIMARY KEY UNIQUE,
	`asset_id` VARCHAR(20) NOT NULL UNIQUE ,
	`assignment_id` VARCHAR(20) NOT NULL UNIQUE ,
	`employee_id` VARCHAR(10) NOT NULL ,
	`checkout_at` DATE,
	`checkout_type` VARCHAR(10),
	FOREIGN KEY (`asset_id`) REFERENCES `assets`(`asset_id`),
	FOREIGN KEY (`assignment_id`) REFERENCES `assignments`(`assignment_id`),
	FOREIGN KEY (`employee_id`) REFERENCES `employees`(`employee_id`)
);
```

</div>

<br>

## 

<div>
	
### 조직·사용자 (권한/대상 관리)
	
#### 부서 테이블 
```SQL
CREATE OR REPLACE TABLE `departments`(
	`department_id` VARCHAR(20) PRIMARY KEY,
	`department_name` VARCHAR(20)
);
```

#### 직원 테이블 
```SQL
CREATE OR REPLACE TABLE `employees`(
	`employee_id` VARCHAR(20) PRIMARY KEY, 
	`department_id` VARCHAR(10) NOT NULL,
	`name` VARCHAR(10) NOT NULL,
	`email` VARCHAR(20) NOT NULL UNIQUE,
	`created_at` DATE,
	`password` VARCHAR(30) NOT NULL,
	FOREIGN KEY (`department_id`) REFERENCES `departments`(`department_id`)
);
```
#### 사용자 누적 상태 테이블 
```SQL
CREATE OR REPLACE TABLE `user_policy_state` (
	`employee_id` VARCHAR(20) PRIMARY KEY ,
	`overdue_count` INT,
	`warning_count` INT,
	`restriction_level` VARCHAR(5),
	`updated_at` DATE,
	FOREIGN KEY (`employee_id`) REFERENCES `employees`(`employee_id`)
);
```
</div>

<br>

## 

<div>
	
### 시스템 운영 (알림·정책·로그)

#### 알림 테이블 
```SQL
CREATE OR REPLACE TABLE `notices`(
	`notice_id` VARCHAR(30) PRIMARY KEY,
	`employee_id` VARCHAR(20) UNIQUE ,
	`policy_id` VARCHAR(20) ,
	`assignment_id` VARCHAR(20) UNIQUE ,
	`notice_type` VARCHAR(10),
	`message` VARCHAR(50),
	`is_read` CHAR(1),
	FOREIGN KEY (`employee_id`) REFERENCES `employees`(`employee_id`),
	FOREIGN KEY (`assignment_id`) REFERENCES `assignments`(`assignment_id`)
);
```

#### 정책 이벤트 테이블 
```SQL
CREATE OR REPLACE TABLE `policy_event_logs` (
	`event_id` VARCHAR(20) PRIMARY KEY,
	`policy_id` VARCHAR(20) NOT NULL ,
	`employee_id` VARCHAR(20) NOT NULL ,
	`assignment_id` VARCHAR(20) NOT NULL,
	`occurred_at` DATE,
	`event_type` VARCHAR(10),
	FOREIGN KEY (`policy_id`) REFERENCES `policy_rules`(`policy_id`),
	FOREIGN KEY (`employee_id`) REFERENCES `employees`(`employee_id`)
);
```

</div>

<br />
</details>

---

<a id="테스트-진행-과정"></a>
## 🧪테스트 진행 과정

> TODO: 테스트 시나리오/케이스/결과 추가

- 테스트 항목(예시)
  - 대여 → 반납요청 → 반납/검수 정상 플로우
  - 반납예정일 경과 → 요구/경고/제재 누적 조건
  - 상태 역전/중복 처리 차단(제약/트리거 동작 확인)
- 산출물(예시)
  - `./테스트/테스트케이스.md`
  - `./테스트/결과캡처/`

---

<a id="향후-확장-방향"></a>
## 🧭향후 확장 방향

🔗 [향후계획](./파일/PDF파일/향후계획.pdf)

<div align="center">
<img src="./이미지/로고_포스터/향후계획.png" width="460" alt="향후기획서" />
</div>

<br>
<div align="center">
  <sub><kbd>🛠 Tool</kbd> <kbd>PDF</kbd> <kbd>📌 Output</kbd> <kbd>Planning Doc</kbd></sub>
</div>

---

<a id="회고록"></a>

## 📝 회고록

| 이름 | 담당 | 회고 |
|---|---|---|
| **팀장:신민수** | 프로젝트 총괄 / 종합 피드백 | 일정·역할·산출물 흐름을 통합 관리하면서, 초기에 기준(용어/정의/정책)을 고정하지 않으면 ERD·DDL·테스트 전 구간에서 수정 비용이 급증한다는 점을 체감했다. |
| **김지연** | 프로젝트 배경 / 문서 제작·통일화 | 배경·문서 정리와 용어 통일을 진행하며, 정의가 애매한 상태(대여/반납/검수/상태값)가 남아 있으면 트리거·프로시저 설계까지 흔들려 품질 저하로 직결된다는 걸 확인했다. |
| **모희주** | WBS 작성 / 발표 PPT 준비 | WBS와 발표자료를 구성하면서, 결과 화면보다 “진행 절차→테스트→오류→원인→해결” 구조로 내용을 고정해야 전달력과 평가 기준 충족이 안정적이라는 걸 배웠다. |
| **박지인** | 요구사항 명세서 작성 / GitHub 관리 | 요구사항 명세서 작성과 깃허브 관리를 병행하며, 문서·코드·산출물 버전이 어긋나면 검증이 불가능해져 재현 조건/변경 이력 중심으로 기록을 남기는 습관이 필수라고 정리했다. |
| **김승욱** | 테이블 명세서 제작 / 테이블 오류 검증 | 테이블 명세서 기반으로 컬럼·제약조건을 검증하면서, 컬럼명/NULL/DEFAULT/제약조건 불일치가 대부분의 오류 원인이며 명세와 DDL을 1:1로 맞추는 검증 루틴이 필요하다고 느꼈다. |
| **윤준상** | ERD 작성 / 오류 테스트·수정 | ERD 작성과 수정 테스트를 진행하며, 관계(카디널리티/PK·FK/삭제 정책)를 초기에 합의하지 않으면 데이터 무결성 오류가 반복되므로 설계 단계에서 정책을 확정하는 게 핵심이라고 정리했다. |


