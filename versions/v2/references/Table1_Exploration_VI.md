# Khám phá 5 nguồn dữ liệu ở Table 1 và đề xuất schema/ontology

Ngày kiểm tra: 05/10/2026. Phạm vi: 5 nguồn trong Table 1 của NIH-Pilot_Datasets_and_Data_Plan.docx. Không bao gồm All of Us, N3C hoặc các nguồn xin quyền ở Table 2.

Mục tiêu theo Zoom: hiểu cấu trúc dữ liệu, tìm thuộc tính hữu ích và dùng findings để định hướng thiết kế database/ontology. Đây là khám phá dữ liệu mẫu và các lớp dữ liệu liên quan, không phải đã tải toàn bộ reads, trích toàn bộ corpus PMC, huấn luyện model hoặc nhập PostgreSQL.

## 1. So sánh nhanh

| Nguồn | Dữ liệu thực tế đã kiểm tra | Đơn vị quan sát | Vai trò trong pilot |
|---|---|---|---|
| Dryad HCT BAL | TableS6.csv; metadata và danh sách file Dryad | Gene × subject | Thử biểu diễn host response trong cohort HCT nhỏ |
| PRJNA984933 | Phụ lục 112 bệnh nhân; Table 1–3; 102 hàng RunInfo; một BioSample mẫu | Bệnh nhân, kết quả xét nghiệm, run; bảng tổng hợp riêng | Biểu diễn xét nghiệm âm tính và chẩn đoán sau đánh giá |
| PRJNA808886 | Hai phụ lục; Table 1–3; 66 hàng RunInfo; một BioSample mẫu | Bệnh nhân có nhiều vi sinh vật/kết quả | So sánh mNGS/CMT và biểu diễn đồng nhiễm |
| PRJNA1056765 | Excel S1–S3; 868 hàng RunInfo; ma trận Figshare; metadata GEO; một BioSample mẫu | Mẫu DNA/RNA, run, gene/taxon/feature × mẫu | Thử liên kết nhiều modality và ghi nguồn xử lý |
| PMC Open Access Subset | Hai bài mẫu ở dạng JATS XML; một bài ở BioC XML; tài liệu API | Bài báo, đoạn văn, bảng, hình, citation, phụ lục | Nguồn cho công cụ paper → evidence → knowledge graph |

Điểm chung: cần lưu riêng người bệnh, mẫu sinh học, xét nghiệm, kết quả đo và kết luận lâm sàng. Không thể đặt mọi dữ liệu vào một bảng “patient” hoặc coi một hàng nguồn luôn là một người.

## 2. Dryad HCT BAL

### Cấu trúc đã xác minh

TableS6.csv có 60.619 hàng gene, một cột Ensembl_ID và 22 cột subject (Pt-01, Pt-03, …, Pt-37). Hai dòng đầu là tiêu đề và dòng trống, dòng thứ ba mới là header. Dung lượng 3.789.560 bytes. Các ID ENSG đều duy nhất.

Tất cả 1.333.618 count là số nguyên không âm, từ 0 đến 242.121; không có ô thiếu trong phần ma trận. 93,76% ô bằng 0; 29.083 gene bằng 0 ở tất cả subject. Count là kết quả đếm sau alignment, không phải số bản sao gene trong DNA. Metadata Dryad xác nhận các count được tạo bằng alignment với ENSEMBL GRCh38.

Dryad liệt kê đúng hai file: TableS6.csv và Supplement-Dryad-Data-Repository.pdf. Metadata mô tả cohort 22 người lớn sau HCT, có bệnh hô hấp cấp và được lấy BAL. PDF chứa phương pháp, bảng và hình bổ sung, nhưng máy chủ trả lỗi 401/403 khi tải trong lượt này, nên nội dung PDF chưa được kiểm tra trực tiếp. Không suy ra các trường lâm sàng từ CSV gene.

### Thuộc tính hữu ích

- Mã subject: liên kết trong phạm vi nghiên cứu.
- Ensembl gene ID, count, reference genome/build.
- Theo tài liệu kế hoạch: host state/HCT, BAL specimen, conventional tests, mNGS findings và infection assessment là lớp lâm sàng cần đối chiếu với paper/supplement, chưa có trong CSV.

### Gợi ý thiết kế

