#### 1. COVER

## Group Information
- Group number: 15
- Members:
  - Phạm Vũ Khánh Trường
  - Ka Phu Đông
  - Nguyễn Thị Tiểu Phương
  - Văn Phú Tín 
  - Phan Văn Duy 
  - Hà Tây Nguyên 
  - Nguyễn Đình Thi
  - Võ Lê Trường Huy 
  - Châu Thành Trung 

#### 2. ARCHITECTURE  
User → Frontend → API Gateway → Lambda → Tool Router → 
    ├── Knowledge Base (RAG)
    ├── Database (SQL)
    ├── Monitoring API
    └── LLM (Bedrock Claude)

## Data Flow
1. User asks question
2. System decides: RAG or Tool or both
3. Retrieve data
4. LLM generates answer
5. Response returned to UI

## Screenshot:
![Diagrame](images/diagram.jpg)

#### 3.Decision Log

#### 4. L1 Evidence(RAG)
✅ Screenshot cần có:
câu hỏi → trả lời đúng
có source document

📌 Ví dụ:

Question: Who is Team Platform lead?
Answer: Alex Chen
Source: team_platform.md

📸 Screenshot:

chat UI OR terminal response

📌 Proof log:

Bedrock Retrieve API returned 3 chunks:
- team_platform.md
- onboarding.md

#### 5. L2 Evidence (Multi-document)
🎯 cần thể hiện:
2 document conflict
system chọn đúng cái mới
Question: What is API rate limit for PaymentGW?

System found:
- v1: 500 requests/min (deprecated)
- v2: 1000 requests/min (current)

Final Answer: 1000 requests/min

📸 Screenshot:

output có so sánh / giải thích conflict

#### 6. L3 Evidence 
🎯 phải có TOOL CALL
Question: What was PaymentGW Q1 cost?

Tool used:
Database Query Tool

SQL executed:
SELECT SUM(cost)
FROM monthly_costs
WHERE service = 'PaymentGW'
AND quarter = 'Q1 2026';

Tool result:
$16,500

Final Answer:
PaymentGW Q1 cost = $16,500

📸 BẮT BUỘC screenshot:

log tool call
hoặc terminal showing SQL execution

👉 GIÁM KHẢO CHỈ CHẤM CÁI NÀY

#### 7. L4 Evidence (Memory)
Turn 1:
Which service had highest cost?
→ PaymentGW

Turn 2:
Why did its cost increase?
→ Traffic spike + infra scaling

Turn 3:
Which team owns it?
→ Team Payments

📸 Screenshot:

chat 3–4 turns liên tục

#### 8. Bonus A — Observability Dashboard
Question flow:

User Query → Retrieval → Tool Call → LLM → Answer

Screenshot shows:
- retrieved chunks
- tool executed
- final output

📸 Cực quan trọng:

dashboard của bạn (frontend observation page)
#### 9. Reflection
### Hardest level:
L3 (Tool integration)

### Why:
Hard to force LLM not hallucinate numbers

### What I would improve:
- better prompt
- add retry mechanism for tool calls