---
title: Phân tích vụ hack Bybit từ dữ liệu Onchain
layout: post
date: 2026-02-21 14:13:35
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: true
tag:
- vietnam
category: blog
author: lake
description: Phân tích dữ liệu từ vụ hack Bybit xảy ra ngày 21 tháng 2 năm 2025 gây thiệt hại 1.3 tỷ đô, tìm hiểu điều gì đã xảy ra, số tiền bị hack đã đi đâu, hacker là ai.
---

`14:13:35 21-02-2025 UTC` Từ một giao dịch nghiệp vụ thông thường đã khởi đầu cho một chuỗi sự kiện dẫn đến thảm họa trấn động thế giới Crypto. Bybit - Sàn giao dịch Crypto tập trung lớn thứ 2 thế giới, chịu trách nhiệm xử lý 13,06% giao dịch, phục vụ 50 triệu người dùng toàn cầu đã bị đánh cắp tài sản số với giá trị kỷ lục **1.5 tỷ USD**, gấp 3 lần kỷ lục trước đó trong vụ hack Ronin (03/2022). Điều gì đã xảy ra? Ai là kẻ đứng sau vụ việc này? Số tiền bị đánh cắp đã đi đâu? Hãy cùng với Chainslake lật lại lịch sử blockchain để tìm câu trả lời nhé.

