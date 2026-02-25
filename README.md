# taskmaster---ai
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Phân Chia Công Việc Nhóm</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .loading { border-top-color: #3498db; animation: spinner 1.5s linear infinite; }
        @keyframes spinner { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body class="bg-gray-100 min-h-screen p-4 md:p-8">

    <div class="max-w-3xl mx-auto bg-white rounded-xl shadow-lg p-6">
        <h1 class="text-2xl font-bold text-center text-blue-600 mb-6">Trợ Lý AI Chia Việc Nhóm</h1>

        <div class="mb-4">
            <label class="block font-semibold mb-1">Chủ đề thuyết trình/Dự án:</label>
            <input type="text" id="topic" placeholder="Ví dụ: Tìm hiểu về AI trong giáo dục" 
                   class="w-full p-2 border rounded-lg focus:ring-2 focus:ring-blue-400 outline-none">
        </div>

        <div class="mb-4">
            <label class="block font-semibold mb-1">Danh sách thành viên & Tính cách:</label>
            <textarea id="members" rows="5" 
                      placeholder="Ví dụ:&#10;- Nam: Hoạt ngôn, tự tin, giỏi nói chuyện.&#10;- Lan: Tỉ mỉ, hay soi lỗi, giỏi tìm kiếm.&#10;- Minh: Vẽ đẹp, biết dùng Canva, ít nói."
                      class="w-full p-2 border rounded-lg focus:ring-2 focus:ring-blue-400 outline-none"></textarea>
            <p class="text-xs text-gray-500 mt-1 italic">* Càng mô tả chi tiết tính cách, AI phân tích càng chuẩn.</p>
        </div>

        <button onclick="distributeTasks()" id="btnSubmit"
                class="w-full bg-blue-600 text-white font-bold py-3 rounded-lg hover:bg-blue-700 transition">
            AI PHÂN TÍCH & CHIA VIỆC
        </button>

        <div id="loadingArea" class="hidden flex flex-col items-center mt-6">
            <div class="loading w-10 h-10 border-4 border-gray-200 rounded-full mb-2"></div>
            <p class="text-gray-600">AI đang suy nghĩ...</p>
        </div>

        <div id="resultArea" class="mt-8 hidden">
            <h2 class="text-xl font-bold border-b-2 border-blue-200 pb-2 mb-4">Gợi ý phân chia công việc:</h2>
            <div id="aiResponse" class="space-y-4 text-gray-700 leading-relaxed"></div>
        </div>
    </div>

    <script>
        // --- THAY API KEY CỦA ÔNG VÀO ĐÂY ---
        const API_KEY = "YOUR_API_KEY_HERE"; 
        const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`;

        async function distributeTasks() {
            const topic = document.getElementById('topic').value;
            const members = document.getElementById('members').value;
            const btn = document.getElementById('btnSubmit');
            const loading = document.getElementById('loadingArea');
            const resultArea = document.getElementById('resultArea');
            const aiResponse = document.getElementById('aiResponse');

            if (!topic || !members) {
                alert("Vui lòng nhập đầy đủ chủ đề và thông tin nhóm nhé!");
                return;
            }

            // Giao diện lúc đang tải
            btn.disabled = true;
            loading.classList.remove('hidden');
            resultArea.classList.add('hidden');

            const prompt = `Bạn là một chuyên gia quản lý nhân sự. Tôi có một bài thuyết trình về chủ đề: "${topic}". 
            Dưới đây là danh sách thành viên và tính cách của họ:
            ${members}

            Hãy thực hiện các bước:
            1. Liệt kê các công việc cần làm cho dự án này.
            2. Dựa trên tính cách của mỗi người, hãy phân chia công việc cho họ một cách hợp lý nhất. 
            3. Giải thích ngắn gọn tại sao họ lại hợp với việc đó.
            Trình bày kết quả rõ ràng bằng tiếng Việt, sử dụng các thẻ HTML như <strong>, <p>, <ul>, <li> để hiển thị đẹp mắt.`;

            try {
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: prompt }] }]
                    })
                });

                const data = await response.json();
                const text = data.candidates[0].content.parts[0].text;

                // Hiển thị kết quả
                aiResponse.innerHTML = text.replace(/```html|```/g, ""); // Xóa bỏ markdown nếu AI trả về
                resultArea.classList.remove('hidden');
            } catch (error) {
                aiResponse.innerHTML = "<p class='text-red-500'>Lỗi rồi! Có thể do API Key chưa đúng hoặc lỗi mạng. Hãy kiểm tra lại nhé.</p>";
                resultArea.classList.remove('hidden');
            } finally {
                btn.disabled = false;
                loading.classList.add('hidden');
            }
        }
    </script>
</body>
</html>
