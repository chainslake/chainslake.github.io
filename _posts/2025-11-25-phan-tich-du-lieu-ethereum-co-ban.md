---
title: Phân tích dữ liệu Ethereum cơ bản
layout: post
date: 2025-11-25 06:00
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: true
tag:
- vietnam
category: blog
author: lake
description: Ethereum là một trong những blockchain quan trọng và phổ biến nhất hiện nay, rất nhiều blockchain khác được xây dựng dựa trên công nghệ máy ảo của Ethereum, chúng được gọi là các EVM blockchain. Hiểu rõ về Ethereum bạn sẽ dễ dàng hiểu được tất cả các EVM blockchain khác.
---

## Khái niệm và kiến thức cơ bản về Ethereum <a name="introduction"></a>

*Là một blockchain:* Giống như [Bitcoin](/phan-tich-du-lieu-bitcoin/), Ethereum kế thừa đầy đủ các tính chất cơ bản của một blockchain như là:
- Tính phi tập trung: Không cần một bên trung gian đáng tin cậy, mọi giao dịch đều được chia sẻ và xác thực bởi tất cả các bên tham gia, không ai có toàn quyền chi phối mạng lưới.
- Tính bất biến: Mọi giao dịch đã xảy ra đều được lưu trữ vĩnh viễn trong mỗi bản sao blockchain của tất cả các bên, không thể thay đổi, không thể đảo ngược.
- Tính ẩn danh và bảo mật: Địa chỉ tài khoản trong Ethereum cũng được tạo ra bằng thuật toán mã hóa, hoàn toàn offline do đó đảm bảo ẩn danh và bảo mật. Hiện nay có rất nhiều thiết bị, phần mềm, ứng dụng hỗ trợ việc tạo và quản lý địa chỉ tài khoản trên Ethereum như: Ledger, Metamask... Tuy nhiên hãy luôn nhớ bảo vệ và sao lưu Seed key, private key của bạn vì sẽ không thể lấy lại tiền nếu làm mất chúng.

*Thuật toán đồng thuận:* Để thêm block mới vào blockchain, Ethereum cũng sử dụng một thuật toán đồng thuận. Tuy nhiên khác với Bitcoin, thuật toán đồng thuận của Ethereum không yêu cầu các bên phải cạnh tranh tìm lời giải cho mỗi block (Proof of Work) như Bitcoin, thay vào đó mỗi bên muốn tham gia vào quá trình xác thực giao dịch (được gọi là Validator) đều phải cược (staking) 32 ETH cho mỗi Validator, thuật toán sẽ lựa chọn ngẫu nhiên 1 Validator để làm Block Proposer, người sẽ xác thực các giao dịch và thêm block mới vào blockchain. Cơ chế này của Ethereum được gọi là Proof-of-Stake.

*Phần thưởng:* Cũng giống như thợ đào trong Bitcoin, các Validator của Ethereum sẽ nhận được phần thưởng khi thêm block mới vào blockchain. Tuy nhiên phần thưởng sẽ không phải là cố định cho mỗi block mã sẽ phụ thuộc vào số lượng Validator tham gia. Càng nhiều Validator tham gia mạng lưới thì tổng số phần thưởng sẽ tăng lên tuy nhiên lợi nhuận trên mỗi ETH staking lại giảm đi, điều này khuyến khích có nhiều Validator tham gia để tăng cường bảo mật cho mạng lưới nhưng cũng hạn chế tính trạng một bên nắm giữ nhiều Validator dẫn đến làm giảm sự phi tập trung của mạng lưới. Hiện tại mức lợi nhuận tính trên mỗi ETH staking là khoảng 4.5%/năm.

