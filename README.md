# 사내 IT자산 배정·대여/정산 DB 설계

## 👀목차
1. [👥팀원](#팀원)
2. [📚프로젝트 개요](#프로젝트-개요)
3. [📅WBS](#WBS)
4. [📄프로젝트 기획서](#프로젝트-기획서)
5. [🎬프로젝트 시나리오](#프로젝트-시나리오)
6. [📘요구사항 명세서](#요구사항-명세서)
7. [🗃️테이블 명세서](#테이블-명세서)
8. [📊ERD](#ERD)
9. [💾SQL](#SQL)
10. [회고록](#회고록)

---

### 👥팀원

<table>
  <tr>
    <td align="center" valign="top">
      <img src="https://github.com/beyond-sw-camp/be25-1st-IT-Asset-Ledger/blob/main/%EA%B3%A0%EC%96%91%EC%9D%B4/cat1.jpg?raw=true" width="120" alt="신민수" /><br />
      <b>팀장:신민수</b><br />
      <a href="https://github.com/ZonezIpex">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="https://github.com/beyond-sw-camp/be25-1st-IT-Asset-Ledger/blob/main/%EA%B3%A0%EC%96%91%EC%9D%B4/cat2.jpg?raw=true" width="120" alt="김승욱" /><br />
      <b>김승욱</b><br />
      <a href="https://github.com/KIM-SUNG-UK">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="https://github.com/beyond-sw-camp/be25-1st-IT-Asset-Ledger/blob/main/%EA%B3%A0%EC%96%91%EC%9D%B4/cat3.jpg?raw=true" width="120" alt="김지연" /><br />
      <b>김지연</b><br />
      <a href="https://github.com/wldusdus63">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="https://github.com/beyond-sw-camp/be25-1st-IT-Asset-Ledger/blob/main/%EA%B3%A0%EC%96%91%EC%9D%B4/cat4.jpg?raw=true" width="120" alt="모희주" /><br />
      <b>모희주</b><br />
      <a href="https://github.com/heejudy">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="https://github.com/beyond-sw-camp/be25-1st-IT-Asset-Ledger/blob/main/%EA%B3%A0%EC%96%91%EC%9D%B4/cat5.jpg?raw=true" width="120" alt="박지인" /><br />
      <b>박지인</b><br />
      <a href="https://github.com/mondayziin">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>

  <td align="center" valign="top">
      <img src="https://github.com/beyond-sw-camp/be25-1st-IT-Asset-Ledger/blob/main/%EA%B3%A0%EC%96%91%EC%9D%B4/cat6.jpg?raw=true" width="120" alt="윤준상" /><br />
      <b>윤준상</b><br />
      <a href="https://github.com/wnstkd704">
        <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
      </a>
    </td>
  </tr>
</table>

---

### 📚프로젝트 개요

#### 
사내 IT 자산(노트북, 모니터, 태블릿, 모바일 단말 등)은 **프로젝트 투입/부서 이동/외주·협력사 투입** 같은 이유로 배정과 회수가 자주 반복됩니다. 
그런데 실제 운영에서는 대여·반납 기록이 메신저/엑셀/담당자 개인 기록으로 흩어져 **“누가 어떤 자산을 언제부터 쓰는지”**, **“반납 검수 결과가 무엇인지”**, 
**“파손·연체 비용을 어떤 근거로 산정했는지”**를 한 번에 추적하기 어렵습니다. 이 상태가 길어지면 **분실/중복 지급/재고 불일치** 같은 운영 리스크가 커지고, 
검수 기준이 사람마다 달라 **파손·수리비 정산에서 분쟁**이 생깁니다. 특히 보증금 환급·차감, 연체료 산정이 계약/검수 이력과 분리되면 “정산 근거”가 약해져 운영 난이도가 급격히 올라갑니다. 

본 프로젝트는 **조달(PO→배송→입고→검수) → 자산화 → 대여/반납 → 파손·연체·보증금 정산**까지의 흐름을 DB 모델로 고정하여, 
상태 역전(입고 전 대여 등)과 근거 누락(검수 없이 정산 등)을 예방하는 것을 목표로 합니다. 

#### 🎯서비스 목표 - **조달부터 자산화까지** 상태 전이(PO/Shipment/Receiving/Asset) 정합성 확보 - **자산 단위(시리얼/자산태그)** 추적
+ 상태 이력(언제/누가/왜) 기록 - **대여/반납/검수**를 분리해 책임 경계와 이력 근거 확보 - **보증금 원장(ledger)** 기반 환급/차감으로 정산 재현 가능하게 설계
+ **연체/파손 정산 근거**(정산라인, 정책 버전) 저장으로 분쟁 포인트 방어 - 멀티테넌시 기본값: **company_id 방식**(회사별 데이터 혼합 방지)

---

### 📅WBS

---

### 📄프로젝트 기획서

- **프로젝트 기획서(초안)**: [프로젝트_기획서_초안.txt](./프로젝트_기획서_초안.txt)

---

### 🎬프로젝트 시나리오

![](./이미지/프로젝트%20시나리오.png)

---

### 📘요구사항 명세서

---

### 🗃️테이블 명세서

---

### 📊ERD

---

### 💾SQL

---

### 회고록
