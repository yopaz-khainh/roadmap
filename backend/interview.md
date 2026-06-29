# Interview Junior PHP

## 1. Ba Dự Án Tiêu Biểu

### Dự án 1: 093-KSC - Hệ thống đón con ở trường mầm non

**Loại câu hỏi:** Khó nhất về công nghệ

**Công nghệ:** Next.js, Supabase, Google API Text-to-Speech, Realtime

**Vai trò:** PG

**Lý do chọn:**

- Đây là dự án khó nhất về mặt công nghệ vì hệ thống cần cập nhật trạng thái đón trẻ gần như realtime giữa phụ huynh, giáo viên và màn hình hiển thị.
- Ngoài phần quản lý dữ liệu, hệ thống còn tích hợp Google API Text-to-Speech để phát âm thanh thông báo khi phụ huynh đến đón trẻ.

**Điểm khó:**

- Xử lý realtime để khi trạng thái đón trẻ thay đổi, các màn hình liên quan nhận được cập nhật nhanh và đúng.
- Thiết kế dữ liệu trên Supabase sao cho dễ quản lý thông tin học sinh, phụ huynh, lớp học và trạng thái đón.
- Tích hợp Google API Text-to-Speech để tạo thông báo bằng giọng nói, đồng thời kiểm soát nội dung phát ra rõ ràng và đúng thời điểm.
- Xử lý các trường hợp phát sinh như nhiều phụ huynh thao tác gần cùng lúc, mất kết nối tạm thời hoặc trạng thái đã được cập nhật ở thiết bị khác.

**Cách xử lý:**

- Chia luồng đón trẻ thành các trạng thái rõ ràng: phụ huynh gửi yêu cầu, hệ thống thông báo, giáo viên xác nhận và hoàn tất đón.
- Sử dụng realtime của Supabase để subscribe thay đổi dữ liệu và cập nhật UI ngay khi trạng thái thay đổi.
- Kiểm tra kỹ các case đồng bộ dữ liệu giữa nhiều màn hình để tránh hiển thị trạng thái cũ.
- Với phần Text-to-Speech, chuẩn hóa nội dung thông báo trước khi gọi API để âm thanh phát ra dễ hiểu và phù hợp trong môi trường trường học.

**Kinh nghiệm rút ra:**

- Với chức năng realtime, cần quan tâm đến cả tốc độ cập nhật và tính nhất quán dữ liệu giữa nhiều thiết bị.
- Khi tích hợp API bên ngoài, cần xử lý tốt lỗi, độ trễ và các trường hợp API không trả kết quả như mong muốn.
- Một chức năng kỹ thuật tốt phải phục vụ đúng ngữ cảnh sử dụng thực tế, đặc biệt với môi trường trường học cần sự rõ ràng và ổn định.

---

### Dự án 2: 108-MYP - Hệ thống cho AEON Mobile phục vụ quy trình đăng ký dịch vụ

**Loại câu hỏi:** Khó nhất về nghiệp vụ

**Công nghệ:** Typescript

**Vai trò:** PG

**Lý do chọn:**

- Đây là dự án khó nhất về nghiệp vụ vì liên quan trực tiếp đến quy trình đăng ký dịch vụ cho người dùng AEON Mobile.
- Quy trình đăng ký có nhiều bước, nhiều điều kiện kiểm tra và yêu cầu xử lý đúng theo rule của khách hàng.

**Điểm khó:**

- Hiểu đúng nghiệp vụ đăng ký dịch vụ và các điều kiện thay đổi theo từng trạng thái.
- Phân tích ảnh hưởng khi sửa một màn hình hoặc một rule vì có thể ảnh hưởng đến các bước trước và sau trong quy trình.
- Đảm bảo thông tin người dùng nhập vào được validate đúng, tránh sai sót trong quá trình đăng ký.
- Làm việc với spec và phản hồi từ khách hàng để điều chỉnh chi tiết cho đúng thực tế vận hành.

**Cách xử lý:**

- Vẽ lại luồng đăng ký theo từng bước để nắm rõ input, output và trạng thái của từng màn hình.
- Khi nhận task, kiểm tra các chức năng liên quan trước khi estimate và implement.
- Nếu spec chưa rõ, chuẩn bị câu hỏi cụ thể để xác nhận với leader hoặc khách hàng.
- Test lại theo từng scenario nghiệp vụ thay vì chỉ test từng màn hình riêng lẻ.

