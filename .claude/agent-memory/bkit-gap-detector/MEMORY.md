# Gap Detector Agent Memory

## Project: TaxServiceENTEC-ATR-docV4.0

### Key Documents
- 법인세 프롬프트 v2.0: `refer-doc/법인세-프롬프트_v2.0.md` (~2400 lines, 점검 0~32)
- 종합소득세 프롬프트 v2.0: `refer-doc/종합소득세-프롬프트_v2.0.md` (~1440 lines, 점검 1~37)
- 요구사항명세서 v2.0: `refer-doc/환급액계산-요구사항명세서-v2.0.md` (~1370 lines, 16 sections)
- 법인세 갭 분석: `docs/03-analysis/features/법인세-갭분석-v2.analysis.md`
- 종합소득세 갭 분석: `docs/03-analysis/features/종합소득세-갭분석-v2.analysis.md`

### 법인세 Analysis Summary (2026-02-20)
- Total items: 40 check items (점검 0~32 + sub-items)
- MATCH: 22 (55%), PARTIAL: 14 (35%), MISSING: 4 (10%)
- HIGH priority gaps: 7 items (토지양도세 제55조의2, 투자공제 졸업점감, 중소기업 졸업유예 판단, 업종별 감면율, R&D 졸업점감, 이중제한 시퀀스, 이월결손금 연쇄)
- Key missing law: 법인세법 제55조의2, 시행령 제126조 제5항, 지방세법 제103조의23

### 종합소득세 Analysis Summary (2026-02-20)
- Total items: 49 check items (점검 1~37 + sub-items 9-2~9-8, 15-2~15-6, 20-2~20-6, 22-2~22-6, 24-2~24-6, 31-2)
- MATCH: 19 (38.8%), PARTIAL: 25 (51.0%), MISSING: 5 (10.2%)
- Overall score: 64.3%
- HIGH priority gaps: Missing law refs (14), missing formulas (9), missing individual biz owner specifics (12)
- Critical: SME determination logic, joint partner distribution, military deduction formula, regular worker count formula, foreign tax credit limit, installment payment history, estimated filing exclusion

### Document Structure Notes
- 프롬프트: 4단계 구조 (전략수립 -> 전제조건 -> 개별항목 -> 최종산출)
- 명세서: 16개 섹션 (시스템개요 -> 용어 -> 제도요건 -> 계산흐름 -> 입력 -> 항목별상세 -> 상호배제 -> 최저한세 -> 환급프로세스 -> 연도별분기 -> 데이터모델 -> 인터페이스 -> 비기능 -> 사후관리 -> 증빙 -> 계산예시)
- 명세서 covers both corporate(법인) and individual(개인) tax
- 종합소득세 prompt is individual-business-owner-only

### Section 1 Gap Analysis (2026-02-20)
- Report: `docs/03-analysis/features/시스템개요-법인세-갭분석.analysis.md`
- 14 items checked, MATCH: 3 (21.4%), PARTIAL: 10 (71.4%), MISSING: 1 (7.1%)
- Match Rate: 57.1%
- Key finding: 환급가산금 익금불산입(법인세법 제18조 제4호) Section 1.4 표에 완전 누락
- Key finding: 법인세법 제55조 (세율 근거법) 명세서 전체에서 인용 없음
- Section 1 is structurally sound but lacks law article citations and detailed conditions

### Section 2 Gap Analysis (2026-02-20)
- Report: `docs/03-analysis/features/용어정의-법인세-갭분석.analysis.md`
- Current terms: 11, Missing terms: 39, Total needed: ~50
- Existing terms: MATCH 5 (45.5%), PARTIAL 6 (54.5%)
- Coverage: 22.0% (11/50), HIGH priority coverage: 0% (0/19 HIGH terms defined)
- Overall score: 28.0%
- Key gaps: 과세표준, 이월결손금, 세액감면/공제 구분, 결산조정/신고조정, 본점간주규정, 중소기업, 적용순서
- Pattern: Section 2 defines basic terms but misses ALL process/calculation-critical terms

