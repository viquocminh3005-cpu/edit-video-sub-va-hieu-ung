# Cấu trúc plan.json

```jsonc
{
  "words_file": "words.json",        // tương đối so với plan.json
  "accent": "yellow",                // "yellow" (trắng–vàng) | "cyan" (trắng–xanh)
  "sfx_volume": 1.0,                 // tùy chọn
  "punch": true,                     // false = tắt mọi zoom punch
  "proper_nouns": ["Suối Mơ", "Anh Tuấn", "Đà Lạt"],   // tên riêng viết hoa chữ cái đầu trong sub (ngoài danh sách có sẵn)
  "sub_zones": [{"start": 13.4, "end": 73.2, "y": 0.14}], // tùy chọn: dời sub lên/xuống trong khoảng đó (VD đoạn quay màn hình đã có sub sẵn ở 66%)

  "title": {
    "start": 0,                      // luôn 0
    "end": 7.5,                      // 5–10s
    "lines": ["GIA ĐÌNH", "{HAY SỰ NGHIỆP?}"],   // hoặc "GIA ĐÌNH|{HAY SỰ NGHIỆP?}"
                                     // luôn 1 dòng trắng + 1 dòng màu; đảo "{3 PHONG THỦY}|QUÝ GIÁ" khi dòng trên đè nền sáng
    "letterbox": false,              // true: thêm 2 dải đen điện ảnh (storytelling)
    "dim": 0                         // 0–0.4: làm tối nền sau tiêu đề
  },

  "emphasis": [
    {"start": 15.4, "end": 17.4, "text": "CON KHÓC {KHÔNG NÍN}"},
    {"start": 47.2, "end": 49.2, "text": "BA ĐANG {KHÔNG Ở ĐÂY}", "fx": "zoom"},
    {"start": 63.6, "end": 65.4, "text": "MÌNH ĐANG {XÂY GÌ?}", "variant": "whisper"},
    {"start": 11.4, "end": 13.4, "text": "{MỘT ĐÊM NỌ}", "kicker": "Chương 01", "variant": "chapter"}
  ],

  "illustrations": [
    {"start": 24.0, "end": 27.4, "type": "clock", "label": "Tiến độ"},
    {"start": 76.0, "end": 79.4, "type": "counter", "value": 90, "suffix": "%", "label": "Thời gian bên con"},
    {"start": 57.0, "end": 60.4, "type": "scale", "left_label": "Gia đình", "right_label": "Sự nghiệp"}
  ]
}
```

Cú pháp chữ: `{...}` = dòng/cụm màu nhấn · `|` = xuống dòng · không dùng `\`.
Chữ nhấn/tiêu đề luôn IN HOA (engine tự đổi).
`words.json` do `transcribe` tạo: `{"words": [{"w": "Mình", "s": 0.0, "e": 0.22}, ...]}`.

## Camera zoom gần/xa + chuyển cảnh (thêm 2026-10)
Tham khảo video mẫu chủ kênh: mỗi khi đổi ý/câu lớn → chuyển cảnh + đổi khung (xa 1.0 ↔ gần 1.25–1.3), đoạn xa đẩy chậm vào.
```jsonc
"cam_y": 0.5,     // điểm đứng yên khi zoom (0.5 giữ cằm → mặt không lấn xuống vùng chữ nhấn)
"camera": [       // đoạn liền nhau, không chồng; ngoài đoạn = 1.0
  {"start": 0,   "end": 5.6,  "zoom": 1.0, "to": 1.08},   // "to" = đẩy/kéo chậm trong đoạn
  {"start": 5.6, "end": 12.5, "zoom": 1.25},              // khung gần
  {"start": 12.5,"end": 19.2, "zoom": 1.0, "sfx": "swish"} // cắt nhảy khung + SFX
],
"transitions": [  // t = điểm giữa, nên trùng mốc đổi khung camera, đặt vào khoảng lặng
  {"t": 5.6,  "type": "dip"},    // tối dần → sáng lại (whoosh_rev + impact_soft)
  {"t": 19.2, "type": "flash"},  // chớp trắng (swish + impact_soft) — bước ngoặt
  {"t": 38.1, "type": "zoom"}    // giật zoom vào rồi cắt (whoosh + pop)
]
```
Tùy chọn: `dur`, `peak` (dip: 0 = đen hẳn, mặc định 20), `punch` (zoom), `sfx` (`"none"` hoặc tên). SFX mới: `whoosh_rev`.
Nhịp gợi ý: đổi khung ~5–8s/lần, chuyển cảnh 6–8 cái / phút, xen kẽ dip–flash–zoom.
**Đoạn đầu (quy tắc 10b)** — cắt nhanh, ít SFX:
```jsonc
"camera": [
  {"start": 0,    "end": 1.35, "zoom": 1.0},
  {"start": 1.35, "end": 2.5,  "zoom": 1.22},
  {"start": 2.5,  "end": 4.05, "zoom": 1.38},
  {"start": 4.05, "end": 5.6,  "zoom": 1.08},
  {"start": 5.6,  "end": 9.0,  "zoom": 1.28}, ...
],
"transitions": [
  {"t": 1.35, "type": "zoom", "punch": 1.1, "sfx": "none"},
  {"t": 4.05, "type": "zoom", "punch": 1.1, "sfx": "none"},
  {"t": 5.6,  "type": "zoom", "sfx": "none"}, ...
]
```

## SFX riêng theo video + khử ồn (thêm 2026-10)
```jsonc
"sfx_dir": "sfx_rieng",               // (hiếm dùng) thư mục WAV của CHÍNH người dùng thay SFX cùng tên — KHÔNG dùng âm tách từ video mẫu
"denoise": {"nr": 18, "nf": -40}      // khử ồn giọng (MẶC ĐỊNH BẬT khi bỏ trống): highpass 70 Hz + afftdn; nf = nền ồn đo ở khoảng lặng (dB); false = tắt
```
