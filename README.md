# Ctest.html
For Aufnahmeprüfung.
```html
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>C-Test Maker</title>

<style>
    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        padding: 20px;
        font-family: Arial, Helvetica, sans-serif;
        background: #f3f5f7;
        color: #222;
    }

    .container {
        max-width: 1000px;
        margin: auto;
        background: white;
        padding: 25px;
        border-radius: 12px;
        box-shadow: 0 3px 15px rgba(0,0,0,0.10);
    }

    h1 {
        text-align: center;
        margin-top: 0;
    }

    h2 {
        margin-top: 30px;
        border-bottom: 1px solid #ddd;
        padding-bottom: 8px;
    }

    textarea {
        width: 100%;
        min-height: 160px;
        resize: vertical;
        padding: 12px;
        font-size: 17px;
        line-height: 1.5;
        border: 1px solid #bbb;
        border-radius: 7px;
        font-family: Arial, Helvetica, sans-serif;
    }

    button {
        border: none;
        border-radius: 7px;
        padding: 11px 18px;
        margin: 8px 5px 8px 0;
        font-size: 16px;
        cursor: pointer;
        background: #1769e0;
        color: white;
    }

    button:hover {
        background: #0d55bd;
    }

    button.secondary {
        background: #666;
    }

    button.secondary:hover {
        background: #444;
    }

    button.danger {
        background: #c62828;
    }

    button.danger:hover {
        background: #a51f1f;
    }

    .timer {
        margin-top: 15px;
        padding: 12px;
        background: #f0f4fa;
        border-radius: 7px;
        font-size: 20px;
        font-weight: bold;
        text-align: center;
    }

    .test-area {
        margin-top: 25px;
        font-size: 20px;
        line-height: 2.2;
        word-wrap: break-word;
    }

    .blank {
        width: 85px;
        min-width: 50px;
        padding: 4px 6px;
        margin: 0 2px;
        font-size: 18px;
        border: 1px solid #888;
        border-radius: 4px;
        text-align: center;
    }

    .blank.correct {
        background-color: #c8f7c5;
        border: 2px solid #39a33c;
    }

    .blank.wrong {
        background-color: #f8caca;
        border: 2px solid #d33;
    }

    .answer {
        color: #c62828;
        font-weight: bold;
        margin-left: 4px;
        white-space: nowrap;
    }

    .result {
        margin-top: 20px;
        padding: 15px;
        background: #f5f5f5;
        border-radius: 8px;
        font-size: 18px;
        line-height: 1.8;
    }

    .result-title {
        font-size: 22px;
        font-weight: bold;
        margin-bottom: 8px;
    }

    .analytics {
        margin-top: 20px;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 12px;
    }

    .stat-card {
        padding: 15px;
        background: #f4f6f8;
        border-radius: 8px;
        text-align: center;
    }

    .stat-number {
        font-size: 25px;
        font-weight: bold;
        margin-top: 5px;
    }

    .history-item {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 10px;
        padding: 12px;
        margin: 8px 0;
        background: #f1f3f5;
        border-radius: 7px;
    }

    .history-info {
        flex: 1;
    }

    .history-title {
        font-weight: bold;
    }

    .history-details {
        font-size: 14px;
        color: #555;
        margin-top: 4px;
    }

    .history-buttons button {
        margin: 2px;
        padding: 7px 10px;
        font-size: 14px;
    }

    .empty {
        color: #777;
        text-align: center;
        padding: 15px;
    }

    .hidden {
        display: none;
    }

    @media (max-width: 700px) {
        body {
            padding: 10px;
        }

        .container {
            padding: 15px;
        }

        h1 {
            font-size: 25px;
        }

        .test-area {
            font-size: 18px;
            line-height: 2.1;
        }

        .blank {
            width: 70px;
            font-size: 16px;
        }

        .analytics {
            grid-template-columns: 1fr;
        }

        .history-item {
            flex-direction: column;
            align-items: stretch;
        }

        .history-buttons {
            display: flex;
        }

        .history-buttons button {
            flex: 1;
        }
    }
</style>
</head>

<body>

<div class="container">

    <h1>C-Test Maker</h1>

    <textarea
        id="inputText"
        placeholder="Füge hier deinen deutschen Text ein..."
    ></textarea>

    <div>
        <button onclick="generateCTest()">C-Test erstellen</button>
        <button onclick="checkAnswers()">Antworten überprüfen</button>
        <button class="secondary" onclick="clearCurrentTest()">Zurücksetzen</button>
    </div>

    <div id="timer" class="timer">
        ⏱️ Zeit: 00:00
    </div>

    <div id="testArea" class="test-area"></div>

    <div id="result" class="result hidden"></div>

    <h2>📊 Leistungsanalyse</h2>

    <div class="analytics">

        <div class="stat-card">
            <div>Tests absolviert</div>
            <div class="stat-number" id="totalTests">0</div>
        </div>

        <div class="stat-card">
            <div>Durchschnittliche Punktzahl</div>
            <div class="stat-number" id="averageScore">0%</div>
        </div>

        <div class="stat-card">
            <div>Durchschnittliche Geschwindigkeit</div>
            <div class="stat-number" id="averageSpeed">0</div>
            <div>Blanks / Minute</div>
        </div>

    </div>

    <h2>📚 Gespeicherte Tests</h2>

    <div id="history"></div>

    <button class="danger" onclick="deleteAllHistory()">
        Alle Ergebnisse löschen
    </button>

</div>


<script>

/* =========================================================
   GLOBAL VARIABLES
========================================================= */

let startTime = null;
let timerInterval = null;
let testFinished = false;


/* =========================================================
   GENERATE C-TEST
========================================================= */

function generateCTest(textOverride = null) {

    let text;

    if (textOverride !== null) {
        text = textOverride;
        document.getElementById("inputText").value = text;
    } else {
        text = document.getElementById("inputText").value.trim();
    }

    if (!text) {
        alert("Bitte zuerst einen Text eingeben.");
        return;
    }

    clearInterval(timerInterval);

    const testArea = document.getElementById("testArea");
    const result = document.getElementById("result");

    testArea.innerHTML = "";
    result.innerHTML = "";
    result.classList.add("hidden");

    testFinished = false;

    /*
        Tokenization:

        Words and punctuation are separated.

        Example:

        "Deutschland, ist schön."

        becomes approximately:

        Deutschland
        ,
        ist
        schön
        .
    */

    const tokens = text.match(
        /[\p{L}\p{N}]+(?:[-'][\p{L}\p{N}]+)*|[^\s\p{L}\p{N}]/gu
    ) || [];

    let wordIndex = 0;
    let blankNumber = 0;

    tokens.forEach(token => {

        /*
            Check whether token is a word.

            Punctuation never enters this section.
        */

        const isWord = /^[\p{L}\p{N}]+(?:[-'][\p{L}\p{N}]+)*$/u.test(token);

        if (isWord) {

            /*
                Every second word becomes a blank.

                Very short words are left untouched because
                there would otherwise be little/no meaningful
                hidden part.
            */

            if (wordIndex % 2 === 1 && token.length >= 4) {

                const cutPosition = Math.ceil(token.length / 2);

                const visiblePart = token.substring(0, cutPosition);
                const hiddenPart = token.substring(cutPosition);

                const span = document.createElement("span");
                span.textContent = visiblePart;

                const input = document.createElement("input");

                input.type = "text";
                input.className = "blank";

                input.dataset.answer = hiddenPart;
                input.dataset.number = blankNumber;

                /*
                    Allow Enter to move to the next blank.
                */

                input.addEventListener("keydown", function(event) {

                    if (event.key === "Enter") {

                        event.preventDefault();

                        const blanks =
                            Array.from(document.querySelectorAll(".blank"));

                        const currentIndex = blanks.indexOf(input);

                        if (currentIndex < blanks.length - 1) {
                            blanks[currentIndex + 1].focus();
                        }
                    }
                });

                testArea.appendChild(span);
                testArea.appendChild(input);

                blankNumber++;

            } else {

                testArea.appendChild(
                    document.createTextNode(token)
                );
            }

            wordIndex++;

        } else {

            /*
                Punctuation is always shown normally.
            */

            testArea.appendChild(
                document.createTextNode(token)
            );
        }

        /*
            Add a normal space after each token where appropriate.

            Punctuation such as "." and "," should not receive
            a space before them.
        */

        const nextTokenIndex = tokens.indexOf(token) + 1;
        const nextToken = tokens[nextTokenIndex];

        if (
            nextToken &&
            !/^[,.;:!?%)\]}]$/.test(nextToken)
        ) {
            testArea.appendChild(
                document.createTextNode(" ")
            );
        }
    });

    /*
        Start stopwatch.
    */

    startTime = Date.now();

    updateTimer();

    timerInterval = setInterval(updateTimer, 1000);
}


/* =========================================================
   TIMER
========================================================= */

function updateTimer() {

    if (!startTime) return;

    const elapsedSeconds =
        Math.floor((Date.now() - startTime) / 1000);

    document.getElementById("timer").textContent =
        "⏱️ Zeit: " + formatTime(elapsedSeconds);
}


function formatTime(totalSeconds) {

    const hours =
        Math.floor(totalSeconds / 3600);

    const minutes =
        Math.floor((totalSeconds % 3600) / 60);

    const seconds =
        totalSeconds % 60;

    if (hours > 0) {

        return (
            String(hours).padStart(2, "0") +
            ":" +
            String(minutes).padStart(2, "0") +
            ":" +
            String(seconds).padStart(2, "0")
        );

    }

    return (
        String(minutes).padStart(2, "0") +
        ":" +
        String(seconds).padStart(2, "0")
    );
}


/* =========================================================
   CHECK ANSWERS
========================================================= */

function checkAnswers() {

    if (!startTime) {
        alert("Bitte zuerst einen C-Test erstellen.");
        return;
    }

    if (testFinished) {
        return;
    }

    clearInterval(timerInterval);

    const inputs =
        Array.from(document.querySelectorAll(".blank"));

    if (inputs.length === 0) {
        alert("Dieser Text enthält keine erstellten Lücken.");
        return;
    }

    let correct = 0;

    inputs.forEach(input => {

        const correctAnswer =
            input.dataset.answer.trim();

        const userAnswer =
            input.value.trim();

        /*
            Remove previous answer display if checking
            again somehow.
        */

        const oldAnswer = input.nextElementSibling;

        if (
            oldAnswer &&
            oldAnswer.classList &&
            oldAnswer.classList.contains("answer")
        ) {
            oldAnswer.remove();
        }

        if (userAnswer === correctAnswer) {

            input.classList.remove("wrong");
            input.classList.add("correct");

            correct++;

        } else {

            input.classList.remove("correct");
            input.classList.add("wrong");

            /*
                Show correct answer immediately next
                to the wrong blank.
            */

            const answerSpan =
                document.createElement("span");

            answerSpan.className = "answer";

            answerSpan.textContent =
                "(" + correctAnswer + ")";

            input.insertAdjacentElement(
                "afterend",
                answerSpan
            );
        }

        input.disabled = true;
    });


    /*
        Calculate results.
    */

    const timeTaken =
        Math.floor((Date.now() - startTime) / 1000);

    const percentage =
        (correct / inputs.length) * 100;

    const minutesTaken =
        timeTaken / 60;

    let speed = 0;

    if (minutesTaken > 0) {
        speed = inputs.length / minutesTaken;
    }

    const roundedPercentage =
        percentage.toFixed(1);

    const roundedSpeed =
        speed.toFixed(2);


    /*
        Display result.
    */

    const result =
        document.getElementById("result");

    result.innerHTML = `
        <div class="result-title">
            Ergebnis
        </div>

        ✅ Richtig:
        <strong>${correct}/${inputs.length}</strong>
        <br>

        📊 Prozent:
        <strong>${roundedPercentage}%</strong>
        <br>

        ⏱️ Zeit:
        <strong>${formatTime(timeTaken)}</strong>
        <br>

        ⚡ Geschwindigkeit:
        <strong>${roundedSpeed} Blanks/Minute</strong>
    `;

    result.classList.remove("hidden");

    document.getElementById("timer").textContent =
        "⏱️ Zeit: " + formatTime(timeTaken);


    /*
        Save result.
    */

    const originalText =
        document.getElementById("inputText").value.trim();

    saveTest({

        id: Date.now(),

        text: originalText,

        correct: correct,

        total: inputs.length,

        percentage:
            Number(roundedPercentage),

        time:
            timeTaken,

        speed:
            Number(roundedSpeed),

        date:
            new Date().toLocaleString()
    });

    testFinished = true;

    loadHistory();
    updateAnalytics();
}


/* =========================================================
   SAVE TEST
========================================================= */

function saveTest(test) {

    let history =
        JSON.parse(
            localStorage.getItem("ctest_history")
        ) || [];

    history.unshift(test);

    localStorage.setItem(
        "ctest_history",
        JSON.stringify(history)
    );
}


/* =========================================================
   LOAD HISTORY
========================================================= */

function loadHistory() {

    const history =
        JSON.parse(
            localStorage.getItem("ctest_history")
        ) || [];

    const container =
        document.getElementById("history");

    container.innerHTML = "";

    if (history.length === 0) {

        container.innerHTML =
            '<div class="empty">Noch keine Tests absolviert.</div>';

        return;
    }


    history.forEach((test, index) => {

        const item =
            document.createElement("div");

        item.className = "history-item";

        const info =
            document.createElement("div");

        info.className = "history-info";

        const title =
            document.createElement("div");

        title.className = "history-title";

        title.textContent =
            "Test " + (index + 1) +
            " — " +
            test.percentage +
            "%";

        const details =
            document.createElement("div");

        details.className = "history-details";

        details.textContent =
            test.correct +
            "/" +
            test.total +
            " richtig • " +
            formatTime(test.time) +
            " • " +
            test.speed +
            " Blanks/min • " +
            test.date;

        info.appendChild(title);
        info.appendChild(details);


        const buttons =
            document.createElement("div");

        buttons.className =
            "history-buttons";


        const loadButton =
            document.createElement("button");

        loadButton.textContent =
            "Öffnen";

        loadButton.onclick =
            function() {

                generateCTest(test.text);

                window.scrollTo({
                    top: 0,
                    behavior: "smooth"
                });
            };


        const deleteButton =
            document.createElement("button");

        deleteButton.textContent =
            "Löschen";

        deleteButton.className =
            "danger";

        deleteButton.onclick =
            function() {

                deleteTest(test.id);
            };


        buttons.appendChild(loadButton);
        buttons.appendChild(deleteButton);

        item.appendChild(info);
        item.appendChild(buttons);

        container.appendChild(item);
    });
}


/* =========================================================
   DELETE ONE TEST
========================================================= */

function deleteTest(id) {

    let history =
        JSON.parse(
            localStorage.getItem("ctest_history")
        ) || [];

    history =
        history.filter(test => test.id !== id);

    localStorage.setItem(
        "ctest_history",
        JSON.stringify(history)
    );

    loadHistory();
    updateAnalytics();
}


/* =========================================================
   DELETE ALL HISTORY
========================================================= */

function deleteAllHistory() {

    const confirmed =
        confirm(
            "Möchtest du wirklich alle gespeicherten Ergebnisse löschen?"
        );

    if (!confirmed) return;

    localStorage.removeItem(
        "ctest_history"
    );

    loadHistory();
    updateAnalytics();
}


/* =========================================================
   PERFORMANCE ANALYTICS
========================================================= */

function updateAnalytics() {

    const history =
        JSON.parse(
            localStorage.getItem("ctest_history")
        ) || [];


    document.getElementById("totalTests").textContent =
        history.length;


    if (history.length === 0) {

        document.getElementById("averageScore").textContent =
            "0%";

        document.getElementById("averageSpeed").textContent =
            "0";

        return;
    }


    const averageScore =
        history.reduce(
            (sum, test) =>
                sum + Number(test.percentage),
            0
        ) / history.length;


    const averageSpeed =
        history.reduce(
            (sum, test) =>
                sum + Number(test.speed),
            0
        ) / history.length;


    document.getElementById("averageScore").textContent =
        averageScore.toFixed(1) + "%";


    document.getElementById("averageSpeed").textContent =
        averageSpeed.toFixed(2);
}


/* =========================================================
   CLEAR CURRENT TEST
========================================================= */

function clearCurrentTest() {

    clearInterval(timerInterval);

    startTime = null;
    testFinished = false;

    document.getElementById("inputText").value = "";

    document.getElementById("testArea").innerHTML = "";

    document.getElementById("result").innerHTML = "";

    document.getElementById("result").classList.add("hidden");

    document.getElementById("timer").textContent =
        "⏱️ Zeit: 00:00";
}


/* =========================================================
   INITIALIZE
========================================================= */

loadHistory();
updateAnalytics();

</script>

</body>
</html>
```
