<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>법인 절세 전략 퀴즈</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">
<style>
body {
  font-family: 'Noto Sans KR', sans-serif;
  line-height: 1.6;
  margin: 0;
  padding: 20px;
  background: linear-gradient(135deg, #4834d4 0%, #686de0 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #333;
}
.container {
  background: white;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  padding: 30px;
  width: 100%;
  max-width: 600px;
}
.emoji-title {
  font-size: 24px;
  margin-bottom: 15px;
  text-align: center;
}
.progress-container {
  background-color: #f0f0f0;
  border-radius: 10px;
  height: 10px;
  margin: 20px 0;
  overflow: hidden;
}
.progress-bar {
  background-color: #4834d4;
  height: 100%;
  border-radius: 10px;
  transition: width 0.3s ease;
  width: 0%;
}
.question {
  font-size: 18px;
  margin-bottom: 20px;
  font-weight: 500;
}
.option-container {
  display: flex;
  align-items: center;
  padding: 12px 15px;
  border: 2px solid #f0f0f0;
  margin: 10px 0;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}
.option-container:hover {
  background-color: #f8f8f8;
  border-color: #4834d4;
}
.option-container.selected {
  background-color: #e6e6ff;
  border-color: #4834d4;
}
.option-container input[type="radio"] {
  margin-right: 12px;
}
.button-group {
  text-align: center;
  margin-top: 20px;
}
.cta-button {
  background-color: #4834d4;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 500;
  transition: background-color 0.3s ease;
}
.cta-button:hover {
  background-color: #686de0;
}
.cta-button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
.result-container {
  text-align: center;
  padding: 20px;
}
.result-message {
  font-size: 20px;
  margin-bottom: 15px;
}
.score {
  font-size: 24px;
  font-weight: bold;
  color: #4834d4;
  margin: 15px 0;
}
.notice {
  background-color: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  margin: 15px 0;
  text-align: center;
  font-weight: 500;
}
</style>
</head>
<body>
<div class="container" id="quizContent">
<div class="emoji-title">💼 법인 절세 전략 퀴즈 📊</div>

<div class="notice">
  ⚠️ 3문제 중 2문제 이상 정답을 맞추면 웨비나 신청이 가능합니다!
</div>

<div class="progress-container">
  <div class="progress-bar" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100"></div>
</div>

<div id="startScreen" class="text-center">
  <button id="startQuizButton" class="cta-button">
    🎯 퀴즈 시작하기
  </button>
</div>

<div id="quizContainer" style="display: none;">
  <div id="questionContainer"></div>
  <div class="button-group">
    <button id="nextButton" class="cta-button">다음 문제 ➡️</button>
  </div>
</div>

<div id="resultContainer" style="display: none;">
</div>
</div>

<script>
const quizData = [
{
  question: "법인의 절세 전략을 시작하기 전에 가장 먼저 점검해야 할 항목은?",
  options: [
    "마케팅 전략 수립",
    "세액공제 가능 항목 확인",
    "직원 복지 제도 검토"
  ],
  correct: 1
},
{
  question: "연구인력개발비 세액공제를 받을 수 있는 업종은?",
  options: [
    "제조업만 가능",
    "서비스업만 가능",
    "모든 업종 가능"
  ],
  correct: 2
},
{
  question: "중소기업 특별세액감면의 주요 혜택은?",
  options: [
    "신규 고용 인원에 대한 지원",
    "법인세 세율 감면",
    "연구개발비 지원"
  ],
  correct: 1
}
];

let currentQuestion = 0;
let score = 0;
let userAnswers = [];

const startScreen = document.getElementById('startScreen');
const quizContainer = document.getElementById('quizContainer');
const questionContainer = document.getElementById('questionContainer');
const nextButton = document.getElementById('nextButton');
const resultContainer = document.getElementById('resultContainer');
const progressBar = document.querySelector('.progress-bar');
const startQuizButton = document.getElementById('startQuizButton');

startQuizButton.addEventListener('click', startQuiz);

function startQuiz() {
startScreen.style.display = 'none';
quizContainer.style.display = 'block';
loadQuestion();
}

function loadQuestion() {
const progress = (currentQuestion / quizData.length) * 100;
progressBar.style.width = `${progress}%`;
progressBar.setAttribute('aria-valuenow', progress);

if (currentQuestion >= quizData.length) {
  showResults();
  return;
}

const question = quizData[currentQuestion];
questionContainer.innerHTML = `
  <div class="question">Q${currentQuestion + 1}. ${question.question}</div>
  ${question.options.map((option, index) => `
    <label class="option-container">
      <input type="radio" name="quiz" value="${index}" required>
      ${option}
    </label>
  `).join('')}
`;

// Update button text for last question
nextButton.textContent = currentQuestion === quizData.length - 1 ? '결과 보기 ✨' : '다음 문제 ➡️';
nextButton.disabled = true;

// Add event listeners to options
const options = document.querySelectorAll('.option-container');
options.forEach(option => {
  option.addEventListener('click', () => {
    options.forEach(opt => opt.classList.remove('selected'));
    option.classList.add('selected');
    nextButton.disabled = false;
  });
});
}

function checkAnswer() {
const selectedOption = document.querySelector('input[name="quiz"]:checked');
if (!selectedOption) return false;

const answer = parseInt(selectedOption.value);
userAnswers.push(answer);

if (answer === quizData[currentQuestion].correct) {
  score++;
}

currentQuestion++;
return true;
}

nextButton.addEventListener('click', () => {
if (checkAnswer()) {
  loadQuestion();
}
});

function showResults() {
quizContainer.style.display = 'none';
resultContainer.style.display = 'block';
progressBar.style.width = '100%';

const passed = score >= 2;

resultContainer.innerHTML = `
  <div class="result-container">
    <div class="result-message">
      ${passed ? '🎉 축하합니다!' : '😢 아쉽네요!'}
    </div>
    <div class="score">
      ${score} / ${quizData.length} 문제 정답
    </div>
    <p>
      ${passed 
        ? '웨비나 신청이 가능합니다!' 
        : '2문제 이상 정답이 필요합니다.'}
    </p>
    ${passed 
      ? `<button onclick="location.href='https://tally.so/r/nP2bQe'" class="cta-button">
           📅 웨비나 신청하기
         </button>`
      : `<button onclick="retryQuiz()" class="cta-button">
           🔄 다시 도전하기
         </button>`
    }
  </div>
`;
}

function retryQuiz() {
currentQuestion = 0;
score = 0;
userAnswers = [];
resultContainer.style.display = 'none';
startScreen.style.display = 'block';
}
</script>
</body>
</html>
