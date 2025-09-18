# Đồ Án Lập Trình Mạng – Báo cáo LaTeX

## Yêu cầu môi trường
- TeX Live 2025 (hoặc tương đương)
- Trình biên dịch: XeLaTeX
- Hệ điều hành: macOS (đã kiểm thử), các hệ khác có thể dùng tương tự

## Cấu trúc dự án (rút gọn)
```
Đồ_Án_Lập_Trình_Mạng_MT/
├─ main.tex                 # Entry LaTeX chính
├─ components/              # Các phần nội dung tách file
│  ├─ title.tex
│  ├─ Section-I/main.tex
│  ├─ Section-II/main.tex
│  ├─ Section-III/main.tex
│  ├─ Section-V/main.tex
│  └─ ...
├─ image/                   # Hình minh họa
│  ├─ MoHinh/
│  ├─ sequence/
│  └─ thucnghiem/
└─ .gitignore               # Bỏ qua file build
```

## Biên dịch nhanh
Biên dịch 2 lần để cập nhật mục lục, tham chiếu:
```bash
xelatex -interaction=nonstopmode -halt-on-error main.tex && \
xelatex -interaction=nonstopmode -halt-on-error main.tex
```
Kết quả: tạo file `main.pdf` ở thư mục gốc.

### Gợi ý dùng latexmk
```bash
latexmk -xelatex -interaction=nonstopmode -halt-on-error main.tex
# Dọn dẹp file tạm
latexmk -c
```

## Phông chữ
- Mặc định đang dùng Arial (qua `fontspec` + XeLaTeX).
- Đổi phông nhanh trong `main.tex`:
```tex
\setmainfont{Arial} % thay bằng tên font hệ thống mong muốn
```
Lưu ý: Khi dùng `fontspec`, cần biên dịch bằng XeLaTeX/LuaLaTeX.

## Ngôn ngữ & mã hóa
- `babel` với tùy chọn `vietnamese` cho tiếng Việt.
- XeLaTeX xử lý UTF-8 tự nhiên, không cần `inputenc`.

## Hình ảnh
- Hình đặt trong thư mục `image/` và các thư mục con.
- Chèn hình ví dụ:
```tex
\begin{figure}[H]
  \centering
  \includegraphics[width=1\textwidth]{image/MoHinh/1.png}
  \caption{Hình ảnh ví dụ}
  \label{fig:example}
\end{figure}
```

## Danh mục cần chú ý
- Không trùng `\label{...}` cho các hình/bảng.
- Một số cảnh báo Overfull/Underfull hbox có thể xuất hiện và thường an toàn.

## Làm sạch file tạm thủ công
```bash
rm -f main.{aux,lof,lot,toc,out,loa,lol,fls,fdb_latexmk,bbl,blg,brf,thm}
```

## Bỏ qua file build trong Git
Đã cấu hình trong `.gitignore` để bỏ qua file sinh ra khi biên dịch (aux, log, pdf, ...).

## Sự cố thường gặp
- “fontspec requires XeLaTeX/LuaLaTeX”: hãy dùng `xelatex`.
- Lỗi nhãn trùng: đổi `\label{...}` cho duy nhất.
- Thiếu hình: kiểm tra đường dẫn trong `\includegraphics{...}`.

## Giấy phép
Tài liệu phục vụ cho mục đích học thuật trong khuôn khổ môn học.
