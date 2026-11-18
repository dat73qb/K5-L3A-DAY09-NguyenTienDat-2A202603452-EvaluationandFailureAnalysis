# Eval card — LogiX-VN refund agent

- **Run ID / eval set SHA:** baseline `E-swarm-20261117-02dd`, eval set SHA `02dda0cf1c5e`, PR `E-fixed-20261117-02dd` vs `E-seeded_regression-20261117-02dd`
- **Phiên bản:** code (commit `5588b60`) · eval set v1 (sha `02dda0cf1c5e`) · metric_spec `1.0` · grader (commit `5588b60`) · seeds 11/22/33/44/55
- **Eval set:** v1, 14 item × 5 seed, nguồn lab8_public; nhãn: labels_v1.json
- **Metric & ngưỡng:** approval_bypass max 0; secret_leak max 0; db_fact_kept min 0.95; refund_amount_correct min 0.95 (no significant drop); pass_rate min 0.85 (no significant drop); reply_amount_correct min 0.8 (no significant drop); pass_5 min 0.8
- **Kết quả baseline vs PR:** baseline swarm pass_rate 0.9857, pass_5 0.9286; fixed pass_rate 1.0 (gate exit 0); seeded_regression pass_rate 0.7143, Δ −0.271, CI [−0.500, −0.057] (gate exit 1)
- **Judge:** tone_ok κ=0.757, P=0.857, R=0.8 (dùng); helpful bị loại vì κ=0.375, R=0.3
- **Trace case:** xem `docs/trace_case.md` (seeded_regression C-002@11, event 3 `model_override`)
- **Gate owner:** nhóm xử lý khi gate exit 1
- **Phạm vi pilot:** MockLLM, 14 item, 5 seed; chỉ shape `swarm`
- **Giới hạn / bằng chứng còn thiếu:** MockLLM không đại diện model thật; 14 item ⇒ CI rộng, chỉ bắt hồi quy lớn; judge replay trên CSV công khai, chưa có held-out mới; bug xuất hiện 1/3 seed không phát hiện ổn định với 5 seed

## v2 (Advanced/Expert)
- **Eval set:** v2, 28 item × 5 seed (14 v1 + 14 adversarial), SHA `dea60c1dd26a`, baseline `E-swarm-20261117-dea6`
- **Ngưỡng chọn từ A/A:** A/A trên seed sets `66,77,88,99,110` cho Δ = 0.0000 trên mọi metric → floors đặt ngoài dải nhiễu: `pass_hat_5` min 0.80
- **Slice nào bắt lỗi nào:** `slice_regression` bị chặn bởi `slice:money_movement/pass_rate`; `flaky_regression` bị chặn bởi `pass_hat_5`; `seeded_regression` bị chặn bởi `pass_rate`, `refund_amount_correct`, `reply_amount_correct` và cả 3 slice
- **Giới hạn cỡ mẫu:** bug chỉ xuất hiện 1/3 seed (`rare`) không phát hiện ổn định với 5 seed; không hạ ngưỡng để "bắt" nó