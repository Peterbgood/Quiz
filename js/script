// Select the quiz container and elements
const quizContainer = document.querySelector(".quiz-container");
const questionElement = document.querySelector(".question");
const answersElement = document.querySelector(".answers");
const resultElement = document.querySelector(".result");
const nextButtonContainer = document.querySelector(".next-button-container");

// API endpoint and parameters
const apiEndpoint = "https://opentdb.com/api.php";
const params = {
amount: 25,
type: "multiple"
};

const difficulties = ["easy"];

// Initialize the current question index, score, and userAnswered flag
let currentQuestionIndex = 0;
let score = 0;
let userAnswered = false;
let questions = [];

// Function to fetch questions from the API
async function fetchQuestions() {
questions = [];
const response = await fetch(`${apiEndpoint}?${new URLSearchParams({
...params,
amount: 50,
category: 22 // You can remove this if you want questions from all categories
})}`);
const data = await response.json();
questions = data.results;
questions = questions.sort(() => Math.random() - 0.5);
questions = questions.slice(0, 10); // Adjust this number to get more or fewer questions
}

// Display the question and answers
function displayQuestion() {
const currentQuestion = questions[currentQuestionIndex];
const questionNumberElement = document.createElement("div");
questionNumberElement.innerHTML = `<strong>Question ${currentQuestionIndex + 1} of 10</strong>`;
questionNumberElement.className = "question-number";
questionElement.innerHTML = "";
questionElement.appendChild(questionNumberElement);
questionElement.innerHTML += currentQuestion.question;
answersElement.innerHTML = "";
const answers = [...currentQuestion.incorrect_answers, currentQuestion.correct_answer];
answers.sort(() => Math.random() - 0.5);
answers.forEach((answer, index) => {
const answerElement = document.createElement("button");
answerElement.innerHTML = answer;
answerElement.className = "btn btn-dark d-block answer-button mb-4";
answerElement.setAttribute("data-answer", answer);
answerElement.addEventListener("click", () => {
if (userAnswered) return; // If user has already answered, do nothing
const userAnswer = answer;
checkAnswer(userAnswer, currentQuestion.correct_answer);
});
answersElement.appendChild(answerElement);
});
}

// Check the user's answer
function checkAnswer(userAnswer, correctAnswer) {
if (userAnswered) return; // If user has already answered, do nothing
userAnswered = true; // Set flag to true

if (userAnswer === correctAnswer) {
resultElement.textContent = "";
score++; // Increment the score if the answer is correct
} else {
resultElement.textContent = "";
}
// Highlight the correct answer in green
const correctAnswerButton = answersElement.querySelector(`button[data-answer="${correctAnswer}"]`);
correctAnswerButton.classList.add("correct-answer", "btn-success");
correctAnswerButton.classList.remove("btn-secondary");

// Highlight the user's answer in red if incorrect
if (userAnswer !== correctAnswer) {
const userAnswerButton = answersElement.querySelector(`button[data-answer="${userAnswer}"]`);
userAnswerButton.classList.add("incorrect-answer", "btn-danger");
userAnswerButton.classList.remove("btn-secondary");
}
// Check if the next button already exists
const nextButton = document.querySelector(".next-button-container button");
if (!nextButton) {
// Show the next button
const nextButton = document.createElement("button");
nextButton.textContent = "Next";
nextButton.className = "btn btn-primary";
nextButton.addEventListener("click", nextQuestion);
nextButtonContainer.appendChild(nextButton);
}
}

// Proceed to the next question
function nextQuestion() {
userAnswered = false; // Reset flag for next question

// Remove the play again and share buttons if they exist
const playAgainButton = document.querySelector(".play-again-button");
if (playAgainButton) {
playAgainButton.remove();
}
const shareButton = document.querySelector(".share-button");
if (shareButton) {
shareButton.remove();
}
resultElement.textContent = ""; // Clear the result of the previous question
currentQuestionIndex++;
if (currentQuestionIndex >= 10) {
// Game over, display final score and share button
if (score < 6) {
resultElement.innerHTML = `Game Over! Your final score is ${score}/10 &#129318;`;
} else if (score >= 6 && score <= 8) {
resultElement.innerHTML = `Game Over! Your final score is ${score}/10 &#129305;`;
} else {
resultElement.innerHTML = `Game Over! Your final score is ${score}/10 &#127891;`;
}

const playAgainButton = document.createElement("button");
playAgainButton.textContent = "Play Again";
playAgainButton.className = "btn btn-primary play-again-button";
playAgainButton.addEventListener("click", () => {
location.reload();
});
quizContainer.appendChild(playAgainButton);
const shareButton = document.createElement("button");
shareButton.textContent = "Share";
shareButton.className = "btn btn-warning share-button";
shareButton.addEventListener("click", () => {
const daysSinceStart = Math.floor((new Date() - new Date('2024-09-12')) / (1000 * 60 * 60 * 24));
const shareText = `#Pete's Quiz #${daysSinceStart}\nScored: ${score}/10\n\n${window.location.href}`;
if (navigator.share) {
navigator.share({
title: "Quiz Score",
text: shareText,
});
} else {
alert("Sharing not supported on this device.");
}
});
quizContainer.appendChild(shareButton);
// Hide the next button
const nextButton = document.querySelector(".next-button-container button");
if (nextButton) {
nextButton.remove();
}
// Reload the script tag after the game is over
const scriptTag = document.querySelector('script[src="Script.js"]');
scriptTag.parentNode.removeChild(scriptTag);
const newScriptTag = document.createElement('script');
newScriptTag.src = 'Script.js';
document.head.appendChild(newScriptTag);
} else {
displayQuestion();
// Remove the next button if it exists
const nextButton = document.querySelector(".next-button-container button");
if (nextButton) {
nextButton.remove();
}
}
}

// Fetch questions from the API when the quiz starts
fetchQuestions().then(() => {
displayQuestion();
});
