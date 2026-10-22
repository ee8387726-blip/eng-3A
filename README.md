<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>A Home Away From Home - Quiz Game</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #0f172a;
    color: #f8fafc;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
    padding: 20px;
    box-sizing: border-box;
  }
  .game-card {
    background-color: #1e293b;
    border-radius: 12px;
    padding: 30px;
    max-width: 700px;
    width: 100%;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
    border: 1px solid #334155;
  }
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 2px solid #334155;
    padding-bottom: 15px;
    margin-bottom: 20px;
  }
  .header h2 {
    margin: 0;
    color: #38bdf8;
    font-size: 1.2rem;
  }
  .score {
    font-weight: bold;
    color: #4ade80;
  }
  .question-box {
    font-size: 1.1rem;
    margin-bottom: 20px;
    line-height: 1.5;
  }
  .options-container {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .btn {
    background-color: #334155;
    color: #f8fafc;
    border: none;
    padding: 12px 18px;
    border-radius: 8px;
    cursor: pointer;
    text-align: left;
    font-size: 1rem;
    transition: background-color 0.2s;
  }
  .btn:hover {
    background-color: #475569;
  }
  textarea {
    width: 100%;
    height: 100px;
    border-radius: 8px;
    background-color: #0f172a;
    color: #f8fafc;
    border: 1px solid #475569;
    padding: 10px;
    box-sizing: border-box;
    font-size: 1rem;
    resize: vertical;
    margin-bottom: 15px;
  }
  .submit-btn {
    background-color: #0284c7;
    color: white;
    font-weight: bold;
  }
  .submit-btn:hover {
    background-color: #0369a1;
  }
  .feedback {
    margin-top: 15px;
    padding: 12px;
    border-radius: 6px;
    display: none;
  }
  .correct {
    background-color: #14532d;
    color: #86efac;
  }
  .wrong {
    background-color: #7f1d1d;
    color: #fca5a5;
  }
  .next-btn {
    margin-top: 15px;
    background-color: #16a34a;
    color: white;
    font-weight: bold;
    display: none;
    width: 100%;
    text-align: center;
  }
  .next-btn:hover {
    background-color: #15803d;
  }
  .end-screen {
    text-align: center;
  }
</style>
</head>
<body>

<div class="game-card">
  <div id="game-body">
    <div class="header">
      <h2 id="q-title">Question 1</h2>
      <div class="score">Score: <span id="score">0</span></div>
    </div>
    <div class="question-box" id="question">Loading...</div>
    <div class="options-container" id="options"></div>
    <div class="feedback" id="feedback"></div>
    <button class="btn next-btn" id="next-btn" onclick="nextQuestion()">Next Question</button>
  </div>
</div>

<script>
const quizData = [
  // 20 MC Questions
  { q: "1. What main topic is discussed in Text 2?", options: ["A. The history of space travel", "B. Predictions about future human habitats and space living", "C. Ocean life in the 22nd century", "D. How SpaceX was founded"], answer: 1 },
  { q: "2. Which organization published the population estimates for Hong Kong and Lagos?", options: ["A. The Global Cities Institute", "B. The United Nations", "C. SpaceX", "D. Pearl River Delta Studies"], answer: 0 },
  { q: "3. What was the population of Lagos in 2010?", options: ["A. 7,419,000", "B. 9,467,000", "C. 10,572,000", "D. 32,630,000"], answer: 2 },
  { q: "4. What is the estimated population of Lagos by 2050?", options: ["A. 11,458,000", "B. 32,630,000", "C. 88,345,000", "D. 10,572,000"], answer: 1 },
  { q: "5. What is predicted to happen to many big cities by 2100?", options: ["A. They will shrink", "B. They will merge into one gigantic urban region", "C. They will move underground", "D. They will run out of power"], answer: 1 },
  { q: "6. The phrase 'a walk in the park' means something will NOT be:", options: ["A. Fun", "B. Easy", "C. Cheap", "D. Fast"], answer: 1 },
  { q: "7. Regular travel between Earth and space is described as a potential:", options: ["A. Adventure", "B. Dream", "C. Nightmare", "D. Reality"], answer: 2 },
  { q: "8. What is explicitly listed as something Space City must produce on its own?", options: ["A. Rocket fuel", "B. Electricity", "C. VR headsets", "D. Satellites"], answer: 1 },
  { q: "9. What force is generated when Space City spins?", options: ["A. Electricity", "B. Gravity", "C. Solar energy", "D. Magnetism"], answer: 1 },
  { q: "10. Which part of Space City contains support systems?", options: ["A. The narrow spokes", "B. The central hub", "C. The outer rim", "D. The solar panels"], answer: 1 },
  { q: "11. Why will most windows offer spectacular views?", options: ["A. They face solar panels", "B. They are on the outer surface", "C. They display VR", "D. They look into spokes"], answer: 1 },
  { q: "12. What daily activities will residents perform in Space City?", options: ["A. Studying, working, relaxing, playing", "B. Farming, building rockets", "C. Mining asteroids", "D. Exercising 24 hours a day"], answer: 0 },
  { q: "13. Which two virtual environments are specifically mentioned?", options: ["A. Desert and ocean floor", "B. Tropical beach and snowy mountain", "C. Jungle and rainy forest", "D. City and quiet lake"], answer: 1 },
  { q: "14. What effect does living in space have on the human body?", options: ["A. Causes appetite loss", "B. The body is not used to it", "C. Improves eyesight", "D. Speeds up aging"], answer: 1 },
  { q: "15. What is recommended to keep residents healthy and happy?", options: ["A. High-protein food", "B. Exercise, sport, and medical checkups", "C. Daily trips to Earth", "D. Solar panel maintenance"], answer: 1 },
  { q: "16. What kind of accommodations is SpaceX expected to offer soon?", options: ["A. Underwater hotels", "B. Floating homes", "C. Space hotels", "D. Underground bunkers"], answer: 2 },
  { q: "17. Which word in paragraph 7 shows people will gradually become accustomed to space?", options: ["A. Dream", "B. Get used to", "C. Escaping", "D. Encouraged"], answer: 1 },
  { q: "18. What is the writer's tone regarding future space living?", options: ["A. Angry", "B. Hopeless", "C. Curious, objective, and realistic", "D. Unconcerned"], answer: 2 },
  { q: "19. Between 2010 and 2100, Hong Kong's population is predicted to:", options: ["A. Decrease", "B. Stay the same", "C. Increase from ~7.4M to over 11.4M", "D. Multiply tenfold"], answer: 2 },
  { q: "20. What is the primary driver forcing humans to look toward space?", options: ["A. Search for alien life", "B. Lack of space on Earth due to overpopulation and rising sea levels", "C. Exhaustion of fossil fuels", "D. Desire to abandon Earth"], answer: 1 },

  // 5 Long Questions
  { type: "text", q: "21. Compare the population growth trends of Hong Kong and Lagos from 2010 to 2100. Which grows faster?", sample: "Sample Answer: Lagos grows significantly faster (from ~10.5M to ~88.3M) compared to Hong Kong (from ~7.4M to ~11.4M)." },
  { type: "text", q: "22. Explain how Space City addresses gravity and food supply.", sample: "Sample Answer: Gravity is created by spinning the giant wheel. Food is grown inside the narrow spokes." },
  { type: "text", q: "23. Why might living in Space City cause boredom, and what is the technological solution?", sample: "Sample Answer: Routines are similar to Earth in an enclosed space. The solution is using virtual reality environments." },
  { type: "text", q: "24. Why is self-sufficiency difficult for a space city?", sample: "Sample Answer: The city must continuously produce 100% of its own food, water, air, and electricity without Earth resupplies." },
  { type: "text", q: "25. Summarize the progression of space living mentioned in paragraph 7.", sample: "Sample Answer: Starts with short tourism trips/space hotels, and eventually evolves into permanent long-term settlement." },

  // 2 Open / Closed Questions
  { q: "26. [Closed] What specific part of Space City is used to generate electricity?", options: ["A. The central hub", "B. The narrow spokes", "C. Solar panels on the outer surface", "D. Virtual reality rooms"], answer: 2 },
  { type: "text", q: "27. [Open] Would you live in Space City for one year? Explain your reason.", sample: "Sample Answer: Any logical response (e.g., 'Yes, to experience living in space and VR' or 'No, due to health risks and missing Earth nature')." }
];

let currentIdx = 0;
let score = 0;

function loadQuestion() {
  const qData = quizData[currentIdx];
  document.getElementById('q-title').innerText = `Question ${currentIdx + 1} of ${quizData.length}`;
  document.getElementById('question').innerText = qData.q;
  document.getElementById('feedback').style.display = 'none';
  document.getElementById('next-btn').style.display = 'none';

  const optionsContainer = document.getElementById('options');
  optionsContainer.innerHTML = '';

  if (qData.type === 'text') {
    const textarea = document.createElement('textarea');
    textarea.id = 'text-answer';
    textarea.placeholder = 'Type your answer here...';
    optionsContainer.appendChild(textarea);

    const submitBtn = document.createElement('button');
    submitBtn.className = 'btn submit-btn';
    submitBtn.innerText = 'Submit Answer';
    submitBtn.onclick = checkTextAnswer;
    optionsContainer.appendChild(submitBtn);
  } else {
    qData.options.forEach((opt, index) => {
      const btn = document.createElement('button');
      btn.className = 'btn';
      btn.innerText = opt;
      btn.onclick = () => checkMCAnswer(index);
      optionsContainer.appendChild(btn);
    });
  }
}

function checkMCAnswer(selectedIdx) {
  const qData = quizData[currentIdx];
  const feedback = document.getElementById('feedback');
  const buttons = document.getElementById('options').getElementsByTagName('button');

  for (let btn of buttons) {
    btn.disabled = true;
  }

  if (selectedIdx === qData.answer) {
    score += 10;
    document.getElementById('score').innerText = score;
    feedback.className = 'feedback correct';
    feedback.innerText = 'Correct!';
  } else {
    feedback.className = 'feedback wrong';
    feedback.innerText = `Incorrect! The correct answer was: ${qData.options[qData.answer]}`;
  }
  feedback.style.display = 'block';
  document.getElementById('next-btn').style.display = 'block';
}

function checkTextAnswer() {
  const userAns = document.getElementById('text-answer').value.trim();
  const feedback = document.getElementById('feedback');
  
  if (!userAns) return;

  score += 10;
  document.getElementById('score').innerText = score;

  feedback.className = 'feedback correct';
  feedback.innerHTML = `<strong>Answer Submitted!</strong><br>${quizData[currentIdx].sample}`;
  feedback.style.display = 'block';
  document.getElementById('next-btn').style.display = 'block';
}

function nextQuestion() {
  currentIdx++;
  if (currentIdx < quizData.length) {
    loadQuestion();
  } else {
    showEndScreen();
  }
}

function showEndScreen() {
  const body = document.getElementById('game-body');
  body.innerHTML = `
    <div class="end-screen">
      <h2>Game Complete!</h2>
      <p>Your Final Score: <strong>${score} / ${quizData.length * 10}</strong></p>
      <button class="btn submit-btn" onclick="location.reload()">Play Again</button>
    </div>
  `;
}

loadQuestion();
</script>
</body>
</html>