# Nội dung
1. [Tóm tắt sự cố](#overview)
2. [Phân tích dữ liệu sự cố ](#incident_analytics)
3. [Phân tích ví hacker](#wallet_analytics)
4. [Phân tích dòng tiền sau sự cố ](#cash_flow_analytics)

## Tóm tắt sự cố <a name="overview"></a>

- *15h20 ngày 21/02/2025*: Trên Telegram của thám tử On-chain [ZachXBT](t.me/investigations) đã thông báo về việc có một lượng tài sản trị giá tới 1.5 tỷ USD vừa được rút khỏi sàn Bybit.

<p style="
    text-align: center;
"><img src="/assets/images/posts/2026-02-21/first_alert.png" alt="First alert" width="50%"></p>

Ngay sau đó, các thám tử On-chain khác đã vào cuộc điều tra và công bố những thông tin đầu tiên về các tài sản đã bị rút bao gồm:

    - 401,347 ETH ~ $1.12B
    - 90,376 stETH ~ $253.16M
    - 15,000 cmETH ~ $44.13M
    - 8,000 mETH ~ $23M

- *15h44 ngày 21/02/2025*: CEO Bybit Ben Zhou đã lên tiếng trên X để thông báo về vụ việc và trấn an người dùng. Ít phút sau, kênh thông tin chính thức của Bybit trên X cũng đã xác nhận sự cố bị đánh cắp tài sản số.

<p style="
    text-align: center;
"><img src="/assets/images/posts/2026-02-21/ceo_alert.png" alt="CEO alert" width="50%"></p>

- *19h09 ngày 21/02/2025*: [ZachXBT](t.me/investigations) đã cung cấp bằng chứng cho thấy LAZARUS GROUP chính là nhóm hacker đã thực hiện vụ tấn công này.

Kể từ đây, cuộc chạy đua tẩu tán tài sản của nhóm Hacker và nỗ lực phong tỏa của các cơ quan chức năng chính thức bắt đầu, đến thời điểm hiện tại mới chỉ có khoảng 150-200 triệu USD được phong tỏa để thu hồi lại, phần còn lại 1.3 tỷ USD vẫn nằm trong sự kiểm soát của Hacker.

## Phân tích dữ liệu sự cố <a name="incident_analytics"></a>

Bây giờ chúng ta sẽ cùng mở lại lịch sử giao dịch trên blockchain để tìm hiểu sâu hơn về sự cố này. Đầu tiên mình sẽ thực hiện một số truy vấn để tìm các giao dịch mà hacker đã thực hiện việc rút tiền:

```sql
SELECT
  block_date, block_number, block_time, tx_hash, "from", "to", value
FROM
  ethereum.traces
WHERE
  block_date = date '2025-02-21'
  AND TO = '0x47666fab8bd0ac7003bce3f5c3585383f09486e2' -- Hacker wallet
ORDER BY
  VALUE DESC
LIMIT
  5
```

Kết quả:

```csv
block_date         , block_number, block_time                   , tx_hash                                                           , from                                      , to                                        , value
"February 21, 2025", "21,895,251", "February 21, 2025, 2:16 PM" , 0xb61413c495fdad6114a7aa863a00b2e3c28945979a10885b12b30316ea9f072c, 0x1db92e2eebc8e0c075a02bea49a2935bcd2dfcf4, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2, "401,346,768,858,404,671,846,374"
"February 21, 2025", "21,895,771", "February 21, 2025, 4:00 PM" , 0x1ca49be8ed3ff5d579d505d9fe49f5a7feaffa0a5538ce34dc30052eaafc2846, 0x3d0dcc4c78009477003b038be734a2dc54748cdf, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2, "100,000,000,000,000,000"
"February 21, 2025", "21,895,762", "February 21, 2025, 3:58 PM" , 0xb4196ce328cb9d39a0b27fa9ce095062ff27cd7021d3959eda6d0c87dc88ec2a, 0x62a631ec4e541e7b7de90e7be01f09b88f671268, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2, "50,000,000,000,000,000"
"February 21, 2025", "21,895,783", "February 21, 2025, 4:03 PM" , 0xb2838baa1e5680c762f9ed41b0b9ce25738ff80314391f56eaa195c0f9e6fd6a, 0xdba4493c76c78fe45a6130f03331e63d27777777, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2, "10,000,000,000,000,000"
```

=> Giao dịch rút ETH: [0xb61413c495fdad6114a7aa863a00b2e3c28945979a10885b12b30316ea9f072c](https://etherscan.io/tx/0xb61413c495fdad6114a7aa863a00b2e3c28945979a10885b12b30316ea9f072c)

```sql
SELECT
  block_date, block_number, block_time, contract_address, tx_hash, "from", "to", value
FROM
  ethereum_decoded.erc20_evt_transfer
WHERE
  block_date = date '2025-02-21'
  AND contract_address = '0xae7ab96520de3a18e5e111b5eaab095312d7fe84' -- Contract address of stETH
  AND TO = '0x47666fab8bd0ac7003bce3f5c3585383f09486e2' -- Hacker wallet
LIMIT
  5
```

Kết quả

```csv
block_date         , block_number, block_time                  , contract_address                          , tx_hash                                                           , from                                      , to                                        , value
"February 21, 2025", "21,895,251", "February 21, 2025, 2:16 PM", 0xae7ab96520de3a18e5e111b5eaab095312d7fe84, 0xa284a1bc4c7e0379c924c73fcea1067068635507254b03ebbbd3f4e222c1fae0, 0x1db92e2eebc8e0c075a02bea49a2935bcd2dfcf4, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2, 90375547907685258392043
"February 21, 2025", "21,895,382", "February 21, 2025, 2:42 PM", 0xae7ab96520de3a18e5e111b5eaab095312d7fe84, 0xd82be7510e417f977d1742d423cfe27ec8e3a5d82f31fb9af672307131d2b625, 0xa4b24033f4c6d70133e4557c2c32a3fb693e449e, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2,          10000100000000
"February 21, 2025", "21,895,423", "February 21, 2025, 2:50 PM", 0xae7ab96520de3a18e5e111b5eaab095312d7fe84, 0xe17af52a5245c8259bc85fee4e55999ccb72f5d4516a89df38c3c3efc0e87bd9, 0xa4b24033f4c6d70133e4557c2c32a3fb693e449e, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2,          15000000000000
"February 21, 2025", "21,895,542", "February 21, 2025, 3:14 PM", 0xae7ab96520de3a18e5e111b5eaab095312d7fe84, 0x47af2063268fe9e48db7bdcdaf7f0c2547296db635827728034d96f06f796cd0, 0xa4b24033f4c6d70133e4557c2c32a3fb693e449e, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2,          13037554791768
"February 21, 2025", "21,895,973", "February 21, 2025, 4:41 PM", 0xae7ab96520de3a18e5e111b5eaab095312d7fe84, 0xc526bf70ca356a4c53aebd341cbdfbae291d6fd779d2a1a6856950215013e650, 0xa7f5d0c2b6dad000abc3a5f0db7386041ef27bb8, 0x47666fab8bd0ac7003bce3f5c3585383f09486e2,        2000000000000000
```
=> Giao dịch rút stETH: [0xa284a1bc4c7e0379c924c73fcea1067068635507254b03ebbbd3f4e222c1fae0](https://etherscan.io/tx/0xa284a1bc4c7e0379c924c73fcea1067068635507254b03ebbbd3f4e222c1fae0)

Tương tự tìm ra các giao dịch còn lại:

- Giao dịch rút cmETH: [0x847b8403e8a4816a4de1e63db321705cdb6f998fb01ab58f653b863fda988647](https://etherscan.io/tx/0x847b8403e8a4816a4de1e63db321705cdb6f998fb01ab58f653b863fda988647)
- Giao dịch rút mETH: [0xbcf316f5835362b7f1586215173cc8b294f5499c60c029a3de6318bf25ca7b20](https://etherscan.io/tx/0xbcf316f5835362b7f1586215173cc8b294f5499c60c029a3de6318bf25ca7b20)


Cả 4 giao dịch rút tiền này đều được thực hiện từ cùng 1 ví [0x0fa...d2e](https://etherscan.io/address/0x0fa09c3a328792253f8dee7116848723b72a6d2e), gọi đến các function *sweepETH* và *sweepERC20* của contract [Bybit cold wallet](https://etherscan.io/address/0x1db92e2eebc8e0c075a02bea49a2935bcd2dfcf4). 

<p style="
    text-align: center;
"><img src="/assets/images/posts/2026-02-21/screen1.png" alt="CEO alert" width="100%"></p>


2 function này cho phép hacker có thể rút toàn bộ tiền từ contract, tuy nhiên 2 function này chỉ được thực hiện nếu ví gọi có quyền Admin hoặc Owner, điều này chứng tỏ hacker đã chiếm được quyền Owner của contract trước khi sự cố xảy ra.

Kiểm tra các giao dịch gửi đến contract này trước khi sự cố xảy ra, ta thấy rằng Hacker đã thực hiện 2 giao dịch:

<p style="
    text-align: center;
"><img src="/assets/images/posts/2026-02-21/screen2.png" alt="CEO alert" width="100%"></p>

Giao dịch đầu tiên [0x46...82](https://etherscan.io/tx/0x46deef0f52e3a983b67abf4714448a41dd7ffd6d32d32da69d62081c68ad7882) là một giao dịch gọi đến function *execTransaction*. Giao dịch này đã thực hiện một *delegatecall* đến một contract do Hacker đã triển khai từ trước.

<p style="
    text-align: center;
"><img src="/assets/images/posts/2026-02-21/screen3.png" alt="CEO alert" width="100%"></p>

> delegatecall là một lệnh gọi từ 1 contract đến một contract khác và thực thi logic trong contract được gọi tới nhưng với dữ liệu của contract hiện tại. Trong trường hợp này contract của Bybit đã gọi đến contract của Hacker và thực thi mã độc làm thay đổi dữ liệu (ở đây là quyền Owner) của contract Bybit.

Theo công bố từ Bybit, Hacker đã thâm nhập và làm thay đổi giao diện hiển thị trên trình duyệt của nhân viên Bybit. Thay vì ký một giao dịch nghiệp vụ thông thường, họ đã bị lừa ký lên giao dịch mà Hacker đã chuẩn bị sẵn, từ đó khiến cho quyền Onwer của contract rơi vào tay Hacker. Sau khi có quyền Owner, chỉ 3 phút sau, Hacker đã tiến hành rút toàn bộ số tiền trong ví lạnh khiến cho các nhân viên của Bybit không kịp trở tay.


## Phân tích ví hacker <a name="wallet_analytics"></a>

Mặc dù việc tạo ví trên Ethereum là hoàn toàn offline và ẩn danh, tuy nhiên để có thể thực hiện bất kỳ giao dịch nào, mỗi ví cần được nạp 1 số lượng ETH ban đầu để sử dụng làm phí gas, giao dịch nạp tiền lần đầu này được gọi là giao dịch *Fund*, và ví thực hiện giao dịch *Fund* gọi là ví *Creator*. Việc theo dõi các giao dịch *Fund* và truy vết ví *Creator* là một manh mối cực kỳ quan trọng để điều tra về nguồn gốc của một ví bất kỳ. 

Để thuận tiện cho việc điều tra nguồn gốc của ví, Chainslake đã xây dựng bảng *ethereum_address.creator*, cho phép điều tra nguồn gốc của bất kỳ ví nào. 

```sql
SELECT
  *
FROM
  ethereum_address.creator
WHERE
  address = '0x0fa09c3a328792253f8dee7116848723b72a6d2e' -- hacker wallet
```

Kết quả

```csv
path_id                                                    , address                                   , creator                                   , address_id, creator_id                                               , ancestor_id, level, block_date         , block_number, block_time                  , tx_hash                                                           , tx_index
401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946.1.2, 0x0fa09c3a328792253f8dee7116848723b72a6d2e, 0xe8b36709dd86893bf7bb78a7f9746b826f0e8c84,          2, 401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946.1,         401,    18, "February 18, 2025", "21,873,979", "February 18, 2025, 2:53 PM", 0x0f7998d0563163b86df4a5f1eb8f23fc755e1873e14bded71e1c8ade58cb5419,      137
```
Giải thích:
- `creator`: địa chỉ của ví mà đã thực hiện giao dịch *fund* cho ví hacker
- `tx_hash`, `block_date`, `block_time`, `block_number`: Transaction hash và thời gian của giao dịch *fund*
- `path_id`: Đường dẫn đến ví hacker

Bằng cách sử dụng `path_id` ta có thể truy ngược lại nguồn gốc của ví hacker thông qua bảng *ethereum_address.path*

```sql
SELECT
  *
FROM
  ethereum_address.path
WHERE
  path_id = '401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946.1.2'
  OR path_id = '401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946.1'
  OR path_id = '401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946'
  OR path_id = '401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695'
ORDER BY
  level DESC
```

Kết quả

```csv
path_id                                                    , address                                   , creator                                   , address_id , creator_id                                               , ancestor_id, level, block_date         , block_number, block_time                  , tx_hash                                                           , tx_index
401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946.1.2, 0x0fa09c3a328792253f8dee7116848723b72a6d2e, 0xe8b36709dd86893bf7bb78a7f9746b826f0e8c84, 2          , 401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946.1,         401,    18, "February 18, 2025", "21,873,979", "February 18, 2025, 2:53 PM", 0x0f7998d0563163b86df4a5f1eb8f23fc755e1873e14bded71e1c8ade58cb5419,      137
401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946.1  , 0xe8b36709dd86893bf7bb78a7f9746b826f0e8c84, 0x3b48fa59c2bbdf8d00d70ac40b2cda576fc519e3, 1          , 401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946  ,         401,    17, "February 18, 2025", "21,873,968", "February 18, 2025, 2:51 PM", 0x2d44b15f7ba8c870bb960faf7edb0131d03c399849f063c1736a20959c8a7f40,      265
401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946    , 0x3b48fa59c2bbdf8d00d70ac40b2cda576fc519e3, 0x21a31ee1afc51d94c2efccaa2092ad1028285549, "1,356,946", 401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695          ,         401,    16, "February 18, 2025", "21,873,780", "February 18, 2025, 2:13 PM", 0x0afa81cc9c0b0bfc4a9cd46c33bcdecf58199513e7c051e5a9df1617c211f69f,       19
401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695            , 0x21a31ee1afc51d94c2efccaa2092ad1028285549, 0x00799bbc833d5b168f0410312d2a8fd9e0e3079c, 695        , 401.2.2.1.2251.3.1.1.948.186.1.2.213.349348              ,         401,    15, "April 22, 2021"   , "12,288,434", "April 22, 2021, 6:34 AM"   , 0x66f8ad7ed22c30a2cb36d8400a5fba2be3d3ad1e26b29ca95c7d200fc4544f86,      264
```

Ta thấy rằng 3 ví của hacker đều được *fund* vào ngày 18/2/2025 (3 ngày trước cuộc tấn công) với ví nguồn là [0x21...49](https://etherscan.io/address/0x21a31ee1afc51d94c2efccaa2092ad1028285549), đây là 1 ví thuộc về sàn Binance, điều này chứng tỏ Hacker đã rút ETH từ sàn Binance để thưc hiện cuộc tấn công. Cần lưu ý rằng Binance là sàn giao dịch tập trung có yêu cầu KYC, việc Hacker sử dụng ETH rút từ sàn này cho thấy Hacker đã đánh cắp được thông tin của một user nào đó trên Binance hoặc giả mạo KYC để có thể thực hiện rút tiền.

Tiếp tục mở rộng để điều tra các ví có chung nguồn gốc với ví hacker:

```sql
SELECT
  block_date,
  count(*) AS number_wallets
FROM
  ethereum_address.path
WHERE
  path_id LIKE '401.2.2.1.2251.3.1.1.948.186.1.2.213.349348.695.1356946.%'
GROUP BY
  block_date
```

Kết quả (click vào ảnh để xem biểu đồ)

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/public/question/20f5c26c-4915-4a86-a938-c12598990ed2" target="_blank"><img src="/assets/images/posts/2026-02-21/screen4.png" alt="number_wallets" width="100%"></a></p>

Có thể thấy rằng ngay sau vụ tấn công, Hacker đã thực hiện chuyển số tiền bị đánh cắp tới hàng ngàn ví mới để né tránh sự truy vết, từ đó đưa lên các sàn giao dịch hoặc máy trộn để xóa dấu vết và rửa tiền.

## Phân tích dòng tiền sau sự cố <a name="cash_flow_analytics"></a>

Sau sự cố, điều quan tâm lớn nhất của cộng đồng là theo dõi số tiền bị đánh cắp để từ đó tìm cách phong tỏa, ngăn chặn Hacker rửa tiền. Để thuận lợi cho việc theo dõi dòng tiền trong sự cố này, Chainslake đã xây dựng bảng `ethereum_balances.bybit_exploiter` thống kê tất cả các giao dịch chuyển ETH từ nhóm ví của Hacker ra các địa chỉ khác. Dưới đây là biểu đồ thống kê lượng ETH đã bị chuyển ra ngoài hàng ngày.

```sql
SELECT
  block_date,
  sum(VALUE) / pow (10, 18) AS out_value
FROM
  ethereum_balances.bybit_exploiter
GROUP BY
  block_date
```

Kết quả (click vào ảnh để xem biểu đồ)

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/public/question/7b8c3f87-d4d8-40e1-9af3-80a7c466ca8d" target="_blank"><img src="/assets/images/posts/2026-02-21/screen5.png" alt="out_flow" width="100%"></a></p>

Kiểm tra một số giao dịch lớn nhất:

```sql
SELECT
  block_date, tx_hash,
  VALUE / pow (10, 18) AS out_value
FROM
  ethereum_balances.bybit_exploiter
ORDER BY
  VALUE
LIMIT
  5
```

Kết quả

```csv
block_date,tx_hash,out_value
"March 9, 2025",0x6419b9050363b5507ab2ae9785d7416235979b1f12330220ff2d586bc7ffe9cc,"-2,632.66"
"March 9, 2025",0x344c0da4370023996a1a9c6ecd7d24f1160a7b21abb15b07dbc3b33ef3cb2ab0,"-1,353.67"
"March 6, 2025",0x9b1ad0f967a06891e444477fbd254bea40179acd074ea697a305a2b4e91cbe86,"-1,225.33"
"March 3, 2025",0x6d1b5e62b456212bd3ffa2eabe93734c14391fe15cc47232e3f3e03705e5db72,"-1,076.44"
"March 3, 2025",0xae3722a1e1b54fec0aa1329f73dae7ee5a89d9b50fcd6c565a72107a0d1a3e63,"-1,003.1"
```

(to be continue)


