---
name: edit-video-sub-va-hieu-ung
description: PHỦ CHỮ & HIỆU ỨNG VIDEO CHIA SẺ — phủ sub, tiêu đề hook, chữ nhấn, minh họa, hiệu ứng động và SFX lên video dọc 9:16 hoặc ngang 16:9 (engine tự đổi bố cục theo khung) theo phong cách chữ khối VÀNG (mặc định; xanh cyan chỉ khi người dùng yêu cầu) — KHÔNG cắt ghép video. Có CAMERA ZOOM GẦN/XA + CHUYỂN CẢNH (dip tối, chớp trắng, giật zoom) như video mẫu chủ kênh, đoạn đầu cắt khung nhịp nhanh nhưng ít SFX; tự KHỬ ỒN giọng nói. Tiêu đề và chữ nhấn dùng mẫu chữ khối Montserrat Black 2 dòng (dòng trắng nhỏ + dòng màu lớn vàng), bóng đen đặc đổ chéo, dòng sát nhau, ở giữa khung; tiêu đề 5–10s từ frame đầu và ẩn sub; sub 2–3 từ karaoke giữ kiểu riêng; cứ 8–10s có một hiệu ứng (chữ nhấn slam/zoom/stagger/wipe/type hoặc minh họa) kèm SFX. Có chế độ CHỮ CHÌM SAU NHÂN VẬT (3D) — tách người khỏi nền, chữ nằm sau đầu/vai, không che mặt (scripts/text3d). Dùng khi người dùng nói "phủ chữ & hiệu ứng video chia sẻ", "phủ chữ khối vàng/xanh", "làm sub chuyên gia", "phủ chữ cho video", "thêm tiêu đề + sub + hiệu ứng", "làm sub giống video mẫu", "chữ phía sau nhân vật", "chữ chìm sau người", "chữ 3D sau người".
---

# Skill: Phủ chữ & Hiệu ứng Video chia sẻ

## QUY TẮC CHUNG — TỰ QUYẾT ĐỊNH, KHÔNG HỎI LẠI (chủ kênh chốt)
- **Độ phân giải: mặc định xuất 1080p** (9:16 = 1080×1920, 16:9 = 1920×1080), không phóng to video nhỏ hơn. Chỉ xuất 2K (`--res 2k`) hoặc 4K (`--res 4k`) khi người dùng YÊU CẦU rõ. Bản gốc 4K vẫn được hạ về 1080p khi xuất.
- **Tiêu đề hook: tự nghĩ và tự chốt**, không đưa phương án để người dùng chọn, không hỏi "tiêu đề này được chưa". Chỉ dùng tiêu đề người dùng đưa nếu họ tự nêu.
- **Nội dung sub / chữ hiển thị: nghe không rõ thì tự suy từ bối cảnh** (chủ đề video, các câu trước–sau, tên riêng/thuật ngữ cùng ngành) rồi sửa chính tả; không hỏi lại về từ nghe không rõ, tên riêng, màu nhấn, kiểu chèn.
- **Mọi lựa chọn khác (màu nhấn, kiểu chữ, hiệu ứng, bố cục, đoạn cắt, bảng đề xuất…) Claude tự quyết** và làm tới khi ra video hoàn chỉnh. Chỉ dừng hỏi khi người dùng yêu cầu rõ "cho tôi duyệt/hỏi trước", hoặc thiếu file đầu vào, hoặc có hành động không thể hoàn tác. Cuối cùng báo ngắn gọn những gì đã tự quyết.
- Các bước "duyệt / xác nhận / hỏi" ghi ở dưới là bước **Claude tự kiểm tra bằng ảnh/âm thanh**, KHÔNG phải chờ người dùng trả lời (trừ khi người dùng yêu cầu duyệt).

Skill **chỉ phủ lớp chữ + minh họa + hiệu ứng + âm thanh** lên video đã dựng. Không cắt, không ghép, không đổi thứ tự. Hiệu ứng đụng vào hình: **zoom punch** (phóng nhẹ 5–10% lúc chữ nhấn đập vào), **camera zoom gần/xa** theo đoạn + **chuyển cảnh** phủ lên (dip/flash/zoom) — vẫn giữ nguyên thời lượng và thứ tự, không cắt khung nào. Âm thanh gốc được **khử ồn** nhẹ.

