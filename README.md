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
- 반납 검수 결과와 증빙(수리 필요/불필요) 및 책임 경계
- 반납예정일 경과 시 미반납 대상이 누구인지
- 반납 요구/경고/제재가 언제 발생했는지(반납관리 이력)

이 상태가 지속되면 **분실/중복 지급/재고 불일치** 같은 운영 리스크가 커지고, 
반납 기준이 사람마다 달라 **미반납 처리(요구/경고/제재)** 가 누락되거나 지연됩니다.  
특히 반납예정일이 지났는데도 체계적인 **알림/경고**가 없으면, 
미반납이 장기화되어 자산 회전율이 낮아지고 운영 난이도가 상승합니다.

본 프로젝트는 **자산 등록 → 대여/출고 → 반납 요청 → 반납/검수(수리 여부 판정) → 가용 복귀/수리 처리** 흐름을 DB 모델로 고정하고,  
반납예정일 경과 시 **반납 요구 메시지**, 미반납 누적 기준(예: 3회 이상) 도달 시 **경고 메시지**, 필요 시 **제재(대여 제한 등)** 를 이력으로 남깁니다

- 상태 역전(반납/검수 없이 대여중 복귀 등)
- 근거 누락(요구/경고 없이 제재 적용 등)
위 사항을 구조적으로 예방하는 것을 목표로 합니다.

---

### 🧾참고자료(링크)

- 플래텀(2025): IT 자산관리 솔루션 ‘셀리즈’, ITAM 기능 확장(QR 기반 대여/반납 등)
  https://platum.kr/archives/252140

  ![](./이미지/플래텀.png)

- Sellease(셀리즈) 공식: QR코드 기반 임직원 자산 대여 프로세스(스캔 1회 대여/반납, 실시간 현황 등)
  https://landing.sellease.io/ko/post/revolutionizing-asset-management-with-sellease-qr-code-based-employee-asset-rental

  ![](./이미지/셀리즈.png)
  

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