**Kinh nghiệm rút ra:**

- Với dự án nghiệp vụ phức tạp, lập trình chỉ là một phần; quan trọng hơn là hiểu đúng quy trình vận hành.
- Cần suy nghĩ theo góc nhìn người dùng cuối để phát hiện case thiếu hoặc bất hợp lý.
- Trước khi sửa code cần phân tích ảnh hưởng để tránh tạo regression cho các bước liên quan.

---

### Dự án 3: 097-MPM - Hệ thống quản lý hiệu suất và hoạt động hằng ngày của nhân viên và cửa hàng

**Loại câu hỏi:** Hiệu quả kinh doanh

**Công nghệ:** Typescript, Tailwind CSS

**Vai trò:** PG

**Lý do chọn:**

- Đây là dự án thể hiện hiệu quả kinh doanh rõ nhất vì hỗ trợ quản lý hiệu suất nhân viên và hoạt động hằng ngày của cửa hàng.
- Hệ thống giúp dữ liệu vận hành được tập trung hơn, giảm phụ thuộc vào trao đổi thủ công hoặc file rời rạc.

**Giá trị mang lại:**

- Quản lý có thể theo dõi tình hình hoạt động của nhân viên và cửa hàng nhanh hơn.
- Giảm thời gian tổng hợp dữ liệu thủ công, đặc biệt với các thông tin cần cập nhật hằng ngày.
- Giúp việc đánh giá hiệu suất minh bạch hơn vì dữ liệu được ghi nhận trên hệ thống.
- Hỗ trợ phát hiện vấn đề vận hành sớm hơn, từ đó cải thiện chất lượng quản lý cửa hàng.

**Đóng góp của bản thân:**

- Tham gia phát triển màn hình và xử lý logic theo yêu cầu nghiệp vụ.
- Chú ý trải nghiệm nhập liệu và hiển thị để người dùng thao tác nhanh trong công việc hằng ngày.
- Phối hợp sửa lỗi, kiểm tra UI và đảm bảo dữ liệu hiển thị đúng theo từng trường hợp.

**Kinh nghiệm rút ra:**

- Một hệ thống có hiệu quả kinh doanh tốt không chỉ cần chạy đúng mà còn phải giúp người dùng tiết kiệm thời gian.
- UI rõ ràng, dễ thao tác có thể ảnh hưởng trực tiếp đến hiệu quả vận hành.
- Khi phát triển chức năng quản lý, cần luôn đặt câu hỏi: chức năng này giúp người dùng ra quyết định hoặc làm việc nhanh hơn như thế nào?

---

## 2. Câu Hỏi Phỏng Vấn Thường Gặp

### Câu 1: Vì sao bạn ứng tuyển vị trí Junior PHP?

**Trả lời:**

- Em muốn phát triển sâu hơn theo hướng backend với PHP vì PHP được dùng nhiều trong các hệ thống web thực tế.
- Em đã có nền tảng về lập trình web, database, API và muốn được làm nhiều hơn với Laravel, MySQL/PostgreSQL, queue, authentication và các hệ thống backend thực tế.
- Với vị trí Junior, em mong muốn được tham gia dự án thật, học thêm từ senior và nâng dần khả năng phân tích, thiết kế, debug.

### Câu 2: Bạn đã từng làm gì với PHP?

**Trả lời:**

- Em đã tham gia các dự án có sử dụng PHP, ví dụ dự án CLE dùng Blade, PHP và Tailwind CSS.
- Công việc của em gồm implement màn hình, xử lý logic theo spec, validate dữ liệu, hiển thị dữ liệu từ backend và sửa bug theo feedback của QA.
- Em cũng có hiểu biết về cách PHP xử lý request, response, form submit, session, database query và mô hình MVC trong framework như Laravel.

### Câu 3: PHP là gì? PHP thường dùng để làm gì?

**Trả lời:**

- PHP là ngôn ngữ lập trình phía server, thường dùng để xây dựng website, web application và API.
- PHP nhận request từ client, xử lý logic, làm việc với database, sau đó trả về HTML, JSON hoặc response phù hợp.
- Hiện nay PHP thường được dùng cùng framework như Laravel để phát triển nhanh, có cấu trúc rõ ràng và dễ bảo trì hơn.

