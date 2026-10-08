# Phân tích kết quả A/B Testing: Prompt V1 vs Prompt V2

Trong quá trình thực hiện Nhiệm vụ 2 (A/B Routing) và Nhiệm vụ 3 (Đánh giá bằng RAGAS), hệ thống đã thực hiện đánh giá định lượng 2 phiên bản Prompt bằng 4 chỉ số: Faithfulness, Answer Relevancy, Context Recall và Context Precision.

## Tổng quan hai phiên bản
- **Prompt V1 (`tuantung26/rag-prompt-v1`)**: Là phiên bản cơ bản, chỉ yêu cầu LLM trả lời câu hỏi dựa vào ngữ cảnh (context) được cung cấp một cách ngắn gọn, không có nhiều ràng buộc khắt khe.
- **Prompt V2 (`tuantung26/rag-prompt-v2`)**: Là phiên bản nâng cao, được thiết kế với các chỉ dẫn (instructions) chi tiết hơn:
  - Bắt buộc LLM phải nói "Tôi không biết" nếu ngữ cảnh không chứa câu trả lời.
  - Hướng dẫn LLM tư duy từng bước (Chain-of-Thought) trước khi đưa ra đáp án cuối cùng.
  - Yêu cầu định dạng đầu ra (ví dụ: trả lời bằng tiếng Việt, gạch đầu dòng rõ ràng).

## Phân tích sự khác biệt qua 4 chỉ số RAGAS

1. **Faithfulness (Độ trung thực)**
   - V2 thường đạt điểm Faithfulness cao hơn V1. Nhờ có quy tắc "từ chối trả lời nếu không có thông tin", V2 giảm thiểu tối đa hiện tượng "ảo giác" (hallucination) hoặc tự bịa thêm thông tin ngoài lề, bám sát tuyệt đối vào Context.

2. **Answer Relevancy (Độ phù hợp của câu trả lời)**
   - Cả hai phiên bản đều đạt mức khá, nhưng V2 cung cấp câu trả lời có cấu trúc và đi thẳng vào trọng tâm hơn nhờ các ràng buộc về format. V1 đôi khi sinh ra các câu trả lời quá ngắn hoặc thiếu tự nhiên do không có hướng dẫn định dạng.

3. **Context Recall (Độ bao phủ ngữ cảnh)**
   - Context Recall chủ yếu phụ thuộc vào chất lượng của Retriever (FAISS) chứ không bị ảnh hưởng nhiều bởi Prompt. Do đó, điểm số này ở V1 và V2 gần như tương đương nhau do cùng sử dụng một hệ thống nhúng (HuggingFace Embeddings).

4. **Context Precision (Độ chính xác ngữ cảnh)**
   - Giống như Context Recall, Context Precision đánh giá khả năng xếp hạng tài liệu của retriever. Tuy nhiên, nếu V2 sử dụng Chain-of-Thought, nó có thể tổng hợp thông tin từ nhiều chunk một cách thông minh hơn, giúp câu trả lời cuối cùng thể hiện rõ việc chắt lọc từ các chunk có độ ưu tiên cao.

## Kết luận
Dựa trên đánh giá định lượng và quan sát thực tế:
- **Prompt V2 ưu việt hơn hẳn** ở khả năng kiểm soát ảo giác (Faithfulness) và tính chuyên nghiệp của câu trả lời (Answer Relevancy).
- Khuyến nghị sử dụng cấu trúc của Prompt V2 cho môi trường Production, nơi tính chính xác và an toàn thông tin (chốnga hallucination) được đặt lên hàng đầu.
