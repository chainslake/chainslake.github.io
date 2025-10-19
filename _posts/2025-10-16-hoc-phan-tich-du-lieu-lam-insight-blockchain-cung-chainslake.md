---
title: Học phân tích dữ liệu, làm Insight Blockchain cùng Chainslake
layout: post
date: 2025-10-16 06:00
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: true
tag:
- vietnam
category: blog
author: lake
description: Chainslake là một nền tảng chia sẻ Insight Blockchain, chúng tôi đã trích xuất và tổ chức dữ liệu blockchain thành các bảng để bạn có thể truy vấn bằng ngôn ngữ SQL, phục vụ cho các mục đích như truy xuất, nghiên cứu, phân tích dữ liệu.
---

Hiểu một cách đơn giản, blockchain là một cơ sở dữ liệu phi tập trung, được chia sẻ công khai, trong đó chứa tất cả các giao dịch đã được xác thực. Từ những giao dịch chuyển tiền đơn giản đến những giao dịch với logic cực kỳ phức tạp đều được lưu trữ vĩnh viễn, không thể thay đổi hay xóa bỏ, trở thành bằng chứng không thể chối cãi cho mỗi giao dịch. Tuy được chia sẻ công khai, nhưng dữ liệu blockchain tương đối khó để khai thác hiệu quả do blockchain vốn được thiết kế để đảm bảo an toàn và bảo mật cao chứ không phù hợp để phân tích. Để giúp tất cả mọi người có thể dễ dàng truy vấn dữ liệu blockchain cho các mục đích liên quan đến khai thác, nghiên cứu và phân tích dữ liệu, [Chainslake](https://chainslake.com/public/local) đã trích xuất và tổ chức dữ liệu blockchain thành các bảng trong Data Lake, từ đó giúp cho các nhà phân tích, nhà nghiên cứu, các Insight Creator có thể dễ dàng truy vấn bằng ngôn ngữ SQL.

## Tại sao nên học phân tích dữ liệu, làm Insight Blockhchain cùng Chainslake?

*Hiểu một cách sâu sắc và toàn diện về những gì đã diễn ra thông qua dữ liệu*. Thay vì chỉ có thể thể quan sát từng giao dịch rời rạc, bạn có một công cụ SQL mạnh mẽ cho phép truy vấn không giới hạn, giúp bạn có thể tìm ra mối liên hệ ẩn giữa các giao dịch cách xa nhau, có thể tổng hợp so sánh để phát hiện những giao dich bất thường.

*Chia sẻ Insight và xây dựng thương hiệu cá nhân*. Bạn có thể tạo ra các Insight từ dữ liệu blockchain và chia sẻ chúng cho tất cả mọi người, với những Insight chất lượng bạn có thể thu hút được nhiều sự quan tâm từ cộng đồng từ đó mở ra nhiều cơ hội cho bản thân.

*Cơ hội kiếm thêm thu nhập*. Khi bạn là một Insight Creator uy tín được nhiều người theo dõi, có nhiều Insight chất lượng, bạn có thể nhận được lời mời làm Insight cho các dự án blockchain, do các phân tích có thể dễ dàng nhúng trên bất kỳ website nào thông qua [Chainslake SDK](https://github.com/chainslake/chainslake-sdk).

## Lộ trình học tập và phát triển

*Làm quen với nền tảng Chainslake*. Bước đầu tiên, bạn sẽ học cách sử dụng ngôn ngữ SQL để truy vấn dữ liệu và xây dựng Insight trên nền tảng [Chainslake](https://chainslake.com/public/local). Ngay sau khi đăng ký tài khoản (miễn phí) bạn sẽ được tiếp cận với toàn bộ mã nguồn SQL của các Insight đang được chia sẻ trên cộng đồng, từ đó bạn có thể dễ dàng học được cách viết truy vấn, cách xây dựng Insight, dần dần tìm hiểu ý nghĩa của các bảng dữ liệu đang có [tại đây](https://data.chainslake.com/#!/overview).

*Tham gia đóng góp xây dựng các bảng dữ liệu mới*. Sau khi đã hiểu rõ và thành thạo truy vấn trên các bảng đang có sẵn, bạn có thể tham gia xây dựng, phát triển thêm các bảng dữ liệu mới để phục vụ cho nhu cầu của chính bạn và cộng đồng. Tất cả thay đổi đều sẽ phải đi qua quy trình review và kiểm thử bởi admin và cộng đồng trước khi được triển khai.

*Tự vận hành nền tảng dữ liệu của riêng mình*. Chainslake được thiết kế theo hướng phi tập trung, tối ưu để có thể chạy trên những phần cứng thông dụng, điều này cho phép người dùng (khi có đủ kinh nghiệm) có thể tự mình vận hành Data Lake cá nhân, tự do phát triển theo nhu cầu, đảm bảo sự riêng tư và bảo mật tuyệt đối mà vẫn có thể chia sẻ Insight trên nền tảng chung.

## Hướng dẫn để bắt đầu 

Đây là màn hình chính sau khi [đăng ký tài khoản](https://metabase.chainslake.com/). Từ trang này bạn có thể tạo và quản lý các Insight của mình, khám phá các bảng dữ liệu của Chainslake.

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/" target="_blank"><img src="/assets/images/posts/2025-10-16/metabase_home.png" alt="metabase_home" width="100%"></a></p>

1. Cây thư mục trong tài khoản của bạn. Bạn chỉ có thể tạo và quản lý các Insight trong thư mục cá nhân của bạn. Thư mục Demo chưa các Insight được chia sẻ chung cho tất cả mọi người, bạn chỉ có thể xem, nếu cần sửa bạn cần copy về thư mục cá nhân.
2. Dẫn đến trang cơ sở dữ liệu, tại đó bạn sẽ nhìn thấy tất cả các bảng đang có.
3. Thư mục Trash chứa các Insight hoặc Collection (thư mục) bị xóa.
4. Dẫn đến các trang để bạn có thể viết truy vấn, tạo dashboard, hoặc tạo Collection.


Đây là màn hình của một trang Insight (Demo), trong trang này có đầy đủ các công cụ để bạn có thể viết truy vấn, vẽ biểu đồ và tạo Insight:

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question/318-daily-prices?token_address=0x2260fac5e5542a773aa44fbcfedf7c193bc2c599&block_date=past30days" target="_blank"><img src="/assets/images/posts/2025-10-16/insight_demo_1.png" alt="insight_demo_1" width="100%"></a></p>

1\. URL dẫn đến trang Insight, bạn có thể sử dụng URL này để chia sẻ trên trang cá nhân của bạn [Chainslake](https://chainslake.com/home).

<p style="
    text-align: center;
"><a href="" target="_blank"><img src="/assets/images/posts/2025-10-16/insight_sharing.png" alt="insight_sharing"></a></p>

2\. Đường dẫn thư mục đến trang

3\. Tiêu đề của Insight 

4\. Các biến tham số, cho phép điều chỉnh các tham số trong câu truy vấn dữ liệu

5\. Mở mục điều chỉnh hiển thị để lựa chọn loại biểu đồ

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question/318-daily-prices?token_address=0x2260fac5e5542a773aa44fbcfedf7c193bc2c599&block_date=past30days" target="_blank"><img src="/assets/images/posts/2025-10-16/insight_demo_2.png" alt="insight_demo_2" width="100%"></a></p>

6\. Tiểu mục lựa chọn loại biểu đồ, mỗi loại biểu đồ sẽ có các cài đặt khác nhau

7\. Nút mở mục cài đặt cho loại biểu đồ đang chọn

8\. Nút thay đổi chế độ xem từ dạng biểu đồ sang dạng bảng

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question/318-daily-prices?token_address=0x2260fac5e5542a773aa44fbcfedf7c193bc2c599&block_date=past30days" target="_blank"><img src="/assets/images/posts/2025-10-16/insight_demo_3.png" alt="insight_demo_3" width="100%"></a></p>

9\. Tiểu mục cài đặt cho loại biểu đồ đang chọn. 

10\. Nút reload cho phép thực thi lại câu truy vấn để lấy dữ liệu mới về.

11\. Nút mở trang mã nguồn truy vấn SQL

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question/318-daily-prices?token_address=0x2260fac5e5542a773aa44fbcfedf7c193bc2c599&block_date=past30days" target="_blank"><img src="/assets/images/posts/2025-10-16/insight_demo_4.png" alt="insight_demo_4" width="100%"></a></p>

12\. Các nút chức năng bao gồm: format lại câu truy vấn, mở tiểu mục xem nhanh các bảng dữ liệu, mở tiểu mục cấu hình biến tham số

13\. Tiểu mục xem nhanh các bảng dữ liệu, rất hữu ích khi cần tra cứu nhanh thông tin của 1 bảng dữ liệu bất kỳ.

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question/318-daily-prices?token_address=0x2260fac5e5542a773aa44fbcfedf7c193bc2c599&block_date=past30days" target="_blank"><img src="/assets/images/posts/2025-10-16/insight_demo_5.png" alt="insight_demo_5" width="100%"></a></p>

14\. Các biến tham số đặt trong câu truy vấn, cho phép điều chỉnh lại câu truy vấn, mỗi biến được đặt trong 2 dấu ngoặc nhọn {{}}

15\. Tiểu mục cấu hình cho các biến tham số.

## Các nội dung khác nếu bạn quan tâm

1. [Giới thiệu Chainslake - Blockchain Lakehouse Framework](/gioi-thieu-chainslake-blockchain-lakehouse-framework/): Mô tả sơ lược về kiến trúc thiết kế và triển khai của Chainslake, dành cho bạn nếu muốn tự vận hành Data Lake cá nhân.
2. [Truy vấn dữ liệu trong Chainslake](/truy-van-du-lieu-trong-chainslake/): Giới thiệu tổ chức các bảng dữ liệu trong Chainslake, hướng dẫn viết truy vấn SQL hiệu quả.
3. [Dữ liệu Ethereum trong Chainslake](/du-lieu-ethereum-trong-chainslake/): Tìm hiểu sâu về các bảng dữ liệu của Ethereum, từ đó hiểu được tất cả các EVM chain khác.

## Tài liệu tham khảo

1. Trang cộng đồng chia sẻ Insight blockchain: [https://chainslake.com/](https://chainslake.com/)
2. Trang tài liệu chính: [https://docs.chainslake.com/](https://docs.chainslake.com/)
3. Trang blog chia sẻ thông tin, hướng dẫn: [https://chainslake.github.io/](https://chainslake.github.io/) 
4. Trang tài liệu kỹ thuật tổ chức dữ liệu: [https://data.chainslake.com/](https://data.chainslake.com/)

## Contacts

1. Email: [lakechain.nguyen@gmail.com](mailto:lakechain.nguyen@gmail.com)
2. Telegram: [chainslake](https://t.me/chainslake)