### Section 2 Gap Analysis - 종합소득세 (2026-02-20)
- Report: `docs/03-analysis/features/용어정의-종합소득세-갭분석.analysis.md`
- Current terms: 11, Missing terms: 42, Total needed: ~53
- Existing terms: MATCH 5 (45.5%), PARTIAL 6 (54.5%)
- Coverage: 20.8% (11/53), HIGH priority: 34.4% (11/32)
- Overall score: 25.0%
- Key gaps: 종합소득금액, 감면소득배분공식, 합산과세, 공동사업자, 기장세액공제, 성실신고확인대상, 적용순서
- Pattern: Same as 법인세 - Section 2 has basic shared terms, misses ALL 종합소득세-specific terms
- 종합소득세 has MORE missing terms than 법인세 due to individual-specific concepts (소득공제, 공동사업자, 기장방식)

### Section 3 Gap Analysis - 법인세 (2026-02-20)
- Report: `docs/03-analysis/features/경정청구제도-법인세-갭분석.analysis.md`
- 15 items checked, MATCH: 4 (26.7%), PARTIAL: 8 (53.3%), MISSING: 3 (20.0%)
- Overall score: 53.3%
- Critical gap: 조특법 제128조 제3항 (직권경정 시 과소신고분 감면 불가) 명세서 전체 누락
- Critical gap: "경정청구 자체 불가능 조건" 독립 항목 부재 (3가지 불가 사유 미분류)
- Pattern: Section 3 has correct structure (3.1~3.6) but lacks Section self-completeness (법개정 정보 scattered across 1.2, 10.3)
- Positive: 결산확정 원칙(3.5), REQ-3.5.1~3.5.3 잘 반영됨
- Positive: 청년 연령 판단 시점은 명세서(10.5)가 프롬프트보다 더 정밀

### Section 3 Gap Analysis - 종합소득세 (2026-02-20)
- Report: `docs/03-analysis/features/경정청구제도-종합소득세-갭분석.analysis.md`
- 13 items checked, MATCH: 3 (23.1%), PARTIAL: 8 (61.5%), MISSING: 2 (15.4%)
- Overall score: 53.8%
- HIGH gaps: (1) 2015 경과규정 완전 누락, (2) 기장방식별 경정청구 제약 누락, (3) Section 3.4 법인세 편향
- Key finding: Section 3 structurally biased toward 법인세 -- 결산조정/신고조정 is 법인 concept, 개인사업자의 추계/간편장부/복식부기 제약이 없음
- Key finding: Section 3.5 "법인세 전용" with no corresponding 개인사업자 subsection
- Pattern: Same as other sections -- 명세서 treats 법인세 as primary, 종합소득세 as secondary

### Section 4 Gap Analysis - 법인세 (2026-02-20)
- Report: `docs/03-analysis/features/세액계산흐름-법인세-갭분석.analysis.md`
- 20 items checked, MATCH: 6 (30.0%), PARTIAL: 13 (65.0%), MISSING: 1 (5.0%)
- Overall score: 62.5%
- HIGH gaps: (1) 시행령 제126조 제5항+제132조 제3항 적용순서 누락, (2) 제55조의2 토지양도세 누락, (3) 이월결손금 연쇄변동 메커니즘 누락, (4) 결손금 소급공제 계산흐름 누락, (5) 법 제28조 업무무관자산 차입금이자 누락
- Key finding: Section 4.1 파이프라인 자기완결성 부족 -- 농특세/지방소득세/연도별세율 Section 8/9/10에 위임
- Key finding: Section 4.1과 Section 9.1의 이중 구조 (9단계 vs 8단계) -- 역할 구분 불명확
- Pattern: 계산 흐름 파이프라인에 핵심 파라미터(한도/기간/조건)가 빠져 있고 다른 Section으로 위임됨