Biểu diễn một giá trị gene bằng (study, subject/sample, gene, count, processing method). Lưu Ensembl ID kèm namespace và genome/annotation version. Giữ matrix gốc như một data asset; có thể tạo bảng dài khi cần truy vấn.

22 cột là mã subject, chưa đủ chứng minh một subject có đúng một mẫu hay một lần xét nghiệm. Chưa nối được ma trận với thông tin bệnh nền/chẩn đoán. Cohort nhỏ phù hợp thử schema, không đủ cơ sở tự ước lượng priors ổn định cho nhiều nhóm.

Nguồn: https://datadryad.org/dataset/doi:10.5061/dryad.800tj ; metadata API: https://datadryad.org/api/v2/datasets/doi%3A10.5061%2Fdryad.800tj ; CSV địa phương: Downloads/Dataset/TableS6.csv.

## 3. PRJNA984933 — kết quả âm tính trong bệnh ác tính huyết học

### Cấu trúc đã xác minh

- Supplementary_Table_1.xlsx: 112 bệnh nhân duy nhất, 5 cột: patient, Sex, Age,year, Underlying disease, mNGS results (Number of reads). Tuổi trong file: 16–74.
- Table 1: thống kê theo nhóm, không phải từng bệnh nhân.
- Table 2: 15 người âm tính thật; Table 3: 14 người âm tính giả. Mỗi bảng chi tiết có 7 cột: mã bệnh nhân, tuổi/giới, bệnh nền, triệu chứng, chẩn đoán cuối, đánh giá/điều trị, hình ảnh phổi.
- RunInfo: 102 runs × 47 cột, 102 BioSamples được liên kết từ các runs. Trang BioProject trước đó ghi 106 BioSamples; đây là hai phạm vi đếm khác nhau. Không coi 102 runs hay 106 hồ sơ mẫu là số bệnh nhân.

Phụ lục có 27 ô trống ở kết quả mNGS. Không tự coi chúng là NA, zero hoặc nhãn âm tính. Kết quả có thể gồm nhiều tên vi sinh vật và reads trong cùng ô, ví dụ tên_taxon_số_reads, phân tách bằng dấu chấm phẩy.

Bài báo phân loại 83 người mNGS dương tính và 29 âm tính. Bài giải thích kết quả chỉ phát hiện Torque teno virus được coi là âm tính về lâm sàng; trong phụ lục, bệnh nhân 10 và 24 có chuỗi Torque teno virus và cũng nằm trong Table 2. Vì vậy “ô không trống” không tương đương “mNGS dương tính”.

Trong cả 102 hàng RunInfo, Subject_ID, Sex, Disease và Body_Site đều trống. Một BioSample mẫu có host, collection year, geographic location; isolation source ghi “not collected”. Những giá trị này là trạng thái metadata, không phải bằng chứng không có mẫu BAL.

### Thuộc tính hữu ích và thiết kế

Tuổi, giới, bệnh nền, HCT/CAR-T, triệu chứng, imaging, conventional tests, mNGS result, final diagnosis và treatment là các nhóm thuộc tính hữu ích. WBC, ANC, PCT, CRP hiện thấy trong lớp bài báo/bảng tổng hợp; không được khẳng định tất cả có giá trị từng người trong phụ lục 5 cột.

Lưu riêng:
1. Phát hiện vi sinh vật và count.
2. Diễn giải xét nghiệm (positive/negative theo quy tắc nghiên cứu).
3. Chẩn đoán nhiễm trùng/noninfection sau đánh giá.
4. True-negative/false-negative như phân loại so với quy trình tham chiếu.

Các thông tin xét nghiệm/điều trị trong Table 2–3 là văn bản, cần trích có nguồn và giữ nguyên văn. Không dùng thống kê nhóm của Table 1 để điền giá trị cho từng người.

Khóa nối patient số 1–112 với tên mẫu số dài trong RunInfo chưa xác minh. “sample1” trong một BioSample chưa đủ chứng minh là patient 1. Lưu linkage_status = unresolved, không nối theo thứ tự hàng.

Nguồn: https://www.ncbi.nlm.nih.gov/bioproject/PRJNA984933 ; https://www.frontiersin.org/journals/medicine/articles/10.3389/fmed.2023.1195629/full ; files: Downloads/Dataset/PRJNA984933.

## 4. PRJNA808886 — mNGS/CMT và đồng nhiễm

### Cấu trúc đã xác minh

