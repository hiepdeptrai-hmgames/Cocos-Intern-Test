# Bài test Intern Cocos — Word Merge

> **Thời gian: 4 giờ.** Hết giờ nộp nguyên trạng, làm tới đâu nộp tới đó.
> Engine: **Cocos Creator 2.4.10**, ngôn ngữ **TypeScript**. Không dùng thư viện ngoài.

## 1. Game là gì?
Màn chơi có 20 word (chữ) rơi xuống vùng chơi và xếp chồng lên nhau theo vật lý. Mỗi word thuộc 1 category (vd `Cow`, `Pig` thuộc **Farm Animals**). Người chơi kéo thả word để gom các word cùng category lại. Gom đủ 4 word của một category thì category đó hoàn thành. Hoàn thành hết thì hiện End Card.

🎬 Video gameplay mẫu: _(sẽ cập nhật link)_

## 2. Có sẵn trong repo
| Thứ | Đường dẫn |
|---|---|
| Scene trống | `assets/Scene/game.fire` |
| Level data | `assets/resources/LevelData/level-0.json` (5 category × 4 word) |
| Nền | `assets/Arts/2D/GamePlay/bg_sticker.png` |
| Bubble | `assets/Arts/2D/GamePlay/sticker merge 2.png` |
| Font chữ word | `assets/Font/URBANEROUNDED-BOLD.TTF` |
| Nút Hint (không bắt buộc dùng) | `assets/Arts/2D/GamePlay/shot.png` |
| End Card | `assets/Arts/2D/EndCard/logo.png`, `btn.png`, `play_now_yellow.png` |
| API Cocos hay dùng | [`docs/cocos-cheatsheet.md`](cocos-cheatsheet.md) |

Format level:
```json
{ "name": "Level 0", "categories": [ { "category": "Farm Animals", "words": ["Cow", "Horse", "Goat", "Pig"] } ] }
```

Bạn **tự thiết kế** file, class, prefab. Cách tổ chức code là một phần được đánh giá.

## 3. Yêu cầu
Làm **Must** trước, còn thời gian mới làm **Bonus**.

### Scene (gợi ý ~30')
- **Must:** Canvas dọc 720×1280, có nền, vùng chơi có sàn và 2 tường bên, chỗ hiển thị Goal ở trên cùng.
- **Bonus:** Layout không vỡ khi đổi tỉ lệ màn hình.

### T1 — Physics (~50')
- **Must:**
  - Load `level-0`, xáo trộn thứ tự và thả lần lượt 20 word từ phía trên xuống.
  - Word rơi theo trọng lực, xếp đống lên nhau, **không lọt ra ngoài** tường/sàn.
  - Word là **sticker chữ**, không có sprite nền, dùng font `URBANEROUNDED-BOLD`: chữ màu, viền trắng dày (`cc.LabelOutline`), đổ bóng (`cc.LabelShadow`). Chữ luôn đọc được (không bị xoay ngược).
  - Mỗi word có màu chữ **ngẫu nhiên** lấy từ một bảng ~5 màu (vd tím, cam, xanh dương, xanh lá, xanh ngọc). Màu **KHÔNG** được gắn với category, nếu không người chơi nhìn màu là đoán ra nhóm. Tránh màu đỏ vì đỏ dùng cho phản hồi thả sai.
- **Bonus:** Word không bị văng mạnh khi vừa sinh ra hoặc khi có vật mới xuất hiện giữa đống.

### T2 — Kéo thả & Merge (~75')
- **Must:**
  - Chạm giữ một word rồi kéo theo ngón tay. Thả ra thì xử lý theo vật nằm dưới chỗ thả.
  - **Word → word cùng category:** cả 2 word biến mất, tạo ra 1 **bubble** tại chỗ đó, hiển thị tên category và số word hiện có (vd `Farm Animals 2/4`).
  - **Word → bubble cùng category:** bubble tăng số word.
  - **Thả sai category:** có phản hồi lỗi rõ ràng (vd nháy đỏ), word quay về chỗ cũ.
  - Thả vào chỗ trống: word tiếp tục rơi như bình thường.
  - Bubble cũng có vật lý giống word.
- **Bonus:**
  - Kéo được bubble: bubble → word, bubble → bubble cùng category (tổng không quá 4).
  - Highlight vật sẽ nhận khi đang kéo ngang qua nó.
  - **Kéo bằng ghost:** khi chạm giữ, tạo một bản sao chỉ có hình (không RigidBody/collider) đi theo tay; word thật nằm yên trong đống (làm mờ). Thả trúng → merge; thả chỗ trống hoặc thả sai (kèm nháy đỏ) → ghost bay về word thật rồi biến mất, word thật không đổi vị trí. Chỉ merge mới làm thay đổi sân. Kết quả: kéo không làm xô lệch các word khác.

### T3 — Complete Category (~30')
- **Must:**
  - Bubble đủ 4 word: chạy anim hoàn thành, rồi bubble biến mất.
  - Goal ở trên cùng hiển thị category **tiếp theo chưa hoàn thành** (gợi ý cho người chơi). Lúc bắt đầu màn, Goal hiện category đầu tiên.
- **Bonus:** Bubble đang chạy anim biến mất thì không kéo được, cũng không thả gì lên được.

### T4 — End Card (~15')
- **Must:**
  - Không còn word nào thì hiện End Card (lớp phủ tối, logo, nút) **đúng 1 lần**.
  - Khi End Card hiện, không tương tác được với word/bubble bên dưới, nhưng nút trên End Card vẫn bấm được.
- **Bonus:** Có anim xuất hiện (fade/scale).

## 4. Thứ tự gợi ý
Scene → T1 → T2 Must → T3 → T4 → Bonus. Nên commit sau mỗi task.

## 5. Nộp bài
1. Push code lên repo của bạn (fork hoặc repo riêng) và gửi link.
2. Video quay màn hình chơi hết level (hoặc tới chỗ bạn làm được).
3. `README.md` ngắn gồm:
   - Đã làm task nào (Must/Bonus), chưa làm gì.
   - Cách tổ chức code: mỗi file/class làm gì.
   - Gặp khó ở đâu.