*Phí gas:* Bên cạnh phần thưởng, Validator còn nhận được phí gas cho mỗi giao dịch. Để thực hiện xác thực 1 giao dịch, Validator sẽ cần tiêu tốn một lượng tài nguyên tính toán và bộ nhớ nhất định, Ethereum sử dụng Gas để đo lường lượng tài nguyên này, giao dịch càng phức tạp thì sẽ càng tốn Gas. Tuy nhiên Validator chỉ nhận được 1 phần phí Gas, phần còn lại sẽ bị đốt (burn) điều này sẽ giúp cân bằng lại với lượng ETH được tạo mới, từ đó giữ cho nguồn cung ETH ổn định và kiểm soát lạm phát.

Nếu như Bitcoin chỉ cho phép thực hiện các giao dịch đơn giản, khả năng lập trình rất hạn chế, thì ở Ethereum với sự xuất hiện của EVM đã cho phép các nhà phát triển có thể viết các chương trình chạy trên nó, từ đó mở ra nhiều ứng dụng đa dạng cho công nghệ này.

*Máy ảo Ethereum (EVM):* Gọi là máy ảo là bởi vì bản chất của nó là một phần mềm, tuy nhiên phần mềm này đặc biệt ở chỗ nó cho phép các chương trình chạy trên nó. Cũng giống như ở Bitcoin, phần mềm máy ảo EVM được chạy trên máy tính của tất cả các bên tham gia mạng lưới (gọi là các Validator). Nhiệm vụ của Validator cũng giống như thợ đào (Miner) của Bitcoin là xác thực giao dịch để nhận phần thưởng 

*Hợp đồng thông minh (Smart contract):* Là chương trình được cài đặt trên máy ảo Ethererum, Smart contract thường được viết bằng ngôn ngữ Solidity sau đó được biên dịch thành ngôn ngữ máy (byte code) của EVM, từ đó EVM mới có thể thực thi được. Ví dụ:

```solidity
pragma solidity ^0.8.0;

contract HelloWorld {

    string private message = "Hello, World!";

    // Event bắn ra khi message thay đổi
    event MessageUpdated(string oldMessage, string newMessage, address updatedBy);

    // Hàm cập nhật message
    function setMessage(string calldata newMessage) external {
        string memory old = message;
        message = newMessage;

        emit MessageUpdated(old, newMessage, msg.sender);
    }

    // Hàm lấy message
    function getMessage() external view returns (string memory) {
        return message;
    }
}
```
- Deploy Smart Contract: Sau khi được biên dịch thành Byte code, hợp đồng thông minh sẽ được cài đặt lên blockchain của Ethereum thông qua một giao dịch, quá trình này gọi là Deploy Smart Contract. Sau khi deploy, Smart contract sẽ có một địa chỉ xác định và nằm vĩnh viễn trên blockchain (không thể xóa được). Địa chỉ của Smart Contract có định dạng giống như địa chỉ của tài khoản user (0x...) tuy nhiên địa chỉ này không có private key, do đó không thể thực hiện giao dịch từ Smart Contract mà chỉ có thể tương tác với nó thông qua các function được lập trình sẵn.
- Dữ liệu của Smart Contract: Mỗi Contract có dữ liệu riêng của nó, và do chính contract đó quản lý, toàn bộ giá trị dữ liệu của contract tại một thời điểm sẽ tạo nên trạng thái của Contract tại thời điểm đó.
- Trạng thái của Smart Contract: Sau khi deploy, Smart Contract ở trạng thái đầu tiên, trong ví dụ trên thì message có giá trị "Hello, World!". Sau khi thực hiện một giao dịch tương tác với contract này, ví dụ như gọi function setMessage, trạng thái của Smart Contract sẽ thay đổi (message nhận giá trị mới). Có những function chỉ đơn giản là lấy thông tin hiện tại của Smart Contract sẽ không làm thay đổi trạng thái của nó gọi là các view function.
- Event của contract: Để tiện cho việc theo dõi sự thay đổi trạng thái của Contract, các lập trình viên thường (không bắt buộc) định nghĩa các Event tương ứng cho các thay đổi này và emit chúng khi sự thay đổi được thực hiện. Bạn cần lưu ý điều này vì việc phân tích dữ liệu của Smart contract sẽ chủ yếu dựa trên các event phát ra từ smart contract đó.
- ABI của contract: Dữ liệu giao dịch, dữ liệu event log từ Smart Contract đều được mã hóa trước khi đóng vào block, để có thể giải mã những dữ liệu này, chúng ta cần một bản thiết kế ABI của contract. ABI sẽ được tạo ra trong quá trình biên dịch Smart Contract, dưới đây là ABI của contract HelloWorld trong ví dụ của chúng ta. 