Additional_file_1.xlsx có 69 mã bệnh nhân PT1–PT69, 152 hàng nội dung và 8 cột: Patient ID, Age,year, Sex, Underlying disease, Pathogens identified by CMTs, mNGS results, Number of unique reads, CovRate. Một bệnh nhân có thể có nhiều hàng; 83 hàng tiếp theo không lặp ID.

Additional_file_2.xlsx có 19 mã bệnh nhân thuộc nhóm đồng nhiễm, 63 hàng nội dung và 7 cột; thay hai cột reads/CovRate bằng co-infection. 44 hàng tiếp theo không lặp ID.

Cả hai file có một sheet chứa dữ liệu và hai sheet trống. Dòng đầu là tiêu đề, dòng thứ hai là header, cuối bảng có chú thích. Ô trống ở ID trên hàng tiếp theo không phải bệnh nhân “thiếu ID”; phải dùng ranh giới nhóm bệnh nhân để khôi phục quan hệ. Chỉ carry-forward các cột ngữ cảnh bệnh nhân sau khi kiểm tra nhóm; không carry-forward kết quả xét nghiệm.

Table 1–3 là các bảng tổng hợp đặc điểm, đồng thuận và hiệu năng chẩn đoán. Bài mô tả 60 người viêm phổi và 9 bệnh không nhiễm trùng. RunInfo hiện có 66 runs × 47 cột, 66 BioSamples. Không đồng nhất 66 runs với cohort 69 người.

Có chuỗi Negative, ô trống và tên taxon dạng viết tắt/không chuẩn hóa. Một hàng có thể liệt kê CMT và mNGS khác nhau: ở PT1, CMT ghi Staphylococcus hemolysis nhưng mNGS ghi Human herpesvirus 6B. Hai giá trị cùng hàng không có nghĩa là cùng tác nhân hay một cặp kết quả tương ứng.

### Thuộc tính hữu ích và thiết kế

Dữ liệu cho phép lưu bệnh nhân, điều kiện suy giảm miễn dịch/bệnh nền, assay family (CMT/mNGS), organism text, unique reads, coverage, và tập tác nhân được đánh giá là đồng nhiễm.

Tách CMT findings và mNGS findings thành các record riêng. Organism giữ tên gốc và tên/taxonomy đã chuẩn hóa; không tự ép “Pneumocystis” ở cấp genus thành species nếu nguồn chưa xác nhận. Coverage cần metric_definition và unit; không gộp với reads.

Thiết kế InfectionEpisode ↔ Organism theo quan hệ nhiều–nhiều. Đồng nhiễm là kết luận có căn cứ, không phải cứ thấy nhiều taxon là tự coi đồng nhiễm.

Khóa PT… ↔ SampleName Wz… chưa xác minh. BioSample mẫu Wz10 có collection date, host, location và biological replicate 10 nhưng không ghi rõ PT10. Model ghi trong RunInfo là BGISEQ-500, còn methods bài báo nói MGISEQ-2000; phải giữ hai phát biểu theo nguồn, chưa chọn một giá trị chung.

Nguồn: https://www.ncbi.nlm.nih.gov/bioproject/PRJNA808886 ; https://link.springer.com/article/10.1186/s12879-022-07381-8 ; files: Downloads/Dataset/PRJNA808886.

## 5. PRJNA1056765 — dữ liệu nhiều modality

### Cấu trúc đã xác minh

File Excel Supplementary_Tables_S1-S3.xlsx có:

| Sheet | Hàng dữ liệu | Cột | Nội dung |
|---|---:|---:|---|
| Table S1 | 434 | 18 | DNA metadata |
| TableS2 | 434 | 18 | RNA metadata |
| Table S3 | 32 | 8 | Thông tin lâm sàng nhóm đối chứng |

S1/S2 gồm 402 người thuộc cohort chính và 32 NC: bacterial infection 114, lung cancer 123, pulmonary tuberculosis 86, fungal infection 79, NC 32. Các trường: Patients ID, Group, BioSample, SRA run, total/clean reads và bases, clean rate, Q20, Q30, GC, host/unhost reads, host ratio, non-rRNA reads, microbial reads và ratio. Không có ô trống trong vùng dữ liệu S1/S2 đã kiểm tra.

