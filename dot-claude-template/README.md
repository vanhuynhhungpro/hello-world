# Bộ khung `.claude/` chuẩn — Ông Chú Vibe Coding

Bộ khung này dùng để copy vào **bất kỳ repo sản phẩm nào** trước khi bán ra
(app tài chính, app gia phả, app quản lý kho...). Mục tiêu:

1. Giữ Antigravity / Claude Code không bị **drift** (tự bịa schema, route,
   business logic không có trong tài liệu gốc).
2. Đảm bảo mọi sản phẩm đều đạt chuẩn bảo mật tối thiểu (RLS, không lộ
   service_role key...) trước khi handover cho khách.
3. Khách mua source về, mở Antigravity/Claude Code lên là có ngay context
   đúng chuẩn, không cần anh ngồi giải thích lại từ đầu.

## Cách dùng cho sản phẩm mới

```bash
cp -r dot-claude-template/.claude ten-repo-moi/.claude
```

1. Mở `.claude/rules/04-product-specific.md` → điền thông tin riêng của
   sản phẩm (tên, đối tượng dùng, business logic đặc thù, có multi-tenant
   hay không).
2. Nếu sản phẩm có tài liệu 5-file quen thuộc (RULES.md, SRS.md,
   ARCHITECTURE.md, SITEMAP.md, SCHEMA.md) → để chúng ở root repo, các rule
   trong `.claude/rules/` sẽ tham chiếu tới đó.
3. Trước khi đóng gói bán / handover: chạy `.claude/scripts/verify.sh`.

## Cấu trúc

```
.claude/
  commands/
    model.md          # nhắc lại tech-stack contract, chống drift khi agent quên
    qa.md              # chạy checklist QA nhanh trước khi báo "xong"
    new-feature.md     # khởi tạo 1 feature mới đúng quy trình 5-tài-liệu
  rules/
    00-core.md          # stack bắt buộc, coding convention
    01-architecture.md  # cấu trúc thư mục, server actions vs API routes
    02-security-rls.md  # RLS, secrets — không được bỏ qua
    03-anti-drift.md     # nguyên tắc chống agent tự bịa
    04-product-specific.md # điền riêng cho từng sản phẩm
  scripts/
    verify.sh           # build + lint + check RLS/secrets trước khi ship
  skills/
    qa-verify/
      SKILL.md           # skill để Claude tự chạy verify trước handover
```

Bộ này là **base dùng chung** — mỗi sản phẩm chỉ sửa `04-product-specific.md`,
không sửa các file còn lại trừ khi có lý do kỹ thuật rõ ràng (để giữ chuẩn
đồng nhất giữa các sản phẩm, dễ maintain khi bán nhiều repo cùng lúc).
