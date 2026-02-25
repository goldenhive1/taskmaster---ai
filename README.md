<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Phân công công việc thông minh</title>
<style>
body {
    font-family: Arial, sans-serif;
    background: #f2f4f7;
    padding: 20px;
}
textarea, input, button {
    width: 100%;
    margin-top: 10px;
    padding: 8px;
}
button {
    background: #007bff;
    color: white;
    border: none;
    cursor: pointer;
}
.result {
    margin-top: 20px;
    background: white;
    padding: 15px;
}
</style>
</head>
<body>

<h2>Nhập danh sách người</h2>
<p>Mỗi dòng: Tên | Tính cách | Sở thích</p>
<textarea id="peopleInput" placeholder="An | tỉ mỉ, cẩn thận | đọc sách
Bình | hướng ngoại | giao tiếp"></textarea>

<h2>Nhập danh sách công việc</h2>
<p>Mỗi dòng: Tên việc | Yêu cầu</p>
<textarea id="taskInput" placeholder="Viết báo cáo | tỉ mỉ
Thuyết trình | giao tiếp"></textarea>

<button onclick="assignTasks()">Phân công</button>

<div class="result" id="result"></div>

<script>
function assignTasks() {
    const peopleLines = peopleInput.value.trim().split("\n");
    const taskLines = taskInput.value.trim().split("\n");
    const resultDiv = document.getElementById("result");

    if (!peopleLines[0] || !taskLines[0]) {
        alert("Phải nhập người và công việc!");
        return;
    }

    // xử lý người
    let people = peopleLines.map(line => {
        let [name, personality, hobby] = line.split("|").map(x => x.trim());
        return {
            name,
            traits: (personality + "," + hobby).toLowerCase(),
            tasks: []
        };
    });

    // xử lý công việc
    let tasks = taskLines.map(line => {
        let [taskName, requirement] = line.split("|").map(x => x.trim());
        return {
            name: taskName,
            requirement: requirement.toLowerCase()
        };
    });

    // phân việc
    tasks.forEach(task => {
        let bestPerson = null;
        let bestScore = -1;

        people.forEach(person => {
            let score = 0;

            if (person.traits.includes(task.requirement)) {
                score += 2;
            }

            score -= person.tasks.length * 0.5;

            if (score > bestScore) {
                bestScore = score;
                bestPerson = person;
            }
        });

        bestPerson.tasks.push(task.name);
    });

    // hiển thị
    resultDiv.innerHTML = "<h3>Kết quả phân công</h3>";
    people.forEach(p => {
        resultDiv.innerHTML += `<b>${p.name}</b>: ${
            p.tasks.length ? p.tasks.join(", ") : "Không có việc"
        }<br>`;
    });
}
</script>

</body>
</html>