<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>AI phân chia công việc (AI thật)</title>
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
.result {
  background: white;
  padding: 15px;
  margin-top: 15px;
  border-radius: 6px;
}
.error {
  color: red;
  margin-top: 10px;
}
</style>
</head>

<body>

<h2>Danh sách thành viên</h2>
<textarea id="people">
An | tỉ mỉ, cẩn thận, thích viết
Bình | hướng ngoại, giao tiếp tốt
Chi | sáng tạo, nhiều ý tưởng
</textarea>

<h2>Danh sách công việc</h2>
<textarea id="tasks">
Viết báo cáo | cần tỉ mỉ
Điều phối nhóm | cần giao tiếp
Trang trí | cần sáng tạo
</textarea>

<button onclick="runAI()">AI phân tích & phân công</button>

<div id="status"></div>
<div id="output" class="result"></div>

<script>
const API_KEY = "DÁN_API_KEY_CỦA_M_VÀO_ĐÂY";
const API_URL =
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash-latest:generateContent?key=" + API_KEY;

async function runAI() {
  const people = document.getElementById("people").value.trim();
  const tasks = document.getElementById("tasks").value.trim();
  const status = document.getElementById("status");
  const output = document.getElementById("output");

  status.innerText = "AI đang phân tích...";
  output.innerHTML = "";

  const prompt = `
Bạn là trưởng nhóm.

Thành viên:
${people}

Công việc:
${tasks}

Hãy phân công công việc hợp lý, có giải thích.
Trình bày bằng HTML, không markdown.
`;

  try {
    const res = await fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        contents: [
          { role: "user", parts: [{ text: prompt }] }
        ]
      })
    });

    if (!res.ok) {
      const errText = await res.text();
      throw new Error("HTTP " + res.status + ": " + errText);
    }

    const data = await res.json();
    console.log("API response:", data);

    const text = data?.candidates?.[0]?.content?.parts?.[0]?.text;

    if (!text) {
      throw new Error("AI không trả nội dung");
    }

    output.innerHTML = text;
    status.innerText = "Hoàn thành";

  } catch (err) {
    console.error(err);
    status.innerHTML = "";
    output.innerHTML =
      "<div class='error'>❌ Lỗi: " + err.message + "</div>";
  }
}
</script>

</body>
</html>