```json
[
  {
    "anonymous": false,
    "inputs": [
      {
        "indexed": false,
        "internalType": "string",
        "name": "oldMessage",
        "type": "string"
      },
      {
        "indexed": false,
        "internalType": "string",
        "name": "newMessage",
        "type": "string"
      },
      {
        "indexed": true,
        "internalType": "address",
        "name": "updatedBy",
        "type": "address"
      }
    ],
    "name": "MessageUpdated",
    "type": "event"
  },
  {
    "inputs": [],
    "name": "getMessage",
    "outputs": [
      {
        "internalType": "string",
        "name": "",
        "type": "string"
      }
    ],
    "stateMutability": "view",
    "type": "function"
  },
  {
    "inputs": [
      {
        "internalType": "string",
        "name": "newMessage",
        "type": "string"
      }
    ],
    "name": "setMessage",
    "outputs": [],
    "stateMutability": "nonpayable",
    "type": "function"
  }
]
```
Trong ABI sẽ định nghĩa các function, event của contract, từ đó giúp cho người dùng và các ứng dụng có thể dễ dàng tương tác và hiểu được dữ liệu của contract.

*Decentralized Application (DApp):* Không chỉ là smart contract, DApp là một ứng dụng hoàn chỉnh bao gồm cả giao diện tương tác người dùng (web, mobile app), xử lý logic trong smart contract, có thể có cả server backend. DApp ẩn đi sự phức tạp của công nghệ, giúp người dùng dễ dàng làm việc với Smart Contract thông qua các tương tác trên giao diện

