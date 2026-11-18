# Trace case: từ trace tới quyết định sai đầu tiên

Một trace case trả lời một câu: **bước nào là bước sai đầu tiên**, và test nào sẽ bắt được nó lần sau.

```bash
cd src
python -m lab9.trace_case --shape seeded_regression --claim C-002 --seed 11
python -m lab9.trace_case --shape fixed --claim C-002 --seed 11   # đối chứng
```

## Case đã phân tích

| Trường | Nội dung |
|---|---|
| Trial | `seeded_regression C-002@11`, run ID `E-seeded_regression-20261117-02dd` |
| Triệu chứng | Grader Lab 8 fail: `status resolved != denied`; ledger ghi 20.000đ trong khi nhãn là 0đ. Reply báo "đã hoàn 20.000đ". |
| Bước sai đầu tiên | Event 3 `model_override` {"delay_days": 5}: agent ghi đè `delay_days` của DB (2) bằng số trong ticket (5), rồi quote 20.000đ và gọi host. |
| Vì sao là bước *đầu tiên* | Event 1 handoff và event 2 investigator đọc DB đúng (`delay_days=2`, status `delivered`); chỉ từ event 3 mới sai. |
| Failure mode | FM-03 `ticket_trust` (tin ticket hơn DB) + FM-02 (bỏ qua policy denied do quote > 0). |
| Đối chứng | `fixed C-002@11`: không có `model_override`; quote 0đ (no_entitlement), status `denied`, ledger 0đ. |
| Regression test | `ApprovalBypass` / `RefundAmountCorrect` / `DbFactKept` (event `model_override`) bắt được; đã fail trên `seeded_regression` và pass trên `fixed`. |
| Chưa chắc chắn | Trace không chứng minh được agent có *ý định* ghi đè hay chỉ parse sai ticket; chỉ thấy hành vi. |