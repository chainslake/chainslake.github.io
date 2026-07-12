---
title: Giới thiệu Chainslake On-premise - Trải nghiệm dễ dàng với sự hỗ trợ AI Agent
layout: post
date: 2026-07-12 01:00
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: true
tag:
- vietnam
category: blog
author: lake
description: Chainslake On-premise là giải pháp blockchain data warehouse tại chỗ, giúp khách hàng xây dựng và vận hành hệ thống phân tích dữ liệu blockchain riêng tư ngay trên hạ tầng của mình. Với sự hỗ trợ của AI Agent, việc triển khai và vận hành trở nên đơn giản hơn bao giờ hết.
---

Chào mọi người, mình là Lake Nguyen, founder của Chainslake. Trong bài viết trước mình đã giới thiệu tổng quan về Chainslake framework và kiến trúc thiết kế. Hôm nay mình muốn nói về một khía cạnh mà mình nghĩ sẽ khiến nhiều bạn quan tâm: **làm thế nào để việc sử dụng Chainslake On-premise trở nên thật sự dễ dàng**, ngay cả khi bạn không phải là một data engineer chuyên nghiệp.

# Nội dung
1. [Chainslake On-premise là gì](#overview)
2. [Vấn đề: Tại sao lại khó?](#problem)
3. [Giải pháp: AI Agent hỗ trợ vận hành](#solution)
4. [Bắt đầu với Opencode](#opencode)
5. [Ví dụ thực tế](#example)
6. [Kết luận](#conclusion)

## Chainslake On-premise là gì <a name="overview"></a>

Nếu bạn đã đọc bài viết trước của mình, bạn sẽ biết Chainslake là một blockchain Lakehouse Framework. Phiên bản **On-premise** cho phép bạn chạy toàn bộ hệ thống trên server vật lý hoặc máy local của riêng bạn. Điều này đem lại:

- **Sở hữu hoàn toàn dữ liệu** — Dữ liệu không rời khỏi hạ tầng của bạn
- **Tùy chỉnh theo nhu cầu** — Bạn quyết định chạy chain nào, protocol nào
- **Kiểm soát chi phí** — Không phụ thuộc vào chi phí cloud tăng dần

Hệ thống bao gồm các thành phần chính: **HDFS** (lưu trữ), **Apache Spark** (xử lý dữ liệu), **Trino** (query engine), **Apache Airflow** (tự động hóa pipeline), và **Metabase** (trực quan hóa). Toàn bộ được đóng gói trong Docker, giúp triển khai nhanh chóng trên bất kỳ môi trường nào.

| Storage | Execution |
| :---: | :---: |
| <img src="/assets/images/posts/2025-08-10/Storage-layer-design.png" height="400"> | <img src="/assets/images/posts/2025-08-10/Execution-layer-design.png" height="400"> |

Tuy nhiên, việc vận hành một hệ thống Big Data với nhiều thành phần như vậy rõ ràng là không đơn giản. Và đây chính là lúc AI Agent phát huy tác dụng.

## Vấn đề: Tại sao lại khó? <a name="problem"></a>

Mình hiểu rằng nhiều khách hàng của Chainslake đến từ lĩnh vực blockchain, crypto — nơi mà thế mạnh nằm ở việc phân tích dữ liệu, phát triển protocol, hoặc xây dựng sản phẩm hơn là quản trị hạ tầng Big Data. Một số thách thức thường gặp:

- Làm sao để cài đặt hệ thống lên server?
- Pipeline nào cần chạy trước, chạy sau?
- Khi job lỗi, làm sao để debug?
- Làm sao thêm một blockchain mới vào hệ thống?
- Làm sao query dữ liệu từ các bảng đã có?

Đây đều là những câu hỏi hoàn toàn hợp lý, và trước đây bạn cần phải đọc tài liệu kỹ hoặc liên hệ support thì mới xử lý được. Nhưng giờ đây, mọi thứ đã khác.

## Giải pháp: AI Agent hỗ trợ vận hành <a name="solution"></a>

Chainslake On-premise được thiết kế đi kèm với một **AI Agent** — một trợ lý AI chuyên biệt, hiểu sâu về kiến trúc và cách vận hành hệ thống. Agent này có thể giúp bạn:

### 1. Triển khai hệ thống

Bạn chỉ cần mô tả môi trường của mình (số lượng server, dung lượng ổ cứng...), Agent tự động thực thi các lệnh cần thiết.

![AI Agent hỗ trợ cài đặt hệ thống](/assets/images/posts/2026-07-12/agent-setup-system.png)

### 2. Quản lý pipeline

Thêm blockchain mới? Tạo job mới? Sửa đổi DAG? Agent hiểu rõ convention của dự án và có thể tự động tạo các file cần thiết (`.sh`, `.sql`, DAG Python) theo đúng chuẩn mà hệ thống yêu cầu.

![AI Agent hỗ trợ quản lý pipline](/assets/images/posts/2026-07-12/agent-pipeline-manager.png)

### 3. Query và kiểm tra dữ liệu

Bạn muốn xem dữ liệu trong bảng `ethereum.transactions`? Hay kiểm tra xem job hôm nay đã chạy thành công chưa? Agent có thể viết và thực thi câu query giúp bạn.

### 4. Xử lý lỗi chủ động

Khi một job gặp lỗi, Agent sẽ tự động phân tích log, tìm nguyên nhân, đề xuất cách fix và thử lại — thay vì bạn phải mò mẫm trong hàng trăm dòng log.

### 5. Vẽ chart dựng dashboard phân tích

Bạn mô tả yêu cầu mong muốn, Agent sẽ tự động phân tích và vẽ chart, làm dashboard (trình diễn dữ liệu) trên Metabase

### 6. Tự học hỏi

Điều đặc biệt nhất: Agent có khả năng **tự ghi lại kinh nghiệm** sau mỗi lần làm việc thành công. Lần sau khi gặp cùng một nhiệm vụ, Agent có thể thực thi ngay mà không cần hỏi lại bạn. Càng sử dụng lâu, Agent càng thông thạo hệ thống của bạn.

## Bắt đầu với Opencode <a name="opencode"></a>

Để sử dụng AI Agent này, bạn cần một công cụ giao diện. Mình đang sử dụng [**Opencode**](https://opencode.ai) — một AI coding agent mã nguồn mở, **hiện đang miễn phí**.

### Tại sao là Opencode?

- **Miễn phí** — Không cần trả phí subscription, bạn có thể sử dụng ngay với mô hình Big-pickle hoàn toàn miễn phí và không giới hạn (hiện tại)
- **Mã nguồn mở** — Hoàn toàn minh bạch, cộng đồng lớn với hơn 160,000 stars trên GitHub
- **Chạy trên terminal** — Phù hợp cho các tác vụ vận hành server,DevOps
- **Hỗ trợ nhiều model AI** — Bạn có thể dùng ChatGPT, Claude, Gemini hoặc bất kỳ model nào
- **Bảo mật** — Opencode không lưu trữ code hay dữ liệu của bạn
- **Đa nền tảng** — Chạy trên macOS, Windows, Linux

### Cài đặt Opencode

Bạn chỉ cần chạy một lệnh:

```bash
curl -fsSL https://opencode.ai/install | bash
```

Sau khi cài đặt, mở terminal tại thư mục dự án Chainslake On-premise của bạn và khởi động Opencode:

```bash
cd /path/to/chainslake-onprem
opencode
```

![Giao diện Opencode trong terminal](/assets/images/posts/2026-07-12/opencode-terminal.png)

### Cấu hình Opencode cho Chainslake

Khi bạn mở Opencode trong thư mục dự án, Agent sẽ tự động đọc các file hướng dẫn (`AGENT_INSTRUCTION.md`, `README.md`, `skill/index.md`) để hiểu kiến trúc hệ thống. Bạn không cần cấu hình thêm gì phức tạp — chỉ cần đảm bảo file `AGENT_INSTRUCTION.md` nằm trong thư mục gốc của dự án (đã được bao gồm sẵn trong Chainslake On-premise).

### Bắt đầu sử dụng

Sau khi khởi động Opencode, bạn có thể bắt đầu tương tác ngay. Dưới đây là một số ví dụ:

**Cài đặt hệ thống:**
> "Hãy giúp tôi cài đặt Chainslake On-premise trên server Ubuntu 22.04 với 32GB RAM"

**Thêm pipeline mới:**
> "Tạo pipeline cho BNB Chain, bao gồm origin, extract và decode ERC-20"

**Query dữ liệu:**
> "Cho tôi xem 10 giao dịch gần nhất trong bảng ethereum.transactions"

**Kiểm tra trạng thái:**
> "Kiểm tra xem các DAG trên Airflow có đang chạy đúng không"

**Xử lý lỗi:**
> "Job extract/blocks đang bị lỗi, hãy phân tích log và giúp tôi fix"


## Ví dụ thực tế <a name="example"></a>

Để các bạn hình dung rõ hơn, mình sẽ mô tả một tình huống thực tế:

**Bạn vừa nhận server mới và muốn triển khai Chainslake On-premise:**

1. Cài Docker trên server
2. Clone repo Chainslake On-premise
3. Mở Opencode tại thư mục dự án
4. Nhờ Agent thực hiện cài đặt

Agent sẽ đọc kiến trúc hệ thống, kiểm tra server của bạn, và thực thi hoặc hướng dẫn bạn chạy từng lệnh. Nếu có lỗi xảy ra (ví dụ: Docker chưa đủ phiên bản, port bị trùng...), Agent sẽ tự động phát hiện và xử lý.

**Bạn muốn thêm dữ liệu từ một blockchain mới:**

1. Nhờ Agent tạo pipeline cho chain đó
2. Agent sẽ tạo thư mục job, file `application.properties`, các script `.sh`, file `.sql`, và DAG tương ứng
3. Bạn review lại code, confirm, rồi chạy thử
4. Agent giúp kiểm tra dữ liệu đầu ra

Toàn bộ quá trình có thể chỉ mất vài phút thay vì vài giờ nếu làm thủ công.

## Kết luận <a name="conclusion"></a>

Chainslake On-premise được thiết kế với triết lý: **dữ liệu của bạn, hệ thống của bạn, nhưng vận hành không cần phải phức tạp**. Với sự hỗ trợ của AI Agent, bạn không cần phải trở thành chuyên gia Big Data để có thể xây dựng và vận hành một blockchain data warehouse chuyên nghiệp.

Nếu bạn quan tâm đến Chainslake On-premise và muốn trải nghiệm, hãy:
- Truy cập [chainslake.com](https://chainslake.com/public/local) để xem demo
- Clone repo [chainslake-onprem](https://github.com/chainslake/chainslake-onprem) và bắt đầu với Opencode
- Liên hệ mình qua:
    - Github: [Chainslake](https://github.com/chainslake)
    - Email: [lakechain.nguyen@gmail.com](mailto:lakechain.nguyen@gmail.com)
    - Telegram: [chainslake](https://t.me/chainslake)

Rất mong nhận được phản hồi từ các bạn. Hẹn gặp lại trong các bài viết tiếp theo!
