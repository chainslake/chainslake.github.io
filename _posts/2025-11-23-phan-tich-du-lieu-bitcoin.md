---
title: Phân tích dữ liệu Bitcoin
layout: post
date: 2025-11-23 06:00
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: true
tag:
- vietnam
category: blog
author: lake
description: Bitcoin là đồng tiền điện tử đầu tiên và cũng là quan trọng nhất trong thế giới Crypto. Trong bài viết này mình sẽ giới thiệu những kiến thức cơ bản nhất về Bitcoin và một số phân tích dữ liệu cơ bản xung quanh blockchain này.
---

Để mở đầu cho chuỗi các bài học về phân tích dữ liệu blockchain cùng Chainslake, mình sẽ chia sẻ với các bạn các kiến thức về Bitcoin - Blockchain đầu tiên được giới thiệu và cũng là đồng tiền điện tử có giá trị nhất hiện tại. Để tiện cho việc theo dõi của các bạn thì các bài học mình sẽ tổ chức thành 2 phần: Phần 1 sẽ giới thiệu các khái niệm, kiến thức cơ bản cần biết về chain, giao thức... phần 2 các bạn sẽ được hướng dẫn thực hành trực tiếp trên dữ liệu của Chainslake. 

## Khái niệm và kiến thức cơ bản về Bitcoin <a name="introduction"></a>

