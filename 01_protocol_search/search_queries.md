# TÀI LIỆU ĐẶC TẢ TRUY VẤN TÌM KIẾM TRÊN BỐN NGUỒN DỮ LIỆU

**Mã tài liệu:** `prisma_audit/search_queries.md`  
**Chuẩn báo cáo:** PRISMA 2020 & PRISMA-S  
**Phạm vi thời gian:** 2016 đến 2026  
**Ngôn ngữ:** Tiếng Anh (English)  
**Loại tài liệu:** Bài báo tạp chí (Journal Articles)  

---

## 1. CẤU TRÚC LOGIC BA KHỐI VÀ BỐN NHÁNH BÀI TOÁN

Bộ truy vấn được thiết kế đảm bảo bao phủ toàn diện hai nhóm cây trồng và bốn nhánh bài toán thị giác máy tính, **tuyệt đối không chứa bất kỳ tên bộ dữ liệu cụ thể nào**.

### Khối A: Cây trồng (Crop)
```text
rice OR "Oryza sativa" OR maize OR corn OR "Zea mays"
```

### Khối B: Bệnh trên lá (Foliar Disease)
```text
"leaf disease" OR "foliar disease" OR "leaf blight" OR "leaf spot" OR "leaf rust" 
OR blast OR "brown spot" OR tungro OR "bacterial blight" OR "northern leaf blight" 
OR "gray leaf spot" OR "grey leaf spot" OR "common rust" OR "streak virus" OR "lethal necrosis"
```

### Khối C: Học sâu và Kiến trúc nơ-ron (Deep Learning)
```text
"deep learning" OR "deep neural network" OR "convolutional neural network" OR CNN 
OR "vision transformer" OR transformer OR YOLO OR "Faster R-CNN" OR "Mask R-CNN" 
OR "U-Net" OR "UNet" OR DeepLab OR "generative adversarial network" OR GAN OR "diffusion model"
```

### Bốn nhánh bài toán mở rộng (Tasks T1–T4)
- **T1 — Phân loại:** `classification OR recognition OR diagnosis`
- **T2 — Phát hiện/Định vị:** `"object detection" OR localization OR localisation OR "bounding box" OR "lesion detection"`
- **T3 — Phân đoạn:** `segmentation OR "semantic segmentation" OR "instance segmentation" OR "lesion segmentation"`
- **T4 — Mô hình sinh & Tăng cường:** `"image synthesis" OR "synthetic data" OR "generative adversarial network" OR GAN OR diffusion OR "generative data augmentation"`

*Lưu ý logic:* Các nhánh bài toán T1–T4 được liên kết bằng toán tử `OR`, hoặc được tích hợp trong nhóm từ khóa phương pháp. Tuyệt đối không dùng `T1 AND T2 AND T3 AND T4`.

---

## 2. CHUYỂN ĐỔI CÚ PHÁP CHO TỪNG NGUỒN DỮ LIỆU

### Nguồn 1: Scopus (Elsevier)
- **Query ID:** `Q_SCOPUS_01`
- **Mục tiêu:** Truy vấn nền toàn diện bao phủ hai cây trồng và bốn nhóm bài toán.
- **Trường tìm kiếm:** `TITLE-ABS-KEY`
- **Bộ lọc:** `PUBYEAR > 2015 AND PUBYEAR < 2027 AND DOCTYPE ( ar ) AND LANGUAGE ( english )`
- **Chuỗi truy vấn nguyên văn:**
```text
TITLE-ABS-KEY (
  ( rice OR "Oryza sativa" OR maize OR corn OR "Zea mays" )
  AND ( "leaf disease" OR "foliar disease" OR "leaf blight" OR "leaf spot"
        OR "leaf rust" OR blast OR "brown spot" OR tungro OR "bacterial blight"
        OR "northern leaf blight" OR "gray leaf spot" OR "grey leaf spot"
        OR "common rust" OR "streak virus" OR "lethal necrosis" )
  AND ( "deep learning" OR "convolutional neural network" OR CNN
        OR "vision transformer" OR transformer OR YOLO OR "Faster R-CNN"
        OR "Mask R-CNN" OR "U-Net" OR "UNet" OR "semantic segmentation"
        OR "instance segmentation" OR "object detection" OR "image classification"
        OR "generative adversarial network" OR GAN OR "diffusion model" )
)
AND PUBYEAR > 2015 AND PUBYEAR < 2027
AND DOCTYPE ( ar )
AND LANGUAGE ( english )
```
- **Số bản ghi xuất thô:** 499 bản ghi (`scopus.csv`).