### Câu 4: PHP và Node.js có điểm mạnh, điểm yếu gì? Dự án nào nên dùng PHP, dự án nào nên dùng Node.js?

**Trả lời:**

- PHP mạnh ở web truyền thống, CRUD, admin dashboard, CMS, hệ thống quản lý nội bộ và các dự án cần phát triển nhanh với framework như Laravel.
- PHP có hệ sinh thái ổn định, dễ deploy, nhiều hosting hỗ trợ, Laravel cung cấp sẵn nhiều phần như routing, validation, ORM, migration, queue, authentication.
- Điểm yếu của PHP là nếu thiết kế không tốt thì code dễ bị rối trong dự án lớn; với các bài toán realtime hoặc xử lý nhiều kết nối đồng thời, PHP thường cần thêm queue, websocket server hoặc service hỗ trợ.
- Node.js mạnh ở các hệ thống realtime, chat, notification, streaming, API gateway, microservice hoặc dự án dùng JavaScript/TypeScript cả frontend và backend.
- Node.js xử lý I/O bất đồng bộ tốt, phù hợp với nhiều request nhẹ, gọi API bên ngoài hoặc cập nhật realtime.
- Điểm yếu của Node.js là nếu xử lý tác vụ CPU nặng không đúng cách có thể làm nghẽn event loop; ngoài ra hệ sinh thái package thay đổi nhanh nên cần quản lý dependency cẩn thận.
- Nếu là website quản trị, hệ thống nghiệp vụ, CRUD nhiều, yêu cầu ổn định và team mạnh Laravel thì em sẽ chọn PHP/Laravel.
- Nếu là hệ thống cần realtime nhiều như chat, notification, socket, hoặc team muốn dùng TypeScript full-stack thì em sẽ cân nhắc Node.js.
- Theo em, không có ngôn ngữ nào tốt hơn tuyệt đối; quan trọng là chọn theo yêu cầu dự án, kinh nghiệm team, chi phí vận hành và khả năng bảo trì lâu dài.

### Câu 5: Sự khác nhau giữa GET và POST là gì?

**Trả lời:**

- GET thường dùng để lấy dữ liệu, tham số thường nằm trên URL, ví dụ search hoặc filter.
- POST thường dùng để gửi dữ liệu lên server, ví dụ tạo mới user, submit form hoặc login.
- Với dữ liệu nhạy cảm hoặc dữ liệu làm thay đổi hệ thống, em sẽ ưu tiên dùng POST, PUT/PATCH hoặc DELETE tùy mục đích API.

### Câu 6: Session và Cookie khác nhau như thế nào?

**Trả lời:**

- Cookie được lưu ở phía browser, thường dùng để lưu thông tin nhỏ như token, setting hoặc session id.
- Session lưu dữ liệu ở phía server, browser thường chỉ giữ session id để server nhận biết người dùng.
- Cookie có thể bị người dùng xem hoặc chỉnh sửa, nên không nên lưu trực tiếp dữ liệu nhạy cảm nếu không mã hóa hoặc bảo vệ đúng cách.

### Câu 7: MVC là gì?

**Trả lời:**

- MVC là mô hình chia ứng dụng thành Model, View và Controller.
- Model xử lý dữ liệu và làm việc với database.
- View hiển thị giao diện cho người dùng.
- Controller nhận request, gọi Model xử lý logic và trả dữ liệu ra View hoặc JSON response.
- MVC giúp code rõ trách nhiệm hơn, dễ sửa và dễ bảo trì hơn.

### Câu 8: Bạn hiểu gì về Laravel?

**Trả lời:**

- Laravel là framework PHP theo mô hình MVC, hỗ trợ nhiều chức năng có sẵn như routing, controller, middleware, validation, Eloquent ORM, migration, queue và authentication.
- Laravel giúp phát triển ứng dụng nhanh hơn vì có cấu trúc chuẩn và nhiều công cụ hỗ trợ.
- Với Junior, em tập trung nắm chắc route, controller, request validation, model, migration, relationship và cách debug lỗi.

### Câu 9: Middleware trong Laravel dùng để làm gì?

**Trả lời:**

