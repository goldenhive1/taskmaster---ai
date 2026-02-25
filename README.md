<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Phân chia công việc nhóm bằng AI</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      margin: 0;
      padding: 20px;
    }

    .container {
      max-width: 600px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.1);
    }

    h1 {
      text-align: center;
      color: #4a6cf7;
    }

    label {
      font-weight: bold;
      display: block;
      margin-top: 15px;
    }

    input, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 8px;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 14px;
    }

    button {
      width: 100%;
      margin-top: 20px;
      padding: 12px;
      font-size: 16px;
      border: none;
      border-radius: 10px;
      background: #4a6cf7;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background: #3b5be0;
    }

    .warning {
      margin-top: 15px;
      color: #e67e22;
      font-size: 14px;
    }

    .result {
      margin-top: 20px;
      background: #f0f3ff;
      padding: 15px;
      border-radius: 10px;
      white-space: pre-line;
    }
  </style>
</head>

<body>
  <div class="container">
    <h1>Công Việc Nhóm</h1>

    <label>Chủ đề thuyết trình / dự án</label>
    <input id="topic" placeholder="Ví dụ: Ứng dụng của AI trong dạy học" />

    <label>Danh sách thành viên & mô tả</label>
    <textarea id="description" rows="5" placeholder="Tên + điểm mạnh, điểm yếu..."></textarea>

    <button onclick="analyze()">AI PHÂN TÍCH & GỢI Ý</button>

    <div id="warning" class="warning"></div>
    <div id="result" class="result"></div>
  </div>

  <script>
    function analyze() {
      const topic = document.getElementById("topic").value.trim();
      const description = document.getElementById("description").value.trim();
      const warning = document.getElementById("warning");
      const result = document.getElementById("result");

      warning.innerText = "";
      result.innerText = "";

      if (!topic || !description) {
        result.innerText = "❌ Vui lòng nhập đầy đủ thông tin.";
        return;
      }

      if (description.length < 40) {
        warning.innerText = "⚠️ Mô tả hơi ngắn, AI sẽ gợi ý ở mức cơ bản.";
      }

      // Giả lập AI phân tích
      result.innerText =
        "📌 Gợi ý phân chia công việc:\n\n" +
        "• Người giỏi thuyết trình: Trình bày nội dung chính\n" +
        "• Người sáng tạo: Thiết kế slide / Canva\n" +
        "• Người thích tìm tòi: Nghiên cứu nội dung, ví dụ thực tế\n" +
        "• Người tổng hợp tốt: Góp ý, chỉnh sửa, hoàn thiện bài\n\n" +
        "💡 Mẹo: Phân công linh hoạt, có thể 1 người đảm nhiệm nhiều vai trò.";
    }
  </script>
</body>
</html>