S3 gồm ID, tuổi, giới, ngày lấy mẫu, BAL microbiology, mNGS DNA/RNA pathogens_reads và Diagnosis. Không có ô trống trong vùng này; một số giá trị là Negative, cần giữ khác với missing.

RunInfo: 868 runs × 47 cột, 466 BioSamples; 434 METAGENOMIC và 434 METATRANSCRIPTOMIC. Con số 466 BioSamples không tương đương 466 bệnh nhân. S1 và S2 có cùng 434 ID trong cột Patients ID; NC dùng các hồ sơ mẫu tách theo modality trong repository, cần giữ lớp specimen/assay/run riêng.

Các file Figshare đã kiểm tra trực tiếp:

| File | Kích thước dữ liệu | Ý nghĩa |
|---|---|---|
| RNAseq_featurecount.txt trong ZIP | 28.278 gene × 402 mẫu | Gene labels ở hàng, mẫu ở cột; integer counts |
| Hai genus_abundances.txt | 1.630 và 1.500 genera × 402 mẫu | Hai ma trận genus; API flat listing có tên file trùng, nên giữ file ID; chưa gán DNA/RNA nếu thiếu mapping xác thực |
| CNV_info_table.txt | 49 feature × 402 mẫu | Có TumorFraction và các feature nhiễm sắc thể |
| CIBERSORTx_cell_type_abs.txt.txt | 402 mẫu × 26 cột | ID, 22 loại tế bào và P-value/Correlation/RMSE |

Các ma trận này dùng cùng tập 402 ID của cohort chính trong S1/S2; việc nối theo ID đã được kiểm tra. Gene matrix có count từ 0 đến 1.021.762, không có số âm/phần thập phân, 37,91% count bằng 0. Gene labels là dạng DDX11L1, WASH7P…; không được gộp trực tiếp với ENSG của Dryad chỉ dựa trên chuỗi tên.

### Vấn đề chất lượng tìm thấy

- Hai species_abundances.txt và Transposable_element_count_table.zip trong Figshare chỉ 2 bytes. Hai species files không có matrix; TE file không phải ZIP hợp lệ. Đây là file không dùng được ở bản đã tải, không phải tất cả values bằng 0.
- Cả 32 NC có cùng SRA Accession ở S1 và S2 dù là DNA/RNA. Các accession đều tồn tại trong RunInfo, nhưng cần xác minh modality. BioSample khớp RunInfo; tên mẫu của 32 NC không khớp Patients ID trong từng sheet. Không nối DNA/RNA chỉ bằng run ID như thể hai measurement độc lập.
- Không có raw host sequences public theo bài báo; có derived host expression trên GEO GSE252118 và Figshare. Đã xem metadata GEO, chưa tải toàn bộ GEO supplementary files.

### Thuộc tính hữu ích và thiết kế

Tạo modality (DNA/RNA), patient/source ID, disease group, control role, accession mapping, QC metrics, feature type, feature namespace, measure kind và pipeline version. Cần giữ read count, relative abundance, estimated cell score và tumor fraction là các phép đo khác nhau.

Tên CIBERSORTx file là cell_type_abs: giữ score theo phương pháp, chưa tự diễn giải thành tỷ lệ phần trăm tế bào. Ma trận genus có số thập phân; xác nhận công thức chuẩn hóa/đơn vị từ pipeline trước khi dùng như RPM hay proportion.

Cohort loại trừ leukemia và không chuyên HCT theo bài báo/tài liệu. Phù hợp thử ingestion/provenance, không dùng trực tiếp ước lượng HCT priors.

Nguồn: https://www.nature.com/articles/s41597-025-06171-6 ; https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1056765 ; https://doi.org/10.6084/m9.figshare.29388539.v1 ; https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE252118.

## 6. PMC Open Access Subset — nguồn paper cho extraction

Đây là kho bài báo, không phải bảng bệnh nhân chung. Đã lấy hai record JATS qua OAI-PMH và một record BioC qua API chính thức:

| PMCID | JATS đã kiểm tra |
|---|---|
| PMC9052728 | 3 table-wrap, 3 figures, 24 references, 2 supplementary-material links |
| PMC10338636 | 3 table-wrap, 5 figures, 34 references, 1 supplementary-material link |

