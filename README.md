# IT자산원정
## 사내 IT자산 배정·대여/정산 DB 설계

<p align="center">
  <img src="./이미지/IT자산원정.png" width="360" alt="대표 아이콘: IT자산원정" />
  <br />
  <sub><b>대표 아이콘</b></sub>
</p>

## 👀목차
1. [👥팀원](#팀원)
2. [📚프로젝트 개요](#프로젝트-개요)
3. [📅WBS](#wbs)
4. [📄프로젝트 기획서](#프로젝트-기획서)
5. [🎬프로젝트 시나리오](#프로젝트-시나리오)
6. [📘요구사항 명세서](#요구사항-명세서)
7. [🧩유스케이스 다이어그램](#유스케이스-다이어그램)
8. [🗃️테이블 명세서](#테이블-명세서)
9. [📊ERD](#erd)
10. [💾SQL](#sql)
11. [📝회고록](#회고록)

---

### 👥팀원

<table>
  <tr>
    <td align="center" valign="top">
      <img src="./동물%20이미지/siba.png" width="120" alt="신민수" /><br />
      <b>팀장:신민수</b><br />
      <a href="https://github.com/ZonezIpex">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="./동물%20이미지/raven.png" width="120" alt="김승욱" /><br />
      <b>김승욱</b><br />
      <a href="https://github.com/KIM-SUNG-UK">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="./동물%20이미지/squirrel.png" width="120" alt="김지연" /><br />
      <b>김지연</b><br />
      <a href="https://github.com/wldusdus63">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="./동물%20이미지/rabbit.png" width="120" alt="모희주" /><br />
      <b>모희주</b><br />
      <a href="https://github.com/heejudy">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="./동물%20이미지/mouse.png" width="120" alt="박지인" /><br />
      <b>박지인</b><br />
      <a href="https://github.com/mondayziin">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="./동물%20이미지/cat.png" width="120" alt="윤준상" /><br />
      <b>윤준상</b><br />
      <a href="https://github.com/wnstkd704">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>
  </tr>
</table>

---

## 📚프로젝트 개요

사내 IT 자산(노트북, 모니터, 태블릿, 모바일 단말 등)은 **프로젝트 투입/부서 이동/외주·협력사 투입** 등의 이유로 배정과 회수가 반복됩니다.  
하지만 실제 운영에서는 대여·반납 기록이 메신저/엑셀/담당자 개인 기록으로 분산되어 아래 항목을 **한 번에 추적**하기 어렵습니다.

- 누가 어떤 자산을 언제부터 사용 중인지
- 반납 검수 결과(정상/파손/분실/수리필요 등)와 책임 경계
- **파손·연체 비용을 어떤 근거로 산정했는지(정산 재현성)**

이 상태가 지속되면 **분실/중복 지급/재고 불일치** 같은 운영 리스크가 커지고, 검수 기준이 사람마다 달라 **파손·수리비 정산 분쟁**이 발생합니다.  
특히 **보증금 환급·차감, 연체료 산정**이 계약/검수/정책 이력과 분리되면 “정산 근거”가 약해져 운영 난이도가 급격히 상승합니다.

본 프로젝트는 **조달(PO→배송→입고→검수) → 자산화 → 대여/반납 → 파손·연체·보증금 정산**까지의 흐름을 **DB 모델로 고정**하여,
- **상태 역전(입고 전 대여 등)**,
- **근거 누락(검수 없이 정산 등)**  
을 구조적으로 예방하는 것을 목표로 합니다.

---

### 🎯서비스 목표

##### 1) 상태 전이 정합성(프로세스 고정)
- 조달~자산화까지 **상태 전이(PO/Shipment/Receiving/Asset)** 정합성 확보
- **상태 이력(언제/누가/왜)** 기록으로 변경 추적 가능하게 설계

##### 2) 자산 단위 추적(Asset Unit Tracking)
- 자산 단위(시리얼/자산태그)로 **단일 개체 추적**
- 창고 재고(Inventory)와 사용 중(Deployed) 자산을 분리 관리

##### 3) 대여/반납/검수 분리(책임 경계)
- “대여”와 “반납”, “검수”를 분리해 **책임 경계 + 이력 근거** 확보
- QR/라벨 기반 운영(스캔→대여/반납) 흐름을 고려한 구조

##### 4) 정산 재현성(ledger 기반)
- **보증금 원장(ledger)** 기반 환급/차감으로 “정산 재현” 가능하게 설계
- **연체/파손 정산 근거(정산라인, 정책 버전)** 저장 → 분쟁 포인트 방어

##### 5) 멀티테넌시 기본값
- **company_id** 기본 분리(회사별 데이터 혼합 방지)

---

### 🧾참고자료(링크)

- 플래텀(2025): IT 자산관리 솔루션 ‘셀리즈’, ITAM 기능 확장 (자동 등록/사용량 분석/QR 기반 대여·순환)  
  https://platum.kr/archives/252140

  ![](./이미지/플래텀.png)

- Sellease(셀리즈) 공식: QR코드 기반 임직원 자산 대여 프로세스(스캔 1회 대여/반납, 실시간 대여 현황 등)  
  https://landing.sellease.io/ko/post/revolutionizing-asset-management-with-sellease-qr-code-based-employee-asset-rental

  ![](./이미지/셀리즈.png)

- Ajdari et al.(2017): Definition and Implementation of Procedures for IT Assets Managing (LCAM 기반 생애주기/상태전이/이력·컴플라이언스 관점)  
  http://doi.org/10.17261/Pressacademia.2017.478

---

### 📅WBS

---

### 📄프로젝트 기획서

- **프로젝트 기획서(초안)**: [프로젝트_기획서_초안.txt](./프로젝트_기획서_초안.txt)
- **프로젝트 기획서(2차수정안)**: [2차_수정안.txt](./2차_수정안.txt)
- **프로젝트 기획서(3차수정안)**: [3차_수정안.txt](./3차_수정안.txt)

---

### 🎬프로젝트 시나리오

![](./이미지/프로젝트%20시나리오.png?v=1)

---

### 📘요구사항 명세서

![](./이미지/요구사항명세서.png)

---

### ★유스케이스 다이어그램

![](./이미지/유스케이스%20다이어그램.png?v=1)

---

### 🗃️테이블 명세서

---

### 📊ERD

---

### 💾SQL

---

### 회고록