Bitcoin được giới thiệu lần đầu trong bài báo cùng tên của tác giả ẩn danh Satoshi Nakamoto, bạn có thể xem đầy đủ [tại đây](https://bitcoin.org/bitcoin.pdf). Mình sẽ tóm tắt lại các nội dung chính như sau:

*Bitcoin hoạt động theo cơ chế ngang hàng (peer-to-peer)*. Trong hệ thống tiền tệ truyền thống, để thực hiện được một giao dịch ta luôn cần một bên thứ 3 như ngân hàng, nhà nước hoặc một tổ chức mà cả 2 bên tham gia đều tin tưởng đứng ra làm trung gian ghi lại lịch sử giao dịch và cập nhật số dư cho cả 2 bên. Trong khi đó Bitcoin vẫn có thể thực hiện giao dịch mà không cần một bên thứ 3 nào khác, 2 bên tham gia cũng không cần tin tưởng nhau, đây được gọi là cơ chế ngang hàng.

*Sổ cái phi tập trung*. Tất cả lịch sử giao dịch được lưu trong 1 cuấn sổ cái, trong hệ thống tập trung thì chỉ có bên thứ 3 mới được giữ và có quyền thêm giao dịch vào sổ cái. Còn với Bitcoin, mỗi bên tham gia sẽ giữ một bản sao đầy đủ của sổ cái, điều này đảm bảo việc không ai có thể thay đổi thông tin trong sổ cái nếu không có sự đồng thuận của tất cả các bên.

*Cơ chế đồng thuận*. Để thêm một giao dịch vào sổ cái phi tập trung thì giao dịch đó phải hợp lệ tức là không thể chi tiêu số tiền lớn hơn số dư, nếu điều này xảy ra các bên khác sẽ từ chối ghi lại giao dịch vào sổ cái của họ và giao dịch sẽ thất bại. Vấn đề ở đây là để biết một giao dịch có hợp lệ hay không ta cần biết tất cả các giao dịch đã xảy ra trước nó, tức là các giao dịch phải có thứ tự. Trong hệ thống tập trung ta có thể đảm bảo thứ tự của giao dịch bằng cách sử dụng hàng đợi, tuy nhiên trong hệ thống phi tập trung, nơi mà có rất nhiều bên đều muốn thêm giao dịch vào sổ cái, chúng ta cần một cơ chế đồng thuận.

*Bằng chứng công việc (Proof-of-Work)*. Bitcoin sử dụng cơ chế đồng thuận bằng chứng công việc, theo đó tất cả các bên sẽ cùng giải một bài toán khó, các dữ liệu đầu vào của bài toán chỉ có khi giao dịch cuối cùng đã được thêm vào sổ cái, điều này đảm bảo không ai có thể bắt đầu tìm lời giải cho bài toán trước những người khác. Bên nào tìm ra lời giải trước sẽ được quyền thêm giao dịch vào sổ cái.

*Blockchain:* Thực tế trong mạng lưới Bitcoin sẽ có những bên sở hữu khả năng tính toán mạnh hơn những người khác nên có ưu thế lớn trong việc tìm ra lời giải cho bài toán. Thay vì tự mình thêm giao dịch vào sổ cái những người khác có thể gửi giao dịch đến cho các node tính toán mạnh, những node này sẽ tập hợp các giao dịch thành một khối và thêm vào sổ cái lúc này là một chuỗi các khối (blockchain).

*Thợ đào:* Các node làm nhiệm vụ thêm block mới vào blockchain được gọi là thợ đào. Mỗi khi thêm được một block mới, thợ đào sẽ nhận được phần thưởng là một số lượng bitcoin nhất định, họ cũng sẽ nhận được phí giao dịch từ mỗi giao dịch được gửi đến.

*Bitcoin Halving:* Là một cơ chế được lập trình sãn trong Bitcoin để cứ sau 210.000 blocks thì phần thưởng của thợ đào sẽ giảm đi 1 nửa. Bắt đầu từ 50 Bitcoin cho mỗi block được tạo ra, phần thưởng sẽ giảm dần đến 0, lúc này tổng số BTC đạt cực đại là 21 triệu đồng. Thuật toán bitcoin cũng tự động điều chỉnh độ khó để đảm bảo rằng sự kiện Bitcoin Halving 4 năm mới xảy ra 1 lần, tức là phải đến năm 2140 toàn bộ số Bitcoin mới được đào hết.

*Cơ chế xác thực:* Để gửi và nhận bitcoin, người dùng cần có một cặp <địa chỉ, khoá bí mật>. Để nhận bitcoin thì chỉ cần đưa địa chỉ cho người gửi còn nếu muốn gửi bitcoin cho người khác thì người dùng cần tạo một giao dịch với địa chỉ của người nhận, số bitcoin muốn chuyển đi và một chữ ký số được tạo ra bằng cách sử dụng khoá bí mật ký lên giao dịch. Thợ đào khi nhận được giao dịch sẽ kiểm tra giao dịch và chữ ký số để xác thực thông tin trước khi giao dịch được thêm vào blockchain.

*Tính ẩn danh:* Trong các hệ thống thanh toán truyền thống, bạn cần đăng ký tài khoản và phải cung cấp thông tin cá nhân cho hệ thống để được sử dụng dịch vụ. Tuy nhiên đối với Bitcoin, tài khoản (hay địa chỉ) được tạo ra bằng thuật toán ngay trên thiết bị của bạn mà không cần kết nối mạng, từ đó đảm bảo sự ẩn danh tuyệt đối khi giao dịch. Sự ẩn danh cũng là một con dao 2 lưỡi, nếu bạn làm mất khóa bí mật, sẽ không có cách nào để chuyển tiền trong tài khoản và nếu bạn chuyển nhầm tiền vào một địa chỉ sai bạn cũng không thể lấy lại được.

*Đồng tiền có thể lập trình:* Một trong những chức năng thú vị nhất của Bitcoin là nó cho phép người dùng có thể lập trình điều kiện chi tiêu cho khoản tiền của họ. Trường hợp phổ biến nhất là khoản tiền chỉ được chi tiêu nếu có chữ ký số hợp lệ của người nhận (trường hợp chuyển tiền thông thường), một trường hợp khác với điều kiện chi tiêu cần có chữ ký của 2 hoặc nhiều người (trường hợp chuyển tiền đến 1 ví đa chữ ký), thậm chí bạn cũng có thể đặt điều kiện chi tiêu khoản tiền sau 1 thời gian nhất định.

## Phân tích dữ liệu Bitcoin cùng Chainslake <a name="analyst"></a>

Bạn có thể xem lại bài viết này nếu cần: [Học phân tích dữ liệu, làm Insight blockchain cùng Chainslake](/hoc-phan-tich-du-lieu-lam-insight-blockchain-cung-chainslake/)

### Phân tích thô
Là blockchain đầu tiên được giới thiệu nên Bitcoin có thiết kế khá đơn giản (khi so với các blockchain ra đời sau này), dù đồng tiền có chức năng lập trình, nhưng rất hạn chế về chức năng, do đó dữ liệu của Bitcoin không quá phức tạp, chúng chỉ gồm các giao dịch chuyển tiền được đóng trong các khối liên tiếp nhau. Hãy thực hiện câu truy vấn sau:

```sql
select * from bitcoin.blocks where block_date = date '2025-11-22' order by block_number asc limit 10
```

Kết quả:

```csv
block_date         , block_number, block_time                   , hash                                                            , size       , stripped_size, weight     , version        , version_hex, merkle_root                                                     , median_time                  , nonce          , bits    , difficulty             , chain_work                                                      , n_tx   , previous_block_hash
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", 000000000000000000016b3da523d70d8165c7cfce96eb82c4706df65037c7a0, "1,658,317", "778,554"    , "3,993,979", "537,051,136"  , 2002c000   , 7d7976db9e8830509b36c46426068803a852bc7ce84c4763382679dfc4b782a0, "November 21, 2025, 11:34 PM", "3,323,527,047", 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72b2555a640b22de660e2db, "2,655", 00000000000000000000eac66a5d19aecdb20c8f36ac8a685bbcb8e3a00cf19b
"November 22, 2025", "924,637"   , "November 22, 2025, 12:10 AM", 0000000000000000000025dc0aa51418872b7b1daacb2a11884d7df8c77fc90e, "696,457"  , "353,301"    , "1,756,360", "838,860,800"  , 32000000   , adf4a26d2e1e1e91c6112ef04984966d82666de4ed7ba3d2699476fd0458861b, "November 21, 2025, 11:35 PM", "3,025,786,101", 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72bafd3a3c6a9613d112bd2, "1,659", 000000000000000000016b3da523d70d8165c7cfce96eb82c4706df65037c7a0
"November 22, 2025", "924,638"   , "November 22, 2025, 12:12 AM", 0000000000000000000024ce3f010a24ca3cc392798df9029d6cccaa69dfc474, "2,024,012", "656,655"    , "3,993,977", "785,285,120"  , 2ece8000   , 9b2f72b39e4d33d1131c7553a6241c6766562598b85149d51f5dee7f12141788, "November 21, 2025, 11:44 PM", "418,682,159"  , 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72c3a51a14ca09493c174c9, "1,467", 0000000000000000000025dc0aa51418872b7b1daacb2a11884d7df8c77fc90e
"November 22, 2025", "924,639"   , "November 22, 2025, 12:24 AM", 00000000000000000001417451be1068dc4d71f41959330677b2e2f88a597adc, "1,677,576", "772,051"    , "3,993,729", "560,185,344"  , 2163c000   , 260f52765676210c18e3cf1c0643d7d54426924d51ce200d8e96001ae91a5b11, "November 21, 2025, 11:46 PM", "2,857,355,194", 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72cc4cf9ed297c7ea71bdc0, "3,800", 0000000000000000000024ce3f010a24ca3cc392798df9029d6cccaa69dfc474
"November 22, 2025", "924,640"   , "November 22, 2025, 12:42 AM", 00000000000000000000ca66fa6a97f7429a59566a810226698cee2db7562728, "1,551,696", "814,002"    , "3,993,702", "1,073,676,288", 3fff0000   , e5feb033bfe6f458e3b072f40d376a3561057968949e46f9ad46e06a6b7eabb9, "November 21, 2025, 11:47 PM", "1,435,497,658", 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72d4f4d9c588efb412206b7, "3,129", 00000000000000000001417451be1068dc4d71f41959330677b2e2f88a597adc
"November 22, 2025", "924,641"   , "November 22, 2025, 12:54 AM", 000000000000000000010930362613cc08be26e5099f9ad07b0e8ca20a0b7f8f, "1,580,061", "804,484"    , "3,993,513", "541,065,216"  , 20400000   , 2a8b47df235f544b88abb999a35ed321458c429b4dbf593aaf7ebe71b5cf42fd, "November 22, 2025, 12:07 AM", "112,140,592"  , 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72dd9cb99de862e97d24fae, "3,921", 00000000000000000000ca66fa6a97f7429a59566a810226698cee2db7562728
"November 22, 2025", "924,642"   , "November 22, 2025, 1:21 AM" , 000000000000000000012d48f9186393a5ab788270b53da9f014687caeb09e77, "1,596,400", "799,159"    , "3,993,877", "537,223,168"  , 20056000   , 032944704cdcb9dff8b45adb10ce882ed2782ad660cbea35970be5da657f3620, "November 22, 2025, 12:10 AM", "817,328,725"  , 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72e644997647d61ee8298a5, "3,461", 000000000000000000010930362613cc08be26e5099f9ad07b0e8ca20a0b7f8f
"November 22, 2025", "924,643"   , "November 22, 2025, 1:54 AM" , 000000000000000000013b40893f45b357d6e4ef4a8bfd1f507d5a330e08660d, "1,539,162", "818,254"    , "3,993,924", "872,415,232"  , 34000000   , 37ce196ca88506373f46f9c123edda65a6bae341d349279dc7316d9171a91e7d, "November 22, 2025, 12:12 AM", "3,360,186,592", 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72eeec794ea74954532e19c, "3,626", 000000000000000000012d48f9186393a5ab788270b53da9f014687caeb09e77
"November 22, 2025", "924,644"   , "November 22, 2025, 1:58 AM" , 0000000000000000000040e3184c589b195a8df871d6bb8ea63914a27f59641d, "1,486,952", "835,470"    , "3,993,362", "554,590,208"  , 210e6000   , c10d9bbbca77bd30e3c4a25bba3317239cae9c5b2763fa3bfc76e0fa1773c462, "November 22, 2025, 12:24 AM", "3,006,172,609", 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f72f794592706bc89be32a93, "2,161", 000000000000000000013b40893f45b357d6e4ef4a8bfd1f507d5a330e08660d
"November 22, 2025", "924,645"   , "November 22, 2025, 1:59 AM" , 00000000000000000000315892f7c308c79da6519a0a1483e9ba79fd7170f976, "487,783"  , "246,026"    , "1,225,861", "536,870,912"  , 20000000   , 1822d09c1f17a0be042b1680fc2e2bf47954f58ac0f6ecf51b098e7b2b7075c7, "November 22, 2025, 12:42 AM", "1,427,865,833", 1701d936, "152,271,405,447,597.4", 0000000000000000000000000000000000000000f73003c38ff662fbf293738a, "1,309", 0000000000000000000040e3184c589b195a8df871d6bb8ea63914a27f59641d

```

Câu truy vấn trên sẽ lấy 10 blocks đầu tiên trong ngày 22/11/2025. Dựa vào bảng kết quả ta có thể có một số nhận xét sau:
- Thời gian để đóng 1 block là không cố định từ 3 - 20 phút (trung bình sẽ là 10') do phụ thuộc vào thời điểm mà thợ đào tìm được lời giải cho bài toán.
- Bạn có thể để ý thấy mỗi block có 1 chuỗi Hash và một số Nonce. Chuỗi Hash được bắt đầu bằng 1 số lượng chữ số 0 nhất định, nó được tạo ra bằng cách sử dụng thuật toán băm trên dữ liệu của một khối cùng với số Nonce được thợ đào thêm vào, nhiệm vụ của thợ đào là tìm được số Nonce để kết quả Hash có đủ số chữ số 0 như yêu cầu (càng nhiều số 0 thì việc tìm ra số Nonce càng khó). Trong 1 block bạn còn có thể nhìn thấy `previous_block_hash` đây là Hash của block ngay trước đó, điều này đảm bảo việc phải có được Hash của block trước đó mới có thể bắt đầu tìm số Nonce của block hiện tại.
- `n_tx` cho biết số lượng giao dịch được đóng trong mỗi block, số lượng giao dịch của mỗi khối có thể khác nhau nhưng bị giới hạn bởi dung lượng dữ liệu của mỗi khối.
- Giao dịch được gửi lên để thự hiện sẽ được đưa vào một pool và chia sẻ đến tất cả thợ đào, mỗi thợ đào sẽ lấy giao dịch từ pool theo thứ tự ưu tiên cho đến khi đủ dung lượng của một khối thì sẽ thực hiện tính toán `merkle_root` là hash của [Merkle Tree](https://en.wikipedia.org/wiki/Merkle_tree) từ các giao dịch. Khi bắt đầu đào, thợ đào sẽ kết hợp `merkle_root`, `previous_block_hash` và thử hash block với rất nhiều sô Nonce khác nhau cho đến khi tìm ra được Hash thỏa mãn. Nếu trong lúc đào mà nhận được hash đúng từ các thợ đào khác, quá trình đào sẽ dừng lại (do đã thất bại) để bắt đầu đào block tiếp theo.

Hãy tiếp tục thực hiện truy vấn dưới đây:

```sql
select * from bitcoin.transactions where block_number = 924636 limit 10
```
Kết quả
```csv
block_date         , block_number, block_time                   , in_active_chain, hex                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           , tx_id                                                           , hash                                                            , size, v_size, weight , version, lock_time, block_hash, fee
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 010000000001010000000000000000000000000000000000000000000000000000000000000000ffffffff5603dc1b0e194d696e656420627920416e74506f6f6c200601f307f8cf59defabe6d6d177b6f9fd7306db7006276f3fee3e84cda820be3845dd41e80e0c433a3978cdf100000000000000000004a07b190010000000000ffffffff07220200000000000017a91442402a28dd61f2718a4b27ae72a4791d5bbdade7877f53d1120000000017a9145249bdf2c131d43995cff42e8feee293f79297a8870000000000000000266a24aa21a9ed6ddd3a4dd2d6508eb42952bcd34130dd329ddaaab71fb2cfa5dc6e822a31788c00000000000000002f6a2d434f52450164db24a662e20bbdf72d1cc6e973dbb2d12897d54e3ecda72cb7961caa4b541b1e322bcfe0b5a0300000000000000000146a12455853415401000d130f0e0e0b041f1200130000000000000000296a27737973997d93ed870de4bd1a847ae501192c7637d26e25f2ee5b895e73997e98f8cc7538a7200000000000000000002b6a2952534b424c4f434b3a19831b85228b2f005636ca186d37a0aa7946061d143308541946be19007d9e4f0120000000000000000000000000000000000000000000000000000000000000000000000000, 53e071864d69af7904509965f6e3ed51f8199116da7ea6faf0f5c81dc298b95a, 6c9305ed60695a55f5c3a900b7f4153ac1cceaddce7dca22608d7435ea8b3b1e,  471,    444, "1,776",       1, 0        ,           , 0
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 0100000001e3a29897ae4ef882cd2740a5b262ad2d7dcf83498b5a897124c88efa39ce3ee0000000006a47304402205cad06f479ba3c6a237629320f64c8058607f9301e555d412914cbb1c1e637ac022062f9559b7a72923104595977a51dfee979e7c218feab3a8e4beb93d7eddff2b9012102671f1ae5ef3966dc547de554cccf34a0aaf1f245be325ca00dc54934f10736f30000008002f07e0e000000000017a9145da1a4da424bdc7f113eb22e48e017a43b6ad2148779ea0300000000001976a91456c0bc2f50bc150d4ea122e66db7c48b01b9722988ac00000000                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                , 673f6687ca520145a2659cd90adc34254fd2ce39d9aab471349ba73cd6e14070, 673f6687ca520145a2659cd90adc34254fd2ce39d9aab471349ba73cd6e14070,  223,    223, 892    ,       1, 0        ,           , 0.00040035
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 01000000015a05b4ab1425709e8323e11f4123289ad443513ac22f44bd06febb490239a42d000000006b4830450221009fc3249b9908cfcae063c3e466c3786ca0c347e1c37f38799acb2d4caefc3eba022040a1e6a86b50f9315ba36754ed7a0b39356b1b7a113c58829244bb1be1a69508012102671f1ae5ef3966dc547de554cccf34a0aaf1f245be325ca00dc54934f10736f30000008002108a2e000000000017a914bc4e0a73ab46084bd3f152824c3bf9d18cde356e872d5b0300000000001976a91456c0bc2f50bc150d4ea122e66db7c48b01b9722988ac00000000                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              , 91a7da1cb33d8b19bf71dc65f75e150bfd3bbb20170329b90cd6e9d566268613, 91a7da1cb33d8b19bf71dc65f75e150bfd3bbb20170329b90cd6e9d566268613,  224,    224, 896    ,       1, 0        ,           , 0.00040035
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 01000000000101378c1cd49f14e0f8d1d83a1c55804d4904ed52f4b8306ca762fb6e555d7ac1bc0600000000fdffffff0330954e0000000000220020e8c919a22e5fc0c47c068b1b607dcf6fa4550198e85a2cd87041d050c6eccd39108ccb0000000000220020f9b9cce6219637b0f6c42ae752f84e3a0a3c953be0d0b107cee6190b957c28cce0ba710c000000002200207bb8f5802ae446be4e5f7dde387ca1624faee09a8643a8c16ff87df4235e258404004730440220785778d64036e5a71b9d6c5ffdaecef0628447b003bdd21abe665ea945f0aa9d02206ee25fa5a0ad0a5f19e0d54e869f66e401588de0a0c5160a7136b40b5fc07074014730440220712a9508c5ac965d6c841f81426048073b9ffcc6f1321da124b96c0e6984b9ba022054c45960be64614183e1ecbd86f190a97720482dd44d7b640ea9ab1f7b67f838016952210244da06e88d078b8230ef441f5719c898ff6f1dd6b68b5b8a72897635add62c9e210226f659f2d6168f8b79f88e93ae03510b6c98fb98eba825269b646d06fa0655a421038591e2d4fae9da8efd9e0bb595d09750e4ccaf682776f0277de253a740685b4d53ae00000000                                                                          , b646674c86f193a6e283a5166dba9e02e24d77ccbbf234efc27976edd373727d, 8af4231d5250898d563822a12691483b311b3d3630759aba4fb82ead4a901e82,  434,    244, 974    ,       1, 0        ,           , 0.00037
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 01000000000102ee7bf1a314f07e82a01cfdfbf3bcc911b9c7272f697ed1a573fbdd5868ef439a0000000000ffffffff35408c9cc7e5f32fb6cc79fac5bfdafe0d9a63aefe6117082959a923fb0213ef0000000000ffffffff0184ac0100000000001600146050d22d6ca254711f419273d03bb465baf2259902483045022100efe72ab634fec9e8270ff00387307bda1e6e3c5f55318644ed86b52f702e044e02205effc73f9b983687d5588357ec3099eeb1fac2fe3878bbf5dd4d7ce615c342de0121024254849e7bf97e4d0f14362dfec87016b05e13e65d3ec69369257ffddfc61e0b02483045022100f7dc716ef8d1e559f9a7efbbae90511765531483bfcfc2d0a027650af252898402201ac68bdaef1d33e39f5b518a6dadb34acf0d2cb3b7bd5d55bba9e154e9bf294e0121024254849e7bf97e4d0f14362dfec87016b05e13e65d3ec69369257ffddfc61e0b00000000                                                                                                                                                                                                                                                                    , f0b1a0ee8bbcfcb99e6807fa85c646062df161a3dff2ef00e5ccbe04103fcde5, 3909bb5b315b9d760bdea3c2d2d45a70129b5ed649e08ac126f13239f99027ea,  341,    178, 710    ,       1, 0        ,           , 0.00021
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 0200000000010161bf0316ad79158093542602eb58cbc9f2c8f41a888259868ad06174c6f705020100000000fdffffff02052a0a0000000000160014ce149257fbff6e0d7a34159f31378055e39f4e8f8696150000000000160014602c206828f5a35b06c7abb0eed9c678de3a84500247304402203b1eb485fa11907765396ad9765fa4c06a6f6a8c1846f4bfdcf2f2e6db56592202206cc7ba937a30b1ef33c3a72428fa37207f87933d640591068feef0f59f4f2780012102e5f754ad98306e0f54c854e319ec7e344cddaba82758dc4cac927dc18664045a811b0e00                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  , 1f86acfd3ca979a873a16cc8aa95da71acbb4a86819e3805477b85f67b0e2959, 42846d832b5906499c9a50610fdaa883bd93ae088ace26f3f0541a109ff7a5fd,  222,    141, 561    ,       2, "924,545",           , 0.00014
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 010000000001019923ca58364daee7d6098022b33f720178ed797c27ec3c80e666920b542351940000000000ffffffff0234a66b0000000000160014819231d8efd05c00277c59bcbc7c3df5aa713a82b06337000000000016001428e6bc13f2973c7b5d0b30e88ce90de5735edf9a02473044022065bb9ff04cae85258945bd21de26f478319a5be3b57b216201f2ac87b277ede00220297244fc644f706101ff046c241d76f02093bdf0745cf93157557489926b2c3e012102bb1260dc0900e66a0713cc49935513bfd82d87c7078d6184270f97dc66bb283d00000000                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  , 878d76af340e77ac8a736e7cd80c2aebceb4a647fec169c29516b3077d7aa061, 5556d1402ed899396105429e2f52d43609fd25051508b4f49a802622b058d457,  222,    141, 561    ,       1, 0        ,           , 0.00014
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 0100000000010135d3b2cb0bf5f7228acf8a3845b9f5c6e66284089a2caa29d3947464981d3f650100000000ffffffff02a06806000000000017a9146844cafe2fa03b46c9ee74263f9e986acd51912a87c36f2f0000000000220020701a8d401c84fb13e6baf169d59684e17abd9fa216c8cc5b9fc63d622ff8c58d040047304402207d7fc9a8cedec01e792db471e168870cb6cd43cea506aed34d0e485de1a1682b02205d19481f881d7b7d6965e5509ce5921075cefbb7092dc5830e6c79855ccf7d2b014730440220644a8717b1497dd88e07db9ee99e2e705c7165197b77f89b5132fd0896590ca002201aac5a879bf1c3042f1b11f5e9f16742f23206be1f33f4c0b992c122568fcb61016952210375e00eb72e29da82b89367947f29ef34afb75e8654f6ea368e0acdfd92976b7c2103a1b26313f430c4b15bb1fdce663207659d8cac749a0e53d70eff01874496feff2103c96d495bfdd5ba4145e3e046fee45e84a8a48ad05bd8dbb395c011a32cf9f88053ae00000000                                                                                                                                                                                      , e8ff28712f49e39caeb40e3b4b9395759860cfe1876d4b179097b8e1af973847, d5b8df21a7f99c8871842c4a1581ec6e7512e27f431f13b23a9363d4b65cf1d6,  380,    190, 758    ,       1, 0        ,           , 0.00011
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 01000000000101c5d76f7a9a356b82edd3235f51cd13c0a4d4638aaa867c117cd41c778db60c160100000000ffffffff02ea6b0200000000001976a914eff4ee2011112e27f79d0ab001ae6189689a864388ac821f030000000000220020701a8d401c84fb13e6baf169d59684e17abd9fa216c8cc5b9fc63d622ff8c58d040047304402203e3985142dbaa6237fcbaef993727047e041e609e1faf0a15de67598656855fc02201983569312e1e1624c20b248fd93445d4e601a4a2557001720a237ca0dc82baf01473044022013618a819958cfb0226ddf7a9771a41590c776305fdae4f9e819f31183fa200f02203f82f3e0c6d45381d97efa87209333a4320b7cb7e8f0b2ba4cd5e2fb6b055505016952210375e00eb72e29da82b89367947f29ef34afb75e8654f6ea368e0acdfd92976b7c2103a1b26313f430c4b15bb1fdce663207659d8cac749a0e53d70eff01874496feff2103c96d495bfdd5ba4145e3e046fee45e84a8a48ad05bd8dbb395c011a32cf9f88053ae00000000                                                                                                                                                                                  , a3aaed05590242da52e5e9c2cacd3f761bb7f6a305cef6a393e8c7a56ba0d8ef, 653a2ba98ef2458bdae17bd68407c1661f9393120c8ad92f89fde507363fc619,  382,    192, 766    ,       1, 0        ,           , 0.00012
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", false          , 01000000000101978c2ed0db199735df7eee2a01f9bbea240475a59e20e6518211af78dfc7132b0100000000ffffffff02e81002000000000016001471b50a1310961958cd55451c4d482e15d70f833e4a862800000000001600140792facf45a414300b08aac46a23eb05934b825002473044022065298468a7117c47d930d0262180113d0db7ee733b9f1f20e7591172f8e79ac202205ccad65ddea8ebaaba30bdad8c6ff39d78075d3071560ec6bdbcc86cfb5c0eef012102a48970c911a1276e4f6cb7cefa376705a6f76249cac312b49d52e15b9a4a786a00000000                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  , 215b1b1d72205fa4e9af64df06630cd4fb474ade5c9b74eddb07b1d31dfa7166, 07381d099a84d72cf6314c74740750f44bed6ade4524d7f8d4dbd8245890f4bd,  222,    141, 561    ,       1, 0        ,           , 0.000072

```

Đây là 10 giao dịch đầu tiên trong block 924636. Quan sát kết quả ta có một số nhận xét sau:
- Mỗi giao dịch đều có một khoản `fee` chính là phí giao dịch, duy chỉ có giao dịch đầu tiên có fee = 0, đây là giao dịch được chính thợ đào thêm vào được gọi là Coinbase Transasaction dùng để thu thập phí từ tất cả các giao dịch khác và nhận phần thưởng khi đào.
- Các giao dịch sẽ được sắp xếp theo fee, fee càng cao thì sẽ càng được thợ đào ưu tiên thực hiện trước.
- Mỗi giao dịch sẽ có một `tx_id` là kết quả hash của dữ liệu trong giao dịch đó.

Tiếp theo ta sẽ tìm hiểu chi tiết dữ liệu có bên trong 1 giao dịch. Hãy thực hiện các truy vấn sau:

```sql
SELECT
  *
FROM
  bitcoin.inputs
WHERE
  block_number = 924636
  AND burn_tx_id = '673f6687ca520145a2659cd90adc34254fd2ce39d9aab471349ba73cd6e14070'
LIMIT
  10
```
Kết quả
```csv
block_date         , block_number, block_time                   , burn_tx_id                                                      , tx_id                                                           , coinbase, v_out, script_sig_asm                                                                                                                                                                                                      , script_sig_hex                                                                                                                                                                                                      , sequence       , tx_in_witness
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", 673f6687ca520145a2659cd90adc34254fd2ce39d9aab471349ba73cd6e14070, e03ece39fa8ec82471895a8b4983cf7d2dad62b2a54027cd82f84eae9798a2e3,         ,     0, 304402205cad06f479ba3c6a237629320f64c8058607f9301e555d412914cbb1c1e637ac022062f9559b7a72923104595977a51dfee979e7c218feab3a8e4beb93d7eddff2b9[ALL] 02671f1ae5ef3966dc547de554cccf34a0aaf1f245be325ca00dc54934f10736f3, 47304402205cad06f479ba3c6a237629320f64c8058607f9301e555d412914cbb1c1e637ac022062f9559b7a72923104595977a51dfee979e7c218feab3a8e4beb93d7eddff2b9012102671f1ae5ef3966dc547de554cccf34a0aaf1f245be325ca00dc54934f10736f3, "2,147,483,648", 

```

```sql
SELECT
  *
FROM
  bitcoin.outputs
WHERE
  block_number = 924636
  AND tx_id = '673f6687ca520145a2659cd90adc34254fd2ce39d9aab471349ba73cd6e14070'
LIMIT
  10
```
Kết quả
```csv
block_date         , block_number, block_time                   , tx_id                                                           , n, value , script_pub_key_asm                                                                   , script_pub_key_hex                                , script_pub_key_req_sigs, type      , addresses, address
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", 673f6687ca520145a2659cd90adc34254fd2ce39d9aab471349ba73cd6e14070, 0, 0.0095, OP_HASH160 5da1a4da424bdc7f113eb22e48e017a43b6ad214 OP_EQUAL                         , a9145da1a4da424bdc7f113eb22e48e017a43b6ad21487    ,                       0, scripthash,          , 3AE6QbvQLwoctsHBmoZddkXu2ofnxjMeAQ
"November 22, 2025", "924,636"   , "November 22, 2025, 12:07 AM", 673f6687ca520145a2659cd90adc34254fd2ce39d9aab471349ba73cd6e14070, 1, 0.0026, OP_DUP OP_HASH160 56c0bc2f50bc150d4ea122e66db7c48b01b97229 OP_EQUALVERIFY OP_CHECKSIG, 76a91456c0bc2f50bc150d4ea122e66db7c48b01b9722988ac,                       0, pubkeyhash,          , 18uhzy546Qz7CxRNkHohg4W9VSkfTkbSvY

```

Đây là một giao dịch nằm trong block 924636 có `tx_id` là `673f668...`, ta có một số nhận xét sau:
- Nhìn vào kết quả truy vấn từ bảng `bitcoin.outputs` giao dịch này tạo ra 2 UTXO (Khoản tiền chưa được chi tiêu) đến 2 địa chỉ: `3AE6Q...` và `18uhz...` với số lượng BTC tương ứng là 0.0095 và 0.0026. Có một số n cho mỗi UTXO để xác định chính xác trong mỗi transaction.
- Kết quả truy vấn từ bảng `bitcoin.outputs` cho thấy giao dịch này sẽ chi tiêu từ 1 UTXO khác được tạo ra từ giao dịch `e03ece3...` có vị trí v_out = 0. Các thông tin như số lượng và địa chỉ của khoản tiền input này không có trong giao dịch nhưng ta có thể tính được giá trị của nó bằng: 0.0095 + 0.0026 + 0.00040035 (fee của giao dịch).
- Trước khi thực hiện giao dịch này, thợ đào sẽ kiểm tra để xác nhận sự tồn tại của UTXO trong input của giao dịch.

### Phân tích cơ bản

Một trong những bài toán phân tích cơ bản của Bitcoin là tính số dư của một địa chỉ bất kỳ. Như bạn đã thấy ở phần phân tích thô, mỗi giao dịch Bitcoin chúng ta chỉ biết các thông tin về UTXO được tạo ra trong giao dịch, còn số dư của một địa chỉ sẽ bằng tổng tất giá trị của các UTXO thuộc về địa chỉ đó. Do đó để tính được số dư thì ta cần xây dựng bảng dữ liệu các UTXO từ toàn bộ dữ liệu của Bitcoin. 

Chainslake đã xây dựng sẵn bảng dữ liệu UTXO, bạn có thể thử câu truy vấn sau đây:

```sql
SELECT
  *
FROM
  bitcoin_balances.utxo_latest_day
WHERE
  address = '3AE6QbvQLwoctsHBmoZddkXu2ofnxjMeAQ'
```
kết quả
```csv
version, address                           , tx_id                                                           , n, value , utxo, key_partition
    149, 3AE6QbvQLwoctsHBmoZddkXu2ofnxjMeAQ, 6c6a0b0dcb849fa7106366f9b4dce8c89353dd2919dba8e9d1ee5166d45af359, 0, 0.022 ,    1, 63A
    149, 3AE6QbvQLwoctsHBmoZddkXu2ofnxjMeAQ, 673f6687ca520145a2659cd90adc34254fd2ce39d9aab471349ba73cd6e14070, 0, 0.0095,    1, 63A

```

Đây là 1 trong 2 địa chỉ đã nhận BTC trong giao dịch mà chúng ta đã xem xét trước đó, có thể thấy rằng địa chỉ này đang có 2 khoản tiền chưa chi tiêu (UTXO), tổng giá trị là 0.022 + 0.0095 = 0.0315 BTC. Bạn có thể dùng dashboard [này](https://metabase.chainslake.com/public/dashboard/f8f50169-643b-41a0-8258-08442342f43a?address=3AE6QbvQLwoctsHBmoZddkXu2ofnxjMeAQ) để tra cứu số dư BTC bất kỳ.

Về cách để xây dựng bảng dữ liệu `bitcoin_balances.utxo_latest_day` bạn có thể tham khảo mô hình dưới đây:

<p style="
    text-align: center;
"><a href="https://data.chainslake.com/#!/model/model.chainslake.bitcoin_balances.utxo_latest_day" target="_blank"><img src="/assets/images/posts/2025-11-23/bitcoin_balances.utxo_latest_day.png" alt="bitcoin_balances_flow" width="100%"></a></p>


### Phân tích nâng cao

Bạn có thể tự mình xây dựng thêm các phân tích nâng cao khác từ dữ liệu của Bitcoin theo nhu cầu của riêng bạn. Đừng quên tham khảo các phân tích về Bitcoin của Chainslake nhé!

- [chainslake.com/@bitcoin](https://chainslake.com/@bitcoin)