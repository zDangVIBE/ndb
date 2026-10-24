# NgocDuyBoutique — Chuyên hàng Quảng Châu

Website **trưng bày hàng** cho shop NgocDuyBoutique (đồ likeauth, thời trang & phụ kiện Nam/Nữ, nước hoa, mỹ phẩm). Khách xem mẫu trên web rồi **nhắn Zalo 0986 491 209** để hỏi giá chốt và đặt hàng — web không có giỏ hàng, không có thanh toán.

Dữ liệu sản phẩm lưu trong `localStorage`, nên chủ shop tự thêm/sửa/xóa hàng ngay trên giao diện mà không cần database.

## Chạy dự án

Máy đã có sẵn Node.js portable trong thư mục `.node/` (không cần cài đặt gì thêm):

```bash
cd aura
# Windows PowerShell / cmd:
set PATH=%CD%\.node;%PATH%
npm run dev
# hoặc Git Bash:
export PATH="$PWD/.node:$PATH" && npm run dev
```

Truy cập **http://localhost:5173**. Nếu máy bạn đã cài Node.js ≥ 18, chỉ cần `npm install && npm run dev`.

Build bản production (file tĩnh, có thể mở bằng `npm run preview` hoặc copy thư mục `dist/` đi bất kỳ đâu):

```bash
npm run build
npm run preview
```

## Cách khách mua hàng

1. Duyệt hàng ở `#/shop` hoặc trang chi tiết sản phẩm.
2. Bấm **Nhắn Zalo hỏi mua** (hoặc **Nhắn Zalo đặt hàng** ở drawer) → web soạn sẵn tin nhắn gồm tên mẫu, size/màu, số lượng, tạm tính và chỗ điền tên/sđt/địa chỉ; tin nhắn được **chép vào clipboard** rồi tự mở `zalo.me/0986491209` — khách chỉ việc dán.
3. Bấm icon ❤ để lưu vào **Danh sách quan tâm** (drawer góc phải) nếu muốn hỏi nhiều món một lượt.

Giá trên web là giá tham khảo; shop xác nhận còn hàng và báo giá chốt qua Zalo. Gửi COD toàn quốc.

## Cấu trúc

```
aura/
├── index.html
├── vite.config.js
├── public/images/          # Ảnh sản phẩm & banner (hoạt động offline)
└── src/
    ├── main.jsx            # Điểm khởi động: HashRouter + StoreProvider
    ├── App.jsx             # Layout, routes, overlay toàn cục
    ├── index.css           # Design tokens (Tailwind v4 @theme), animation
    ├── context/StoreContext.jsx   # State trung tâm: products, danh sách quan tâm, toast
    ├── data/products.js    # Danh mục sản phẩm khởi đầu (đang để trống)
    ├── lib/
    │   ├── constants.js    # Danh mục, size, số Zalo + link nhóm, mật khẩu admin
    │   ├── zalo.js         # Soạn tin nhắn Zalo + mở cuộc chat (kèm clipboard)
    │   └── utils.js        # VND, slug, ảnh nén, localStorage
    ├── components/
    │   ├── layout/         # Header (marquee, nav, mobile menu), Footer
    │   ├── shop/           # ProductCard (hover đổi ảnh, lưu quan tâm)
    │   ├── cart/           # CartDrawer = "Danh sách quan tâm" + CTA Zalo
    │   ├── search/         # SearchModal (tìm không dấu, live result)
    │   └── ui/             # Button, Modal, Accordion, Reveal, Toaster…
    └── pages/
        ├── Home.jsx        # Hero, marquee, giới thiệu shop, nhóm Zalo, danh mục, hàng mới
        ├── Shop.jsx        # Lọc danh mục/giá/size qua URL param + sắp xếp
        ├── ProductDetail.jsx # Gallery, size/dung tích/màu, nút Zalo, accordion, related
        └── Admin.jsx       # Mật khẩu, đăng/sửa/xóa sản phẩm, chỉnh tồn kho
```

## Trang quản trị

- Đường dẫn: `#/admin` (icon người dùng trên header hoặc link "Quản trị" dưới footer).
- Mật khẩu: **`ngocduy2026`** — đổi trong `src/lib/constants.js` (`ADMIN_PASSWORD`).
- Đăng sản phẩm: tên, danh mục, giá, giá gạch, tồn kho, nhãn, size (quần áo) hoặc dung tích (nước hoa/mỹ phẩm, tự nhập), màu, ảnh (dán link hoặc tải file lên — tự nén 900px JPEG), mô tả, chất liệu, bảo quản, bật "nổi bật" để hiện ở trang chủ.
- Sửa tồn kho và trạng thái **Đang bán / Nháp** ngay trên bảng; **Xoá hết sản phẩm** khi cần làm lại từ đầu.
- Cửa hàng khởi đầu **trống** — chủ shop tự thêm hàng thật.

## Tùy chỉnh nhanh

| Muốn đổi | Sửa ở |
|---|---|
| Số Zalo, link group, SĐT liên hệ | `src/lib/constants.js` (`ZALO_PHONE`, `ZALO_URL`, `ZALO_GROUPS`) |
| Tên thương hiệu, logo | `Header.jsx`, `Footer.jsx`, `index.html` |
| Bảng màu, font | `@theme` trong `src/index.css` |
| Danh mục / size / dung tích | `src/lib/constants.js` (`PRODUCT_CATEGORIES`, `APPAREL_SIZES`, `VOLUME_PRESETS`) |
| Sản phẩm mẫu (để ai cũng thấy) | `src/data/products.js` |
| Ảnh hero / editorial | `public/images/brand-hero.png`, `campaign.jpg` |

Lưu ý quan trọng: dữ liệu thêm trong trang Quản trị chỉ lưu trên **trình duyệt của người nhập** (localStorage); khách truy cập từ máy khác sẽ thấy cửa hàng trống. Muốn mọi người cùng thấy kho hàng thật, cần nhập sẵn sản phẩm vào `src/data/products.js` rồi build lại, hoặc nối một backend/database.
