# Hướng dẫn sử dụng cấu hình Neovim (Kickstart.nvim)

> Tài liệu này hướng dẫn cách dùng bộ cấu hình Neovim của bạn — dựa trên **kickstart.nvim**,
> dùng trình quản lý plugin **`vim.pack`** tích hợp sẵn trong Neovim 0.12+.

---

## 1. Tổng quan

- **Phím Leader:** phím `Space` (dấu cách). Mọi tổ hợp viết `<leader>` nghĩa là bấm `Space` trước.
- **Trình quản lý plugin:** `vim.pack` (built-in), file khóa phiên bản là `nvim-pack-lock.json`.
- **Theme:** `tokyonight-night`.
- **Cấu trúc file:** toàn bộ cấu hình nằm trong `init.lua`, chia thành 9 SECTION rõ ràng:

  | Section | Nội dung |
  |---------|----------|
  | 1 | Cài đặt nền tảng: options, leader, phím tắt cơ bản |
  | 2 | Giới thiệu `vim.pack`, build hook |
  | 3 | Plugin giao diện / UX: gitsigns, which-key, theme, mini.nvim |
  | 4 | Tìm kiếm & điều hướng: Telescope |
  | 5 | LSP (gợi ý code, go-to-definition...) + Mason |
  | 6 | Định dạng code: conform.nvim |
  | 7 | Tự động hoàn thành & snippet: blink.cmp + LuaSnip |
  | 8 | Treesitter: tô màu cú pháp, fold, thụt lề |
  | 9 | Plugin tùy chọn / mở rộng thêm |

---

## 2. Mẹo khám phá phím tắt

Bạn **không cần nhớ hết**. Cấu hình có 2 công cụ giúp tra cứu:

- **which-key:** bấm `<leader>` (Space) rồi đợi một chút → cửa sổ popup hiện ra tất cả phím tắt còn lại.
- **Telescope:** `<leader>sk` → tìm kiếm mọi phím tắt đang có (kèm mô tả).

> Quy ước đặt tên: chữ in hoa trong mô tả là phím gợi nhớ. Ví dụ `[S]earch [F]iles` → `<leader>sf`.

---

## 3. Phím tắt cơ bản (Section 1)

| Phím | Chức năng |
|------|-----------|
| `<Esc>` | Xóa highlight kết quả tìm kiếm |
| `<C-h/j/k/l>` | Chuyển focus giữa các cửa sổ split (trái/dưới/trên/phải) |
| `<leader>q` | Mở danh sách lỗi (diagnostic quickfix) |
| `[d` / `]d` | Nhảy tới lỗi/cảnh báo trước / sau (bật sẵn của Neovim) |
| `<Esc><Esc>` | Thoát chế độ terminal (khi đang ở terminal tích hợp) |

**Các tùy chọn đã bật sẵn:** số dòng, chuột (`mouse=a`), đồng bộ clipboard với hệ điều hành,
lưu undo vĩnh viễn, tìm kiếm không phân biệt hoa thường (trừ khi gõ chữ hoa), xem trước
khi `:substitute`, highlight dòng con trỏ, highlight khi yank (copy).

---

## 4. Tìm kiếm với Telescope (Section 4)

Tất cả bắt đầu bằng `<leader>s` (**S**earch):

| Phím | Chức năng |
|------|-----------|
| `<leader>sf` | Tìm **file** trong dự án |
| `<leader>sg` | **Grep** — tìm theo nội dung trong toàn bộ dự án |
| `<leader>sw` | Tìm **từ** đang nằm dưới con trỏ |
| `<leader>sh` | Tìm trong tài liệu trợ giúp (`:help`) |
| `<leader>sk` | Tìm phím tắt |
| `<leader>sd` | Tìm các lỗi/diagnostic |
| `<leader>sr` | **Resume** — mở lại lần tìm kiếm trước |
| `<leader>s.` | File mở gần đây |
| `<leader>sc` | Tìm lệnh (command) |
| `<leader>ss` | Chọn một picker bất kỳ của Telescope |
| `<leader>sn` | Tìm file trong thư mục cấu hình Neovim |
| `<leader>s/` | Grep trong các file đang mở |
| `<leader>/` | Tìm mờ (fuzzy) trong **file hiện tại** |
| `<leader><leader>` | Danh sách buffer đang mở |

**Khi đang ở trong cửa sổ Telescope:**
- Chế độ Insert: `<C-/>` — xem tất cả phím tắt của picker.
- Chế độ Normal: `?` — xem tất cả phím tắt.
- `<C-n>`/`<C-p>` hoặc mũi tên: chọn item lên/xuống.

