# ĐỀ CƯƠNG VÀ GIAO THỨC TỔNG QUAN THEO PRISMA 2020

**Dự án:** Tổng quan hệ thống về ứng dụng Deep Learning trong nhận diện bệnh lá lúa và ngô (2016–2026)  
**Tiêu chuẩn hướng dẫn:** PRISMA 2020 Statement (Page et al., 2021, DOI: 10.1136/bmj.n71)  
**Ngày hoàn thiện:** 2026-09-25  
**Thực hiện:** Nhóm tác giả  
**Khai báo sử dụng AI tạo sinh:** Bước sàng lọc tiêu đề và tóm tắt có hỗ trợ AI. Ngoài ra, AI tạo sinh (Google Gemini 2.5 Flash) chỉ được dùng để chỉnh sửa ngữ pháp, hỗ trợ định dạng và sửa lỗi nhỏ trong mã lệnh. Công cụ AI không tham gia vào việc xây dựng giả thuyết khoa học, diễn giải dữ liệu thực nghiệm hoặc đưa ra kết luận; toàn bộ các ý tưởng học thuật và diễn giải cuối cùng đều hoàn toàn do các tác giả thực hiện.  

---

## 1. MỤC TIÊU VÀ CÂU HỎI NGHIÊN CỨU

Tổng quan nhằm khảo sát, phân tích và hệ thống hóa các nghiên cứu học sâu (deep learning) phục vụ nhận diện bệnh trên lá của hai cây trồng lương thực chủ lực là lúa (*Oryza sativa*) và ngô (*Zea mays*) trong giai đoạn 10 năm (2016–2026).

Các câu hỏi nghiên cứu cốt lõi:
- **Q1 (Phạm vi & Bài toán):** Các bài toán thị giác máy tính nào (Phân loại, Phát hiện/định vị, Phân đoạn, Mô hình sinh) đã được phát triển trên bệnh lá lúa và ngô? Phân bố giữa hai loại cây trồng này ra sao?
- **Q2 (Phương pháp & Kiến trúc):** Những kiến trúc mạng nơ-ron nào (CNN, Vision Transformer, YOLO, U-Net, GAN/Diffusion) chiếm ưu thế qua các giai đoạn?
- **Q3 (Dữ liệu & Nguồn gốc nhãn):** Các bộ dữ liệu được sử dụng có đặc điểm gì về quy mô, điều kiện thu nhận (phòng thí nghiệm vs ngoài đồng ruộng), tính cân bằng, và nguồn gốc gán nhãn (ai gán, bằng công cụ gì, có công khai không)?
- **Q4 (Giao thức đánh giá & Khoảng trống thực tế):** Các nghiên cứu áp dụng chiến lược phân chia dữ liệu thế nào? Có kiểm thử ngoài miền (out-of-domain / cross-dataset) hoặc đánh giá thực địa không?

---

## 2. TIÊU CHÍ LỰA CHỌN VÀ LOẠI TRỪ (ELIGIBILITY CRITERIA)

### 2.1 Tiêu chí nhận (Inclusion Criteria)
1. **Thời gian xuất bản:** Từ 01/01/2016 đến ngày chốt tìm kiếm thực tế trong năm 2026.
2. **Ngôn ngữ:** Toàn văn bằng tiếng Anh.
3. **Loại tài liệu & Mô hình tạp chí:** Bài báo nghiên cứu gốc (original research articles) xuất bản trên các **tạp chí truy cập mở hoàn toàn (Fully Open Access journals)** được lập chỉ mục chính thức (DOAJ, Scopus, Web of Science).
4. **Đối tượng cây trồng:** Cây lúa (*Oryza sativa*) hoặc cây ngô (*Zea mays*). Bài nghiên cứu đa cây trồng chỉ được nhận nếu có báo cáo thực nghiệm định lượng riêng biệt cho lúa hoặc ngô.
5. **Bộ phận & Đối tượng bệnh học:** Bệnh biểu hiện trên lá (foliar/leaf diseases and lesions). Ảnh chụp cận cảnh lá, tán cây hoặc ảnh viễn thám/UAV chụp tán ruộng bị bệnh lá đều được chấp nhận.
6. **Phương pháp:** Ứng dụng các kiến trúc mạng nơ-ron học sâu (Deep Neural Networks) có huấn luyện trọng số.
7. **Bài toán thực nghiệm:** Giải quyết ít nhất một trong bốn nhóm bài toán:
   - T1: Phân loại bệnh (Image-level classification/diagnosis).
   - T2: Phát hiện/định vị tổn thương bằng khung bao (Object detection/localization with bounding box).
   - T3: Phân đoạn lá hoặc tổn thương phục vụ phân tích bệnh (Semantic/instance segmentation).
   - T4: Sinh/tăng cường dữ liệu bằng mô hình học sâu phục vụ chẩn đoán bệnh (GAN, Diffusion, generative augmentation).