- Middleware dùng để xử lý request trước hoặc sau khi đi vào controller.
- Ví dụ thường gặp là kiểm tra user đã đăng nhập chưa, kiểm tra quyền truy cập, set locale hoặc ghi log request.
- Nếu request không hợp lệ, middleware có thể trả response luôn thay vì cho đi tiếp vào controller.

### Câu 10: Migration trong Laravel là gì?

**Trả lời:**

- Migration dùng để quản lý thay đổi cấu trúc database bằng code.
- Ví dụ tạo bảng, thêm cột, sửa kiểu dữ liệu hoặc tạo index.
- Migration giúp các thành viên trong team đồng bộ database dễ hơn thay vì sửa thủ công trên từng máy.

### Câu 11: Eloquent ORM là gì?

**Trả lời:**

- Eloquent là ORM của Laravel, giúp thao tác với database thông qua Model thay vì viết SQL trực tiếp trong nhiều trường hợp.
- Ví dụ bảng users sẽ có model User, từ đó có thể query, create, update, delete bằng cú pháp PHP rõ ràng.
- Eloquent cũng hỗ trợ relationship như one-to-one, one-to-many, many-to-many.

### Câu 12: Bạn biết những loại relationship nào trong database hoặc Laravel?

**Trả lời:**

- One-to-one: một user có một profile.
- One-to-many: một lớp có nhiều học sinh.
- Many-to-many: một user có nhiều role và một role có nhiều user.
- Khi làm Laravel, các quan hệ này có thể được khai báo trong Model để query dữ liệu dễ hơn.

### Câu 13: Validation là gì? Vì sao cần validation?

**Trả lời:**

- Validation là kiểm tra dữ liệu đầu vào trước khi xử lý hoặc lưu vào database.
- Cần validation để tránh dữ liệu sai, thiếu hoặc không đúng định dạng.
- Ví dụ kiểm tra email đúng format, số điện thoại không rỗng, ngày tháng hợp lệ, file đúng loại.
- Em thường muốn validate cả phía frontend để người dùng dễ thao tác và phía backend để đảm bảo an toàn dữ liệu.

### Câu 14: Bạn xử lý lỗi như thế nào khi gặp bug?

**Trả lời:**

- Đầu tiên em tái hiện lỗi theo bước QA hoặc người dùng mô tả.
- Sau đó kiểm tra log, request payload, response, dữ liệu trong database và đoạn code liên quan.
- Khi tìm được nguyên nhân, em sửa ở phạm vi nhỏ nhất có thể và test lại cả case lỗi lẫn các case liên quan để tránh regression.
- Nếu spec chưa rõ, em sẽ hỏi leader hoặc QA trước khi sửa.

### Câu 15: SQL Injection là gì? Phòng tránh như thế nào?

**Trả lời:**

- SQL Injection là lỗi bảo mật xảy ra khi dữ liệu người dùng nhập vào bị ghép trực tiếp vào câu SQL, làm kẻ tấn công có thể thay đổi ý nghĩa câu query.
- Cách phòng tránh là dùng prepared statement, query builder hoặc ORM như Eloquent.
- Ngoài ra cần validate input, không tin tưởng dữ liệu từ client và phân quyền database hợp lý.

### Câu 16: Index trong database dùng để làm gì?

**Trả lời:**

- Index giúp database tìm kiếm dữ liệu nhanh hơn trên các cột thường dùng để search, filter, join hoặc sort.
- Ví dụ cột email trong users hoặc foreign key như user_id thường nên có index.
- Tuy nhiên không nên tạo index quá nhiều vì có thể làm chậm thao tác insert/update và tăng dung lượng lưu trữ.

### Câu 17: REST API là gì?

**Trả lời:**

- REST API là cách thiết kế API dựa trên resource và HTTP method.
- Ví dụ GET dùng để lấy dữ liệu, POST để tạo mới, PUT/PATCH để cập nhật, DELETE để xóa.
- API thường trả dữ liệu dạng JSON để frontend hoặc hệ thống khác sử dụng.

### Câu 18: HTTP status code thường gặp?

**Trả lời:**

- 200: request thành công.
- 201: tạo mới dữ liệu thành công.
- 400: request không hợp lệ.
- 401: chưa đăng nhập hoặc token không hợp lệ.
- 403: không có quyền truy cập.
- 404: không tìm thấy dữ liệu.
- 422: validation lỗi.
- 500: lỗi server.

