---
title: "Phân tích vụ hack Bybit - Phần 2: Hành trình rửa 1.4 tỷ USD"
layout: post
date: 2026-02-22 10:00:00
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: true
tag:
- vietnam
category: blog
author: lake
description: Phần 2 của series phân tích vụ hack Bybit - theo dõi hành trình rửa tiền của Lazarus Group, từ 499,000 ETH đến 12,836 BTC phân tán trên 9,117 ví, qua THORChain, eXch và Wasabi Mixer, chỉ trong 10 ngày.
---

Ở [phần 1](/phan-tich-vu-hack-bybit-tu-du-lieu-onchain), chúng ta đã cùng nhau phân tích cơ chế tấn công của Lazarus Group vào Bybit, truy vết nguồn gốc ví hacker, và quan sát những giao dịch lớn đầu tiên sau sự cố. Câu hỏi còn bỏ ngỏ là: **499,000 ETH trị giá 1.4 tỷ USD đã đi đến đâu, và làm thế nào Lazarus Group rửa sạch số tiền đó chỉ trong 10 ngày?**

Bài viết này sẽ đi sâu vào từng giai đoạn của quá trình rửa tiền, từ dữ liệu on-chain thực tế.

# Nội dung
1. [Tổng quan quá trình rửa tiền](#overview)
2. [Giai đoạn 1: Phân tán ETH và swap qua stETH/mETH](#phase1)
3. [Giai đoạn 2: ETH → BTC qua THORChain](#phase2)
4. [Giai đoạn 3: Xóa dấu vết trên BTC](#phase3)

---

## Tổng quan quá trình rửa tiền <a name="overview"></a>

Trước khi đi vào chi tiết từng giai đoạn, hãy xem bức tranh tổng thể. Theo báo cáo của CEO Bybit Ben Zhou vào ngày 4/3/2025 — đúng 10 ngày sau vụ hack — toàn bộ 499,000 ETH đã được xử lý:

```
Tổng bị hack:    ~499,000 ETH (~$1.4B)
├── 83%  (~417,000 ETH) → Swap sang BTC qua THORChain → 12,836 BTC / 9,117 ví
├── 6%   (~79,000 ETH)  → Qua eXch mixer → Phần lớn đã "tối"
├── 8%   (~40,000 ETH)  → Qua OKX Web3 Proxy và DEX khác
└── 3%   (~15,000 ETH)  → Đã bị phong tỏa bởi các sàn và cơ quan chức năng
```

Điều đáng chú ý là tốc độ: trong vòng 48 giờ đầu tiên, ít nhất 160 triệu USD đã được chuyển qua các kênh bất hợp pháp. Đây là tốc độ rửa tiền chưa từng có tiền lệ, ngay cả so với các vụ hack lớn trước đó của chính Lazarus Group.


## Giai đoạn 1: Phân tán ETH và swap stETH/mETH → ETH <a name="phase1"></a>

Ngay sau khi drain được 4 loại tài sản (ETH, stETH, cmETH, mETH), việc đầu tiên Lazarus Group làm là **đồng nhất hóa**: convert toàn bộ về ETH thuần. Lý do đơn giản — ETH thanh khoản hơn, dễ swap hơn, và ít để lại dấu vết giao dịch đặc trưng hơn các liquid staking token.

Trong vòng vài giờ sau cuộc tấn công, toàn bộ 90,376 stETH, 15,000 cmETH và 8,000 mETH đã được swap về ETH thông qua các DEX như Uniswap và Paraswap. Hành động này tạo ra một áp lực bán đột ngột trên thị trường, khiến giá stETH/ETH peg bị lệch nhẹ trong vài phút trước khi arbitrageur cân bằng lại.

Song song với việc swap token, hacker bắt đầu **phân tán ETH ra hàng nghìn ví trung gian** theo kỹ thuật *peel chain* — mỗi ví chỉ nhận và giữ một lượng nhỏ (200–500 ETH) trước khi tiếp tục chuyển sang ví tiếp theo. Mục đích là làm tăng số lượng địa chỉ cần theo dõi, vượt quá khả năng xử lý thủ công của các nhà điều tra.

Như đã thấy ở Phần 1, chỉ riêng trong nhánh ví có cùng `path_id` với ví tấn công chính, hàng nghìn ví mới đã được tạo ra chỉ trong vài ngày sau vụ hack.

---

## Giai đoạn 2: ETH → BTC qua THORChain <a name="phase2"></a>

Đây là giai đoạn quan trọng nhất và cũng gây tranh cãi nhất. Thay vì dùng Tornado Cash như trong các vụ hack trước, Lazarus Group lần này chọn **THORChain** — một giao thức cross-chain swap phi tập trung — làm kênh chính để chuyển ETH sang BTC.

Lý do lựa chọn THORChain rất rõ ràng:
- **Không có KYC**: swap hoàn toàn không cần xác minh danh tính
- **Phi tập trung**: không có một entity nào có thể đơn phương dừng giao dịch
- **Tính thanh khoản cao**: pool ETH/BTC của THORChain đủ lớn để hấp thụ khối lượng khổng lồ
- **Native asset swap**: BTC nhận về là BTC thật sự trên Bitcoin network, không phải wrapped token — khó theo dõi cross-chain hơn

Khoảng 361,255 ETH (trị giá ~900 triệu USD) đã được chuyển qua THORChain để swap sang BTC. Làn sóng giao dịch này khiến THORChain thu về khoảng 5 triệu USD tiền phí giao dịch và đẩy khối lượng 24h của protocol lên mức kỷ lục.

Quan sát một giao dịch mà ví hacker đã thực hiện nạp tiền lên THORChain: [0x641...cc](https://etherscan.io/tx/0x0d9c49ba577b1dac9eff4744624ac9f5ea44d3e43a7e3486c6f27266384a8f35)

<p style="
    text-align: center;
"><img src="/assets/images/posts/2026-02-23/thorchain_tx.png" alt="THORChain Transaction" width="50%"></p>

Trong giao dịch này, Hacker đã thực hiện việc nạp tiền lên THORChain thông qua function `depositWithExpiry` để từ đó chuyển sang Bitcoin ở địa chỉ [1Gm...eJ](https://www.blockchain.com/explorer/addresses/btc/1GmpE7dnqeT77nXcYMsGFXp9hZk6p7kreJ)

Để theo dõi dòng tiền mà Hacker đã nạp lên Thorchain, Chainslake đã decode dữ liệu từ tất cả giao dịch deposit của hacker trên Thorchain vào bảng `bybit_hack.deposit_to_thorchain`

```csv
block_date      , block_number, block_time               , updated_time                , contract_address                          , tx_hash                                                           , from                                      , value             , success, vault                                     , asset                                     , amount            , memo                                                                                                  , expiration
"March 15, 2025", "22,053,322", "March 15, 2025, 3:57 PM", "February 23, 2026, 5:52 AM", 0xd37bbe5744d730a1d98d8dc97c42f0ca46ad7146, 0x5093f3d82aedb5190b8dcfa126e5531dc92efa616f589bce9c03d3b965c2178c, 0xbabd8b1939f7b01c167450ced99d1853150b964e,  32518780000000000, true   , 0x85098119eab098f62d542714f82001be30a208af, 0x0000000000000000000000000000000000000000,  32518780000000000, +:ETH.ETH:thor12jm3ph350lhj523sxhfsm5jstsduqsr24cfvlm:dx:0                                            , 1742055119
"March 16, 2025", "22,057,288", "March 16, 2025, 5:14 AM", "February 23, 2026, 5:52 AM", 0xd37bbe5744d730a1d98d8dc97c42f0ca46ad7146, 0x3aba5444f5a203bb8cf30a6ff33c20f2d23ac8f59fe575d57dc0411cee947ed6, 0xbd76dbdb6f26e1536e58872d79cc65028cd6efa5, 112000000000000000, true   , 0x3b4aa5be6301829285f336e6b7f1f9543ca76872, 0x0000000000000000000000000000000000000000, 112000000000000000, =:LTC.LTC:ltc1qtjwqcrtwv3a3p2ra4ye6msjrepmgc8p72hz946:214876687/1/0:td:70                             , 1742102963
"March 21, 2025", "22,095,437", "March 21, 2025, 1:07 PM", "February 23, 2026, 5:53 AM", 0xd37bbe5744d730a1d98d8dc97c42f0ca46ad7146, 0x4ef0315a7e68d0ae3ca48d7979f6399e687664f318069739a7e5c5de2cd7ce69, 0xba7f89e48770a93b33281044b3a27f3d4e5f6708,                  0, true   , 0x996b24069c31a485e07408370125952b0ff44c0a, 0xdac17f958d2ee523a2206206994597c13d831ec7,          200000000, =:BTC.BTC:bc1qskss5xdcf5ylyfw4cfqauy4atjg0k8zr00f598:229159/1/0:ti:70                                 , 1742563329
"March 15, 2025", "22,053,733", "March 15, 2025, 5:20 PM", "February 23, 2026, 5:52 AM", 0xd37bbe5744d730a1d98d8dc97c42f0ca46ad7146, 0x7b48ca55239132341efec994261fff6005f4f5af7be257ee9a177a4bce36d465, 0xbabd8b1939f7b01c167450ced99d1853150b964e,  15288970000000000, true   , 0x72eb2f7fe7b3e672443dd9639c9ecd3f8bd09393, 0x0000000000000000000000000000000000000000,  15288970000000000, +:ETH.ETH:thor12jm3ph350lhj523sxhfsm5jstsduqsr24cfvlm:dx:0                                            , 1742060087
"March 15, 2025", "22,054,179", "March 15, 2025, 6:49 PM", "February 23, 2026, 5:52 AM", 0xd37bbe5744d730a1d98d8dc97c42f0ca46ad7146, 0xd77848d098e7f3160cadc5e2f246afd8d9579d489c3561bd4da00cbbe177fe50, 0xbabd8b1939f7b01c167450ced99d1853150b964e,                  0, true   , 0x69a836a822a3c7dde5caa82ed05157032d168da9, 0xdac17f958d2ee523a2206206994597c13d831ec7,           30956072, +:ETH.USDT-0XDAC17F958D2EE523A2206206994597C13D831EC7:thor12jm3ph350lhj523sxhfsm5jstsduqsr24cfvlm:dx:0, 1742065439
```

Từ đây chúng ta có thể xác định được tất cả các địa chỉ ví BTC đã nhận tiền từ Hacker qua Thorchain (phần tiếp theo). 

Thống kê số lượng giao dịch và khối lượng ETH đã deposit:

```sql
WITH
  daily_deposit AS (
    SELECT
      block_date,
      count(*) AS number_tx,
      sum(cast(VALUE AS double) / pow (10, 18)) AS eth_value
    FROM
      bybit_hack.deposit_to_thorchain
    GROUP BY
      block_date
  )
SELECT
  *,
  SUM(eth_value) OVER (
    ORDER BY
      block_date
  ) AS total_eth
FROM
  daily_deposit
where block_date <= date '2025-03-03'
```

Kết quả (click vào ảnh để xem biểu đồ)

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/public/question/76bec28d-3dfb-457b-a601-a5dc56db28f1" target="_blank"><img src="/assets/images/posts/2026-02-23/deposit_to_thorchain.png" alt="deposit to thor chain" width="100%"></a></p>

---

## Giai đoạn 3: Xóa dấu vết trên BTC <a name="phase4"></a>

Khi ETH đã được chuyển thành BTC và phân tán ra hàng nghìn ví, giai đoạn tiếp theo bắt đầu: **làm mờ dấu vết trên Bitcoin network**. Lazarus Group đã nạp một phần đầu tiên của BTC vào các mixer như Wasabi và CryptoMixer.

Wasabi Wallet là một Bitcoin wallet với tính năng CoinJoin tích hợp sẵn — kỹ thuật trộn coin cho phép nhiều người dùng kết hợp các UTXO của họ vào cùng một transaction, khiến việc xác định ai gửi cho ai trở nên cực kỳ khó khăn.

Trước hết cần tạo bảng danh sách các ví BTC đã nhận tiền từ Hacker qua Thorchain:

```sql
CREATE TABLE bybit_hack.btc_wallets USING delta AS
WITH
  wallet_tx AS (
    SELECT
      split(memo, ':') [2] AS btc_wallet,
      value / pow(10, 18) AS eth_value
    FROM
      bybit_hack.deposit_to_thorchain
  )
SELECT
  btc_wallet,
  sum(eth_value) AS total_eth
FROM
  wallet_tx
GROUP BY
  btc_wallet
```

Để theo dõi BTC sau khi đã bridge từ Ethereum, chúng ta cần query trên Bitcoin chain, mình sẽ sử dụng bảng `bitcoin_balances.utxo_transfer_day` kết hợp với danh sách wallets đã có để theo dõi:

```sql
WITH
  utxo_hacker_wallet AS (
    SELECT
      t.block_date,
      t.address,
      VALUE
    FROM
      bitcoin_balances.utxo_transfer_day t
      INNER JOIN bybit_hack.btc_wallets w ON t.address = w.btc_wallet
    WHERE
      t.block_date >= date '2025-02-20'
      AND t.block_date <= date '2025-03-10'
  )
SELECT
  block_date,
  sum(VALUE) AS balance_change
FROM
  utxo_hacker_wallet
GROUP BY
  block_date
```

Kết quả: 

<p style="
    text-align: center;
"><a href="https://metabase.chainslake.com/public/question/1e691d4d-6a6b-4d18-bb07-a125ac7d4226" target="_blank"><img src="/assets/images/posts/2026-02-23/bitcoin_transfer_flow.png" alt="Bitcoin transfer flow" width="100%"></a></p>

Ta thấy rằng trong vòng 10 ngày sau vụ tấn công, Hacker đã swap ETH sang BTC thông qua Thorchain, sau đó tẩu tán toàn bộ số BTC sang các địa chỉ ví khác nhau. Việc theo dõi số tiền này là cực kỳ phức tạp, đòi hỏi các phân tích chuyên sâu hơn. Chainslake sẽ tiếp tục nghiên cứu và cập nhật những phân tích tiếp theo về vụ hack này. 

Cảm ơn bạn đọc đã theo dõi!

---

*Dữ liệu sử dụng trong bài viết này được lấy từ Chainslake Blockchain Data Warehouse tính đến ngày 04/03/2025. Một số địa chỉ ví và con số cụ thể có thể thay đổi khi có thêm dữ liệu mới từ các nguồn điều tra.*