Hai record có CC BY 4.0. JATS có cấu trúc front/article-meta (title, authors, IDs, journal/date/license), body (sections, paragraphs, tables, figures, xrefs), back (references), supplementary links. BioC chia text thành passages, có section metadata và offsets; hữu ích cho vị trí bằng chứng trong bản BioC. Offset không tự coi là vị trí trong PDF gốc.

PMC5821905 (bài Dryad) bị OAI trả “unsupported for the given prefix” khi yêu cầu full text. Không phải mọi bài trên PMC đều là OA có thể tải JATS. Phải giữ eligibility/retrieval status, không đưa lỗi API vào corpus như một bài trống.

Thuộc tính hữu ích: PMCID/PMID/DOI, title/authors, section, table/figure ID, đoạn bằng chứng, assay/specimen/protocol/threshold/host context, claim, final interpretation, citation links, license, retrieval date, version/correction status. Correction/retraction status là trường đề xuất cần kiểm tra thêm, chưa audit cho toàn corpus.

Thiết kế Document, DocumentSection, EvidenceUnit, Claim, Citation và SourceAsset. Mỗi claim liên kết với đoạn/bảng/hình cụ thể, không chỉ toàn bài. Metadata thiếu/không tìm được claim khác với nghiên cứu chứng minh “không có hiệu quả”. PMC là nguồn để xây bộ evidence-unit chuẩn 50–100 nghiên cứu như đề xuất trong tài liệu; lượt này mới thử 2 bài, chưa tạo gold-standard corpus đó.

API chính thức cho retrieval gồm Cloud Service, OAI-PMH, E-Utilities và BioC; lưu license từng bài. Nguồn: https://pmc.ncbi.nlm.nih.gov/tools/openftlist/ ; https://pmc.ncbi.nlm.nih.gov/tools/oai/ ; https://pmc.ncbi.nlm.nih.gov/articles/PMC9052728/ ; https://pmc.ncbi.nlm.nih.gov/articles/PMC10338636/.

## 7. Schema chung đề xuất

Đây là thiết kế rút ra từ exploration, chưa phải database đã triển khai.

| Bảng/entity | Thuộc tính chính | Quan hệ |
|---|---|---|
| Study | study_id, title, cohort, site, inclusion/exclusion | Một study có nhiều subject, documents, assets |
| Subject | subject_id, study_id, source_patient_id, age, sex | Unique theo (study_id, source_patient_id), không ghép PT1 giữa hai study |
| HostContext | subject_id, disease/immune condition, HCT/CAR-T, treatment exposure, timing | Nhiều context trên một người; không tạo timeline chính xác khi nguồn chỉ nói “previous” |
| Specimen | specimen_id, subject_id nullable, specimen_type, collection_time, control_role | Một người có thể có nhiều mẫu; cho phép subject chưa nối được |
| ExternalIdentifier / Linkage | entity_id, namespace, accession, source, linkage_status | patient ID, BioSample, run và GEO ID là các loại khóa khác nhau |
| Assay | assay_id, specimen_id, method, modality, platform, threshold | Một mẫu có nhiều assay; assay có nhiều runs/results |
| SequencingRun | run_id, assay_id, SRA experiment, library layout/source, QC, asset | Run không đồng nghĩa specimen/patient |
| Feature | feature_id, namespace, taxon rank/gene ID/cell type, original label, mapped label | Không gộp genus, species, gene symbol và ENSG |
| Observation | observation_id, assay_id, feature_id, numeric/text value, unit, measure_type, result_status | Tách reads/coverage/abundance/count và positive/negative |
| DiagnosticAssessment | subject/episode_id, diagnosis, infection_status, certainty, adjudication_method, time | Lưu definite/probable và cách clinicians review; không coi reference standard là chân lý độc lập |
| SourceAsset / Processing | URI, format, checksum, license, retrieval/version, pipeline/software/reference build | Matrix giữ như file; observations tham chiếu asset/processing |
| Document / EvidenceUnit / Claim / Citation | document IDs, section/table/cell/offset, text, claim type, source relations | Nối kiến thức từ paper với observations và assessments |
| AggregateStatistic | study/group, variable, value, n, denominator, unit | Table 1/diagnostic performance không được nhập như observation từng người |

Có thể dùng PostgreSQL cho các entity, keys, observations và linkage; JSONB để giữ payload gốc chưa chuẩn hóa. Matrix lớn/PDF/images/raw reads lưu ở file/object storage, database giữ URI/checksum/version. Không cần đặt mọi byte vào PostgreSQL.

