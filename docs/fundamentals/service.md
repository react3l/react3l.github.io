---
layout: default
title: Service
parent: Fundamentals
nav_order: 2
---

# Service

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## Mục đích
* Đảm nhiệm toàn bộ phần xử lý Logic Business trong ứng dụng.
* Nhận dữ liệu về từ Repository sẽ được xử lý để phục trong tầng Service. Những thông tin sau khi được tầng Service xử lý có thể được truyền lên Repository để trả lại về Backend.

## Sử dụng
* Mỗi chức năng sẽ được viết thành 1 function có thể truyền tham số hoặc không.
* Đi kèm với tham số truyền vào sẽ cần kiểu dữ liệu tương ứng.
* Kết quả trả về cũng cần phải được gán kiểu dữ liệu.
* Ví dụ

```tsx
//...

public useBanners(
 list: (
   bannerFilter: BannerFilter,
 ) => Observable<Banner[]> = bannerRepository.list,
): [Banner[], boolean, () => void] {   //Kiểu dữ liệu cho kết quả trả về
 const [bannerList, setBannerList] = React.useState<Banner[]>([]);

 const [loading, setLoading] = React.useState<boolean>(false);

 const [subscription] = commonService.useSubscription();

 const handleRefresh = React.useCallback(() => {
   subscription.add(
     list({...new BannerFilter(), take: DEFAULT_TAKE})
       .pipe(
         finalize(() => {
           setLoading(false);
         }),
       )
       .subscribe(
         (bannerList: Banner[]) => {
           setBannerList(bannerList);
         },
         () => {},
       ),
   );
 }, [list, subscription]);

 React.useEffect(() => {
   setLoading(true);
   handleRefresh();
 }, [handleRefresh]);

 return [bannerList, loading, handleRefresh];
}

//...

```
---

## Business layer