8. **Đánh giá thực nghiệm:** Phải có kết quả đo lường định lượng trên tập kiểm thử giữ lại (test set).

### 2.2 Tiêu chí loại trừ (Exclusion Criteria)
- **X1 (Sai cây trồng):** Không liên quan đến lúa hoặc ngô; hoặc bài đa cây trồng nhưng chỉ báo cáo kết quả gộp chung, không tách riêng lúa/ngô.
- **X2 (Sai bộ phận):** Bệnh trên hạt, rễ, thân; đếm bông lúa, dự đoán năng suất tổng thể, phát hiện cỏ dại.
- **X3 (Không dùng Deep Learning):** Chỉ dùng học máy truyền thống (SVM, Random Forest, k-NN thuần) trên đặc trưng thủ công (GLCM, HOG, Color Histograms).
- **X4 (Thiếu đánh giá định lượng):** Chỉ mô tả ý tưởng, đề xuất khung kiến trúc hệ thống IoT, công bố dataset mà không có thực nghiệm huấn luyện và kiểm thử mô hình.
- **X5 (Không truy xuất được toàn văn):** Không thể tải hoặc trích xuất nội dung toàn văn sau các nỗ lực truy xuất.
- **X6 (Loại hình công bố ngoài phạm vi):** 
  - Kỷ yếu hội nghị (Conference Proceedings / Procedia series).
  - Tạp chí thuê bao (Subscription) hoặc Tạp chí lai (Hybrid Journals), kể cả khi bài viết riêng lẻ được mở quyền truy cập (Open Access under hybrid).
  - Bài tổng quan (Review / Survey), sách, chương sách, luận án, bài xã luận, preprint chưa qua bình duyệt tạp chí.

---

## 3. NGUỒN THÔNG TIN VÀ CHIẾN LƯỢC TÌM KIẾM

Quy trình PRISMA giới hạn tuyệt đối ở bốn cơ sở dữ liệu học thuật điện tử:
1. **Scopus** (Elsevier)
2. **IEEE Xplore** (IEEE)
3. **ScienceDirect** (Elsevier)
4. **MDPI** (Multidisciplinary Digital Publishing Institute)

*Quy tắc loại trừ nguồn:* Không sử dụng OpenAlex, Google Scholar hoặc kỹ thuật truy ngược tài liệu tham khảo (citation chasing) làm nguồn tìm kiếm trong PRISMA. DOAJ và Crossref chỉ dùng làm công cụ xác minh metadata và mô hình truy cập mở của tạp chí.

Bộ truy vấn được thiết kế theo cấu trúc logic 3 khối:
$$\text{Query} = \text{Crop (A)} \land \text{Leaf Disease (B)} \land \text{Deep Learning (C)}$$
kết hợp mở rộng bao phủ bốn nhánh bài toán T1, T2, T3, T4.

---

## 4. QUẢN LÝ DỮ LIỆU VÀ QUY TRÌNH SÀNG LỌC

