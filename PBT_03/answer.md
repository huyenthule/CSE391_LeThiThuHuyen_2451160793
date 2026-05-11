# PHẦN A — ĐỌC HIỂU CSS

---

# Câu A1 — Ba cách nhúng CSS

## 1) Inline CSS

CSS được viết trực tiếp ngay trong thẻ HTML thông qua thuộc tính `style`.

Ví dụ:

```html
<h1 style="color:red; font-size:24px;">Tiêu đề</h1>
```

Đặc điểm:

* Viết nhanh
* Tác động trực tiếp lên đúng phần tử đó
* Độ ưu tiên rất cao

Tuy nhiên:

* Khó bảo trì nếu project lớn
* Không tái sử dụng được
* HTML và CSS bị trộn lẫn làm code khó đọc

Thông thường chỉ nên dùng khi:

* test nhanh giao diện
* debug
* style riêng cho một element đặc biệt

---

## 2) Internal CSS

CSS được đặt bên trong cặp thẻ `<style>` ở file HTML.

```html
<head>
    <style>
        h1{
            color:red;
        }

        .btn{
            background:blue;
        }
    </style>
</head>
```

Ưu điểm:

* Không cần file CSS riêng
* Dễ viết cho trang đơn giản
* Toàn bộ style nằm cùng một file

Hạn chế:

* Không dùng lại được giữa nhiều trang
* File HTML dễ dài và rối
* Browser không cache riêng CSS

Phù hợp với:

* bài thực hành nhỏ
* prototype
* trang đơn lẻ

---

## 3) External CSS

Đây là cách phổ biến nhất trong thực tế.

HTML:

```html
<head>
    <link rel="stylesheet" href="styles.css">
</head>
```

CSS được tách riêng thành file `.css`.

Điểm mạnh:

* Dễ bảo trì
* Tái sử dụng cho nhiều trang
* HTML sạch hơn
* Browser cache được file CSS

Nhược điểm:

* Cần thêm request để tải file CSS
* Nếu CSS load chậm thì giao diện có thể bị mất style tạm thời

Đây là cách gần như luôn được dùng trong production.

---

## Nếu một phần tử nhận cả 3 loại CSS thì cái nào ưu tiên?

Inline CSS sẽ thắng.

Thứ tự ưu tiên:

```text
Inline > Internal / External
```

Ví dụ:

```html
<h1 style="color:red;">
```

Dù bên ngoài có:

```css
h1{
    color:blue;
}
```

thì chữ vẫn màu đỏ vì inline có specificity cao hơn.

Ngoài ra:

* Internal và External có mức ưu tiên ngang nhau
* Nếu selector giống nhau thì rule khai báo sau sẽ được áp dụng

---

# Câu A2 — CSS Selectors

HTML được cho:

```html
<div id="app">
    <header class="top-bar dark">
        <h1>ShopTLU</h1>

        <nav>
            <a href="/" class="active">Home</a>
            <a href="/products">Products</a>
            <a href="/about">About</a>
        </nav>
    </header>

    <main>

        <article class="product">
            <h2>iPhone 16</h2>

            <p class="price">25.990.000đ</p>
            <p>Mô tả sản phẩm...</p>
        </article>

        <article class="product featured">
            <h2>MacBook Pro</h2>

            <p class="price">45.990.000đ</p>
            <p>Mô tả sản phẩm...</p>
        </article>

    </main>
</div>
```

---

## Selector `h1`

Rule này tác động tới toàn bộ thẻ `<h1>` trong tài liệu.

Trong đoạn HTML chỉ có một thẻ:

```html
<h1>ShopTLU</h1>
```

=> Kết quả:

```text
ShopTLU
```

---

## Selector `.price`

Dấu `.` biểu thị class selector.

CSS sẽ tìm tất cả element chứa class `price`.

Có 2 phần tử thỏa mãn:

```text
25.990.000đ
45.990.000đ
```

---

## Selector `#app header`

Đây là descendant selector.

Nó tìm thẻ `<header>` nằm bên trong phần tử có id `app`.

Element được chọn:

```html
<header class="top-bar dark">
```

---

## Selector `nav a:first-child`

Pseudo-class `:first-child` chọn phần tử đầu tiên.

Trong `<nav>`, thẻ `<a>` đầu tiên là:

```text
Home
```

---

## Selector `.product.featured h2`

Element phải đồng thời có:

* class `product`
* class `featured`

Chỉ article thứ hai thỏa điều kiện.

Do đó `<h2>` được chọn là:

```text
MacBook Pro
```

---

## Selector `article > p`

Dấu `>` nghĩa là con trực tiếp.

Tất cả thẻ `<p>` nằm trực tiếp trong `<article>` đều được match.

Danh sách kết quả:

```text
25.990.000đ
Mô tả sản phẩm...
45.990.000đ
Mô tả sản phẩm...
```

---

## Selector `a[href="/"]`

Đây là attribute selector.

CSS sẽ tìm thẻ `<a>` có:

```html
href="/"
```

Kết quả cuối cùng:

```text
Home
```

---

## Selector `.top-bar.dark h1`

Phần tử cha phải có đồng thời:

* top-bar
* dark

Sau đó chọn `<h1>` bên trong.

Text nhận được:

```text
ShopTLU
```

---

