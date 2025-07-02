# Hệ thống gợi ý xe ô tô kết hợp nội dung và mô hình ngôn ngữ lớn

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-13+-blue.svg)](https://www.postgresql.org/)

> **Trình bày tại:** HỘI THẢO CÔNG NGHỆ THÔNG TIN VÀ TRUYỀN THÔNG 2025 lần thứ 10  
> **Địa điểm:** Đại học Đà Lạt  
> **Ngày:** 04/7/2025

## 📋 Tổng quan

Nghiên cứu này trình bày một hệ thống gợi ý xe ô tô tiên tiến dựa trên nội dung (content-based filtering), được tăng cường bởi kỹ thuật nhúng văn bản (text embedding) và mô hình ngôn ngữ lớn (LLM) để tạo giải thích minh bạch.

### 🎯 Vấn đề giải quyết
- **Quá tải thông tin** trong thương mại điện tử ô tô trực tuyến
- **Hạn chế của các hệ thống truyền thống:**
  - Lọc cộng tác: Cold-start, dữ liệu thưa thớt
  - Lọc nội dung cổ điển: Chỉ dùng metadata cấu trúc, bỏ qua thông tin ngữ nghĩa

### 💡 Giải pháp đề xuất
Hệ thống content-based "lai ghép" kết hợp:
- **Dữ liệu cấu trúc** (129 chiều): Giá, hãng, năm, km, kiểu dáng...
- **Vector ngữ nghĩa** (768 chiều): Text embedding từ mô tả sản phẩm
- **XAI với LLM**: Google Gemini tạo giải thích tự nhiên và minh bạch

## 🏗️ Kiến trúc hệ thống

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Raw Data      │    │ Feature Vector  │    │  Recommendation │
│  Collection     │───▶│   Generation    │───▶│   & XAI Engine  │
│                 │    │  (897D Hybrid)  │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
        │                       │                       │
        ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  PostgreSQL     │    │   pgvector      │    │  Google Gemini  │
│   Database      │    │  HNSW Index     │    │  API (XAI)      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Luồng xử lý chính:
1. **Offline**: Tiền xử lý dữ liệu → Tạo vector đặc trưng → Lưu vào DB
2. **Online**: Xây dựng hồ sơ user → k-NN search → Tạo giải thích XAI

## 📊 Kết quả thực nghiệm

### Hiệu suất gợi ý (Metrics@10)
| Phương pháp | Precision@10 | Recall@10 | NDCG@10 |
|-------------|--------------|-----------|---------|
| Popularity  | 0.042       | 0.051     | 0.045   |
| CB-Struct   | 0.115       | 0.128     | 0.121   |
| CB-Embed    | 0.138       | 0.145     | 0.142   |
| **Hybrid-Rec** | **0.162** | **0.171** | **0.168** |

### Đánh giá XAI (20 users, thang 1-5)
| Tiêu chí | Điểm trung bình |
|----------|-----------------|
| Độ rõ ràng | 4.65 |
| Độ hữu ích | 4.52 |
| Tính thuyết phục | 4.38 |

## 🛠️ Công nghệ sử dụng

- **Backend**: Python, FastAPI
- **Database**: PostgreSQL + pgvector extension
- **Text Embedding**: Google text-embedding-004 (768 chiều)
- **LLM**: Google Gemini API for XAI
- **ML Libraries**: scikit-learn, numpy, pandas
- **Vector Search**: HNSW index với khoảng cách Euclidean L2

## 📝 Dữ liệu thực nghiệm

- **Xe ô tô**: 5,000+ records
- **Tương tác**: 12,000+ interactions (xem, nhấp chuột)
- **Người dùng**: 800+ users
- **Nguồn**: Trang web rao vặt xe lớn tại Việt Nam (Q4 2024 - Q1 2025)

## 🔮 Hướng phát triển

- [ ] **Hybrid approach**: Kết hợp với collaborative filtering
- [ ] **Learnable weights**: Học trọng số tối ưu cho các thành phần vector
- [ ] **Cost optimization**: Tối ưu chi phí gọi LLM API
- [ ] **Advanced prompting**: Cải tiến kỹ thuật prompt engineering
- [ ] **Real-time learning**: Cập nhật mô hình theo thời gian thực
- [ ] **Multi-modal features**: Tích hợp hình ảnh xe

## 👥 Tác giả

- **Ngô Nguyễn Tường Nghi** - Khoa Công nghệ thông tin, Trường Đại học Nha Trang
  - Email: nghinnt@ntu.edu.vn
  
- **Nguyễn Đình Hưng** - Khoa Công nghệ thông tin, Trường Đại học Nha Trang
  - Email: hungnd@ntu.edu.vn

## 📄 Trích dẫn

Nếu bạn sử dụng nghiên cứu này, vui lòng trích dẫn:

```bibtex
@inproceedings{ngo2025car,
  title={Hệ thống gợi ý xe ô tô kết hợp nội dung và mô hình ngôn ngữ lớn},
  author={Nghi, Ngô Nguyễn Tường and Hưng, Nguyễn Đình},
  booktitle={Hội thảo Công nghệ thông tin và Truyền thông 2025 lần thứ 10},
  year={2025},
  organization={Đại học Đà Lạt}
}
```

## 📜 License

Dự án này được phát hành dưới [MIT License](LICENSE).

## 🤝 Đóng góp

Chúng tôi chào đón mọi đóng góp! Vui lòng:

1. Fork repository này
2. Tạo feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit thay đổi (`git commit -m 'Add some AmazingFeature'`)
4. Push lên branch (`git push origin feature/AmazingFeature`)
5. Mở Pull Request

## 📞 Liên hệ

Nếu có thắc mắc về nghiên cứu, vui lòng liên hệ qua email hoặc tạo issue trong repository này.

---

⭐ **Nếu nghiên cứu này hữu ích, hãy cho chúng tôi một star!** ⭐