Engine: `scripts/sm_overlay.py` (Python + ffmpeg/libass; font Montserrat trong `fonts/`). Gọi bằng đường dẫn tuyệt đối.

## Quy tắc đã chốt với chủ kênh

| # | Quy tắc |
|---|---|
| 1 | **Mẫu chữ khối** (chỉ cho TIÊU ĐỀ và CHỮ NHẤN): Montserrat Black IN HOA, dòng trắng + dòng màu **bản to**, **2 dòng tự đổi cỡ để luôn cân bằng bề rộng** (khối chữ vuông như mẫu), **không viền, bóng đen đổ sát chữ** (lệch 1.6% cỡ chữ, chéo phải-dưới, mép sắc), không hộp nền, 2 dòng gần nhau như mẫu. **Mỗi chữ chỉ 1 lớp** (bóng nằm trong chính lớp chữ), **không mờ dần vào/ra** — chữ vào bằng chuyển động, ra bằng cắt gọn. Chi tiết: [references/mau-chu-khoi.md](references/mau-chu-khoi.md). |
| 2 | **Màu nhấn: LUÔN `"accent": "yellow"`** (trắng–vàng `#F4C80F`) — chủ kênh không thích xanh cyan. Chỉ dùng `"cyan"` khi người dùng tự yêu cầu, kể cả video công nghệ. Minh họa dùng cùng màu nhấn. |
| 3 | **Tiêu đề**: hook rút gọn **2–6 từ**, hiện **ngay frame 0**, dài **5–10s**, hợp ý đoạn mở đầu, **đứng yên sắc nét — không hiệu ứng ánh sáng đầu video**. **Không có sub** trong lúc tiêu đề hiện. Dòng màu nên 1–2 từ ngắn để chữ to như mẫu. |
| 3b | **Tiêu đề LUÔN là 1 dòng trắng + 1 dòng màu** (màu = xanh cyan hoặc vàng, theo `accent`). Hai cách, chọn theo nền: **trắng trên – màu dưới** `"DẤU HIỆU|{THÀNH CÔNG}"` hoặc **màu trên – trắng dưới** `"{3 PHONG THỦY}|QUÝ GIÁ"`. Không bao giờ 2 dòng cùng màu hay xanh + vàng. Dòng trắng mà đè lên nền sáng (trời, áo trắng) → đảo cho dòng đó thành dòng màu. Kiểm tra bằng snap 0s. |
| 3c | **Màu tiêu đề quyết định màu cả video**: tiêu đề xanh → chữ nhấn, từ đang nói trong sub, minh họa đều xanh; tiêu đề vàng → tất cả vàng. Engine tự làm theo `accent`. |
| 4 | **Sub KHÔNG dùng mẫu chữ khối**: 2–3 từ (tối đa 18 ký tự/dòng), **cỡ 60 = 60 px Full HD / 120 px 4K** (~3.1% chiều cao, ở 66% chiều cao; engine tự nhân theo độ phân giải — chủ kênh chốt), trắng ngà, từ đang nói **theo màu tiêu đề** (vàng `#F4C80F` hoặc xanh `#00FFFF`), **viền đen thật mỏng + bóng nhẹ khép sát** (cùng kiểu với tiêu đề). **Tên riêng** (người, địa danh, thương hiệu) viết hoa chữ cái đầu. |
| 5 | **Nhịp hiệu ứng: 8–10s một lần**, chữ nhấn và minh họa **xen kẽ**, không bao giờ trống quá 10s. |
| 6 | Mọi chữ trong dải giữa khung (44–72% chiều cao), không đè mặt, không xuống vùng UI đáy. |
| 6b | **Có hiệu ứng 3D**: ~30% chữ nhấn dùng `flip3d` / `swing3d` / `tilt3d`; icon xoay 3D khi hiện; có minh họa `cube` (khối 3D xoay). Chữ 3D vẫn là 1 lớp. |
| 7 | **Mỗi hiệu ứng có SFX**: whoosh + impact cho chữ nhấn, SFX riêng cho minh họa, SFX cho chuyển cảnh (xem bảng). Âm lượng tổng hiệu ứng **-7 dB** (`SFX_MASTER_DB`). Không nhạc nền. **Chỉ dùng SFX của skill** (engine tự tạo hoặc file trong `sfx/` của skill). Người dùng gửi video mẫu để tham khảo → chỉ học **vị trí, nhịp, loại** SFX/hiệu ứng; **KHÔNG tách/cắt âm thanh từ video mẫu** để dùng. |
| 7b | **Minh họa sang trọng**: nét mảnh bo tròn, **gradient trên màu chính** (vàng nhạt→#F4C80F→vàng đồng / xanh nhạt→cyan→xanh biển sâu); icon đặt trong **huy hiệu vòng đôi + điểm kim cương**. |
| 8 | **Không cắt ghép.** Người dùng muốn cắt → báo skill không làm việc đó. |
| 9 | **Khung NGANG 16:9** (engine tự nhận khi video rộng hơn cao — cùng bộ engine với skill edit-video-toan-dien): canvas 1920×1080, chữ khối thu nhỏ (×0.56, rộng tối đa 62% khung) và hạ xuống y 0.68 cho khỏi đè cằm, minh họa dời xuống 12%, **sub cỡ 50 = 50 px Full HD / 100 px 4K ở 87% chiều cao** (đáy khung). Bản `--preview` ra 1920 ngang. Luật 6 (dải 44–72%) chỉ áp cho khung dọc. Video ngang dài: xem thêm nhịp thưa ~30s/hiệu ứng nếu chủ kênh yêu cầu; luôn `snap` kiểm cỡ chữ trước khi render (4K có lúc cần `"fs"` riêng). |
| 10 | **Camera zoom gần/xa + chuyển cảnh** (`"camera"`, `"transitions"` — xem plan-schema.md): mỗi lần đổi ý/câu lớn → đổi khung **xa 1.0 ↔ gần 1.22–1.38**; đoạn xa có thể đẩy chậm (`"to"`). Chuyển cảnh đặt **đúng mốc đổi khung, trong khoảng lặng giữa câu**, xen kẽ `dip` (tối dần–sáng lại) · `flash` (chớp trắng, dùng ở bước ngoặt/CTA) · `zoom` (giật zoom rồi cắt). Sau đoạn mở đầu: đổi khung ~5–8s/lần, 6–8 chuyển cảnh/phút. `"cam_y": 0.5` (giữ cằm đứng yên → mặt không lấn xuống chữ nhấn). |
| 10b | **Đoạn đầu (0–~12s) nhịp NHANH, ÍT SFX**: không đẩy chậm; cắt khung mỗi **1–1.5s** theo từng cụm lời (VD 1.0 → 1.22 → 1.38 → 1.08 → 1.28), chuyển cảnh đầu dùng `zoom` (`"punch": 1.1`) thay vì `dip`. **Tối đa ~3 SFX trong 12s đầu** (impact nhẹ lúc tiêu đề hiện + whoosh lúc tiêu đề rút + 1 SFX minh họa) → các chuyển cảnh/cắt khung đoạn đầu đặt `"sfx": "none"`, camera đoạn đầu không có `"sfx"`. Tiêu đề vẫn đứng yên. |
| 11 | **Khử ồn giọng nói mặc định** (`"denoise"`, engine bật sẵn): highpass 70 Hz + afftdn. Đo nền ồn ở khoảng lặng (bước 2b) để đặt `nf`; mặc định `{"nr": 18, "nf": -40}`. Giọng méo/rè → giảm `nr`; còn ồn → tăng `nr` (≤ 25). Tắt bằng `"denoise": false`. |

## Chế độ CHỮ CHÌM SAU NHÂN VẬT (3D)
Khi người dùng nói "chữ phía sau nhân vật", "chữ chìm sau người", "chữ 3D sau người", "tách nhân vật ra chồng lớp chữ" → làm theo [references/chu-3d-sau-nguoi.md](references/chu-3d-sau-nguoi.md), công cụ trong `scripts/text3d/`:
`matte.py` (tách người, Robust Video Matting) → `place.py` (tự đặt khối chữ) → `engine3d.py render` (lớp chữ, không zoom punch) → `compose.py` (đè lớp người lên chữ).
Luật chốt: mỗi chữ cái bị che **tối đa 1/2**, **≤ 2 chữ cái** bị chạm; các dòng **rộng bằng nhau** (`"tw"`); cảnh không có người → `--no-mask` (chữ nằm trên). Báo người dùng đánh đổi: người ngồi giữa khung → khối chữ dạt 1 bên, nhỏ hơn bản thường.
## Quy trình

1. **Video**: đường dẫn file. Thư mục làm việc `<tên video>__edit/` cạnh video.
2. **Nhận giọng nói**: `python sm_overlay.py transcribe "<video>" -o "<work>/words.json"` → có thêm `words.txt` để đọc. (Engine giải mã âm thanh bằng ffmpeg — không phụ thuộc PyAV.) Trên Windows đặt `PYTHONIOENCODING=utf-8` để in tiếng Việt không lỗi.
2b. **Đo nền ồn** (cho `denoise.nf`): `ffmpeg -i "<video>" -vn -af "astats=metadata=1:reset=1,ametadata=print:key=lavfi.astats.Overall.RMS_level" -f null -` → lấy mức RMS thấp nhất (khoảng lặng), làm tròn → `nf`.
3. **Sửa chính tả `words.json`** (bắt buộc; chỉ sửa `w`, không đổi thời gian). Có kịch bản gốc thì đối chiếu.
   Liệt kê **tên riêng** xuất hiện trong lời nói (tên người, địa danh, thương hiệu, sản phẩm) vào `"proper_nouns"` của plan — engine tự viết hoa chữ cái đầu trong sub (đã có sẵn danh sách phổ biến: Facebook, YouTube, TikTok, Zalo, Việt Nam, Hà Nội, Sài Gòn…).
4. **Màu nhấn**: `"accent": "yellow"` (quy tắc 2). Chỉ đổi khi người dùng yêu cầu.
5. **Tiêu đề** (quy tắc 3b–3c): đọc 5–10s đầu → tự chốt 1 hook 2–6 từ theo mẫu `DÒNG TRẮNG|{DÒNG MÀU}` (VD `QUY TẮC|{90/10}`, `GẶP NHAU TẠI|{CYPRUS}`), không hỏi người dùng. Đặt `end` = hết câu mở đầu, trong 5–10s. Ý hook chọn theo công thức trong [references/100-tieu-de-thu-hut.txt](references/100-tieu-de-thu-hut.txt) (ghi số công thức), rồi rút gọn còn 2–6 từ.
6. **Lên lịch hiệu ứng 8–10s/lần** — xen kẽ chữ nhấn ↔ minh họa, bám đúng lúc người nói nói ý đó. Xem [references/chu-nhan-hieu-ung.md](references/chu-nhan-hieu-ung.md) và [references/minh-hoa-sfx.md](references/minh-hoa-sfx.md).
6b. **Camera + chuyển cảnh** (quy tắc 10–10b): chia video theo câu/ý → danh sách `camera` liền nhau (xa ↔ gần), `transitions` ở các mốc đổi khung nằm trong khoảng lặng (xem `s`/`e` trong words.json). Đoạn đầu cắt nhanh, tắt SFX.
7. **Viết `plan.json`** theo [references/plan-schema.md](references/plan-schema.md).
8. `check` → sửa hết cảnh báo.
9. `snap` tại 0s, giữa tiêu đề, mỗi hiệu ứng (≈ start + 0.8s), vài khung cận (zoom cao nhất) → **mở ảnh xem thật** (mặt không bị chữ/minh họa đè ở khung cận; nhãn minh họa không chồng sub — chồng thì `"hide_subs": true`) theo [references/checklist-qa.md](references/checklist-qa.md).
10. `render --preview` (1080p nhanh) tự kiểm tra → `render` bản cuối **1080p** (CRF 17; thêm `--res 2k|4k` chỉ khi người dùng yêu cầu).
11. Sửa theo góp ý: chỉ chỉnh `plan.json`/`words.json` rồi render lại.

```
python "<skill>/scripts/sm_overlay.py" transcribe "<video>" -o "<work>/words.json"
python "<skill>/scripts/sm_overlay.py" check  "<video>" "<work>/plan.json"
python "<skill>/scripts/sm_overlay.py" snap   "<video>" "<work>/plan.json" --at 0,4,16.2 -o "<work>/qa"
python "<skill>/scripts/sm_overlay.py" render "<video>" "<work>/plan.json" -o "<work>/preview.mp4" --preview
python "<skill>/scripts/sm_overlay.py" render "<video>" "<work>/plan.json" -o "<work>/<tên>_final.mp4"
```

## Ảnh mẫu kết quả (xem trước khi làm)
(Bản này không kèm ảnh chụp mẫu `examples/captures/`; danh sách dưới chỉ để tham khảo.)
- [01-tong-quan-hieu-ung.png](examples/captures/01-tong-quan-hieu-ung.png) — tiêu đề, sub karaoke, 4 kiểu chữ nhấn (wipe · flip3d · stagger · slam), 3 minh họa (quote · growth · clock).
- [02-chuyen-canh-dip.png](examples/captures/02-chuyen-canh-dip.png) · [03-chuyen-canh-flash.png](examples/captures/03-chuyen-canh-flash.png) · [04-chuyen-canh-zoom.png](examples/captures/04-chuyen-canh-zoom.png) — diễn biến từng khung của 3 kiểu chuyển cảnh.
- [05-doan-dau-nhip-nhanh.png](examples/captures/05-doan-dau-nhip-nhanh.png) — đoạn đầu cắt khung xa ↔ cận mỗi 1–1.5s (quy tắc 10b).
- [06-clip-doan-dau-13s.mp4](examples/captures/06-clip-doan-dau-13s.mp4) — 13s đầu có âm thanh (nhịp + SFX + khử ồn).

Kết quả mới phải cùng phong cách với các ảnh này. Thêm/thay mẫu khi chủ kênh chốt kiểu mới.

## Tham chiếu
- [references/mau-chu-khoi.md](references/mau-chu-khoi.md) — phân tích mẫu tiêu đề/chữ nhấn, thông số, chọn màu.
- [references/chu-nhan-hieu-ung.md](references/chu-nhan-hieu-ung.md) — các kiểu hiệu ứng chữ nhấn + lớp phụ + SFX.
- [references/minh-hoa-sfx.md](references/minh-hoa-sfx.md) — thư viện minh họa và SFX.
- [references/plan-schema.md](references/plan-schema.md) — cấu trúc plan.json.
- [references/checklist-qa.md](references/checklist-qa.md) — kiểm tra trước khi giao.
- [references/chu-3d-sau-nguoi.md](references/chu-3d-sau-nguoi.md) — chữ chìm sau nhân vật (3D): tách người, luật che chữ, quy trình.
- [examples/plan-mau.json](examples/plan-mau.json) — plan hoàn chỉnh cho video mẫu.

## SFX riêng (tùy chọn)
Thả file **WAV 16-bit** vào `sfx/` của skill, đặt tên theo SFX muốn thay: `whoosh.wav`, `whoosh_rev.wav`, `impact.wav`, `riser.wav`, `sparkle.wav`, `pop.wav`, `swish.wav`, `tick.wav`, `chime.wav`, `shimmer.wav`, `whoosh_soft.wav`, `impact_soft.wav`. Engine tự dùng file đó thay âm tạo bằng code. **Không** lấy âm thanh từ video mẫu của người khác (quy tắc 7).

Yêu cầu máy: Python 3.10+, ffmpeg/ffprobe (có libass) trên PATH, `pip install numpy faster-whisper` (+ `onnxruntime` cho chế độ chữ 3D — `text3d/matte.py`).

**GPU NVIDIA (tự dùng khi có):** `render` nén bằng `h264_nvenc` (nhanh ~2×, CQ 18 bản cuối / 23 preview), không chạy được thì tự về libx264; ép CPU bằng `--cpu`. `transcribe` chạy whisper trên CUDA (nhanh ~3.4×) — cần `pip install nvidia-cublas-cu12 "nvidia-cudnn-cu12>=9,<10"`, engine tự nạp DLL; thiếu thì tự về CPU.
