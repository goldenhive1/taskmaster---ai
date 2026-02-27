import React, { useState } from "react";

export default function SmartPlannerApp() {
  const [step, setStep] = useState("landing");

  const [project, setProject] = useState({
    name: "",
    description: "",
  });

  const [members, setMembers] = useState([]);
  const [memberForm, setMemberForm] = useState({
    name: "",
    personality: "",
    interest: "",
    strength: "",
    weakness: "",
  });

  const [result, setResult] = useState([]);

  // ===== AI MOCK (sau này thay API thật) =====
  const runAI = () => {
    const baseTasks = [
      "Lập kế hoạch tổng thể",
      "Nội dung & truyền thông",
      "Thiết kế & hình ảnh",
      "Theo dõi & báo cáo tiến độ",
    ];

    const output = members.map((m, i) => ({
      member: m.name,
      task: baseTasks[i % baseTasks.length],
      reason: `Phù hợp với tính cách ${m.personality} và sở thích ${m.interest}`,
    }));

    setResult(output);
    setStep("result");
  };

  // ===== LANDING =====
  if (step === "landing") {
    return (
      <div className="min-h-screen bg-white text-blue-900">
        <header className="max-w-6xl mx-auto px-10 py-20 grid md:grid-cols-2 gap-12 items-center">
          <div>
            <h1 className="text-5xl font-bold leading-tight mb-6">
              AI Smart Task Planner
            </h1>
            <p className="text-slate-600 mb-6">
              Công cụ hỗ trợ phân chia công việc thông minh dựa trên
              tính cách, sở thích và đặc điểm của từng thành viên.
            </p>
            <p className="text-slate-500 mb-10">
              Người dùng chỉ cần nhập thông tin – việc phân tích và
              chia việc đã có AI xử lý.
            </p>
            <button
              onClick={() => setStep("project")}
              className="bg-blue-600 text-white px-8 py-4 rounded-lg text-lg hover:bg-blue-700 transition"
            >
              Bắt đầu ngay
            </button>
          </div>

          <div>
            <img
              src="https://images.unsplash.com/photo-1521737604893-d14cc237f11d"
              alt="Teamwork"
              className="rounded-2xl shadow-lg"
            />
          </div>
        </header>
      </div>
    );
  }

  // ===== STEP 1: PROJECT =====
  if (step === "project") {
    return (
      <div className="min-h-screen bg-blue-50 flex items-center justify-center">
        <div className="bg-white p-10 rounded-xl shadow-lg w-full max-w-2xl">
          <h2 className="text-2xl font-bold mb-6">Bước 1: Công việc chính</h2>

          <input
            className="w-full border p-3 rounded mb-4"
            placeholder="Tên công việc (VD: Tổ chức sự kiện)"
            value={project.name}
            onChange={(e) =>
              setProject({ ...project, name: e.target.value })
            }
          />

          <textarea
            className="w-full border p-3 rounded mb-6"
            placeholder="Mô tả quy mô, thời gian, mục tiêu..."
            rows={4}
            value={project.description}
            onChange={(e) =>
              setProject({ ...project, description: e.target.value })
            }
          />

          <div className="flex justify-end">
            <button
              onClick={() => setStep("members")}
              className="bg-blue-600 text-white px-6 py-3 rounded"
            >
              Tiếp tục
            </button>
          </div>
        </div>
      </div>
    );
  }

  // ===== STEP 2: MEMBERS =====
  if (step === "members") {
    return (
      <div className="min-h-screen bg-white p-10 max-w-5xl mx-auto">
        <h2 className="text-2xl font-bold mb-6">
          Bước 2: Thông tin thành viên
        </h2>

        <div className="grid grid-cols-2 gap-4 mb-6">
          <input
            className="border p-2 rounded"
            placeholder="Tên *"
            value={memberForm.name}
            onChange={(e) =>
              setMemberForm({ ...memberForm, name: e.target.value })
            }
          />
          <input
            className="border p-2 rounded"
            placeholder="Tính cách *"
            value={memberForm.personality}
            onChange={(e) =>
              setMemberForm({ ...memberForm, personality: e.target.value })
            }
          />
          <input
            className="border p-2 rounded"
            placeholder="Sở thích *"
            value={memberForm.interest}
            onChange={(e) =>
              setMemberForm({ ...memberForm, interest: e.target.value })
            }
          />
          <input
            className="border p-2 rounded"
            placeholder="Điểm mạnh (không bắt buộc)"
            value={memberForm.strength}
            onChange={(e) =>
              setMemberForm({ ...memberForm, strength: e.target.value })
            }
          />
          <input
            className="border p-2 rounded col-span-2"
            placeholder="Điểm yếu (không bắt buộc)"
            value={memberForm.weakness}
            onChange={(e) =>
              setMemberForm({ ...memberForm, weakness: e.target.value })
            }
          />
        </div>

        <div className="flex gap-4">
          <button
            onClick={() => {
              if (
                memberForm.name &&
                memberForm.personality &&
                memberForm.interest
              ) {
                setMembers([...members, memberForm]);
                setMemberForm({
                  name: "",
                  personality: "",
                  interest: "",
                  strength: "",
                  weakness: "",
                });
              }
            }}
            className="border border-blue-600 text-blue-600 px-4 py-2 rounded"
          >
            Thêm thành viên
          </button>

          <button
            onClick={runAI}
            className="bg-blue-600 text-white px-6 py-2 rounded"
          >
            AI phân tích & chia việc
          </button>
        </div>
      </div>
    );
  }

  // ===== RESULT =====
  if (step === "result") {
    return (
      <div className="min-h-screen bg-blue-50 p-10 max-w-6xl mx-auto">
        <h2 className="text-3xl font-bold text-center mb-10">
          Kết quả phân công công việc
        </h2>

        <table className="w-full bg-white border rounded-lg overflow-hidden">
          <thead className="bg-blue-100">
            <tr>
              <th className="p-4 border">Thành viên</th>
              <th className="p-4 border">Công việc</th>
              <th className="p-4 border">Lý do phân công</th>
            </tr>
          </thead>
          <tbody>
            {result.map((r, i) => (
              <tr key={i}>
                <td className="p-4 border">{r.member}</td>
                <td className="p-4 border">{r.task}</td>
                <td className="p-4 border text-slate-600">{r.reason}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    );
  }
}