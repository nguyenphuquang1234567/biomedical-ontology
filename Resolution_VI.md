# Xử lý PARTIAL/GAP — bắt đầu bằng 3 ví dụ

Ngày 27/10/2026. Bản mở rộng 0.3.0-draft; bản v2 được giữ nguyên.

## 1. Số tiền grant

Trong stg_grant_funding.csv có appl_id=2457869, award_key=P01AI039808, fiscal_year=1997 và award_amount=780565.

Trước: GAP vì chưa có chỗ lưu tài chính.

Sau:
```
GrantFundingRecord 2457869
  fiscalYear: 1997
  awardAmount: 780565.0
  fundingFor → GrantAward P01AI039808
```
Không gán USD vì CSV không cung cấp currency; không cộng thành tổng grant khi chưa rõ phạm vi tiền.

## 2. Người phụ trách grant

contact_pi_name=BUCHMEIER, MICHAEL JOSEPH của award P01AI039808.

Trước: GAP vì chưa có vai trò PI.

Sau:
```
GrantAward P01AI039808
  contactPIMention → PersonNameMention
    label: BUCHMEIER, MICHAEL JOSEPH
    nameResolutionStatus: reported_only
```
Đây là tên người và vai trò được nguồn ghi nhận; không phải Patient, cũng chưa tự coi là danh tính cá nhân đã xác minh toàn cục. TBD được đánh dấu placeholder; et al. được đánh dấu danh sách chưa đầy đủ.

## 3. Người sáng chế và ngày của patent

Bản PATENT_0001 ghi inventor=Ahmed; Mahiuddin et al., filing_date=2020-08-19, date=2021-12-14.

Trước: GAP/PARTIAL.

Sau:
```
Patent US 11197910 B1
  inventorMention → PersonNameMention
    label: Ahmed; Mahiuddin et al.
    nameResolutionStatus: incomplete_list
  filingDate: 2020-08-19
  reportedDate: 2021-12-14
  reportedDateMeaning: unverified source date role
```
Không tự gọi date là publication date; không bịa tên những inventor còn thiếu. Assignee và applicant có hai quan hệ riêng, giữ PartyMention vì có thể là cá nhân hoặc tổ chức.

## Các phần đã bổ sung

- GrantAward và GrantFundingRecord; fundingFor dùng award_key được kiểm tra.
- Organization, Location; recipient và administering institute tách biệt; địa chỉ tổ chức không phải mặc định nơi nghiên cứu.
- PartyMention và PersonNameMention với đúng vai trò PI/inventor/assignee/applicant/examiner.
- Patent là subclass của Document; kế thừa label, sourceUrl.
- GrantDocument chứa abstract và portal URL, nối đúng funding record; không coi abstract là full text.
- ClassificationCode giữ CPCI/CPCA riêng, không nhập lẫn với bộ lọc tìm kiếm CPC.
- RetrievalMatch và TaxonReference giữ thông tin khớp tìm kiếm; không dùng EvidenceLink supports. Namespace taxon và chuỗi nhiều ID/tên chưa được tự xác minh hay ép ghép cặp.

## Kết quả

- 35 cột PARTIAL/GAP cũ đều đã có phương án lưu và quan hệ cụ thể.
- Thêm 11 classes, 14 quan hệ, 28 attributes; tổng 86 classes trong v3.
- Đã tạo metadata subgraphs cục bộ cho 687 awards, 2426 funding records và 72 patents.
- Toàn bộ 2426 funding records nối được award tương ứng.
- 4 trường hợp có địa chỉ nhưng thiếu tên tổ chức: giữ địa chỉ, không tự tạo recipient organization có danh tính giả.

RESOLVED ở đây là đã có cách mô hình hóa trung thực thông tin nguồn; không có nghĩa mọi thông tin đã được kiểm chứng. Những gì nguồn không cung cấp vẫn giữ unknown. Các trường export và relevance không bị biến thành scientific claim.

## Cách đọc file mới

1. Mở mapping_review.html, đọc ba ví dụ đầu trang trước. Mỗi bảng file có thể mở ra để tra cột cụ thể.
2. ontology_explorer.html chứa catalog v3, các attributes và quan hệ mới. Tìm Document/Patent ở mục Attributes đã tính cả thuộc tính kế thừa.
3. ontology.jsonld hoặc ontology.ttl là ontology v3 đầy đủ. extension.* chỉ chứa phần thay đổi so với v2.
4. metadata_subgraphs.* là graph metadata đã tạo từ ba CSV; example_subgraph.ttl là ví dụ nhỏ. Giữ các graph này ở máy, chưa đẩy lên repo public.
5. resolved_mapping.json có đủ 64 cột, trạng thái trước/sau và diễn giải. validation.json chứa kiểm tra và giới hạn.

## Giới hạn còn giữ rõ

Đơn vị tiền tệ và tính cộng gộp kinh phí, vai trò generic patent date, namespace taxon và danh tính thực của tên người/tổ chức chưa được xác minh. Chưa extract hypothesis/claim/discovery từ toàn văn hoặc abstract. Chưa chạy OWL reasoner/SHACL đầy đủ. Ontology và mapping v3 đã được chuẩn bị để xuất bản trên repo hiện có; dữ liệu CSV và graph metadata đầy đủ giữ cục bộ. Bản public v2 được lưu trong versions/v2.