### Quy tắc quan trọng

1. Mỗi patient/source ID phải có study namespace. Không suy ra patient bằng thứ tự dòng hoặc số trong Wz/PT.
2. Một assay có nhiều findings; một bệnh nhân có nhiều assay. Tách kết quả CMT và mNGS ngay cả khi nằm cùng dòng Excel.
3. Negative, zero, not performed, not collected, not reported, continuation cell và unavailable file là các trạng thái khác nhau. Chỉ gán trạng thái có bằng chứng; unknown vẫn là unknown.
4. Không tự gọi organism là pathogen: Detection và ClinicalInterpretation tách biệt. Giữ kết quả chỉ phát hiện TTV và quyết định coi âm tính trong PRJNA984933.
5. Giữ raw value, normalized value, mapping method/status và vị trí nguồn. Chỉ chuẩn hóa taxon/gene nếu mapping có căn cứ.
6. Provenance phải đến worksheet/row/column, table/patient hoặc XML passage cụ thể. Mỗi observation/claim có nguồn, đơn vị và phương pháp.
7. Linkage thiếu hoặc conflict được lưu riêng để review. Các NC duplicated run IDs của PRJNA1056765 không tự sửa nguồn.

## 8. Ontology/knowledge graph đề xuất

Các loại node tối thiểu: Study, Subject, Specimen, Assay, Observation, Organism/Gene/Feature, Disease, Treatment, DiagnosticAssessment, Document, EvidenceUnit, Claim, ProcessingPipeline.

Các cạnh tối thiểu:
- Subject — enrolled_in → Study; Subject — has_context → Disease/Treatment.
- Subject — provides → Specimen; Specimen — tested_by → Assay; Assay — produces → Observation.
- Observation — measures_or_detects → Feature; Observation — generated_by → ProcessingPipeline.
- DiagnosticAssessment — evaluates → infection/noninfection state; Assessment — considers → Observation/HostContext/TreatmentResponse.
- Claim/Assessment — supported_by → EvidenceUnit; EvidenceUnit — located_in → Document/table/figure/source file.
- Document — cites → Document; SourceAsset — derived_from → SourceAsset.

Ví dụ thực tế: PRJNA984933 patient 10 có một detection Torque teno virus với count 5 từ phụ lục. Table 2 đặt người này trong nhóm true-negative và nêu chẩn đoán drug-induced interstitial lung disease. Graph phải giữ cả detection và assessment: phát hiện virus không buộc chẩn đoán nhiễm trùng.

Theo kiến trúc Zoom: fact layer lưu entities/states; citation layer lưu papers và citations; evidence layer lưu kết quả xét nghiệm, trích đoạn, bảng và diễn giải có nguồn. Một chuỗi truy vấn có thể đi từ bệnh nhân → mẫu → assay → observation → clinical assessment → evidence/table → paper.

## 9. Kết quả để báo trong meeting và bước tiếp theo

Đã nhận diện cấu trúc của cả 5 nguồn. Các cohort cung cấp dữ liệu lâm sàng và kết quả ở nhiều mức; PRJNA1056765 bổ sung matrices/QC/provenance; PMC cung cấp full-text có cấu trúc cho pipeline extraction. Thiết kế cần quan hệ một–nhiều/many-to-many và tách clinical interpretation khỏi raw detection.

Đề xuất pilot đầu tiên: nhập phụ lục PRJNA984933 và Table 2–3 để tạo Subject, Observation, DiagnosticAssessment và SourceAsset; giữ linkage SRA unresolved. Tiếp theo dùng PRJNA808886 kiểm tra nhiều vi sinh vật/multiple tests; dùng PRJNA1056765 kiểm tra matrix và accession mapping; dùng Dryad thử Ensembl counts/host response; dùng hai bài PMC tạo vài evidence units có vị trí nguồn để kiểm tra extraction.

Những việc chưa hoàn thành: Dryad PDF bị chặn; khóa patient↔sample của hai cohort chưa xác minh; 32 NC run mappings cần review; species/TE Figshare không dùng được ở bản đã tải; chưa xây corpus PMC 50–100 bài; chưa nhập PostgreSQL/triển khai ontology hoặc model. Các giới hạn này không cản việc trình bày findings và thiết kế schema sơ bộ, nhưng phải xử lý trước khi phân tích kết hợp.
