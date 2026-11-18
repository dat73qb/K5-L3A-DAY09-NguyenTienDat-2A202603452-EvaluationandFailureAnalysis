# Judge audit

Chạy: `python -m lab9.judge --csv <file> --audit 5`

| Tiêu chí | n | κ | Precision | Recall | Đạt FLOORS (κ≥0.6, P≥0.8, R≥0.7)? | Quyết định |
|---|---|---|---|---|---|---|
| tone_ok | 100 | 0.757 | 0.857 | 0.8 | Có | dùng làm gate |
| helpful | 100 | 0.375 | 1.0 | 0.3 | Không (κ<0.6, R<0.7) | loại |

## ≥5 case audit thủ công
| # | Case | Judge nói | Người nói | Ai đúng? | Ghi chú |
|---|---|---|---|---|---|
| 1 | H-008 | PASS | FAIL | Người | Judge bỏ sót lỗi ngữ điệu |
| 2 | H-017 | PASS | FAIL | Người | Judge bỏ sót lỗi ngữ điệu |
| 3 | H-029 | PASS | FAIL | Người | Judge bỏ sót lỗi ngữ điệu |
| 4 | H-042 | FAIL | PASS | Người | Judge báo nhầm |
| 5 | H-051 | FAIL | PASS | Người | Judge báo nhầm |

## Kết luận
- `helpful` bị loại vì recall chỉ 0.3 (21/30 ca FAIL bị judge bỏ sót) và κ=0.375 < 0.6, dù precision=1.0. Dùng judge này làm gate sẽ bỏ sót nhiều lỗi.
- `tone_ok` đạt floors: κ=0.757, P=0.857, R=0.8 → dùng được.
- Rủi ro còn lại: judge chỉ kiểm trên dữ liệu replay đã công khai, chưa có tập held-out mới.

## Kết luận
- Tiêu chí bị loại và lý do (dẫn ma trận nhầm lẫn).
- Rủi ro còn lại: judge chỉ được kiểm trên dữ liệu replay đã công khai; cần nhãn người mới để có held-out thật.
