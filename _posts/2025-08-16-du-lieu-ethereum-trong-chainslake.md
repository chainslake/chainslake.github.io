---
title: Dữ liệu Ethereum trong Chainslake
layout: post
date: 2025-08-16 06:00
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: true
tag:
- vietnam
category: blog
author: lake
description: Ethereum là một trong những blockchain quan trọng và phổ biến nhất hiện nay, rất nhiều blockchain khác được xây dựng dựa trên công nghệ máy ảo của Ethereum, chúng được gọi là các EVM blockchain. Bài viết này sẽ giúp các bạn hiểu rõ và khai thác được dữ liệu trên Ethereum từ đó dễ dàng mở rộng sang EVM blockchain khác.
---

Trong [bài viết trước](/truy-van-du-lieu-trong-chainslake/) các bạn đã được giới thiệu về tổ chức các bảng cũng như cách truy vấn dữ liệu hiệu quả trên nền tảng Chainslake. Ở bài viết này mình sẽ chia sẻ về dữ liệu của Ethereum trong Chainslake, hiểu rõ Ethereum sẽ giúp các bạn có thể khai thác dữ liệu từ tất cả các EVM blockchain khác.

# Nội dung
1. [Dữ liệu thô trên Ethereum](#raw_tables)
2. [Giải mã dữ liệu, lấy thông tin từ contract](#decode_data)
3. [Xây dựng các bảng insight](#insight_data)
4. [Kết luận](#conclusion)

## Dữ liệu thô trên Ethereum<a name="raw_tables"></a>

Dữ liệu thô trên Ethereum gồm 3 bảng `transactions`, `logs`, `traces` đặt trong thư mục [ethereum](https://metabase.chainslake.com/browse/databases/3/schema/ethereum){:target="_blank"}.

#### Transactions

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjI4fSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/transactions.png" alt="transactions" width="100%"></a></p>

Đây là bảng chứa thông tin của tất cả các giao dịch trên Ethereum, mỗi giao dịch bao gồm:

- block_date, block_number, block_time: Thông tin về thời gian và số block của giao dịch, các column này đều được đánh index.
- hash: Giá trị hash khác nhau với mỗi giao dịch.
- index: Thứ tự của giao dịch trong block
- from: Địa chỉ gửi giao dịch
- to: Địa chỉ nhận giao dịch, có thể là 1 địa chỉ ví thông thường hoặc địa chỉ của 1 contract.
> Contract có thể hiểu đơn giản giống như 1 chương trình chạy trên máy ảo Ethereum. Mã thực thi của contract được gửi lên gửi lên blockchain thông qua 1 giao dịch gọi là Deploy. Sau khi Deploy, contract có thể thực thi các chức năng (method) mà nó được lập trình nếu nó được người dùng hoặc các contract khác gọi đến. Mỗi contract sẽ có 1 địa chỉ duy nhất, không thể xóa hoặc thay đổi logic của contract sau khi đã Deploy.
- gas_limit, gas_price, gas_used: lần lượt là lượng gas giới hạn, giá gas, lượng gas đã sử dụng trong giao dịch
> Trên Ethereum mỗi giao dịch đều cần có gas (nhiên liệu) được mua bằng ETH để thực hiện.
- success: Giao dịch có thành công hay không
> Lưu ý: Nếu giao dịch không thành công (vì bất kỳ nguyên nhân nào như lỗi, thiếu gas,...) trạng thái trước giao dịch sẽ được khôi phục, tuy nhiên địa chỉ gửi vẫn mất phí gas cho giao dịch này.
- value: Số lượng ETH được gửi kèm cùng với giao dịch (trừ trực tiếp trên địa chỉ gửi)
- method_id, data: Dữ liệu (message) đi kèm cùng giao dịch. Nếu đích đến là contract thì data là tham số đầu vào khi gọi đến các chức năng trong contract.

#### Traces

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjI3fSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/traces.png" alt="traces" width="100%"></a></p>

Khi contract thực thi một giao dịch sẽ bao gồm nhiều bước xử lý tuần tự, các bước xử lý này có thể gọi đến các function khác trong cùng contract hoặc của contract khác, đây được gọi là các internal transactions. Bảng traces lưu lại thông tin của toàn bộ các internal transaction trên Ethereum. Bao gồm các thông tin sau:

- block_date, block_number, block_time: Thông tin về thời gian và số block của giao dịch, các column này đều được đánh index.
- hash: Giá trị hash của giao dịch chính
- gas, gas_used: Gas được cấp và gas sử dụng trong internal transaction
- from, to: địa chỉ contract gửi và nhận
- input, output: Dữ liệu đầu vào và dữ liệu trả về
- method_id: id của phương thức được gọi đến
- success: Có thành công không? 
> Lưu ý: Chỉ cần 1 internal transaction thất bại thì toàn bộ transaction sẽ thất bại

#### Logs

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjE5fSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/logs.png" alt="logs" width="100%"></a></p>

Trong quá trình thực thi, một contract có thể emit ra event để ghi nhận việc dữ liệu bên trong contract đã thay đổi, dữ liệu từ các event này được lưu trong bảng logs. Bao gồm các thông tin sau:

- block_date, block_number, block_time: Thông tin về thời gian và số block của giao dịch, các column này đều được đánh index.
- contract_address: địa chỉ contract đã emit ra event
- tx_hash: hash của giao dịch chính
- topic1, topic2, topic3, topic4, data: dữ liệu bên trong event, đều được mã hóa và cần được giải mã nếu muốn sử dụng (sẽ nói rõ hơn trong phần giải mã dữ liệu).

> Lưu ý: Trong bảng logs chỉ chứa các event của các giao dịch thành công

## Giải mã dữ liệu, lấy thông tin từ contract<a name="decode_data"></a>

#### Giải mã dữ liệu
Dữ liệu trong các bảng transactions, traces, logs đều được mã hóa. VD:
- method_id, data trong bảng transactions
- method_id, input, ouput trong bảng traces
- topic1, topic2, topic3, topic4, data trong bảng logs

Để giải mã những dữ liệu này thành dạng có thể đọc hiểu được, chúng ta cần ABI (Application Binary Interface) thường được cung cấp đi kèm với mã nguồn của contract. Hãy xem 1 ví dụ về ABI của contract ERC20:

```json
[
    {
        "anonymous": false,
        "inputs": [
            {
                "indexed": true,
                "name": "from",
                "type": "address"
            },
            {
                "indexed": true,
                "name": "to",
                "type": "address"
            },
            {
                "indexed": false,
                "name": "value",
                "type": "uint256"
            }
        ],
        "name": "Transfer",
        "type": "event"
    }
]
```

Kết quả sau khi decode sẽ nhận được bảng `ethereum_decoded.erc20_evt_transfer` dưới đây:

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjE4fSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/erc20_evt_transfer.png" alt="erc20_evt_transfer" width="100%"></a></p>

Bảng được decode từ bảng logs thông qua ABI, bạn có thể xem các bảng decode khác trong thư mục `ethereum_decoded`

<p style="
    text-align: center;
"><a href="https://data.chainslake.com/#!/model/model.chainslake.ethereum_decoded.erc20_evt_transfer" target="_blank"><img src="/assets/images/posts/2025-08-15/erc20_evt_transfer_diagram.png" alt="erc20_evt_transfer_diagram" width="100%"></a></p>

#### Lấy thông tin từ contract

Để thuận tiện cho việc sử dụng thì Chainslake cần lấy thêm thông tin từ các contract sau khi decode, các bảng thông tin này được đặt trong thư mục `ethereum_contract`. Ví dụ như bảng `erc20_tokens` dưới đây:

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjExfSwidHlwZSI6InF1ZXJ5In0sImRpc3BsYXkiOiJ0YWJsZSIsInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==" target="_blank"><img src="/assets/images/posts/2025-08-15/erc20_tokens.png" alt="erc20_tokens" width="100%"></a></p>

Bảng có các thông tin như tên, symbol, total supply của contract. Contract address được lấy từ bảng `erc20_evt_transfer`:

<p style="
    text-align: center;
"><a href="https://data.chainslake.com/#!/model/model.chainslake.ethereum_contract.erc20_tokens" target="_blank"><img src="/assets/images/posts/2025-08-15/erc20_tokens_diagram.png" alt="erc20_tokens_diagram" width="100%"></a></p>


## Xây dựng các bảng insight<a name="insight_data"></a>

Các bảng dữ liệu thô, bảng decoded và bảng contract là những nguyên liệu khi kết hợp lại theo một logic nhất định sẽ được bảng insight phù hợp với mục truy vấn dữ liệu. Mình sẽ lấy ví dụ với bảng `ethereum_balances.token_transfer_hour`:

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/question#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjozLCJxdWVyeSI6eyJzb3VyY2UtdGFibGUiOjE4N30sInR5cGUiOiJxdWVyeSJ9LCJkaXNwbGF5IjoidGFibGUiLCJ2aXN1YWxpemF0aW9uX3NldHRpbmdzIjp7fX0=" target="_blank"><img src="/assets/images/posts/2025-08-15/token_transfer_hour.png" alt="token_transfer_hour" width="100%"></a></p>

Đây là bảng chứa thông tin về sự thay đổi số dư của mọi ví, mọi token trên Ethereum mỗi giờ, bao gồm các thông tin:
- block_date, block_hour: Thông tin về thời gian (được đánh index)
- wallet_address: Địa chỉ của ví
- token_address: Địa chỉ của token, 0x nếu token là Native ETH
- symbol: Biểu tượng của token
- amount: Số lượng token đã thay đổi trong ví trong 1 giờ

Để xây dựng bảng này chúng ta cần dữ liệu từ các bảng transactions, traces, erc20_evt_transfer, erc20_tokens, sơ đồ như sau:

<p style="
    text-align: center;
"><a href="https://data.chainslake.com/#!/model/model.chainslake.ethereum_balances.token_transfer_hour" target="_blank"><img src="/assets/images/posts/2025-08-15/token_transfer_hour_diagram.png" alt="token_transfer_hour_diagram" width="100%"></a></p>

Bạn có thể xem Source code của bảng này sau khi click vào hình trên, mình sẽ giải thích qua về logic của bảng này:

```sh
frequent_type=hour
list_input_tables=${chain_name}_decoded.erc20_evt_transfer,${chain_name}.traces,${chain_name}.transactions,${chain_name}_contract.erc20_tokens
output_table=${chain_name}_balances.token_transfer_hour
erc20_event_table=${chain_name}_decoded.erc20_evt_transfer
erc20_token_table=${chain_name}_contract.erc20_tokens
transactions_table=${chain_name}.transactions
traces_table=${chain_name}.traces
re_partition_by_range=block_date,key_partition
partition_by=block_date
write_mode=Append
number_index_columns=7
```

Ở phần header của Code chúng ta có các cấu hình của bảng bao gồm:
- frequent_type: Cho biết tần suất cập nhật dữ liệu của bảng 
- list_input_tables: Danh sách các bảng làm đầu vào dữ liệu
- output_table: Tên bảng được ghi ra
- erc20_event_table, erc20_token_table, transactions_table, traces_table: Gán tên cho các bảng input để sử dụng trong câu truy vấn bên dưới
- re_partition_by_range: Cho biết các column sẽ được sắp xếp
- partition_by: Cho biết column sẽ được partition
- write_mode: Chế độ ghi dữ liệu
- number_index_columns: Số column sẽ được đánh index trong bảng

Trong phần body của Code là logic xây dựng bảng được viết bằng SQL. Dữ liệu transfer token được lấy từ bảng `erc20_evt_transfer`, kết hợp cùng `erc20_tokens` để lấy tên, symbol, decimals của token sau đó nhóm theo block_hour, wallet_address, token_address để tính ra số lượng token thay đổi theo giờ. Đối với Native ETH thì cần lấy từ bảng traces (chỉ lấy giao dịch thành công) và transactions (để lấy phí giao dịch).


## Kết luận<a name="conclusion"></a>

Bài viết đến đây tuy đã khá dài, nhưng mình cũng đã giới thiệu được những phần quan trọng nhất về dữ liệu của Ethereum của Chainslake, hi vọng từ đây các bạn đã có thể tự mình khám phá và khai thác dữ liệu của Chainslake. Hẹn gặp lại!
