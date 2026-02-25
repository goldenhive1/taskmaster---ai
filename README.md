<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>AI Phân tích & Phân chia công việc</title>
<style>
body {
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
  background: #f2f4f8;
  padding: 20px;
}
.container {
  max-width: 480px;
  margin: auto;
  background: #fff;
  padding: 20px;
  border-radius: 14px;
  box-shadow: 0 10px 30px rgba(0,0,0,.08);
}
h1 {
  text-align: center;
  color: #2563eb;
}
textarea {
  width: 100%;
  padding: 12px;
  margin-top: 8px;
  border-radius: 10px;
  border: 1px solid #ccc;
  font-size: 15px;
}
button {
  width: 100%;
  margin-top: 16px;
  padding: 14px;
  border: none;
  border-radius: 12px;
  background: #2563eb;
  color: #fff;
  font-size: 16px;
  font-weight: bold;
}
#result {
  margin-top: 20px;
  line-height: 1.6;
}
.role {
  margin-bottom: 12px;
}
.note {
  font-style: italic;
  color: #555;
}
</style>
</head>

<body>
<div class="container">
  <h1>🤖 AI Phân tích & Phân chia công việc</h1>

  <label><b>Mục tiêu / công việc cần làm</b></label>
  <textarea id="goal" placeholder="Ví dụ: Tổ chức ngày hội lớp cuối năm"></textarea>

  <label style="margin-top:12px;"><b>Danh sách thành viên & mô tả</b></label>
  <textarea id="members" rows="6" placeholder="- Linh: cẩn thận, viết lách ổn
- Tuấn: ít nói, làm việc kỹ
- Trang: sáng tạo, có gu
- Đức: nhanh nhẹn, xử lý tình huống tốt"></textarea>

  <button onclick="analyze()">🔍 AI PHÂN TÍCH & GỢI Ý</button>

  <div id="result"></div>
</div>

<script>
function analyze() {
  const goal = document.getElementById("goal").value.trim();
  const membersText = document.getElementById("members").value.trim();
  const result = document.getElementById("result");

  if (!goal || !membersText) {
    result.innerHTML = "⚠️ Vui lòng nhập đầy đủ thông tin.";
    return;
  }

  const members = membersText.split("\n");
  let html = `<h3>🎯 Mục tiêu chung</h3><p>${goal}</p>`;
  html += `<h3>👥 Phân chia công việc đề xuất</h3>`;

  members.forEach(line => {
    if (!line.includes(":")) return;

    const [nameRaw, traitRaw] = line.replace("-", "").split(":");
    const name = nameRaw.trim();
    const trait = traitRaw.trim().toLowerCase();

    let role = "Hỗ trợ chung";
    let task = "Tham gia hỗ trợ các công việc phát sinh";

    if (trait.includes("nhanh") || trait.includes("xử lý")) {
      role = "Điều phối & xử lý tình huống";
      task = "Giải quyết sự cố, hỗ trợ các nhóm, đảm bảo tiến độ chung";
    } 
    else if (trait.includes("sáng tạo") || trait.includes("gu")) {
      role = "Ý tưởng & hình ảnh";
      task = "Lên concept, trang trí, hình ảnh, hoạt động sáng tạo";
    } 
    else if (trait.includes("cẩn thận") || trait.includes("viết")) {
      role = "Nội dung & kế hoạch";
      task = "Soạn kế hoạch chi tiết, nội dung chương trình, checklist";
    } 
    else if (trait.includes("kỹ") || trait.includes("logic")) {
      role = "Hậu cần & kiểm soát";
      task = "Chuẩn bị vật dụng, kiểm tra chi phí, đảm bảo mọi thứ đúng kế hoạch";
    }

    html += `
      <div class="role">
        <b>${name}</b><br>
        • Vai trò: ${role}<br>
        • Nhiệm vụ chính: ${task}
      </div>
    `;
  });

  html += `<p class="note">👉 Phân chia dựa trên đặc điểm cá nhân và mục tiêu chung, có thể linh hoạt điều chỉnh khi triển khai thực tế.</p>`;

  result.innerHTML = html;
}
</script>
</body>
</html>