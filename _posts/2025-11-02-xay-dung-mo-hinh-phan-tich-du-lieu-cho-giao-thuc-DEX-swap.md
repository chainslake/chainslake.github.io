---
title: Xây dựng mô hình phân tích dữ liệu cho giao thức DEX swap
layout: post
date: 2025-11-02 06:00
image: /assets/images/posts/2025-08-10/chainslake_thumbnail.png
headerImage: false
published: false
tag:
- vietnam
category: blog
author: lake
description: Trong bài viết này, chúng ta sẽ cùng tìm hiểu về giao thức của sàn giao dịch token phi tập trung (DEX) và xây dựng mô hình phân tích dữ liệu cho giao thức này nhé.
---

Decentralized Exchange (DEX) - Sàn giao dịch phi tập trung là một trong những thành phần cốt lõi nhất của hệ sinh thái DeFi (Decentralized Finance), nơi mà người dùng có thể mua bán, trao đổi token mà không cần một sàn giao dịch trung gian (ví dụ như Binance hoặc Coinbase). DEX đã bắt đầu được giới thiệu từ năm 2017 với giao thức EtherDelta, nhưng phải đến năm 2018 với sự ra đời của giao thức Uniswap đã mở ra kỷ nguyên mới cho DEX với cuộc cách mạng AMM (Automation Market Maker). 

# Nội dung
1. [Pool than khoản là gì](#liquidity_pool)
2. [Giải mã dữ liệu, lấy thông tin từ contract](#decode_data)
3. [Xây dựng các bảng insight](#insight_data)
4. [Kết luận](#conclusion)

## Pool thanh khoản là gì?<a name="liquidity_pool"></a>

Uniswap được phát triển từ nhu cầu hoán đổi (Swap) một token để lấy 1 token khác thông qua 1 Pool thanh khoản giữa 2 token đó. Pool thanh khoản là một cơ chế cân bằng giữa 2 token 



