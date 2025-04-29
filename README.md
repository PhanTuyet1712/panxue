
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>鼓浪屿打卡任务闯关</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: "Arial", sans-serif;
            background-color: #f2f2f2;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        #quiz-container {
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            width: 95%;
            max-width: 600px;
            text-align: left;
        }
        .option-button, .start-button {
            display: block;
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            background-color: #e0e0e0;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .option-button:hover, .start-button:hover {
            background-color: #d0d0d0;
        }
        #message {
            margin-top: 10px;
            color: red;
            font-weight: bold;
        }
        #student-info {
            display: flex;
            flex-direction: column;
        }
        input {
            margin-bottom: 10px;
            padding: 8px;
        }
    </style>
</head>
<body>
<div id="quiz-container">
    <div id="start-form">
        <h2>请输入姓名和学号</h2>
        <div id="student-info">
            <input type="text" id="student-name" placeholder="姓名">
            <input type="text" id="student-id" placeholder="学号">
            <button class="start-button" onclick="startQuiz()">开始任务</button>
        </div>
    </div>

    <div id="quiz-section" style="display:none;">
        <h2 id="question">题目将显示在这里</h2>
        <div id="options"></div>
        <div id="message"></div>
    </div>
</div>

<script>
    const questions = [
        {
            id: 1,
            title: "第一关：最美转角",
            question: "任务1：回答1题 + 拍摄“彩色墙面+复古路灯”打卡照。\n鼓浪屿的最美转角距离以下哪个码头最近？\nA. 三丘田码头\nB. 内厝澳码头\nC. 钢琴码头\nD. 嵩鼓码头",
            options: ["A", "B", "C", "D"],
            answer: 2,
            tip: "提示词：“24.442，118.067”",
            onFailJumpTo: 2
        },
        {
            id: 2,
            title: "第二关：皓月园",
            question: "任务1：填空题：民间流传 “遇见郑成功，万事都____。”（提示词：“太阳”）\n\n请选择正确答案：\nA. 顺利\nB. 成功\nC. 如意\nD. 有希望",
            options: ["A", "B", "C", "D"],
            answer: 1,
            tip: "提示词：“太阳”",
            onFailJumpTo: 3
        },
        {
            id: 3,
            title: "第四关：菽庄花园",
            question: "任务：判断对错：“在菽庄花园中，有一处名为‘十二洞天’的假山群，它由12个形状各异的山洞组成，每个洞上都刻有12星座的图案。”",
            options: ["对", "错"],
            answer: 1,
            tip: "提示词：“高” + “图片坐标” = 日光岩",
            onFailJumpTo: null
        }
    ];

    let studentName = "";
    let studentID = "";
    let currentIndex = 0;
    let answers = [];

    function startQuiz() {
        studentName = document.getElementById("student-name").value.trim();
        studentID = document.getElementById("student-id").value.trim();
        if (!studentName || !studentID) {
            alert("请输入完整信息！");
            return;
        }
        document.getElementById("start-form").style.display = "none";
        document.getElementById("quiz-section").style.display = "block";
        loadQuestion();
    }

    function loadQuestion() {
        const q = questions[currentIndex];
        document.getElementById("question").textContent = q.title + "\n" + q.question;
        const optionsDiv = document.getElementById("options");
        const messageDiv = document.getElementById("message");
        messageDiv.textContent = "";
        optionsDiv.innerHTML = "";

        q.options.forEach((opt, i) => {
            const btn = document.createElement("button");
            btn.className = "option-button";
            btn.textContent = opt;
            btn.onclick = () => checkAnswer(i);
            optionsDiv.appendChild(btn);
        });
    }

    function checkAnswer(index) {
        const q = questions[currentIndex];
        const messageDiv = document.getElementById("message");

        answers.push({
            id: q.id,
            question: q.title,
            selected: q.options[index],
            correct: index === q.answer,
            tip: index === q.answer ? q.tip : null
        });

        if (index === q.answer) {
            messageDiv.style.color = "green";
            messageDiv.textContent = "回答正确！" + (q.tip ? " " + q.tip : "");
            setTimeout(() => {
                currentIndex++;
                if (currentIndex < questions.length) {
                    loadQuestion();
                } else {
                    endQuiz();
                }
            }, 1500);
        } else {
            messageDiv.style.color = "red";
            messageDiv.textContent = "回答错误，跳转到下一个任务...";
            setTimeout(() => {
                currentIndex = q.onFailJumpTo !== null ? q.onFailJumpTo : currentIndex + 1;
                if (currentIndex < questions.length) {
                    loadQuestion();
                } else {
                    endQuiz();
                }
            }, 1500);
        }
    }

    function endQuiz() {
        console.log("学生姓名:", studentName);
        console.log("学号:", studentID);
        console.log("答题记录:", answers);

        document.getElementById("quiz-container").innerHTML = `
            <h2>🎉 恭喜 ${studentName} 同学，任务完成！</h2>
            <p>提示词已记录，请提交打卡照片完成最终任务。</p>
            <pre>${JSON.stringify(answers, null, 2)}</pre>
        `;
    }
</script>
</body>
</html>
