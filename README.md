<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Chia công việc tự động</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      padding: 30px;
    }
    h1 {
      text-align: center;
    }
    .box {
      background: white;
      padding: 20px;
      margin-bottom: 20px;
      border-radius: 8px;
    }
    input {
      width: 100%;
      padding: 8px;
      margin: 5px 0;
    }
    button {
      padding: 10px 20px;
      margin-top: 10px;
      cursor: pointer;
    }
    .result {
      background: #eef;
      padding: 15px;
      border-radius: 8px;
    }
  </style>
</head>
<body>

<h1>Chia công việc tự động</h1>

<div class="box">
  <h3>Nhập thông tin từng người</h3>
  <input id="name" placeholder="Tên">
  <input id="personality" placeholder="Tính cách (ví dụ: cẩn thận, năng động)">
  <input id="hobby" placeholder="Sở thích (ví dụ: thiết kế, nói chuyện)">
  <button onclick="addPerson()">Thêm người</button>
</div>

<div class="box">
  <button onclick="assignJobs()">Chia công việc</button>
</div>

<div class="box result" id="output"></div>

<script>
  const people = [];

  const jobs = [
    { name: "Thiết kế", keywords: ["thiết kế", "sáng tạo", "mỹ thuật"] },
    { name: "Thuyết trình", keywords: ["nói", "giao tiếp", "tự tin"] },
    { name: "Lập trình", keywords: ["logic", "code", "máy tính"] },
    { name: "Quản lý", keywords: ["cẩn thận", "tổ chức", "quản lý"] }
  ];

  function addPerson() {
    const name = document.getElementById("name").value;
    const personality = document.getElementById("personality").value;
    const hobby = document.getElementById("hobby").value;

    if (!name) {
      alert("Chưa nhập tên");
      return;
    }

    people.push({
      name,
      text: (personality + " " + hobby).toLowerCase()
    });

    document.getElementById("name").value = "";
    document.getElementById("personality").value = "";
    document.getElementById("hobby").value = "";

    alert("Đã thêm " + name);
  }

  function assignJobs() {
    const output = document.getElementById("output");
    output.innerHTML = "";

    const usedPeople = new Set();

    jobs.forEach(job => {
      let bestPerson = null;
      let bestScore = -1;

      people.forEach(person => {
        if (usedPeople.has(person.name)) return;

        let score = 0;
        job.keywords.forEach(k => {
          if (person.text.includes(k)) score++;
        });

        if (score > bestScore) {
          bestScore = score;
          bestPerson = person;
        }
      });

      if (bestPerson) {
        usedPeople.add(bestPerson.name);
        output.innerHTML += `<p><b>${job.name}</b>: ${bestPerson.name}</p>`;
      }
    });

    people.forEach(p => {
      if (!usedPeople.has(p.name)) {
        output.innerHTML += `<p><b>Hỗ trợ chung</b>: ${p.name}</p>`;
      }
    });
  }
</script>

</body>
</html>