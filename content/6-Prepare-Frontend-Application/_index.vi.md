+++
title = "Chuẩn bị ứng dụng Frontend"
date = 2025
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

![frontend-overview.png](/images/6-Prepare-Frontend-Application/6.1.png)

#### Ứng dụng Frontend

Frontend của ứng dụng blog là một giao diện web được xây dựng bằng **React** và **Vite**, cho phép người dùng xem danh sách bài viết và tạo bài viết mới thông qua API Gateway. Frontend sẽ được host trên S3 và phân phối qua CloudFront (các bước sau).

#### Frontend trong Workshop
Trong phần này, chúng ta sẽ:
- Tạo một dự án React/Vite.
- Cấu hình ứng dụng để gọi API Gateway (`GET /posts`, `POST /posts`).
- Thêm các thành phần giao diện cơ bản (danh sách bài viết, form tạo bài viết).
- Đóng gói ứng dụng để triển khai lên S3.

#### Lợi ích khi sử dụng React/Vite:
- **Hiệu suất cao**: Vite cung cấp thời gian build nhanh và trải nghiệm phát triển mượt mà.
- **Phổ biến**: React có cộng đồng lớn, dễ dàng tích hợp với API RESTful.
- **Tái sử dụng**: Các component React giúp xây dựng giao diện linh hoạt và dễ bảo trì.
- **Tương thích với S3/CloudFront**: Dễ dàng triển khai ứng dụng tĩnh.

#### Giá của Frontend
- **Chi phí phát triển**: Miễn phí (sử dụng công cụ mã nguồn mở như React, Vite).
- **Chi phí triển khai**: Tính trong S3 và CloudFront (bước 7, 8). Free Tier cung cấp:
  - S3: 5GB lưu trữ, 20.000 yêu cầu GET, 2.000 yêu cầu PUT miễn phí/tháng.
  - CloudFront: 1TB dữ liệu truyền đi/tháng miễn phí.

#### Workshop này nên chọn cấu hình nào?
- Chúng ta sẽ sử dụng **React** với **Vite** vì:
  - Vite có tốc độ build nhanh, phù hợp cho ứng dụng nhỏ như blog.
  - React dễ tích hợp với API Gateway qua thư viện `axios` hoặc `fetch`.
  - Cấu trúc đơn giản, dễ triển khai lên S3 dưới dạng static site.

#### Chuẩn bị ứng dụng Frontend

1. **Cài đặt môi trường phát triển**
   - Đảm bảo đã cài **Node.js** (khuyến nghị phiên bản 20.x) và **npm**.
   - Cài Vite và tạo dự án React:
     ```bash
     npm create vite@latest blog-frontend -- --template react
     cd blog-frontend
     npm install
     ```

   ![create-vite.png](/images/6-Prepare-Frontend-Application/6.2.png)

2. **Cài đặt thư viện bổ sung**
   - Cài `axios` để gọi API Gateway:
     ```bash
     npm install axios
     ```

   ![install-axios.png](/images/6-Prepare-Frontend-Application/6.3.png)

3. **Cấu hình mã nguồn**
   - Thay nội dung file `src/App.jsx` bằng mã sau để hiển thị danh sách bài viết và form tạo bài viết:
     ```jsx
     import { useState, useEffect } from 'react';
     import axios from 'axios';
     import './App.css';

     const API_URL = '<Invoke URL>/posts'; // Thay bằng Invoke URL từ bước 5

     function App() {
       const [posts, setPosts] = useState([]);
       const [title, setTitle] = useState('');
       const [content, setContent] = useState('');

       // Lấy danh sách bài viết
       useEffect(() => {
         axios.get(API_URL)
           .then(response => setPosts(response.data))
           .catch(error => console.error('Error fetching posts:', error));
       }, []);

       // Gửi bài viết mới
       const handleSubmit = async (e) => {
         e.preventDefault();
         try {
           await axios.post(API_URL, {
             postId: Date.now().toString(),
             title,
             content
           });
           setTitle('');
           setContent('');
           // Làm mới danh sách bài viết
           const response = await axios.get(API_URL);
           setPosts(response.data);
         } catch (error) {
           console.error('Error creating post:', error);
         }
       };

       return (
         <div>
           <h1>Serverless Blog</h1>
           <h2>Create Post</h2>
           <form onSubmit={handleSubmit}>
             <input
               type="text"
               value={title}
               onChange={(e) => setTitle(e.target.value)}
               placeholder="Title"
               required
             />
             <textarea
               value={content}
               onChange={(e) => setContent(e.target.value)}
               placeholder="Content"
               required
             />
             <button type="submit">Create Post</button>
           </form>
           <h2>Posts</h2>
           <ul>
             {posts.map(post => (
               <li key={post.postId.S}>
                 <h3>{post.title.S}</h3>
                 <p>{post.content.S}</p>
                 <small>{post.createdAt.S}</small>
               </li>
             ))}
           </ul>
         </div>
       );
     }

     export default App;
     ```

   - Cập nhật file `src/App.css` để thêm style cơ bản:
     ```css
     body {
       font-family: Arial, sans-serif;
       max-width: 800px;
       margin: 0 auto;
       padding: 20px;
     }
     input, textarea {
       display: block;
       width: 100%;
       margin: 10px 0;
       padding: 8px;
     }
     button {
       padding: 8px 16px;
       background-color: #007bff;
       color: white;
       border: none;
       cursor: pointer;
     }
     button:hover {
       background-color: #0056b3;
     }
     ul {
       list-style: none;
       padding: 0;
     }
     li {
       margin-bottom: 20px;
       border-bottom: 1px solid #ddd;
       padding-bottom: 10px;
     }
     ```

   ![code-app.png](/images/6-Prepare-Frontend-Application/6.4.png)

4. **Cấu hình API URL**
   - Thay `<Invoke URL>` trong `src/App.jsx` bằng **Invoke URL** từ bước 5 (API Gateway), ví dụ: `https://<api-id>.execute-api.<region>.amazonaws.com/prod/posts`.

   ![configure-api-url.png](/images/6-Prepare-Frontend-Application/6.5.png)

5. **Kiểm tra ứng dụng**
   - Chạy ứng dụng cục bộ:
     ```bash
     npm run dev
     ```
   - Mở trình duyệt tại `http://localhost:5173` để kiểm tra:
     - Xem danh sách bài viết từ API Gateway (`GET /posts`).
     - Tạo bài viết mới qua form (`POST /posts`).

   - Nếu gặp lỗi (ví dụ: CORS hoặc API không phản hồi):
     - Kiểm tra **Invoke URL** và CORS trong API Gateway.
     - Xem console log trong trình duyệt hoặc CloudWatch Logs của Lambda.

6. **Đóng gói ứng dụng**
   - Build ứng dụng để tạo các file tĩnh:
     ```bash
     npm run build
     ```
   - Kết quả sẽ nằm trong thư mục `dist/`, sẵn sàng để upload lên S3.

{{% notice warning %}}
- Đảm bảo **Invoke URL** trong `src/App.jsx` khớp với URL từ API Gateway.
- Kiểm tra CORS trong API Gateway để tránh lỗi khi gọi API từ trình duyệt.
- File trong thư mục `dist/` phải được upload lên S3 ở bước tiếp theo.
{{% /notice %}}

#### Hoàn thành
- Bạn đã tạo và cấu hình ứng dụng frontend React/Vite, sẵn sàng để triển khai lên S3.
