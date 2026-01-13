# IT자산장부
## 사내 IT 자산 대여·반납·검수·반납관리 프로세스 DB 설계

<p align="center">
  <img src="./이미지/IT자산장부.png?v=2" width="360" alt="대표 아이콘: IT자산장부" />
  <br />
  <sub><b>대표 아이콘</b></sub>
</p>

## 👀목차
1. [👥팀원 소개](#-팀원-소개)
2. [📚프로젝트 개요](#-프로젝트-개요)
3. [🎯서비스 목표](#-서비스-목표)
4. [📅WBS](#-wbs)
5. [📄프로젝트 기획서](#-프로젝트-기획서)
6. [🎬프로젝트 시나리오](#-프로젝트-시나리오)
7. [📘요구사항 명세서](#-요구사항-명세서)
8. [🧩유스케이스 다이어그램](#-유스케이스-다이어그램)
9. [🧱데이터 설계](#-데이터-설계)
10. [📊ERD](#-erd)
11. [🗃️테이블 명세서](#️-테이블-명세서)
12. [💾SQL 산출물(DDL/프로시저·트리거)](#-sql-산출물ddl프로시저트리거)
13. [🧪테스트 진행 과정](#-테스트-진행-과정)
14. [🧭향후 확장 방향](#-향후-확장-방향)
15. [📝회고록](#-회고록)

---

## 👥팀원 소개

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
      <a href="https://github.com/KIM-SEUNG-WOOK">
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
반납예정일 경과 시 **반납 요구 메시지**, 미반납 누적 기준(예: 3회 이상) 도달 시 **경고 메시지**, 필요 시 **제재(대여 제한 등)** 를 이력으로 남깁니다.

- 상태 역전(반납/검수 없이 대여중 복귀 등)
- 근거 누락(요구/경고 없이 제재 적용 등)

위 사항을 구조적으로 예방하는 것을 목표로 합니다.

### 🧾참고자료(링크)

- 플래텀(2025): IT 자산관리 솔루션 ‘셀리즈’, ITAM 기능 확장(QR 기반 대여/반납 등)  
  https://platum.kr/archives/252140  
  ![](./이미지/플래텀.png)

- Sellease(셀리즈) 공식: QR코드 기반 임직원 자산 대여 프로세스(스캔 1회 대여/반납, 실시간 현황 등)  
  https://landing.sellease.io/ko/post/revolutionizing-asset-management-with-sellease-qr-code-based-employee-asset-rental  
  ![](./이미지/셀리즈.png)

- IT 자산 관리 절차의 정의 및 구현(Definition and implementation of procedures for IT assets managing)
  https://doi.org/10.17261/Pressacademia.2017.478
  ![](./이미지/논문.png)

---

## 🎯서비스 목표

- **자산 라이프사이클 표준화**: 등록 → 대여/출고 → 반납요청 → 반납/검수 → 가용/수리 처리 흐름을 데이터로 강제
- **미반납 관리 이력화**: 반납예정일 경과 시 요구/경고/제재 발생 내역을 누적 추적
- **운영 리스크 감소**: 분실/중복지급/재고 불일치 및 담당자 주관 판단(검수 기준 불일치) 최소화
- **근거 기반 이력 추적 구조**: 반납/검수/수리/요구·경고·제재 근거가 끊기지 않게 참조 관계 유지

---

## 📅WBS

> TODO: WBS 이미지 또는 문서 링크 추가  
- 예시) `![](./이미지/WBS.png)`  
- 예시) `./산출물/WBS.xlsx`

---

## 📄프로젝트 기획서

- **프로젝트 기획서(초안)**: [프로젝트_기획서_초안.txt](./프로젝트_기획서_초안.txt)
- **프로젝트 기획서(2차수정안)**: [2차_수정안.txt](./2차_수정안.txt)
- **프로젝트 기획서(3차수정안)**: [3차_수정안.txt](./3차_수정안.txt)
- **프로젝트 기획서(4차수정안)**: [4차_수정안.txt](./4차_수정안.txt)
- **프로젝트 기획서(5차수정안)**: [5차_수정안.txt](./5차_수정안.txt)

---

## 🎬프로젝트 시나리오

![](./이미지/프로젝트%20시나리오.png?v=1)

---

## 📘요구사항 명세서

<img width="794" height="827" alt="화면 캡처 2026-01-13 요구명ex2" src="https://github.com/user-attachments/assets/b94c9f05-edf1-43f0-8e31-6f457223f5bd" />

**[요구사항 명세서](https://docs.google.com/spreadsheets/d/1Ud5T2h4dr9DT8dqLcxDZLFeatAoFF_CawhjO1A6-qOo/edit?gid=0#gid=0)

---

## 🧩유스케이스 다이어그램

![](./이미지/유스케이스%20다이어그램.png?v=1)
**[요구사항 명세서(Google Sheets)](https://docs.google.com/spreadsheets/d/1Ud5T2h4dr9DT8dqLcxDZLFeatAoFF_CawhjO1A6-qOo/edit?gid=0#gid=0)
---

## 🧱데이터 설계

> TODO: 데이터 설계 개요(엔티티/관계/핵심 규칙) 요약 추가

- 핵심 엔티티 분류(예시)
  - 자산(Asset) / 자산분류(AssetCategory)
  - 사용자(임직원/외주) / 부서 / 권한
  - 대여(출고) / 반납요청 / 반납검수 / 수리
  - 미반납(요구/경고/제재) 이력
- 무결성/감점 포인트 방지용 설계 규칙(예시)
  - 상태 역전 방지(검수 전 가용복귀 금지)
  - 제재 근거 누락 방지(요구/경고 없이 제재 금지)

---

## 📊ERD

> TODO: ERD 이미지/링크 추가  
- 예시) `![](./이미지/ERD.png)`

---

## 🗃️테이블 명세서

> TODO: 테이블 명세서 이미지/문서 링크 추가  
- 예시) `./산출물/테이블명세서.xlsx`  
- 예시) `![](./이미지/테이블명세서.png)`

---

## 💾SQL 산출물(DDL/프로시저·트리거)

> TODO: SQL 산출물 파일 링크/설명 추가

- DDL
  - `./sql/schema.sql`
- 프로시저/트리거
  - `./sql/procedures.sql`
  - `./sql/triggers.sql`

---

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

## 🧭향후 확장 방향

> TODO: 확장 방향 정리

- QR/바코드 기반 입출고(스캔 로그 테이블 추가)
- 자산 실사(재고조사) 주기 관리 및 불일치 처리 프로세스
- 검수 증빙/수리 이력 고도화(견적/영수증 등 증빙 파일 메타데이터, 업체 테이블 연계)
- 알림 채널 확장(메일/메신저 연동 이벤트 로그)

---

## 📝회고록

> TODO: 회고 작성
