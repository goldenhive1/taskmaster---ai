<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>AI phân chia công việc nhóm</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
body {
  font-family: Arial, sans-serif;
  background: #f3f4f6;
  padding: 20px;
}
textarea, button {
  width: 100%;
  padding: 10px;
  margin-top: 10px;
}
button {
  background: #2563eb;
  color: white;
  border: none;
  cursor: pointer;
}
.card {
  background: white;
  padding: 15px;
  margin-top: 15px;
  border-radius: 6px;
}
.loading {
  margin-top: 10px;
  color: #555;
}
</style>
</head>

<body>

<h2>Danh sách thành viên</h2>
<p>Mỗi dòng: Tên | mô tả tính cách, sở thích</p>
<textarea id="peopleInput">
An | tỉ mỉ, cẩn thận, thích viết
Bình | hướng ngoại, giao tiếp tốt
Chi | sáng tạo, nhiều ý tưởng
</textarea>

<h2>Danh sách công việc</h2>
<p>Mỗi dòng: Tên công việc | mô tả yêu cầu</p>
<textarea id="taskInput">
Viết báo cáo tổng hợp | cần tỉ mỉ và logic
Điều phối nhóm | cần giao tiếp
Trang trí sản phẩm | cần sáng tạo
</textarea>

<button onclick="runAI()">AI phân tích & phân công</button>

<div id="loading" class="loading"></div>
<div id="result"></div>

<script>
const API_KEY = "YOUR_GEMINI_API_KEY";
const API_URL =
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=" + API_KEY;

async function runAI() {
  const people = document.getElementById("peopleInput").value.trim();
  const tasks = document.getElementById("taskInput").value.trim();
  const resultDiv = document.getElementById("result");
  const loadingDiv = document.getElementById("loading");

  if (!people || !tasks) {
    alert("Nhập đầy đủ dữ liệu");
    return;
  }

  loadingDiv.innerText = "AI đang phân tích nhóm...";
  resultDiv.innerHTML = "";

  const prompt = `
Bạn là trưởng nhóm giàu kinh nghiệm.

Danh sách thành viên:
${people}

Danh sách công việc:
${tasks}

Yêu cầu:
- Phân tích điểm mạnh từng người
- Phân chia công việc hợp lý
- Không cần tối ưu tuyệt đối, chấp nhận đánh đổi
- Có thể một người làm nhiều việc
- Giải thích ngắn gọn lý do phân công

Trình bày kết quả bằng HTML, mỗi người một mục rõ ràng.
Không dùng markdown.
`;

  try {
    const res = await fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        contents: [{ parts: [{ text: prompt }] }]
      })
    });

    const data = await res.json();
    const text = data?.candidates?.[0]?.content?.parts?.[0]?.text;

    resultDiv.innerHTML = text || "<p>AI không trả về kết quả.</p>";
  } catch (e) {
    resultDiv.innerHTML =
      "<p style='color:red'>Lỗi kết nối AI hoặc API key.</p>";
  } finally {
    loadingDiv.innerText = "";
  }
}
</script>

</body>
</html>