1. **Thu nhận dữ liệu thô:** Lưu nguyên trạng tại `prisma_audit/raw_exports/`.
2. **Khử trùng lặp:** Áp dụng thuật toán đối soát 2 cấp: (1) Chuẩn hóa DOI; (2) Chuẩn hóa tiêu đề + tác giả đầu + năm xuất bản. Ghi vết toàn bộ các bản ghi bị loại tại `duplicate_log.csv`.
3. **Sàng lọc Tiêu đề / Tóm tắt:** Đánh giá từng bản ghi duy nhất theo 6 mã loại trừ; bài chưa rõ thông tin đánh dấu `UNCLEAR` để chuyển toàn văn.
4. **Thử nghiệm kiểm soát chất lượng (Pilot):** Chạy thử 10 bài ứng viên đa dạng trước khi đọc toàn văn diện rộng; lập `pilot_10_papers.csv` và `PILOT_REPORT.md`.
5. **Truy xuất và Đánh giá toàn văn:** Trích xuất văn bản từ tệp PDF gốc; xác minh tính xác thực của số liệu, bảng biểu và phương pháp; lập `evidence_matrix.csv`.
6. **Xác minh Tạp chí Fully OA:** Đối chiếu danh mục DOAJ và chính sách nhà xuất bản; lập `journal_oa_directory.csv`.


---

## 5. SỬA ĐỔI GIAO THỨC (ghi nhận 2026-09-26)

Các sửa đổi dưới đây thay thế những điểm tương ứng ở Mục 2 và Mục 4. Mọi thay đổi được áp dụng đồng loạt cho toàn bộ bản ghi, kể cả những bài đã có quyết định trước đó; các phiên bản quyết định cũ được giữ lại để truy vết.

1. **Xác minh tạp chí truy cập mở hoàn toàn.** Đối soát theo ISSN với bản xuất toàn bộ danh mục DOAJ ngày 2026-09-26 (`doaj_journals_2026-09-26.csv`). Tạp chí không có trong DOAJ chỉ được xếp là truy cập mở hoàn toàn khi chính sách chính thức của nhà xuất bản quy định mọi bài đều truy cập mở (danh sách trong `scripts/04d_verify_journals_doaj_offline.py`). Bảng cũ lưu ở `journal_oa_directory_v1_backup.csv`; thay đổi từng tạp chí ở `journal_oa_changes.csv`.
2. **Mã X1 và X3 ở bước tiêu đề/tóm tắt.** Chỉ áp dụng khi tóm tắt khẳng định rõ (X1: nêu cây trồng khác và không nhắc lúa/ngô; X3: chỉ nêu phương pháp học máy cổ điển). Trường hợp không chắc chắn được chuyển sang toàn văn.
3. **Sàng lọc tiêu đề/tóm tắt vòng hai.** Sau vòng quy tắc, từng ứng viên được đọc tiêu đề, tóm tắt và từ khóa với sự hỗ trợ của mô hình ngôn ngữ lớn (Google Gemini) làm công cụ gợi ý sơ bộ; một tác giả (T.-H.T.-L.) duyệt từng đề xuất, một tác giả khác (M.-H.L.) kiểm tra các bản ghi bị loại, trường hợp chưa thống nhất do tác giả thứ ba (T.-N.D.) quyết định; quyết định, mã, lý do và mức tin cậy được ghi trong các cột `round2_*` của `screening_decisions.csv`.
4. **Sửa đổi A, phạm vi cây trồng.** Chỉ nhận nghiên cứu có dữ liệu thực nghiệm thuần lúa và/hoặc ngô. Nghiên cứu đa cây trồng bị loại kể cả khi có kết quả tách riêng (mã X1_MULTI ở tóm tắt, G1-POOLED ở toàn văn). Thay thế tiêu chí nhận 2.1.4.
5. **Sửa đổi B, mô thức ảnh.** Chỉ nhận ảnh RGB. Ảnh siêu phổ, đa phổ, viễn thám UAV và ảnh nhiệt bị loại (mã X8_MODALITY ở tóm tắt, G1-SCOPE ở toàn văn). Thay thế phần "ảnh viễn thám/UAV được chấp nhận" ở tiêu chí 2.1.5. Nhật ký 73 bản ghi bị loại theo A và B: `amendment_AB_log_2026-09-26.csv`.
6. **Đánh giá toàn văn bốn cổng.** Thay cho bước kiểm tra từ khóa của script 07. Tiêu chí ở `fulltext_review/REVIEW_INSTRUCTIONS.md`, gồm phụ lục sáu quy tắc bổ sung lập sau đợt đánh giá đầu tiên. Kết quả ở `fulltext_review/fulltext_review_master.csv`. Mỗi báo cáo bị loại được ghi một mã duy nhất là cổng đầu tiên không đạt.
7. **Truy xuất toàn văn.** Sổ truy xuất `fulltext_manifest.csv`; báo cáo không truy cập được ghi ở `not_retrievable.csv` với lý do.
8. **Số trích dẫn** (Scopus, tại thời điểm tìm kiếm) chỉ dùng để sắp thứ tự ưu tiên đọc và để phân tích, không dùng làm tiêu chí lựa chọn.
9. **Rà soát lại nhãn rò rỉ (ghi nhận 2026-10-03).** 92 nghiên cứu có nguy cơ được rà soát lại nhãn theo sáu tiêu chí quy trình L1–L6; nhãn do công cụ AI đề xuất kèm trích đoạn bằng chứng từ toàn văn và một tác giả (T.-H.T.-L.) rà soát, xác nhận; cờ dùng tập kiểm định làm tập test được tách khỏi nhãn rò rỉ. Hồ sơ chi tiết lưu tại `04_included/leakage_review_92.csv`.