---

## 5. LSP — Trợ lý ngôn ngữ (Section 5)

LSP cung cấp: nhảy tới định nghĩa, tìm tham chiếu, đổi tên, code action, hover...

Phím tắt LSP **chỉ hoạt động khi có LSP server gắn vào file** (mở file có ngôn ngữ được hỗ trợ):

| Phím | Chức năng |
|------|-----------|
| `grd` | **G**oto **D**efinition — nhảy tới định nghĩa |
| `grr` | **G**oto **R**eferences — tìm mọi nơi dùng |
| `gri` | **G**oto **I**mplementation — nhảy tới phần triển khai |
| `grt` | **G**oto **T**ype — nhảy tới định nghĩa kiểu dữ liệu |
| `grD` | **G**oto **D**eclaration — nhảy tới khai báo (vd: file header trong C) |
| `grn` | **Re**name — đổi tên biến/hàm (trên toàn bộ file) |
| `gra` | Code **A**ction — sửa lỗi/gợi ý nhanh tại con trỏ |
| `gO` | Danh sách symbol trong file hiện tại |
| `gW` | Danh sách symbol trong toàn workspace |
| `<C-t>` | Nhảy ngược lại (sau khi dùng `grd`) |
| `<leader>th` | Bật/tắt inlay hint (gợi ý kiểu hiển thị inline) |

> Khi con trỏ đứng yên trên một biến vài giây, mọi nơi dùng biến đó sẽ tự được highlight.

### Cài thêm LSP server

LSP server là chương trình ngoài, được cài qua **Mason**:

- Chạy `:Mason` → mở giao diện quản lý. Bấm `g?` để xem trợ giúp.
- Để Neovim **tự cài** server: thêm vào bảng `servers` trong Section 5 của `init.lua`.
  Ví dụ bỏ comment `pyright = {}` để có LSP cho Python, `ts_ls = {}` cho TypeScript.

---

## 6. Định dạng code (Section 6)

| Phím | Chức năng |
|------|-----------|
| `<leader>f` | Định dạng (format) file hiện tại |

- **Format khi lưu (format-on-save):** hiện đang **TẮT** cho mọi ngôn ngữ.
  Để bật, sửa bảng `enabled_filetypes` trong Section 6 — ví dụ bỏ comment `lua = true`.
- Mặc định: dùng formatter ngoài nếu có cấu hình, nếu không thì dùng LSP.

---

## 7. Tự động hoàn thành & Snippet (Section 7)

Dùng **blink.cmp** với preset `default` (giống completion tích hợp của Vim):

| Phím | Chức năng |
|------|-----------|
| `<C-y>` | Chấp nhận gợi ý đang chọn (có thể tự import) |
| `<C-n>` / `<C-p>` (hoặc ↑/↓) | Chọn gợi ý kế tiếp / trước đó |
| `<C-space>` | Mở menu gợi ý, hoặc mở tài liệu nếu menu đã mở |
| `<C-e>` | Ẩn menu gợi ý |
| `<C-k>` | Bật/tắt cửa sổ signature help (gợi ý tham số hàm) |
| `<Tab>` / `<S-Tab>` | Di chuyển qua các điểm dừng trong snippet |

Nguồn gợi ý: LSP, đường dẫn file, snippet.

---

## 8. Treesitter — Tô màu cú pháp (Section 8)

- Treesitter cho tô màu cú pháp chính xác, fold, thụt lề thông minh.
- Các parser cài sẵn: `bash, c, diff, html, lua, luadoc, markdown, markdown_inline, query, vim, vimdoc`.
- Khi mở file ngôn ngữ mới chưa có parser → **tự động tải và cài** parser đó.
- Cập nhật parser: chạy `:TSUpdate`.

> ⚠️ **Lưu ý:** `nvim-treesitter` nhánh `main` cần công cụ `tree-sitter` CLI **≥ 0.25** để biên dịch parser.
> Nếu syntax highlighting không hoạt động, kiểm tra `tree-sitter --version`. Cài bản mới bằng:
> `npm install -g tree-sitter-cli`

---

## 9. Text object & thao tác văn bản (mini.nvim — Section 3)