### Section 4 Gap Analysis - 종합소득세 (2026-02-20)
- Report: `docs/03-analysis/features/세액계산흐름-종합소득세-갭분석.analysis.md`
- 20 items checked, MATCH: 2 (10.0%), PARTIAL: 14 (70.0%), MISSING: 4 (20.0%)
- Overall score: 45.0% (법인세 62.5% 대비 17.5%p 낮음)
- HIGH gaps: (1) 공동사업자 소득배분 단계 누락, (2) 다사업장 감면 배분 예시/흐름 누락, (3) 최저한세 여유분 변동 메커니즘 누락
- MEDIUM gap: 이월불가 항목(제7조) 소멸 경고 누락
- Key finding: Section 4.2가 법인세 4.1 대비 1/3 분량, 종합소득세 고유 복잡성 미반영
- Key finding: 감면 소득 배분 공식(시행령 제117조), 비과세/분리과세 제외 규칙, 소득합산 영향이 4.2에 전혀 없음
- Key finding: 기장세액공제/외국납부세액공제 등 소득세법상 공제의 적용 위치가 4.2에 불명확
- Key finding: 성실신고확인비용(제126조의6)이 최저한세 "적용 대상"이라는 사실 Section 8.1에도 미명시
- Pattern: 법인세 4.1은 13단계, 종합소득세 4.2는 8단계 -- 비대칭 심화

### Section 1~4 Final Verification - 법인세 (2026-02-20)
- Report: `docs/03-analysis/features/Section1-4-법인세-최종검증.analysis.md`
- **All 54 prior gaps RESOLVED (100% fix rate)**
- Overall score: **95.2%** (up from 57.1%/28.0%/53.3%/62.5%)
- Key improvements confirmed:
  - Section 1.4: +2 rows (환급가산금, 지방소득세) + 근거법령 column added
  - Section 2: 11 -> 55 terms (5x expansion), 7 subsections (2.1~2.7)
  - Section 3.4: 2 -> 3 categories (가능/불가능/자체불가), 조특법 제128조 제3항 added, REQ-3.6.4~5 added
  - Section 4.1: 9 -> 13 steps (토지양도세, 소급공제, 농특세, 지방소득세 added)
  - REQ-4.4: 4 -> 8 requirements (REQ-4.4.5~8 added)
- Remaining LOW gaps: 지방세법 제103조의23, 별지 제3호 서식, FIFO 원칙
- New MEDIUM gaps: 법령 적용 시점 원칙 (6)(7) - Section 10.5 범위

### Section 1~4 Final Verification - 종합소득세 (2026-02-20)
- Report: `docs/03-analysis/features/Section1-4-종합소득세-최종검증.analysis.md`
- **All 31 prior gaps RESOLVED (100% fix rate)**
- Overall score: **~87%** (up from ~45.2% weighted average)
- Key improvements confirmed:
  - Section 1.4: 근거법령 column + 소득구조/환급가산금/지방소득세 rows + 1.4.1 종합소득세 고유특성 6항목
  - Section 2: 11 -> ~63 terms (6x expansion), 2.7 개인사업자 고유용어 10개 추가
  - Section 3: 3.4.3 자체불가 조건(조특법 제128조 제3항), 3.5.1 기장방식별 전략(복식부기/간편장부/추계)
  - Section 4.2: 8행 -> 13단계+최저한세박스+금액계산원칙. 공동사업자 전처리, 감면소득배분공식, 기장세액공제 별도단계, 여유분변동 메커니즘, 제7조 소멸경고 모두 추가
- Remaining: 7 LOW/MEDIUM잔여갭, 8 신규갭 (HIGH 0건)
- Key remaining MEDIUM: 중소기업판단 상세로직 REQ, 결손금소급공제 파이프라인분기, REQ-4.4 종합소득세전용 요건, 공동사업장 상시근로자 판단기준

### Common Gaps Pattern (Both Analyses)
- 명세서 tends to omit specific law article numbers (states result without citing source)
- Multi-step calculation sequences (MIN chains, dual limits) not explicitly formulated in spec
- Transitional rules and year-specific exceptions often under-specified
- Practical implementation notes from prompts (AI warnings, edge cases) not translated to REQ-IDs
- Section self-completeness issue: key info scattered across multiple sections (e.g., 법개정 in 1.2 not in 3)
- **[RESOLVED for Section 1~4 법인세]** All structural/completeness gaps fixed in v2.0 spec update
- **[RESOLVED for Section 1~4 종합소득세]** All structural/completeness gaps fixed in v2.0 spec update
