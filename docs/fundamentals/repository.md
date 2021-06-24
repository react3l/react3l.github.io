---
layout: default
title: Repository
parent: Fundamentals
nav_order: 3
---

# Repository

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## Mục đích

* Repository là tầng trung gian giữa tầng xử lý Business - các tác vụ của ứng dụng và tầng truy xuất dữ liệu từ Database, Back-end. Repository có vai trò xử lý dữ liệu từ backend trước khi truyền xuống tầng business, hoặc từ tầng tầng business truyền ngược lại lên back-end

![Repository](https://www.codecompiled.com/wp-content/uploads/2015/07/REPOSITORY-PATTERN-1.png)  

* Khi áp dụng tầng Repository vào trong quá trình phát triển ứng dụng, ta sẽ có được những lợi ích như
   * Giúp phần logic về xử lý dữ liệu và xử lý logic business được quản lý rõ ràng hơn
   * Kiểm thử Unit Test dễ dàng
   * Khi tầng Business thay đổi về mặt logic thì tầng Repository không bị ảnh hưởng


## HTTP APIs

### HTTP là gì
* HTTP hay Hypertext Transfer Protocol là giao thức truyền siêu văn bản của giao tiếp dữ liệu, hoạt động theo giao thức stateless (Giao thức mà Server sẽ không lưu lại dữ liệu của Client, và kết thúc giao tiếp khi đã trả kết quả về cho Client). Phía Client sẽ gửi những thông điệp yêu cầu - request cho phía Server, ngược lại, phía Server trả lại thông điệp phản hồi - response cho phía Client.

### Cấu trúc thông điệp Response, Request
* Về cấu trúc thông điệp Response và Request có những đặc điểm chung
   * Phần Header
   * Phần Body
* `Request Header`
Phần này gồm nhiều thông số quan trọng như
   * `Request URL`
   * `Request Method`: Quy định phương thức của HTTP. Một số phương thức thường dùng như
      * `GET`: Yêu cầu lấy dữ liệu từ máy chủ
      * `POST`: đăng nội dung lên máy chủ
      * `HEAD`: chỉ yêu cầu các Http headers, bỏ qua phần thân của thông điệp. Phương thức này giúp lấy các tùy chọn cấu hình của Server
      * `OPTIONS`: yêu cầu mô tả các tùy chọn giao tiếp cho tài nguyên đích, máy chủ sẽ phản hồi với danh sách các phương thức HTTP được hỗ trợ
      * `DELETE`: Phương thức giúp máy chủ xóa một nội dung, dữ liệu nào đó.
   * `Status Code`: Trạng thái của yêu cầu. Mã trạng thái được tách thành 5 loại, trong đó các chữ số đầu tiên của mã trạng thái xác định trạng thái chung của phản hồi
      * `1xx` - Informational Response: Trạng thái nhận được yêu cầu và đang xử lý phản hồi
      * `2xx` - Successful: Trạng thái đã nhận được yêu cầu, xử lý hoàn tất.
      * `3xx` - Redirection: Thông điệp yêu cầu còn thiếu thông tin
      * `4xx` - Client Error: Thông điệp yêu cầu bị sai cú pháp hoặc không được gửi đi
      * `5xx` - Server Error: Phía Server không thực hiện được yêu cầu
   * `Accept: application/json, text/plain, */*` Đây là các loại kiểu dữ liệu được chấp nhận cho các phản hồi
   * `Content-type: application/json; charset=UTF-8` - Loại dữ liệu của nội dung trong phần thân của thông điệp

* `Response Header` Thông điệp có một vài thông số như
   * `Date: Sun, 20 Jun 2021 14:08:36 GMT` - Ngày và thời gian thông điệp được gửi
   * `Content-type: application/json, charset=utf-8` - Kiểu dữ liệu của nội dung phần thân thông điệp
   * `Connection`: Tùy chọn điều khiển cho kết nối hiện tại
      * `Close`: có nghĩa là kết nối sẽ đóng sau khi hoàn thành phản hồi
      * `Keep-alive`: có nghĩa là sẽ có các thông điệp khác tiếp theo, kết nối được duy trì liên tục.
   * `Content-Length`: Độ dài phần thân của thông điệp phản hồi - tính theo byte
   * `Server`

### Thực hiện truy vấn
* Thực hiện truy vấn với phương thức GET
   * Những tham số sẽ được truyền trực tiếp trong URL của thông điệp theo một chuỗi các cặp `name=value`. Các cặp riêng lẻ `name=value` sẽ được phân tách bằng ký hiệu `&`
   * Ví dụ như: `https://abc.com.vn/server/image/crop/file-name.jpg?url=/server/file-name.jpg&x=0&y=0&width=8000&height=8000`
* Thực hiện truy vấn với phương thức POST
   * Những tham số sẽ được truyền trong phần Body của thông điệp.
   * Tùy theo Content-Type mà ta có thể truyền tham số theo kiểu mã hóa nào.
   * Ví dụ như Content-type: application/json, charset=utf-8 sẽ truyền kiểu json. Thì phần Body của thông điệp Request sẽ là 

```tsx
{
   "skip": 0,
   "take": 10,
   "id": {},
   "code": {},
   "name": {},
   "title": {},
   "priority": {},
   "content": {},
   "statusId": {},
   "creatorId": {},
   "createdAt": {}
}
```
### Sử dụng
1. *Bước 1:* Config các thông điệp Response và Request
* Bước này sẽ sử dụng các config có trong axios để định nghĩa các thông số trong thông điệp Response và Request.
* Quy định kết quả trả về trong thông điệp Response
* Ví dụ như

```tsx
//....
//Config thông điệp Request
Repository.requestInterceptor = ( 
 config: AxiosRequestConfig,
): AxiosRequestConfig => {
 const token: string = React.getGlobal<GlobalState>().user?.token;

 if (token) {
   config.headers = {
     ...config.headers,
     Authorization: `Bearer ${token}`,
     Cookie: `Token=${token}`,
   };
 }

 if (config.data instanceof FormData) {
   config.headers['Content-Type'] = 'multipart/form-data';
   return config;
 }
};

//Config thông điệp Response
Repository.responseInterceptor = (response: AxiosResponse): AxiosResponse => { 
 if (TypeChecking.isObject(response.data)) {
   response.data = deserialize(response.data);
 }

 return response;
};

//Config Status Code
Repository.errorInterceptor = async (error: AxiosError): Promise<void> => {
 if (error?.response?.status) {
   switch (error.response.status) {
     case 401:
       break;

     case 502:
       break;
   }
 }

 throw error;
};
//....
```

2. *Bước 2:* Sử dụng các API được cung cấp từ Backend
* Chỉnh sửa lại để giúp việc bảo trì, phát triển dễ dàng hơn
* Tách nhỏ các phần trong địa chỉ URL để dễ dàng sử dụng
* Ví dụ như
```tsx
API: https://abc.com.vn/server/list-banner
```
* Tách được thành 2 phần
   * Phần `BASE_URL`: https://abc.com.vn/
   * Phần `API_BANNER`: server/list-banner

3. *Bước 3:*: Thực hiện truy vấn
* Truyền tham số vào thông điệp để thực hiện truy vấn. 
* Ngoài ra cũng sử dụng thêm các kiểu dữ liệu đi kèm với các tham số truyền vào.
* Sử dụng thêm thư viện rxjs để dùng chức năng của httpObservable.
* Ví dụ như

```tsx
public list = (bannerFilter: BannerFilter): Observable<Banner[]> => {
 return this.httpObservable
   .post<Banner[]>(kebabCase(nameof(this.list)), bannerFilter)
   .pipe(
     map((response: AxiosResponse<Banner[]>) => {
       return response.data;
     }),
   );
};
```
---

## Database

## Other data sources
