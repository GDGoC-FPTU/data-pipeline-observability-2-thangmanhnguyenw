# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600710

 **Name:** Nguyễn Trần Mạnh Thắng

 **Date:** 10/6/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:


| Scenario                          | Agent Response                                                   | Accuracy (1-10) | Notes                                                                                                               |
| --------------------------------- | ---------------------------------------------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------- |
| Clean Data (`processed_data.csv`) | Based on my data, the best choice is Laptop at $1200.            | 10              | Trả lời hợp lý: Laptop là sản phẩm điện tử có giá cao nhất trong dữ liệu đã qua ETL (Laptop $1200, Monitor $300).   |
| Garbage Data (`garbage_data.csv`) | Based on my data, the best choice is Nuclear Reactor at $999999. | 1               | Sai hoàn toàn: Agent chọn giá trị ngoại lai $999999 thay vì Laptop $1200 vì dữ liệu chưa được kiểm tra và làm sạch. |


---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

(Viet nhan xet cua ban o day — it nhat 50 tu)

(Hay phan tich cac van de nhu Duplicate IDs, wrong data types, outliers, null values
va giai thich tai sao chung anh huong den ket qua cua Agent.)

- **Giá trị ngoại lai (outliers):** "Nuclear Reactor" có giá $999999 — giá trị bất thường bị Agent coi là "tốt nhất".
- **ID trùng lặp:** id=1 xuất hiện 2 lần (Laptop và Banana), gây nhầm lẫn về bản ghi hợp lệ.
- **Sai kiểu dữ liệu:** "Broken Chair" có price = "ten dollars" (chuỗi, không phải số) — có thể gây lỗi hoặc bị bỏ qua khi xử lý.
- **Giá trị null / không hợp lệ:** "Ghost Item" có id rỗng, price = 0, category rỗng — dữ liệu không hợp lệ vẫn nằm trong file.

---

Khi dữ liệu chưa qua bước **kiểm tra** (loại giá ≤ 0, category rỗng) và **biến đổi** (chuẩn hóa category), Agent đọc trực tiếp file CSV thô và đưa ra kết luận sai, dù câu hỏi (prompt) không thay đổi.

## 3. Ket luan

**Quality Data > Quality Prompt?** (Dong y hay khong? Giai thich ngan gon.) **(Viet ket luan cua ban o day)**

 **Đồng ý.** Trong thí nghiệm này, câu hỏi và logic Agent giống nhau cho cả hai kịch bản; chỉ khác nguồn dữ liệu. Với `processed_data.csv` (sau ETL), Agent trả lời Laptop $1200 — hợp lý. Với `garbage_data.csv`, cùng một prompt nhưng Agent trả lời Nuclear Reactor $999999 — vô nghĩa. Điều này cho thấy **chất lượng dữ liệu đầu vào** quyết định độ tin cậy của AI/RAG hơn là cách viết prompt. Pipeline ETL (kiểm tra, biến đổi, lưu trữ) là tầng quan trọng để đảm bảo Agent "học" từ cơ sở tri thức đáng tin cậy.