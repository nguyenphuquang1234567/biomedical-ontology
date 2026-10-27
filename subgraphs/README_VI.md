# Subgraph từ 3 CSV

Định dạng chính: **RDF Turtle (.ttl)**, phù hợp trực tiếp với ontology hiện tại và RDF triplestore. Bản JSON-LD gộp là cùng một graph ở định dạng khác.

## Chọn file nào?

- **all_subgraphs.ttl**: graph gộp đầy đủ cả ba nguồn; dùng file này nếu muốn nhập toàn bộ metadata vào RDF graph DB.
- **stg_grant_award.ttl**: 687 award records, tổ chức, địa chỉ, tên PI và các mã định danh.
- **stg_grant_funding.ttl**: 2.426 funding records, abstract/document, kinh phí và liên kết fundingFor trỏ tới award. Chi tiết award nằm trong file award.
- **patent_evidence.ttl**: 72 patent records, tên inventor/assignee/applicant/examiner, mã phân loại và bản ghi khớp tìm kiếm.
- **ontology_v3.ttl**: định nghĩa classes/properties, không phải bản ghi nguồn.
- **all_subgraphs.jsonld**: phương án thay thế all_subgraphs.ttl; không cần import cả hai.
- **validation.json**: số triples, số entity từng loại, checksum inputs và kiểm tra liên kết.

## Đọc ví dụ

```
GrantFundingRecord 2457869
  fiscalYear: 1997
  awardAmount: 780565.0
  fundingFor → GrantAward P01AI039808
```

Trong Turtle, quan hệ đó là một triple có subject, predicate, object. URI nguồn đã có sẵn để dùng lại khi ghép graph; các mã ID được phân namespace để không trộn grant và patent.

Ba file riêng phân chia theo nguồn tạo bản ghi. Một số cạnh trỏ tới entity ở file khác: đây là liên kết có chủ đích, không phải thiếu ID. Tổng hợp ba file cho đúng cùng tập triples với file all_subgraphs.ttl.

**Chỉ là metadata graph**, không có scientific claim/hypothesis được tự suy ra. Currency chưa biết, generic patent date chưa được gán nghĩa publication/issue, taxon IDs và danh tính toàn cục chưa được xác minh. Có bốn award có địa chỉ nhưng thiếu tên tổ chức; không tự tạo recipient giả.

Đã đưa các subgraph lên repo public theo yêu cầu của anh; chưa import vào graph DB. CSV gốc giữ nguyên. Không cần dùng graph DB mới đọc hoặc kiểm tra được các file Turtle.