---

### Nguồn 2: IEEE Xplore
- **Query ID:** `Q_IEEE_01`
- **Mục tiêu:** Tìm kiếm qua trường Command Search với cú pháp chuẩn IEEE.
- **Trường tìm kiếm:** `All Metadata`
- **Bộ lọc giao diện:** Year: 2016–2026; Content Type: **Journals** (loại bỏ Conferences).
- **Chuỗi truy vấn nguyên văn:**
```text
(("All Metadata":rice OR "All Metadata":"Oryza sativa" OR "All Metadata":maize OR "All Metadata":corn OR "All Metadata":"Zea mays")
 AND ("All Metadata":"leaf disease" OR "All Metadata":"foliar disease" OR "All Metadata":"leaf blight" OR "All Metadata":"leaf spot"
      OR "All Metadata":"leaf rust" OR "All Metadata":blast OR "All Metadata":"brown spot" OR "All Metadata":tungro
      OR "All Metadata":"bacterial blight" OR "All Metadata":"northern leaf blight" OR "All Metadata":"gray leaf spot"
      OR "All Metadata":"common rust")
 AND ("All Metadata":"deep learning" OR "All Metadata":CNN OR "All Metadata":transformer
      OR "All Metadata":YOLO OR "All Metadata":"U-Net" OR "All Metadata":segmentation
      OR "All Metadata":"object detection" OR "All Metadata":GAN))
```
- **Số bản ghi xuất thô:** 41 bản ghi (`IEEE.csv`).

---

### Nguồn 3: ScienceDirect (Elsevier)
- **Giới hạn cú pháp:** ScienceDirect áp đặt giới hạn không quá 8 toán tử Boolean trên mỗi trường và không hỗ trợ lồng dấu ngoặc phức tạp trong giao diện công cộng. Do đó, truy vấn được phân tách theo hai lượt chuyên biệt cho từng cây trồng kết hợp các từ khóa cốt lõi.
- **Lượt A (Cây lúa):** `Q_SD_RICE`
  - Chuỗi tìm kiếm: `rice leaf disease deep learning`
  - Bộ lọc: Năm 2016–2026; Loại bài báo: Research articles.
  - Số bản ghi xuất thô: 438 bản ghi (gồm 5 đợt tải: `CS_Lượt A, lúa_1.ris` đến `5.ris`).
- **Lượt B (Cây ngô):** `Q_SD_MAIZE`
  - Chuỗi tìm kiếm: `maize leaf disease deep learning`
  - Bộ lọc: Năm 2016–2026; Loại bài báo: Research articles.
  - Số bản ghi xuất thô: 336 bản ghi (gồm 4 đợt tải: `CS_Lượt B, ngô_1.ris` đến `4.ris`).
- **Tổng bản ghi xuất thô từ ScienceDirect:** 774 bản ghi.
- **Xử lý trùng lặp:** Hợp nhất và khử trùng giữa Lượt A và Lượt B theo DOI và tiêu đề chuẩn hóa qua script `03_deduplicate_records.py`.

---

### Nguồn 4: MDPI (Multidisciplinary Digital Publishing Institute)
- **Giới hạn cú pháp:** Công cụ tìm kiếm của MDPI không hỗ trợ nhóm ngoặc Boolean nâng cao. Tìm kiếm được thực hiện qua trường tìm kiếm tổng thể kết hợp bộ lọc loại hình công bố.
- **Lượt A (Cây lúa):** `Q_MDPI_RICE`
  - Chuỗi tìm kiếm: `rice leaf disease deep learning`
  - Bộ lọc: Article Type = Article; Publication Year = 2016–2026.
  - Tệp lưu trữ: `MPDI_Lượt A, lúa.ris` (18 bản ghi).
- **Lượt B (Cây ngô):** `Q_MDPI_MAIZE`
  - Chuỗi tìm kiếm: `maize leaf disease deep learning`
  - Bộ lọc: Article Type = Article; Publication Year = 2016–2026.
  - Tệp lưu trữ: `MPDI_Lượt B, ngô.ris` (1 bản ghi).
- **Tổng bản ghi xuất thô từ MDPI:** 19 bản ghi.
