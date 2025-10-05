<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Математический тест</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 800px;
            overflow: hidden;
        }

        header {
            background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        .subtitle {
            font-size: 1.2rem;
            opacity: 0.9;
        }

        .progress-container {
            background: #f8f9fa;
            padding: 15px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #e9ecef;
        }

        .progress-bar {
            flex-grow: 1;
            height: 10px;
            background: #e9ecef;
            border-radius: 5px;
            margin: 0 20px;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background: linear-gradient(90deg, #2ecc71, #1abc9c);
            width: 0%;
            transition: width 0.5s ease;
        }

        .question-container {
            padding: 30px;
        }

        .question {
            font-size: 1.4rem;
            margin-bottom: 25px;
            color: #2c3e50;
            line-height: 1.5;
        }

        .options {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 30px;
        }

        @media (max-width: 600px) {
            .options {
                grid-template-columns: 1fr;
            }
        }

        .option {
            padding: 15px 20px;
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1.1rem;
        }

        .option:hover {
            background: #e9ecef;
            transform: translateY(-3px);
        }

        .option.selected {
            background: #3498db;
            color: white;
            border-color: #2980b9;
        }

        .input-answer {
            width: 100%;
            padding: 15px;
            font-size: 1.2rem;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: center;
        }

        .input-answer:focus {
            border-color: #3498db;
            outline: none;
        }

        .buttons {
            display: flex;
            justify-content: space-between;
        }

        button {
            padding: 15px 30px;
            border: none;
            border-radius: 10px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
        }

        .btn-prev {
            background: #95a5a6;
            color: white;
        }

        .btn-prev:hover:not(:disabled) {
            background: #7f8c8d;
        }

        .btn-next {
            background: #3498db;
            color: white;
        }

        .btn-next:hover:not(:disabled) {
            background: #2980b9;
        }

        .btn-submit {
            background: #2ecc71;
            color: white;
        }

        .btn-submit:hover {
            background: #27ae60;
        }

        button:disabled {
            background: #bdc3c7;
            cursor: not-allowed;
        }

        .results-container {
            padding: 40px;
            text-align: center;
            display: none;
        }

        .score {
            font-size: 5rem;
            font-weight: bold;
            color: #2c3e50;
            margin: 20px 0;
        }

        .score-text {
            font-size: 1.5rem;
            margin-bottom: 30px;
            color: #7f8c8d;
        }

        .feedback {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
            text-align: left;
        }

        .feedback-item {
            padding: 10px 0;
            border-bottom: 1px solid #e9ecef;
        }

        .feedback-item:last-child {
            border-bottom: none;
        }

        .correct {
            color: #27ae60;
        }

        .incorrect {
            color: #e74c3c;
        }

        .math-expression {
            font-family: 'Times New Roman', serif;
            font-size: 1.6rem;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Математический тест</h1>
            <div class="subtitle">Проверьте свои знания по математике</div>
        </header>

        <div class="progress-container">
            <div class="progress-text">Вопрос <span id="current-question">1</span> из <span id="total-questions">10</span></div>
            <div class="progress-bar">
                <div class="progress" id="progress"></div>
            </div>
            <div class="score-text">Счет: <span id="current-score">0</span></div>
        </div>

        <div class="question-container" id="question-container">
            <!-- Вопросы будут загружены через JavaScript -->
        </div>

        <div class="results-container" id="results-container">
            <h2>Результаты теста</h2>
            <div class="score" id="final-score">0/10</div>
            <div class="score-text" id="result-message"></div>
            <button onclick="restartTest()" class="btn-submit">Пройти тест снова</button>
            
            <div class="feedback" id="feedback">
                <!-- Отзывы о вопросах будут загружены через JavaScript -->
            </div>
        </div>
    </div>

    <script>
        // Вопросы для теста
        const questions = [
            {
                type: "multiple-choice",
                question: "Чему равно значение выражения: 15 + 27?",
                options: ["40", "42", "32", "38"],
                correctAnswer: 1,
                explanation: "15 + 27 = 42"
            },
            {
                type: "multiple-choice",
                question: "Какое из этих чисел является простым?",
                options: ["15", "21", "29", "27"],
                correctAnswer: 2,
                explanation: "29 - простое число, так как делится только на 1 и на само себя."
            },
            {
                type: "input",
                question: "Решите уравнение: 3x + 7 = 22. Чему равен x?",
                correctAnswer: "5",
                explanation: "3x + 7 = 22 → 3x = 15 → x = 5"
            },
            {
                type: "multiple-choice",
                question: "Чему равен квадратный корень из 144?",
                options: ["11", "12", "13", "14"],
                correctAnswer: 1,
                explanation: "12 × 12 = 144, поэтому √144 = 12"
            },
            {
                type: "input",
                question: "Вычислите площадь круга с радиусом 5 см (π ≈ 3.14).",
                correctAnswer: "78.5",
                explanation: "Площадь круга = π × r² = 3.14 × 5² = 3.14 × 25 = 78.5 см²"
            },
            {
                type: "multiple-choice",
                question: "Какая из этих дробей является наибольшей?",
                options: ["3/4", "2/3", "5/6", "7/8"],
                correctAnswer: 3,
                explanation: "7/8 = 0.875, что больше чем 3/4 = 0.75, 2/3 ≈ 0.667, 5/6 ≈ 0.833"
            },
            {
                type: "input",
                question: "Чему равно 4! (факториал 4)?",
                correctAnswer: "24",
                explanation: "4! = 4 × 3 × 2 × 1 = 24"
            },
            {
                type: "multiple-choice",
                question: "Чему равен синус 90°?",
                options: ["0", "0.5", "1", "√2/2"],
                correctAnswer: 2,
                explanation: "sin(90°) = 1"
            },
            {
                type: "input",
                question: "Решите: 2³ + 3²",
                correctAnswer: "17",
                explanation: "2³ = 8, 3² = 9, 8 + 9 = 17"
            },
            {
                type: "multiple-choice",
                question: "Какое из этих чисел является иррациональным?",
                options: ["√4", "√9", "√16", "√2"],
                correctAnswer: 3,
                explanation: "√2 ≈ 1.41421356... - иррациональное число, так как его десятичное представление бесконечно и не периодично."
            }
        ];

        let currentQuestionIndex = 0;
        let userAnswers = new Array(questions.length).fill(null);
        let score = 0;

        // Инициализация теста
        function initTest() {
            updateProgress();
            showQuestion();
        }

        // Показать текущий вопрос
        function showQuestion() {
            const questionContainer = document.getElementById('question-container');
            const currentQuestion = questions[currentQuestionIndex];
            
            let questionHTML = `
                <div class="question">${currentQuestion.question}</div>
            `;

            if (currentQuestion.type === "multiple-choice") {
                questionHTML += `<div class="options">`;
                currentQuestion.options.forEach((option, index) => {
                    const isSelected = userAnswers[currentQuestionIndex] === index;
                    questionHTML += `
                        <div class="option ${isSelected ? 'selected' : ''}" onclick="selectOption(${index})">
                            ${option}
                        </div>
                    `;
                });
                questionHTML += `</div>`;
            } else if (currentQuestion.type === "input") {
                const userAnswer = userAnswers[currentQuestionIndex] || '';
                questionHTML += `
                    <input type="text" class="input-answer" id="answer-input" 
                           placeholder="Введите ваш ответ" value="${userAnswer}" 
                           oninput="updateInputAnswer(this.value)">
                `;
            }

            // Кнопки навигации
            questionHTML += `
                <div class="buttons">
                    <button class="btn-prev" onclick="prevQuestion()" ${currentQuestionIndex === 0 ? 'disabled' : ''}>
                        Назад
                    </button>
                    ${currentQuestionIndex === questions.length - 1 ? 
                        `<button class="btn-submit" onclick="submitTest()">Завершить тест</button>` : 
                        `<button class="btn-next" onclick="nextQuestion()">Далее</button>`
                    }
                </div>
            `;

            questionContainer.innerHTML = questionHTML;
        }

        // Выбор варианта ответа
        function selectOption(optionIndex) {
            userAnswers[currentQuestionIndex] = optionIndex;
            showQuestion();
        }

        // Обновление ответа в поле ввода
        function updateInputAnswer(value) {
            userAnswers[currentQuestionIndex] = value;
        }

        // Следующий вопрос
        function nextQuestion() {
            if (currentQuestionIndex < questions.length - 1) {
                currentQuestionIndex++;
                updateProgress();
                showQuestion();
            }
        }

        // Предыдущий вопрос
        function prevQuestion() {
            if (currentQuestionIndex > 0) {
                currentQuestionIndex--;
                updateProgress();
                showQuestion();
            }
        }

        // Обновление прогресса
        function updateProgress() {
            const progress = ((currentQuestionIndex + 1) / questions.length) * 100;
            document.getElementById('progress').style.width = `${progress}%`;
            document.getElementById('current-question').textContent = currentQuestionIndex + 1;
            document.getElementById('total-questions').textContent = questions.length;
            document.getElementById('current-score').textContent = score;
        }

        // Завершение теста
        function submitTest() {
            calculateScore();
            showResults();
        }

        // Подсчет результатов
        function calculateScore() {
            score = 0;
            questions.forEach((question, index) => {
                if (question.type === "multiple-choice") {
                    if (userAnswers[index] === question.correctAnswer) {
                        score++;
                    }
                } else if (question.type === "input") {
                    // Для числовых ответов допускаем небольшую погрешность и разные форматы
                    const userAnswer = parseFloat(userAnswers[index]);
                    const correctAnswer = parseFloat(question.correctAnswer);
                    if (!isNaN(userAnswer) && Math.abs(userAnswer - correctAnswer) < 0.01) {
                        score++;
                    }
                }
            });
        }

        // Показать результаты
        function showResults() {
            document.getElementById('question-container').style.display = 'none';
            document.getElementById('results-container').style.display = 'block';
            
            const finalScore = document.getElementById('final-score');
            finalScore.textContent = `${score}/${questions.length}`;
            
            const resultMessage = document.getElementById('result-message');
            const percentage = (score / questions.length) * 100;
            
            if (percentage >= 90) {
                resultMessage.textContent = "Отлично! Вы математический гений!";
                finalScore.style.color = "#27ae60";
            } else if (percentage >= 70) {
                resultMessage.textContent = "Хорошо! У вас solid знания математики.";
                finalScore.style.color = "#3498db";
            } else if (percentage >= 50) {
                resultMessage.textContent = "Неплохо, но есть куда стремиться!";
                finalScore.style.color = "#f39c12";
            } else {
                resultMessage.textContent = "Вам стоит подтянуть знания по математике.";
                finalScore.style.color = "#e74c3c";
            }
            
            // Показать обратную связь по каждому вопросу
            const feedbackContainer = document.getElementById('feedback');
            let feedbackHTML = "<h3>Подробные результаты:</h3>";
            
            questions.forEach((question, index) => {
                let userAnswer = userAnswers[index];
                let isCorrect = false;
                
                if (question.type === "multiple-choice") {
                    isCorrect = userAnswer === question.correctAnswer;
                    userAnswer = userAnswer !== null ? question.options[userAnswer] : "Нет ответа";
                } else {
                    const userAnswerNum = parseFloat(userAnswer);
                    const correctAnswerNum = parseFloat(question.correctAnswer);
                    isCorrect = !isNaN(userAnswerNum) && Math.abs(userAnswerNum - correctAnswerNum) < 0.01;
                    userAnswer = userAnswer || "Нет ответа";
                }
                
                feedbackHTML += `
                    <div class="feedback-item">
                        <strong>Вопрос ${index + 1}:</strong> ${question.question}<br>
                        <span class="${isCorrect ? 'correct' : 'incorrect'}">
                            Ваш ответ: ${userAnswer} ${isCorrect ? '✓' : '✗'}
                        </span><br>
                        ${!isCorrect ? `<em>Правильный ответ: ${question.type === 'multiple-choice' ? question.options[question.correctAnswer] : question.correctAnswer}</em><br>` : ''}
                        <em>Объяснение:</em> ${question.explanation}
                    </div>
                `;
            });
            
            feedbackContainer.innerHTML = feedbackHTML;
        }

        // Перезапуск теста
        function restartTest() {
            currentQuestionIndex = 0;
            userAnswers = new Array(questions.length).fill(null);
            score = 0;
            
            document.getElementById('results-container').style.display = 'none';
            document.getElementById('question-container').style.display = 'block';
            
            updateProgress();
            showQuestion();
        }

        // Инициализация при загрузке страницы
        window.onload = initTest;
    </script>
</body>
</html>
