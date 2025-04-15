<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Quiz</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: url('https://wallpaperaccess.com/full/1102408.jpg') no-repeat center center/cover;
            color: white;
            padding: 20px;
        }
        .container {
            background: rgba(0, 0, 0, 0.8);
            width: 50%;
            margin: auto;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 15px cyan;
        }
        .btn {
            display: block;
            width: 100%;
            margin: 10px 0;
            padding: 10px;
            background-color: cyan;
            color: black;
            font-weight: bold;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: 0.3s;
        }
        .btn:hover {
            background-color: white;
            color: black;
        }
    </style>
</head>
<body>
    <div class="container" id="homePage">
        <h1>Welcome to the Online Quiz</h1>
        <p>Test your knowledge in Science, Literature, and History.</p>
        <button class="btn" onclick="startQuiz('history')">History</button>
        <button class="btn" onclick="startQuiz('science')">Science</button>
        <button class="btn" onclick="startQuiz('literature')">Literature</button>
    </div>

    <div class="container" id="quizContainer" style="display: none;">
        <h1 id="quizTitle"></h1>
        <div id="question"></div>
        <div id="options"></div>
        <div id="result"></div>
        <div id="score">Score: 0</div>
        <button class="btn" onclick="goBack()">Back</button>
    </div>

    <script>
        const quizData = {
            history: [
                { question: "Who is known as the national hero of the Philippines?", options: ["Andres Bonifacio", "Jose Rizal", "Emilio Aguinaldo", "Lapu-Lapu"], answer: "Jose Rizal" },
                { question: "In what year did the Philippines gain independence from Spain?", options: ["1896", "1898", "1901", "1946"], answer: "1898" },
                { question: "Who was the first President of the Philippines?", options: ["Manuel Quezon", "Emilio Aguinaldo", "Jose P. Laurel", "Sergio Osmeña"], answer: "Emilio Aguinaldo" },
                { question: "What was the blood compact between Legazpi and Sikatuna called?", options: ["Pacto de Sangre", "Sandugo", "Kasunduan ng Biak-na-Bato", "Treaty of Manila"], answer: "Sandugo" },
                { question: "Which Philippine president declared Martial Law in 1972?", options: ["Manuel Roxas", "Diosdado Macapagal", "Ferdinand Marcos", "Corazon Aquino"], answer: "Ferdinand Marcos" }
            ],
            science: [
                { question: "What is the chemical symbol for gold?", options: ["Ag", "Au", "Pb", "Fe"], answer: "Au" },
                { question: "Which planet is known as the Red Planet?", options: ["Mars", "Venus", "Jupiter", "Saturn"], answer: "Mars" },
                { question: "What is the powerhouse of the cell?", options: ["Nucleus", "Ribosome", "Mitochondria", "Golgi apparatus"], answer: "Mitochondria" },
                { question: "What gas do plants absorb during photosynthesis?", options: ["Oxygen", "Carbon Dioxide", "Nitrogen", "Hydrogen"], answer: "Carbon Dioxide" },
                { question: "Which force keeps planets in orbit around the sun?", options: ["Magnetism", "Gravity", "Friction", "Nuclear force"], answer: "Gravity" }
            ],
            literature: [
                { question: "Who wrote 'Noli Me Tangere'?", options: ["Jose Rizal", "Francisco Balagtas", "Nick Joaquin", "Lualhati Bautista"], answer: "Jose Rizal" },
                { question: "What is the national epic of the Philippines?", options: ["Ibong Adarna", "Florante at Laura", "Biag ni Lam-ang", "El Filibusterismo"], answer: "Biag ni Lam-ang" },
                { question: "Who is the author of 'Florante at Laura'?", options: ["Jose Rizal", "Francisco Balagtas", "Nick Joaquin", "Edgar Allan Poe"], answer: "Francisco Balagtas" },
                { question: "Which famous novel is the sequel to 'Noli Me Tangere'?", options: ["Florante at Laura", "El Filibusterismo", "Biag ni Lam-ang", "Ibong Adarna"], answer: "El Filibusterismo" },
                { question: "Who is known as the 'Father of Tagalog Poetry'?", options: ["Francisco Balagtas", "Jose Corazon de Jesus", "Jose Rizal", "Nick Joaquin"], answer: "Francisco Balagtas" }
            ]
        };

        let currentQuestion = 0;
        let score = 0;
        let selectedTopic = "";
        const questionElement = document.getElementById("question");
        const optionsElement = document.getElementById("options");
        const resultElement = document.getElementById("result");
        const scoreElement = document.getElementById("score");
        const quizTitle = document.getElementById("quizTitle");

        function startQuiz(topic) {
            selectedTopic = topic;
            currentQuestion = 0;
            score = 0;
            document.getElementById("homePage").style.display = "none";
            document.getElementById("quizContainer").style.display = "block";
            quizTitle.textContent = topic.charAt(0).toUpperCase() + topic.slice(1) + " Quiz";
            scoreElement.textContent = "Score: " + score;
            loadQuestion();
        }

        function loadQuestion() {
            if (currentQuestion >= quizData[selectedTopic].length) {
                endQuiz();
                return;
            }

            const q = quizData[selectedTopic][currentQuestion];
            questionElement.textContent = q.question;
            optionsElement.innerHTML = "";
            resultElement.textContent = "";

            q.options.forEach(option => {
                const button = document.createElement("button");
                button.textContent = option;
                button.onclick = () => checkAnswer(option);
                button.classList.add("btn");
                optionsElement.appendChild(button);
            });
        }

        function checkAnswer(selected) {
            const correct = quizData[selectedTopic][currentQuestion].answer;
            if (selected === correct) {
                resultElement.textContent = "Correct!";
                resultElement.style.color = "lime";
                score++;
            } else {
                resultElement.textContent = "Wrong! The correct answer is " + correct;
                resultElement.style.color = "red";
            }

            scoreElement.textContent = "Score: " + score;
            currentQuestion++;

            setTimeout(loadQuestion, 1000);
        }

        function endQuiz() {
            questionElement.textContent = "Quiz Completed!";
            optionsElement.innerHTML = "";
            resultElement.textContent = "Final Score: " + score + " out of " + quizData[selectedTopic].length;
        }

        function goBack() {
            document.getElementById("quizContainer").style.display = "none";
            document.getElementById("homePage").style.display = "block";
            questionElement.textContent = "";
            optionsElement.innerHTML = "";
            resultElement.textContent = "";
            scoreElement.textContent = "Score: 0";
        }
    </script>
</body>
</html>