10. **Nhãn L3 theo số đo ảnh trùng (ghi nhận 2026-10-04).** Sáu bộ công khai được quét ảnh gần trùng (mã băm tri giác và đặc trưng DINOv2, `leakage_experiments/dedup_scan.py`, `dup_criteria.py`; tổng hợp ở `synthesis/dedup_measurements.json`). Quy tắc L3 chung cho bản Kaggle "Corn or Maize" 4 188 ảnh bị bỏ vì số đo chỉ 0,5 % ảnh trùng và không có bản tăng cường sẵn; 18 nghiên cứu được gán lại nhãn theo giao thức của chính chúng (`scripts/32_relabel_l3_by_measurement.py`, nhật ký `synthesis/leakage_relabel_L3_2026-10-04.csv`). Quy tắc L2 cho bộ Sethy được giữ vì 95,8 % ảnh test có bản gần trùng trong tập huấn luyện khi chia ngẫu nhiên.
11. **Phân tích bổ sung sau phản biện nội bộ (2026-10-03/04).** Độ chính xác của 94 báo cáo bị loại vì rò rỉ và cặp chỉ số cùng nguồn/khác nguồn của 16 nghiên cứu có ngoại kiểm được trích từ toàn văn (`synthesis/excluded_leak_metrics_94.csv`, `synthesis/external_delta_16.csv`); phân tích độ nhạy theo `scripts/27_leakage_reanalysis.py`; thí nghiệm có kiểm soát về giao thức chia tập theo `leakage_experiments/leakage_experiment.py`.
12. **Áp dụng nhất quán tiêu chí đủ điều kiện sau rà soát độc lập (ghi nhận 2026-10-05).** (a) Ba báo cáo không có tập giữ lại (L5: REP-0297, REP-0390, REP-0478) trước đó được nhận và xếp nghi rò rỉ, nay bị loại ở cổng 3 với mã G3-PROTOCOL. (b) REP-0229 trước đó bị loại vì rò rỉ rõ ràng được nhận lại với nhãn nghi rò rỉ (L1), vì bài không nêu số ảnh gốc và không nói rõ tăng cường làm trước khi chia. (c) L4 (tập test chứa bản tăng cường của chính ảnh test) chỉ ghi thành cờ trong `leakage_criteria`, không làm nghiên cứu thành nghi rò rỉ, vì không đưa thông tin của tập huấn luyện vào tập test; REP-0973 và REP-0106 (chia trước khi tăng cường, chỉ có L4) chuyển thành sạch, REP-0419 được thêm L1 vì không nêu thứ tự chia và tăng cường. (d) REP-0037 được mã có ngoại kiểm (mô tả ở tr. 6 nhưng không báo cáo con số). Tập nhận còn 202 nghiên cứu; loại ở toàn văn 138 (G3-LEAK 93). Thực hiện bởi `scripts/38_round6_review_fixes.py`; nhật ký `synthesis/review_fixes_round6_2026-10-05.csv`.

Số liệu PRISMA cuối cùng: `prisma_counts.csv`, tính bởi `scripts/08b_calculate_prisma_final.py`.