*Ethereum Standard:* Là các mô tả thiết kế tiêu chuẩn được sử dụng phổ biến trên Ethereum. Khi xây dựng Smart Contract tuân thủ các tiêu chuẩn này sẽ giúp nhiều DApp có thể làm việc với các Contract này. Một số tiêu chuẩn được sử dụng phổ biến nhất như [ERC20](https://eips.ethereum.org/EIPS/eip-20) - Tiêu chuẩn cho các Token trên Ethereum, [ERC721](https://eips.ethereum.org/EIPS/eip-721) và [ERC1155](https://eips.ethereum.org/EIPS/eip-1155) - Tiêu chuẩn cho NFT...

*Upgradeable Contract:* Sau khi 1 contract được deploy lên blockchain, nó sẽ tồn tại vĩnh viễn và không ai có thể thay đổi logic của contract, điều này đem lại sự tin cậy tuyệt đối vào logic đã lập trình của contract. Tuy nhiên nếu trong trường hợp có lỗ hổng bảo mật hoặc contract cần nâng cấp tính năng mới, các lập trình viên buộc phải triển khai một hợp đồng mới, sau đó migrate dữ liệu từ hợp đồng cũ sang hợp đồng mới, thay đổi logic của các hợp đồng khác gọi đến nó. Vấn đề này có thể được giải quyết bằng cách sử dụng kiến trúc Upgradeable gồm có 2 contract, 1 contract quản lý dữ liệu được gọi là proxy và 1 contract implement logic. Khi một yêu cầu được gửi đến proxy contract, nó sẽ sử dụng DELEGATECALL để gọi logic của implementation contract và thực thi trên dữ liệu của proxy contract. Nếu cần thay đổi logic chỉ cần viết một implementation contract mới và trỏ contract proxy vào contract mới. 

Lưu ý: Khi làm việc với 1 contract bất kỳ, bạn cần kiểm tra xem contract đó có phải là Upgradeable không, nếu có thì điều đó có nghĩa là các logic trong hợp đồng có thể bị thay đổi.

## Phân tích dữ liệu Ethereum cùng Chainslake <a name="analyst"></a>

Bạn có thể xem lại bài viết này nếu cần: [Học phân tích dữ liệu, làm Insight blockchain cùng Chainslake](/hoc-phan-tich-du-lieu-lam-insight-blockchain-cung-chainslake/)

### Phân tích thô

*Phân tích transactions*

```sql
select * from ethereum.transactions where block_date = date '2025-12-05' order by block_number desc limit 5
```

Kết quả:

```csv
block_date        , block_number, block_time                  , hash                                                              , updated_time               , block_hash                                                        , method_id , from                                      , to                                        , gas_limit  , gas_price   , gas_used , index, nonce    , success, type, value                  , data
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", 0x540e23b2631558b860c8cdf4ad7560870ae0a82c548e4845431b218531b8e943, "December 6, 2025, 1:41 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56,           , 0x8a605b28f3ed0a60671b2798e1c2ac098255bab1, 0x30c8d8686eb19619c858d0f62f062285c3ace233, "21,200"   , "25,606,970", "21,000" ,   105, 3        , true   , 0x2 , "1,000,000,000"        , 0x
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", 0x3b4357b1a1326612517e6e678a9da104d0c243d87595a8b0c7644d0dd7bf4663, "December 6, 2025, 1:41 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56,           , 0xeb943c230218e0da7b2ca18b81f7eb7fbbbe9665, 0xc07cc26e8e4c5543fd6a681152ca935a576b696f, "21,000"   , "24,298,180", "21,000" ,   125, "127,646", true   , 0x2 , "9,000,432,691,413,001", 0x
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", 0x7d898f9e4a3a0e75af2bb6ebccfdb3fcb0bca242415ca66850df8aa8ae0bdb71, "December 6, 2025, 1:41 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56, 0x00000000, 0xa1de68d07945b1ff1222a73418fa295cf02ed102, 0x6745024462abf2c049f54b381c4ad4a8c8d999c4, "1,000,000", "55,447,795", "185,386",    42, "9,902"  , true   , 0x2 , "269,223,515"          , 0x000000000000000000000000000003803b800b8017000000000000ec0ac8188fbf2df70020006004a6be7f7c6c454b364cda446ea39be9e5e4369de8010300000c812c00200020481629b0259e6e5c315b8eea09fd1a4d0a26291f980103000108012c002000200c4b618087dae7765823bc47ffbf38c8ee8489f5ca0103000128812c
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", 0xc440588f6498bfe79a9767f95cd3caab01490256474258434c2842009932c2c8, "December 6, 2025, 1:41 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56, 0x0965d04b, 0x818e97cefabb0f86736d20cda82a713e35bf8eb1, 0xf81377c3f03996fde219c90ed87a54c23dc480b3, "5,000,000", "31,856,051", "73,722" ,    64, "7,520"  , false  , 0x2 , 0                      , 0x0965d04b00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000704f497df754d5536efcf1b5d43bdbfa7d81b4c08ed2109b9fcbdc9da94f9638cd658e0eaed000000000000000000000000b347abf310504e239c4a96d7fab6d7
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", 0x1d972db5323a5b7cd4e8c9bd8bfe38d8919b1b4823d73a6a775abd77946802db, "December 6, 2025, 1:41 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56, 0x38ed1739, 0xa6c9fb980028fbdf792b454c4b2ac6982d2bc989, 0x7a250d5630b4cf539739df2c5dacb4c659f2488d, "200,000"  , "25,827,766", "163,379",    73, 155      , true   , 0x0 , 0                      , 0x38ed1739000000000000000000000000000000000000000000000000000009184e72a000000000000000000000000000000000000000000000000000000001b8757d49e300000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000a6c9fb980028fbdf792b454c4b2ac6982d2bc9

```

Câu truy vấn trên sẽ lấy 5 transactions trong block đầu tiên trong ngày 05/12/2025. Mình sẽ giải thích một số column quan trọng
- `hash`: giá trị hash của transaction, dùng để xác định duy nhất 1 giao dịch.
- `gas_limit`: Lượng gas dự kiến sẽ cần để thực hiện giao dịch. Đây là giá trị do chủ sở hữu của ví `from` đặt vào khi submit giao dịch
- `gas_price`: Giá gas đo bằng đơn vị wei (1 ETH = 10^18 wei) tại thời điểm thực hiện giao dịch được xác định bởi mạng lưới, càng nhiều giao dịch giá gas sẽ càng cao. Bạn có thể xem biểu đồ giá gas theo giờ mỗi ngày [tại đây](https://chainslake.com/@ethereum/115160430782385400).
- `gas_used`: Lượng gas thực tế mà validator đã sử dụng để xác thực giao dịch.
- `success`: Giao dịch có thành công hay là không. Khác với Bitcoin, cả các giao dịch không thành công vẫn sẽ được lưu lại trên blockchain của Ethereum. Nếu giao dịch thất bại toàn bộ trạng thái sẽ được đưa trở lại thời điểm trước giao dịch, tuy nhiên người dùng vẫn sẽ phải trả phí giao dịch cho phần gas đã sử dụng.
- `value`: Giá trị ETH được tính bằng đơn vị wei được chuyển trong giao dịch.
- `data`: Dữ liệu được gửi trong giao dịch. 
    - Trường hợp chỉ chuyển ETH giữa 2 địa chỉ thì `data` sẽ là 0x
    - Trường hợp gọi 1 function trong contract thì:
        - `to`: là địa chỉ của Smart Contract
        - `data`: sẽ là tham số đầu vào của function tương ứng, 10 byte đầu tiên trong `data` sẽ là `method_id`
    - Trường hợp deploy 1 contract:
        - `to`: bằng null
        - `data`: byte code của contract.
- `from`: Địa chỉ thực hiện giao dịch, luôn là một địa chỉ tài khoản của người dùng (không bao giờ là địa chỉ Contract)

*Phân tích traces*: Trong cùng 1 giao dịch có thể có nhiều contract được gọi đến, không phải gọi trực tiếp từ địa chỉ ví của người dùng mà là gián tiếp thông qua các contract khác, trường hợp này giao dịch chính sẽ được lưu trong bảng `transactions`, còn các giao dịch nội bộ giữa các contract được lưu trong bảng `traces`.

```sql
select * from ethereum.traces where block_date = date '2025-12-01' order by block_number desc limit 5
```

Kết quả

```csv
block_date        , block_time                  , block_number, updated_time               , block_hash                                                        , tx_hash                                                           , method_id , gas        , gas_used , from                                      , to                                        , input                                                                                                                                     , output                                                            , value       , type        , sub_traces, trace_address, success
"December 1, 2025", "December 1, 2025, 11:59 PM", "23,922,066", "December 2, 2025, 2:24 AM", 0x2b153d4a963ee4456cb064402fb2464c1ec854fcdf4db3c9c25c8e1b67dee17f, 0xadff1508e0a87f1693829f8357a4e42092575d2293e6488bd0ed83f245432f72, 0xa9059cbb, "168,420"  , "35,328" , 0x3c3a81e81dc49a522a592e7622a7e711c06bf354, 0xcd368c1d80120b0dd92447c87eb570154f8e685c, 0xa9059cbb000000000000000000000000ebf55f9845c1a6c924ad5159d7df94c17ad8407b000000000000000000000000000000000000000000000000b83825141d8eac00, 0x0000000000000000000000000000000000000000000000000000000000000001, 0           , delegatecall,          0, [0]          , true
"December 1, 2025", "December 1, 2025, 11:59 PM", "23,922,066", "December 2, 2025, 2:24 AM", 0x2b153d4a963ee4456cb064402fb2464c1ec854fcdf4db3c9c25c8e1b67dee17f, 0xb2594968b474f999d424bb727f9eae80b9dcdfadcb680dcd6dddaf497284b052, 0x000000c3, "1,411,916", "903,648", 0x5b43453fce04b92e190f391a83136bfbecedefd1, 0xfbd4cdb413e45a52e2c8312f670e9ce67e794c37, 0x000000c30000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000041a, 0x                                                                , "23,922,066", call        ,         10, []           , true
"December 1, 2025", "December 1, 2025, 11:59 PM", "23,922,066", "December 2, 2025, 2:24 AM", 0x2b153d4a963ee4456cb064402fb2464c1ec854fcdf4db3c9c25c8e1b67dee17f, 0xadff1508e0a87f1693829f8357a4e42092575d2293e6488bd0ed83f245432f72, 0xa9059cbb, "178,356"  , "42,635" , 0x853f4850540d58e1f2f90d4908698df8de62efb6, 0x3c3a81e81dc49a522a592e7622a7e711c06bf354, 0xa9059cbb000000000000000000000000ebf55f9845c1a6c924ad5159d7df94c17ad8407b000000000000000000000000000000000000000000000000b83825141d8eac00, 0x0000000000000000000000000000000000000000000000000000000000000001, 0           , call        ,          1, []           , true
"December 1, 2025", "December 1, 2025, 11:59 PM", "23,922,066", "December 2, 2025, 2:24 AM", 0x2b153d4a963ee4456cb064402fb2464c1ec854fcdf4db3c9c25c8e1b67dee17f, 0xb2594968b474f999d424bb727f9eae80b9dcdfadcb680dcd6dddaf497284b052, 0xa9059cbb, "1,326,698", "12,862" , 0xcd423f3ab39a11ff1d9208b7d37df56e902c932b, 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2, 0xa9059cbb000000000000000000000000fbd4cdb413e45a52e2c8312f670e9ce67e794c3700000000000000000000000000000000000000000000000004ac94549f84cb9b, 0x0000000000000000000000000000000000000000000000000000000000000001, 0           , call        ,          0, "[0, 0]"     , true
"December 1, 2025", "December 1, 2025, 11:59 PM", "23,922,066", "December 2, 2025, 2:24 AM", 0x2b153d4a963ee4456cb064402fb2464c1ec854fcdf4db3c9c25c8e1b67dee17f, 0xb2594968b474f999d424bb727f9eae80b9dcdfadcb680dcd6dddaf497284b052, 0x128acb08, "1,385,451", "77,143" , 0xfbd4cdb413e45a52e2c8312f670e9ce67e794c37, 0xcd423f3ab39a11ff1d9208b7d37df56e902c932b, 0x128acb08000000000000000000000000fbd4cdb413e45a52e2c8312f670e9ce67e794c370000000000000000000000000000000000000000000000000000000000000001, 0x000000000000000000000000000000000000000000000048e20f748db295fad4, 0           , call        ,          4, [0]          , true

```



*Phân tích log:*

```sql
select * from ethereum.logs where block_date = date '2025-12-05' order by block_number desc limit 5
```

Kết quả:

```csv
block_date        , block_number, block_time                  , updated_time                , block_hash                                                        , tx_hash                                                           , index, contract_address                          , topic1                                                            , topic2                                                            , topic3                                                            , topic4, tx_index, data
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", "December 6, 2025, 12:56 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56, 0x6909ad1b76ec414238bc9f4b41a3938c04c5ad526e99197c1a90c6cf3487e46f,     4, 0xca7c2771d248dcbe09eabe0ce57a62e18da178c0, 0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822, 0x000000000000000000000000fbd4cdb413e45a52e2c8312f670e9ce67e794c37, 0x000000000000000000000000fbd4cdb413e45a52e2c8312f670e9ce67e794c37,       ,        0, 0x00000000000000000000000000000000000000000000000065aaf3e7309ea8630000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006e3d46e7055de8b
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", "December 6, 2025, 12:56 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56, 0x6909ad1b76ec414238bc9f4b41a3938c04c5ad526e99197c1a90c6cf3487e46f,     3, 0xca7c2771d248dcbe09eabe0ce57a62e18da178c0, 0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1,                                                                   ,                                                                   ,       ,        0, 0x0000000000000000000000000000000000000000000000612c4b36c4a175570800000000000000000000000000000000000000000000000694033167e21cc8ad
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", "December 6, 2025, 12:56 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56, 0x6909ad1b76ec414238bc9f4b41a3938c04c5ad526e99197c1a90c6cf3487e46f,     0, 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2, 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef, 0x000000000000000000000000fbd4cdb413e45a52e2c8312f670e9ce67e794c37, 0x000000000000000000000000ca7c2771d248dcbe09eabe0ce57a62e18da178c0,       ,        0, 0x00000000000000000000000000000000000000000000000065aaf3e7309ea863
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", "December 6, 2025, 12:56 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56, 0x6909ad1b76ec414238bc9f4b41a3938c04c5ad526e99197c1a90c6cf3487e46f,     2, 0xcf0c122c6b73ff809c693db761e7baebe62b6a2e, 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef, 0x000000000000000000000000ca7c2771d248dcbe09eabe0ce57a62e18da178c0, 0x000000000000000000000000fbd4cdb413e45a52e2c8312f670e9ce67e794c37,       ,        0, 0x00000000000000000000000000000000000000000000000006de89cf6332cbb4
"December 5, 2025", "23,950,139", "December 5, 2025, 11:59 PM", "December 6, 2025, 12:56 AM", 0x4bdff6564db68e319c51e77a6af74c556a94b7b644105c282353bba52187ba56, 0x6909ad1b76ec414238bc9f4b41a3938c04c5ad526e99197c1a90c6cf3487e46f,     1, 0xcf0c122c6b73ff809c693db761e7baebe62b6a2e, 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef, 0x000000000000000000000000ca7c2771d248dcbe09eabe0ce57a62e18da178c0, 0x000000000000000000000000bc530bfa3fca1a731149248afc7f750c18360de1,       ,        0, 0x00000000000000000000000000000000000000000000000000054a9f0d2312d7

```

Câu truy vấn trên sẽ lấy 5 event log trong block đầu tiên trong ngày 05/12/2025. Một số column quan trọng như sau:
- `tx_hash`: Hash của transaction mà event được phát ra
- `contract_address`: Địa chỉ contract phát ra event
- `topic1`, `topic2`, `topic3`, `topic4`, `data`: Dữ liệu của log event

Lưu ý: Dữ liệu trong các giao dịch (bao gồm cả transactions và traces) và dữ liệu event log đều đã được mã hóa, để có thể giải mã và hiểu được chúng cần có ABI của contract tương ứng. Mình sẽ giới thiệu với các bạn các kỹ thuật để giải mã dữ liệu này trong các bài viết tới nhé.

### Phân tích cơ bản

Từ dữ liệu transaction, chúng ta có nghĩ đến một số phân tích cơ bản để theo dõi hoạt động của mạng lưới như theo dõi biến động về số lượng giao dịch, số lượng ví hoạt động, giá gas theo thời gian... Những phân tích cơ bản này bạn đều có thể tìm thấy trong trang phân tích chuyên về Ethereum của Chainslake:
- [chainslake.com/@ethereum](https://chainslake.com/@ethereum)

Bạn cũng có thể xem thêm mã nguồn của các bảng phân tích cơ bản này trên trang tài liệu kỹ thuật của Chainslake:
- [ethereum_perform.transactions_day](https://data.chainslake.com/#!/model/model.chainslake.ethereum_perform.transactions_day): Thống kê hoạt động và giá gas trên Ethereum theo ngày.
- [ethereum_perform.transactions_hour](https://data.chainslake.com/#!/model/model.chainslake.ethereum_perform.transactions_hour): Theo dõi hoạt động và giá gas trên Ethereum theo giờ.