### mini.ai — chọn quanh / bên trong đối tượng
- `va)` — **v**isual chọn **a**round dấu ngoặc `)`
- `ci'` — **c**hange **i**nside dấu nháy `'`
- `yi"` — **y**ank (copy) **i**nside dấu nháy `"`
- `aa` / `ii` — phiên bản "next" (đối tượng kế tiếp), tránh xung đột với phím Treesitter

### mini.surround — thêm/xóa/đổi ký tự bao quanh
- `saiw)` — **s**urround **a**dd **i**nner **w**ord, bao bằng `)`
- `sd'` — **s**urround **d**elete `'` — xóa cặp nháy bao quanh
- `sr)'` — **s**urround **r**eplace: đổi `)` thành `'`

---

## 10. Git (gitsigns — Section 3)

Cột bên trái hiển thị ký hiệu thay đổi git:
- `+` dòng thêm mới  ·  `~` dòng sửa  ·  `_` dòng xóa

> Các phím tắt git hunk (nhóm `<leader>h`) chỉ có đầy đủ khi bật module trong Section 9
> (bỏ comment `require 'kickstart.plugins.gitsigns'`).

---

## 11. Quản lý plugin với `vim.pack`

| Việc cần làm | Lệnh |
|--------------|------|
| Xem trạng thái plugin & bản cập nhật đang chờ | `:lua vim.pack.update(nil, { offline = true })` |
| Cập nhật tất cả plugin | `:lua vim.pack.update()` |
| Thêm plugin mới | Thêm dòng `vim.pack.add { gh 'tac-gia/ten-plugin' }` vào `init.lua` |

- File `nvim-pack-lock.json` ghi lại đúng phiên bản (commit) của từng plugin.
- Sau khi cài/cập nhật, một số plugin tự chạy build (telescope-fzf-native, LuaSnip, nvim-treesitter)
  nhờ autocmd `PackChanged` trong Section 2.

> 📌 File cũ `lazy-lock.json` (từ thời dùng `lazy.nvim`) không còn dùng nữa — có thể xóa.

---

## 12. Plugin mở rộng tùy chọn (Section 9)

Kickstart có sẵn vài plugin ví dụ trong `lua/kickstart/plugins/`. Bỏ comment dòng tương ứng
trong Section 9 của `init.lua` rồi khởi động lại Neovim để bật:

| Dòng | Chức năng |
|------|-----------|
| `require 'kickstart.plugins.debug'` | Trình gỡ lỗi (DAP) |
| `require 'kickstart.plugins.indent_line'` | Hiển thị đường thụt lề |
| `require 'kickstart.plugins.lint'` | Linter (kiểm tra lỗi cú pháp) |
| `require 'kickstart.plugins.autopairs'` | Tự đóng ngoặc/nháy |
| `require 'kickstart.plugins.neo-tree'` | Cây thư mục file |
| `require 'kickstart.plugins.gitsigns'` | Phím tắt git hunk khuyến nghị |

**Plugin riêng của bạn:** thêm file vào `lua/custom/plugins/*.lua`, rồi bỏ comment
`require 'custom.plugins'` ở cuối Section 9.

---

## 13. Khi gặp lỗi

1. Chạy `:checkhealth` — kiểm tra sức khỏe toàn bộ cấu hình.
2. `:checkhealth vim.lsp` / `:checkhealth vim.treesitter` — kiểm tra riêng từng phần.
3. `:Mason` — kiểm tra LSP server/formatter đã cài chưa.
4. `:messages` — xem lại các thông báo lỗi gần đây.
5. Mới dùng Neovim? Chạy `:Tutor` để học các thao tác cơ bản.

---

## 14. Tùy biến nhanh

| Muốn gì | Sửa ở đâu trong `init.lua` |
|---------|----------------------------|
| Bật số dòng tương đối | Section 1: bỏ comment `vim.o.relativenumber = true` |
| Đổi theme | Section 3: đổi `tokyonight.nvim` và dòng `vim.cmd.colorscheme` |
| Có Nerd Font (icon đẹp) | Section 1: đặt `vim.g.have_nerd_font = true` |
| Bật format khi lưu | Section 6: thêm ngôn ngữ vào `enabled_filetypes` |
| Thêm LSP server | Section 5: thêm vào bảng `servers` |
| Bật fold theo Treesitter | Section 8: bỏ comment 2 dòng `foldexpr`/`foldmethod` |

> 💡 `init.lua` có rất nhiều comment giải thích kèm gợi ý `:help X`. Đọc trực tiếp trong file
> là cách tốt nhất để hiểu sâu — đúng tinh thần của kickstart: "đọc được từng dòng cấu hình của mình".
