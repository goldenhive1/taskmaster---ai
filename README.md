<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>AI Phân Chia Công Việc Nhóm</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">

<div class="bg-white w-full max-w-4xl rounded-xl shadow-lg p-6">

  <h1 class="text-2xl font-bold text-center text-blue-600 mb-6">
    🤖 AI Phân Chia Công Việc Nhóm
  </h1>

  <!-- Công việc lớn -->
  <div class="mb-4">
    <label class="font-semibold">Công việc lớn</label>
    <input id="task" type="text"
      placeholder="Ví dụ: Tổ chức sự kiện"
      class="w-full border p-2 rounded-lg mt-1">
  </div>

  <!-- Thành viên -->
  <div class="mb-4">
    <label class="font-semibold">Danh sách thành viên</label>
    <textarea id="members" rows="6"
      placeholder="- Minh: năng động, giao tiếp tốt, thích nói chuyện
- Lan: cẩn thận, thích sắp xếp
- Ngọc: sáng tạo, thích vẽ
- Huy: logic, thích máy tính"
      class="w-full border p-2 rounded-lg mt-1"></textarea>
  </div>

  <button onclick="runAI()"
    class="w-full bg-blue-600 text-white py-3 rounded-lg font-bold hover:bg-blue-700">
    PHÂN CHIA CÔNG VIỆC
  </button>

  <div id="result" class="hidden mt-6">
    <h2 class="font-bold text-lg mb-3">📌 Kết quả phân chia</h2>
    <div id="output" class="space-y-3 text-gray-700"></div>
  </div>

</div>

<script>
function runAI() {
  const task = document.getElementById("task").value.trim();
  const membersText = document.getElementById("members").value.trim();
  const output = document.getElementById("output");
  const result = document.getElementById("result");

  if (!task || !membersText) {
    alert("Nhập đầy đủ công việc và thành viên nhé!");
    return;
  }

  const members = membersText.split("\n").map(line => {
    const parts = line.split(":");
    return {
      name: parts[0]?.trim(),
      desc: parts[1]?.toLowerCase() || ""
    };
  });

  // Tự chia việc nhỏ theo công việc lớn
  const subTasks = [
    "Lên ý tưởng và định hướng",
    "Lập kế hoạch và timeline",
    "Chuẩn bị nội dung",
    "Thiết kế hình ảnh",
    "Truyền thông và liên hệ",
    "Hậu cần và chuẩn bị",
    "Điều phối và theo dõi",
    "Tổng kết và báo cáo"
  ];

  output.innerHTML = "";

  subTasks.forEach(sub => {
    let bestMatch = members[0];
    let maxScore = 0;

    members.forEach(m => {
      let score = 0;
      if (sub.includes("ý tưởng") && m.desc.includes("sáng tạo")) score++;
      if (sub.includes("kế hoạch") && m.desc.includes("cẩn thận")) score++;
      if (sub.includes("thiết kế") && m.desc.includes("vẽ")) score++;
      if (sub.includes("truyền thông") && m.desc.includes("giao tiếp")) score++;
      if (sub.includes("hậu cần") && m.desc.includes("sắp xếp")) score++;
      if (sub.includes("báo cáo") && m.desc.includes("máy tính")) score++;
      if (score > maxScore) {
        maxScore = score;
        bestMatch = m;
      }
    });

    output.innerHTML += `
      <div class="border rounded-lg p-3">
        <strong>${sub}</strong><br>
        👉 Phụ trách: <b>${bestMatch.name}</b>
      </div>
    `;
  });

  result.classList.remove("hidden");
}
</script>

</body>
</html>