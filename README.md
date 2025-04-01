
<!DOCTYPE html><html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>موسسه پیشگامان در راه موفقیت</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; }
        .hidden { display: none; }
    </style>
</head>
<body>
    <div id="admin" class="hidden">
        <h2>پنل مدیریت</h2>
        <h3>افزودن کاربر</h3>
        <input type="text" id="newUser" placeholder="نام کاربری">
        <button onclick="addUser()">افزودن</button>
        <h3>افزودن سوال</h3>
        <input type="text" id="question" placeholder="سوال">
        <input type="text" id="option1" placeholder="گزینه 1">
        <input type="text" id="option2" placeholder="گزینه 2">
        <input type="text" id="option3" placeholder="گزینه 3">
        <input type="text" id="answer" placeholder="پاسخ صحیح">
        <button onclick="addQuestion()">افزودن سوال</button>
        <h3>مدیریت سوالات</h3>
        <button onclick="clearQuestions()">حذف تمام سوالات</button>
    </div><div id="login">
    <h2>ورود به آزمون</h2>
    <input type="text" id="username" placeholder="نام کاربری">
    <button onclick="login()">ورود</button>
</div>

<div id="quiz" class="hidden">
    <h2 id="questionText"></h2>
    <button id="opt1" onclick="submitQuiz(0)"></button>
    <button id="opt2" onclick="submitQuiz(1)"></button>
    <button id="opt3" onclick="submitQuiz(2)"></button>
    <p id="result"></p>
</div>

<script>
    let users = JSON.parse(localStorage.getItem("users")) || {"محمد": true, "عرشیا": true};
    let questions = JSON.parse(localStorage.getItem("questions")) || [
        {question: "پایتخت ایران کجاست؟", options: ["تهران", "اصفهان", "مشهد"], answer: 0}
    ];
    const adminUsername = "مدیر";
    const adminPassword = "M13688598";

    function login() {
        let username = document.getElementById("username").value;
        
        if (username === adminUsername) {
            let password = prompt("رمز مدیریت را وارد کنید:");
            if (password === adminPassword) {
                document.getElementById("login").classList.add("hidden");
                document.getElementById("admin").classList.remove("hidden");
            } else {
                alert("رمز عبور اشتباه است.");
            }
            return;
        }
        
        if (users[username]) {
            document.getElementById("login").classList.add("hidden");
            document.getElementById("quiz").classList.remove("hidden");
            loadQuestion();
        } else {
            alert("نام کاربری اشتباه است.");
        }
    }

    function addUser() {
        let newUser = document.getElementById("newUser").value;
        if (newUser && !users[newUser]) {
            users[newUser] = true;
            localStorage.setItem("users", JSON.stringify(users));
            alert("کاربر اضافه شد.");
        }
    }

    function addQuestion() {
        let q = document.getElementById("question").value;
        let o1 = document.getElementById("option1").value;
        let o2 = document.getElementById("option2").value;
        let o3 = document.getElementById("option3").value;
        let ans = parseInt(document.getElementById("answer").value);
        if (q && o1 && o2 && o3 && !isNaN(ans)) {
            questions.push({ question: q, options: [o1, o2, o3], answer: ans });
            localStorage.setItem("questions", JSON.stringify(questions));
            alert("سوال اضافه شد.");
        }
    }

    function clearQuestions() {
        if (confirm("آیا مطمئن هستید که می‌خواهید تمام سوالات را حذف کنید؟")) {
            questions = [];
            localStorage.setItem("questions", JSON.stringify(questions));
            alert("تمام سوالات حذف شدند.");
        }
    }

    function loadQuestion() {
        if (questions.length === 0) {
            document.getElementById("quiz").innerHTML = "<h2>هیچ سوالی موجود نیست.</h2>";
            return;
        }
        let q = questions[0];
        document.getElementById("questionText").innerText = q.question;
        document.getElementById("opt1").innerText = q.options[0];
        document.getElementById("opt2").innerText = q.options[1];
        document.getElementById("opt3").innerText = q.options[2];
    }

    function submitQuiz(choice) {
        let result = document.getElementById("result");
        if (questions.length > 0 && choice === questions[0].answer) {
            result.innerText = "پاسخ درست است!";
        } else {
            result.innerText = "پاسخ نادرست است.";
        }
    }
</script>

</body>
</html>
