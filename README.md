<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <title>AI Phân Tích & Phân Chia Công Việc Nhóm</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <script src="https://cdn.tailwindcss.com"></script>

  <style>
    .loading {
      border-top-color: #3b82f6;
      animation: spin 1.2s linear infinite;
    }
    @keyframes spin {
      to { transform: rotate(360deg); }
    }
  </style>
</head>

<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">

  <div class="w-full max-w-4xl bg-white rounded-xl shadow-lg p-6">

    <h1 class="text-2xl font-bold text-center text-blue-600 mb-6">
      🤖 AI Phân Tích & Phân Chia Công Việc Nhóm
    </h1>

    <!-- Mô tả -->
    <p class="text-center text-gray-600 mb-6">
      Phù hợp cho thuyết trình, làm bài nhóm, dự án học tập, sự kiện, hoạt động CLB, startup mini…
    </p>

    <!-- Mục tiêu -->
    <div class="mb-4">
      <label class="block font-semibold mb-1">
        Mục tiêu / công việc cần làm
      </label>
      <input id="topic" type="text"
        placeholder="Ví dụ: Làm bài thuyết trình, tổ chức sự kiện lớp, làm dự án môn học…"
        class="w-full p-2 border rounded-lg focus:ring-2 focus:ring-blue-400 outline-none">
    </div>

    <!-- Thành viên -->
    <div class="mb-4">
      <label class="block font-semibold mb-1">
        Danh sách thành viên & mô tả
      </label>
      <textarea id="members" rows="6"
        placeholder="- Nam: tự tin, nói tốt
- Lan: cẩn thận, hay để ý chi tiết
- Minh: ít nói, thiết kế ổn
- Huy: tư duy logic"
        class="w-full p-2 border rounded-lg focus:ring-2 focus:ring-blue-400 outline-none"></textarea>
      <p class="text-xs text-gray-500 italic mt-1">
        * Không cần mô tả quá chi tiết – AI sẽ tự suy luận và phân tích
      </p>
    </div>

    <!-- Nút -->
    <button id="btnSubmit" onclick="distributeTasks()"
      class="w-full bg-blue-600 text-white font-bold py-3 rounded-lg hover:bg-blue-700 transition">
      🔍 AI PHÂN TÍCH & GỢI Ý
    </button>

    <!-- Loading -->
    <div id="loadingArea" class="hidden flex flex-col items-center mt-6">
      <div class="loading w-10 h-10 border-4 border-gray-200 rounded-full mb-2"></div>
      <p class="text-gray-600">AI đang phân tích nhóm...</p>
    </div>

    <!-- Kết quả -->
    <div id="resultArea" class="hidden mt-8">
      <h2 class="text-xl font-bold border-b pb-2 mb-4">
        📌 Gợi ý phân chia công việc
      </h2>
      <div id="aiResponse" class="space-y-4 text-gray-700 leading-relaxed"></div>
    </div>

  </div>

<script>
  const API_KEY = "YOUR_API_KEY_HERE";
  const API_URL =
    "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=" + API_KEY;

  async function distributeTasks() {
    const topic = document.getElementById("topic").value.trim();
    const members = document.getElementById("members").value.trim();
    const btn = document.getElementById("btnSubmit");
    const loading = document.getElementById("loadingArea");
    const result = document.getElementById("resultArea");
    const output = document.getElementById("aiResponse");

    if (!topic || !members) {
      alert("Nhập mục tiêu và danh sách thành viên nhé!");
      return;
    }

    btn.disabled = true;
    loading.classList.remove("hidden");
    result.classList.add("hidden");

    const prompt = `
Bạn là chuyên gia quản lý nhóm và tổ chức công việc.

MỤC TIÊU / NHIỆM VỤ CHUNG:
"${topic}"

THÀNH VIÊN:
${members}

YÊU CẦU:
1. Xác định các nhóm công việc cần thiết (không giới hạn ở thuyết trình).
2. Phân tích điểm mạnh – điểm yếu từng người (được phép suy luận).
3. Gợi ý nhiều công việc phù hợp cho mỗi người, không cố định vai trò.
4. Giải thích ngắn gọn vì sao người đó hợp việc đó.
5. Gợi ý cách phối hợp nhóm cho hiệu quả.

TRÌNH BÀY:
- Chỉ dùng HTML: <h3>, <strong>, <p>, <ul>, <li>
- Không markdown
- Rõ ràng, dễ hiểu, thực tế
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
      const text =
        data?.candidates?.[0]?.content?.parts?.[0]?.text ||
        "<p>AI chưa trả về kết quả. Hãy thử lại.</p>";

      output.innerHTML = text;
      result.classList.remove("hidden");

    } catch (err) {
      output.innerHTML =
        "<p class='text-red-500'>Lỗi AI hoặc API key.</p>";
      result.classList.remove("hidden");
    } finally {
      btn.disabled = false;
      loading.classList.add("hidden");
    }
  }
</script>

</body>
</html>