# Câu A3 — Box Model

## Trường hợp 1 — content-box

```css
.box-1{
    width:400px;
    padding:20px;
    border:5px solid black;
    margin:10px;
}
```

Với `content-box`:

* width chỉ tính phần content
* padding và border cộng thêm ra ngoài

Chiều rộng render:

```text
400 + 40 + 10 = 450px
```

Không gian chiếm trên trang:

```text
450 + 20 = 470px
```

(vì margin trái/phải thêm tổng 20px)

---

## Trường hợp 2 — border-box

```css
.box-2{
    box-sizing:border-box;
    width:400px;
    padding:20px;
    border:5px solid black;
}
```

Lúc này:

```text
Tổng width luôn = 400px
```

Padding và border bị “co vào trong”.

Content thực tế:

```text
400 - 40 - 10 = 350px
```

Nếu cộng margin:

```text
400 + 20 = 420px
```

---

## Margin Collapse

```css
.box-a{
    margin-bottom:25px;
}

.box-b{
    margin-top:40px;
}
```

Khoảng cách cuối cùng KHÔNG phải:

```text
25 + 40
```

mà chỉ là:

```text
40px
```

CSS sẽ lấy margin lớn hơn.

Đây gọi là:

```text
Margin Collapse
```

Hiện tượng này chỉ xảy ra với margin dọc.

---

# Câu A4 — Specificity

CSS:

```css
p{
    color:black;
}

.price{
    color:blue;
}

#main-price{
    color:red;
}

p.price{
    color:green;
}
```

HTML:

```html
<p class="price" id="main-price">
```

---

## Specificity của từng rule

| Rule | Selector    | Score   |
| ---- | ----------- | ------- |
| A    | p           | (0,0,1) |
| B    | .price      | (0,1,0) |
| C    | #main-price | (1,0,0) |
| D    | p.price     | (0,1,1) |

---

## Rule nào thắng?

Rule C thắng vì selector chứa ID.

Màu cuối cùng:

```text
red
```

---

## Nếu thêm inline style

```html
style="color:orange"
```

Thì kết quả chuyển thành:

```text
orange
```

Inline có ưu tiên cao hơn selector thông thường.

---

## Nếu dùng !important

```css
p{
    color:black !important;
}
```

Kết quả cuối cùng:

```text
black
```

`!important` override specificity thông thường.

---

# PHẦN C — DEBUG CSS

---

# Câu C1 — Debug Layout

CSS:

```css
.sidebar{
    width:300px;
    padding:20px;
    border:1px solid #ccc;
}

.content{
    width:660px;
    padding:30px;
    border:1px solid #ccc;
}
```

---

## Tính kích thước thực tế

### Sidebar

```text
300 + 40 + 2 = 342px
```

### Content

```text
660 + 60 + 2 = 722px
```

Tổng:

```text
342 + 722 = 1064px
```

Container chỉ rộng:

```text
960px
```

=> layout bị vỡ.

---

## Nguyên nhân

Mặc định CSS dùng:

```text
box-sizing: content-box
```

Nghĩa là:

* width KHÔNG bao gồm padding
* width KHÔNG bao gồm border

Do đó kích thước thực tế lớn hơn giá trị đã khai báo.

---

## Cách sửa 1

Dùng `border-box`.

```css
*{
    box-sizing:border-box;
}
```

Lúc này:

```text
300 + 660 = 960px
```

Layout sẽ vừa container.

---

## Cách sửa 2

Giảm width thủ công.

Ví dụ:

```css
.sidebar{
    width:258px;
}
```

vì:

```text
258 + 40 + 2 = 300px
```

---

# Câu C2 — Cascade

CSS:

```css
body{
    font-size:16px;
    color:#333;
}

.container{
    font-size:14px;
}

.card{
    color:blue;
}

.card .title{
    font-size:20px;
}

.card p{
    color:inherit;
}

#featured .title{
    color:red;
}

.highlight{
    color:green !important;
}
```

---

## "Sản phẩm A"

Element:

```html
<h2 class="title highlight">
```

Kết quả:

```text
font-size:20px
color:green
```

Giải thích:

* `.card .title` đặt font-size
* `.highlight` có `!important`
* `!important` thắng rule màu đỏ từ `#featured`

---

## "Mô tả sản phẩm"

Thẻ `<p>` thường bên trong `.card`.

Kết quả:

```text
font-size:14px
color:blue
```

Lý do:

* font-size inherit từ `.container`
* `color:inherit` lấy màu từ `.card`

---

## "Sản phẩm B"

Kết quả:

```text
font-size:20px
color:blue
```

Không có ID nên không bị rule đỏ tác động.

---

## "Mô tả sản phẩm B"

Element có class `highlight`.

Do đó:

```text
color:green
```

vì `!important` có độ ưu tiên cao nhất.

---

# Tổng kết

Qua các bài trên có thể thấy:

* Selector quyết định phần tử nào bị tác động
* Specificity quyết định rule nào thắng
* Box Model ảnh hưởng trực tiếp tới layout
* Cascade và inheritance quyết định style cuối cùng của element

Trong thực tế, phần lớn lỗi giao diện CSS đều liên quan tới:

* tính toán box model
* specificity conflict
* inheritance ngoài ý muốn
* margin collapse
* width thực tế lớn hơn container
