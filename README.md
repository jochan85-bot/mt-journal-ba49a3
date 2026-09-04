# mt-journal — 매매일지 (자동 생성)

이 저장소는 `~/.openclaw/workspace/trade_journal.py` 가 `git add -A && git commit && git push`로
자동 발행하는 산출물입니다. **이 저장소를 직접 편집하지 마세요** — 다음 크론 실행(KR 15:50 KST /
US 05:20·06:20 KST)이 통째로 덮어씁니다. 수정은 `trade_journal.py` 쪽에서.

## 거래량 배지 표시 규약 (2026-09-04, `JOURNAL-EOD-FIELDS-20260904`)

카드/상세 페이지의 `예측 N.NNx → 확정 N.NNx` 배지:

- **예측** = 진입 시점 `entry_eod_pred`(B2 예측기 v1.2, `eod_pred = exp(α)·rvol^β`).
  KR은 2026-09-04 배선 이후 진입분부터, US는 아직 미배선(항상 `—`).
- **확정** = D+1 확정 거래량 배수(`eod_ratio`, `pfw_d0_batch.py` BREAKOUT_FAIL 판정 산출).
  D+1 미도달이거나 이미 청산된 포지션은 `—`.
- 배지 title 툴팁 = 중간값(구 RVOL, `entry_eff_rvol`) — 폐기 아님, 표시 위치만 이동.
- 색상: `eod_pred_error`(예측/확정 비율)가 0.85~1.15 밖이면 주황 강조.
- 문턱(하단 안내문 "KR≥1.4 / US≥1.6")은 `market_scanner.py` 의 `BREAKOUT_VOL_PRED_MIN`
  텍스트를 파싱해 읽는다 — 리터럴 하드코딩 아님. 정본 갱신 시 자동 추종.

⚠️ **`eod_ratio`/`eod_pred_error` 는 trade_log 자체에는 없다** — `config/pfw_d0_batch_obs.jsonl`
(관측 로그, `market/ticker/entry_date` 키)을 조인해 읽는다. 정본: 워크스페이스
`fund_study/미러상수_대장.md` §CB-JOURNAL.
