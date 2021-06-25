---
layout: default
title: Presentation
parent: Fundamentals
nav_order: 1
---

# Presentation

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## Atomic design
![atomic_design](/assets/images/atomic-design.png)
- Atomic design là một “phương pháp” thiết kế giao mà ở đó thiết kế được dựa trên việc phân tách và kết hợp các thành phần lại với nhau thay vì thiết kế toàn bộ.
- Mục tiêu: Tạo nên các hệ thống thống nhất, bền vững và có tính tái sử dụng cao.
- Các level trong atomic design bao gồm: Atoms, Molecules, Organisms, Templates, Pages/Screen.

### Atom components
- Atom là các component nhỏ nhất có thể, VD các button, title, input hay font, animation. Chúng có thể được đặt vào bất kỳ bối cảnh nào, toàn cục hay bên trong các component khác.
- Atoms nên được viết mà không sử dụng margin và position.
- Ví dụ: Button, Icon, Title,...

### Molecule components
- Molecule là tập hợp một hay nhiều component của atom.
- Molecule có thể có các thuộc tính của chính nó và tạo ra các hàm mà được dùng bởi atom, trong khi atom sẽ không có bất kỳ hàm hay action nào cả.
- Ví dụ: Input Box, Line Block,...
### Organism components
- Organism là tập hợp nhiều atom, molecule làm việc cùng nhau. Ở mức này, các component bắt đầu có hình dáng cụ thể, nhưng chúng vẫn đảm bảo phải được độc lập, dễ dàng di chuyển và tái sử dụng với bất kỳ nội dung nào.
- Chỉ có molecules và organisms có thể thiết lập position của atom, nhưng chúng cũng có thể không có bất kỳ position hay margin nào.
- Ví dụ: Main Tab Bar, Header, Confirm Box,...

### Template components
- Template sẽ thiết lập ngữ cảnh, xây dựng mối quan hệ giữa các organism và các component khác bằng cách chỉ định các vị trí, sắp xếp và xây dựng khuôn mẫu cho page, nhưng chưa có style, color hay component được render.
- Ví dụ: DefaultLayout, Modal Layout,...

### Page / Screen
- Screen có nhiệm vụ hoàn chỉnh template đã được định nghĩa với các styles, nội dung và logic tương ứng.

