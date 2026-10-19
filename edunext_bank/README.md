# edunext_bank — Kho đáp án dùng chung của EdUnExT STUDIO

Kho đáp án cho bản `EdUnExT_STUDIO_DIRECT` (userscript): khi học sinh gặp câu chưa có trong kho máy,
script **tự tra kho này** qua `raw.githubusercontent.com` / `jsDelivr` — trả đáp án ngay, **0 token AI**.

> Kho chỉ chứa `khoá câu hỏi + đáp án + số lượt xác nhận` (kèm trích đề ngắn ~110 ký tự để đối chiếu).
> Đáp án chỉ vào kho khi được Edunext **chấm CHÍNH XÁC**; đáp án lệch nhau bị đánh dấu xung đột, không tự ghi đè.

## Cấu trúc

| Đường dẫn | Vai trò |
|---|---|
| `index.json` | Danh mục: `total` (tổng số câu), `have` (danh sách shard có dữ liệu), `updatedAt`, `gen`. |
| `shards/s_XX.json` | Dữ liệu chia 256 shard theo hash **FNV-1a 32-bit** của khoá câu hỏi (2 chữ số hex). |
| `.state.json` | Dấu vết các file đóng góp đã gom (sha1) — chống gom trùng một file. |

Bản ghi trong shard:

```json
"<khoá>": { "a": "B) Paris", "t": 1750000000000, "n": 3, "q": "Thủ đô của nước Pháp...", "f": 0 }
```

- `a` — đáp án (với câu trắc nghiệm kèm chữ cái; client tự sửa chữ cái nếu đề xáo phương án).
- `t` — thời điểm cập nhật, `n` — số lượt xác nhận, `q` — trích đề ngắn, `f` — số phản hồi chấm sai.

## Vòng đời một đáp án

1. Học sinh làm đúng trên Edunext → script học vào kho máy → đưa vào **hộp gửi đóng góp**.
2. Học sinh bấm `📡 Đóng góp` → xuất file `DongGop-KhoChung_*.json` → gửi cho người gom.
3. Người gom: bỏ file vào thư mục `contributions/` của bộ tool rồi chạy `node bank_collect.js --commit --push`.
4. Học sinh khác gặp lại câu đó → script tự tra kho chung → trả ngay (0 token).

Người gom xem hướng dẫn đầy đủ trong `scripts/studio_edition_tools/README.md` của repo **Edunext_fe**.
