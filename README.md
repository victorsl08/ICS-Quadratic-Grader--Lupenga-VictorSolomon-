# ICS-Quadratic-Grader--Lupenga-VictorSolomon-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Quadratic Solver & Grade Converter</title>
    <style>
       body {
    font-family: Arial, sans-serif;
    margin: 30px;
    background-color: #121212; /* black background */
    color: #fff; /* text color white */
    display: flex;
    flex-direction: column;
    align-items: center;
}

h1, h2 {
    color: #ff4d4d; /* red headings */
}

.section {
    border: 1px solid #ff4d4d; /* red border */
    padding: 20px;
    margin-bottom: 30px;
    border-radius: 8px;
    background-color: #1a1a1a; /* dark gray section */
}

label {
    display: inline-block;
    width: 100px;
    color: #ff4d4d; /* red labels */
}

input[type="number"] {
    width: 100px;
    padding: 5px;
    margin-bottom: 10px;
    border: 1px solid #ff4d4d; /* red border */
    border-radius: 4px;
    background-color: #222; /* dark input */
    color: #fff; /* white text */
}

button {
    padding: 5px 10px;
    margin-right: 10px;
    background-color: #ff4d4d; /* red button */
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

button:hover {
    background-color: #cc0000; /* darker red on hover */
}

.result {
    margin-top: 15px;
    font-weight: bold;
    color: #ff4d4d; /* red result */
}

.error {
    color: #ff4d4d; /* red error */
    font-size: 0.9em;
}

        
    </style>
</head>
<body>

    <h1>Quadratic Solver & Grade Converter</h1>
    <p>Use the sections below to solve quadratic equations or convert a numeric score to a letter grade.</p>

    <!-- Quadratic Solver Section -->
    <div class="section" id="quadratic-section">
        <h2>Quadratic Solver</h2>
        <div>
            <label for="a">a:</label>
            <input type="number" id="a">
            <span class="error" id="a-error"></span>
        </div>
        <div>
            <label for="b">b:</label>
            <input type="number" id="b">
            <span class="error" id="b-error"></span>
        </div>
        <div>
            <label for="c">c:</label>
            <input type="number" id="c">
            <span class="error" id="c-error"></span>
        </div>
        <button id="compute-quadratic">Compute</button>
        <button id="reset-quadratic">Reset</button>
        <div class="result" id="quadratic-result"></div>
    </div>

    <!-- Grading System Section -->
    <div class="section" id="grading-section">
        <h2>Grade Converter</h2>
        <div>
            <label for="score">Score:</label>
            <input type="number" id="score" min="0" max="100">
            <span class="error" id="score-error"></span>
        </div>
        <button id="compute-grade">Compute</button>
        <button id="reset-grade">Reset</button>
        <div class="result" id="grade-result"></div>
    </div>
<script>
    // Grab elements
    const aInput = document.getElementById('a');
    const bInput = document.getElementById('b');
    const cInput = document.getElementById('c');
    const computeBtn = document.getElementById('compute-quadratic');
    const resetBtn = document.getElementById('reset-quadratic');
    const resultDiv = document.getElementById('quadratic-result');

    const aError = document.getElementById('a-error');
    const bError = document.getElementById('b-error');
    const cError = document.getElementById('c-error');

    // Compute quadratic roots
    computeBtn.addEventListener('click', function() {
        // Clear previous errors
        aError.textContent = '';
        bError.textContent = '';
        cError.textContent = '';
        resultDiv.textContent = '';

        const a = parseFloat(aInput.value);
        const b = parseFloat(bInput.value);
        const c = parseFloat(cInput.value);

        let valid = true;

        // Validation
        if (isNaN(a)) { aError.textContent = 'Enter a number'; valid = false; }
        else if (a === 0) { aError.textContent = 'a cannot be 0'; valid = false; }
        if (isNaN(b)) { bError.textContent = 'Enter a number'; valid = false; }
        if (isNaN(c)) { cError.textContent = 'Enter a number'; valid = false; }

        if (!valid) return;

        const Discriminant = b * b - 4 * a * c;
        let rootsText = `Discriminant (D) = ${Discriminant.toFixed(2)}\n`;


        if (Discriminant > 0) {
            const root1 = (-b + Math.sqrt(Discriminant)) / (2*a);
            const root2 = (-b - Math.sqrt(Discriminant)) / (2*a);
           rootsText += `Two distinct real roots: ${root1.toFixed(2)} , ${root2.toFixed(2)}`;
;
        } else if (Discriminant === 0) {
            const root = -b / (2*a);
            rootsText += `One repeated real root: ${root.toFixed(2)}`;

        } else {
            const realPart = (-b / (2*a)).toFixed(2);
            const imaginaryPart = (Math.sqrt(-Discriminant)/(2*a)).toFixed(2);
            rootsText += `Two complex roots: ${realPart} + ${imaginaryPart}i , ${realPart} - ${imaginaryPart}i`;

        }

        resultDiv.textContent = rootsText;
    });

    // Reset button
    resetBtn.addEventListener('click', function() {
        aInput.value = '';
        bInput.value = '';
        cInput.value = '';
        aError.textContent = '';
        bError.textContent = '';
        cError.textContent = '';
        resultDiv.textContent = '';
    });
</script>
<script>
  // Grading system elements
  const scoreInput = document.getElementById('score');
  const computeGradeBtn = document.getElementById('compute-grade');
  const resetGradeBtn = document.getElementById('reset-grade');
  const gradeResultDiv = document.getElementById('grade-result');
  const scoreError = document.getElementById('score-error');

  // Clear previous messages
  function clearGradeUI() {
    scoreError.textContent = '';
    gradeResultDiv.textContent = '';
  }

  // Determine grade from score
  function getGrade(score) {
    if (score >= 85) return 'A+';
    if (score >= 75) return 'A';
    if (score >= 65) return 'B+';
    if (score >= 60) return 'B';
    if (score >= 55) return 'C+';
    if (score >= 50) return 'C';
    return 'D';
  }

  // Compute button
  computeGradeBtn.addEventListener('click', function () {
    clearGradeUI();
    const raw = scoreInput.value.trim();

    if (raw === '') {
      scoreError.textContent = 'Please enter a score (0–100).';
      return;
    }

    const num = Number(raw);

    if (Number.isNaN(num)) {
      scoreError.textContent = 'Enter a valid number.';
      return;
    }

    if (!Number.isInteger(num)) {
      scoreError.textContent = 'Score must be an integer.';
      return;
    }

    if (num < 0 || num > 100) {
      scoreError.textContent = 'Score must be between 0 and 100.';
      return;
    }

    const grade = getGrade(num);
    gradeResultDiv.textContent = `Score ${num} — Grade ${grade}`;

  });

  // Reset button
  resetGradeBtn.addEventListener('click', function () {
    scoreInput.value = '';
    clearGradeUI();
  });
</script>
</body>
</html>
            
        })};
    </script>
</body>
</html>