### Câu 19: Authentication và Authorization khác nhau như thế nào?

**Trả lời:**

- Authentication là xác thực người dùng là ai, ví dụ đăng nhập bằng email/password.
- Authorization là kiểm tra người dùng đó có quyền làm hành động nào không.
- Ví dụ user đã đăng nhập nhưng không phải admin thì không được truy cập màn hình quản lý admin.

### Câu 20: Bạn dùng Git như thế nào trong công việc?

**Trả lời:**

- Em dùng Git để quản lý source code, tạo branch theo task, commit thay đổi và tạo merge request/pull request.
- Trước khi commit, em kiểm tra lại file thay đổi, chạy test hoặc kiểm tra chức năng nếu có.
- Khi có conflict, em đọc kỹ phần code liên quan, xử lý conflict rồi test lại để đảm bảo không làm mất code của người khác.

### Câu 21: Khi nhận một task mới, bạn sẽ làm gì?

**Trả lời:**

- Em đọc kỹ mô tả task và spec để hiểu yêu cầu.
- Sau đó kiểm tra màn hình, API, database hoặc chức năng liên quan.
- Nếu có điểm chưa rõ, em ghi lại câu hỏi để xác nhận trước khi làm.
- Khi implement, em cố gắng chia nhỏ phần việc, tự test lại và báo cáo tiến độ nếu gặp vấn đề.

### Câu 22: Nếu task bị trễ estimate, bạn xử lý thế nào?

**Trả lời:**

- Em sẽ báo sớm cho leader, nói rõ phần nào đang vướng và lý do bị chậm.
- Nếu cần, em sẽ đề xuất hướng xử lý hoặc nhờ hỗ trợ review logic.
- Em không muốn đợi đến sát deadline mới báo vì như vậy team khó điều chỉnh kế hoạch.

### Câu 23: Bạn làm gì khi không hiểu spec?

**Trả lời:**

- Em sẽ đọc lại spec, kiểm tra màn hình liên quan và thử tự phân tích luồng nghiệp vụ trước.
- Sau đó em tổng hợp câu hỏi cụ thể, ví dụ input này lấy từ đâu, trạng thái này xử lý thế nào, case lỗi hiển thị ra sao.
- Em hạn chế hỏi chung chung, vì câu hỏi cụ thể giúp leader hoặc khách hàng trả lời nhanh hơn.

### Câu 24: Điểm mạnh của bạn là gì?

**Trả lời:**

- Điểm mạnh của em là cẩn thận, chịu khó đọc hiểu nghiệp vụ và không ngại debug.
- Khi sửa bug, em thường cố gắng hiểu nguyên nhân gốc thay vì chỉ sửa cho hết lỗi trước mắt.
- Em cũng có tinh thần học hỏi, nếu gặp công nghệ mới như realtime hoặc API bên ngoài thì em sẽ đọc tài liệu, thử từng phần nhỏ rồi áp dụng vào task.

### Câu 25: Điểm yếu của bạn là gì?

**Trả lời:**

- Điểm yếu của em là đôi khi mất nhiều thời gian ở giai đoạn đầu để hiểu kỹ nghiệp vụ hoặc kiểm tra ảnh hưởng.
- Em đang cải thiện bằng cách ghi chú lại luồng xử lý, hỏi sớm khi chưa rõ và chia task thành các phần nhỏ để tiến độ rõ ràng hơn.

### Câu 26: Bạn mong muốn học thêm gì trong thời gian tới?

**Trả lời:**

- Em muốn học sâu hơn về Laravel, thiết kế database, REST API, authentication/authorization, queue và testing.
- Ngoài ra em cũng muốn cải thiện khả năng đọc source code lớn, phân tích nghiệp vụ và viết code có cấu trúc tốt hơn.

### Câu 27: Vì sao công ty nên chọn bạn cho vị trí Junior PHP?

**Trả lời:**

- Em phù hợp với vị trí Junior PHP vì em có nền tảng web, đã tham gia dự án thực tế và hiểu quy trình làm việc trong team.
- Em không chỉ muốn code cho xong task mà còn cố gắng hiểu nghiệp vụ, test lại chức năng và phối hợp tốt với QA/reviewer.
- Em sẵn sàng học thêm PHP/Laravel sâu hơn và nhận feedback để cải thiện nhanh trong công việc.
