# ICS-Quadratic-Grader--Lupenga-VictorSolomon-
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Quadratic Solver & Grade Converter</title>
       <style>
/* General page styling */
body {
    font-family: Arial, sans-serif;
    margin: 30px;
    background-color: #f9f9f9;
    color: #333;
}

/* Headings */
h1 {
    text-align: center;
    color: #2c3e50;
}
h2 {
    color: #34495e;
    margin-top: 20px;
}

/* Section boxes */
.section {
    background-color: #fff;
    border: 1px solid #ccc;
    padding: 20px;
    margin-bottom: 30px;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

/* Labels and inputs */
label {
    display: inline-block;
    width: 100px;
    font-weight: bold;
}
input[type="number"] {
    width: 120px;
    padding: 5px;
    margin-bottom: 10px;
    border: 1px solid #aaa;
    border-radius: 4px;
}

/* Buttons */
button {
    padding: 6px 12px;
    margin-right: 10px;
    background-color: #3498db;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
button:hover {
    background-color: #2980b9;
}

/* Result and error messages */
.result {
    margin-top: 15px;
    font-weight: bold;
    color: #2c3e50;
}
.error {
    color: red;
    font-size: 0.9em;
    margin-left: 5px;
}
</style>

    </head>
<body>
    <script>
  // Quadratic solver elements
  const aInput = document.getElementById('a');
  const bInput = document.getElementById('b');
  const cInput = document.getElementById('c');
  const computeQuadraticBtn = document.getElementById('compute-quadratic');
  const resetQuadraticBtn = document.getElementById('reset-quadratic');
  const quadraticResultDiv = document.getElementById('quadratic-result');

  const aError = document.getElementById('a-error');
  const bError = document.getElementById('b-error');
  const cError = document.getElementById('c-error');

  // Function to clear errors and results
  function clearQuadraticUI() {
    aError.textContent = '';
    bError.textContent = '';
    cError.textContent = '';
    quadraticResultDiv.textContent = '';
  }

  // Compute quadratic roots
  computeQuadraticBtn.addEventListener('click', function () {
    clearQuadraticUI();

    const a = parseFloat(aInput.value);
    const b = parseFloat(bInput.value);
    const c = parseFloat(cInput.value);

    let valid = true;

    if (isNaN(a)) { aError.textContent = 'Enter a number'; valid = false; }
    else if (a === 0) { aError.textContent = 'a cannot be 0'; valid = false; }
    if (isNaN(b)) { bError.textContent = 'Enter a number'; valid = false; }
    if (isNaN(c)) { cError.textContent = 'Enter a number'; valid = false; }

    if (!valid) return;

    const discriminant = b * b - 4 * a * c;
    let rootsText = `Discriminant (D) = ${discriminant.toFixed(2)}\n`;

    if (discriminant > 0) {
      const root1 = (-b + Math.sqrt(discriminant)) / (2 * a);
      const root2 = (-b - Math.sqrt(discriminant)) / (2 * a);
      rootsText += `Two distinct real roots: ${root1.toFixed(2)} , ${root2.toFixed(2)}`;
    } else if (discriminant === 0) {
      const root = -b / (2 * a);
      rootsText += `One repeated real root: ${root.toFixed(2)}`;
    } else {
      const realPart = (-b / (2 * a)).toFixed(2);
      const imaginaryPart = (Math.sqrt(-discriminant) / (2 * a)).toFixed(2);
      rootsText += `Two complex roots: ${realPart} + ${imaginaryPart}i , ${realPart} - ${imaginaryPart}i`;
    }

    quadraticResultDiv.textContent = rootsText;
  });

  // Reset button
  resetQuadraticBtn.addEventListener('click', function () {
    aInput.value = '';
    bInput.value = '';
    cInput.value = '';
    clearQuadraticUI();
  });
</script>

    
    <script>

        const scoreInput = document.getElementById('score');
        const computeGradeBtn = document.getElementById('compute-grade');
        const resetGradeBtn = document.getElementById('reset-grade');
        const graderesultDiv = document.getElementById('grade-result');
        const scoreError = document.getElementById('score-error');
        
        function clearGradeUI() {
            scoreError.textContent = "";
            gradeResultDiv.textContent = "";
        }

        function getGradeFromScore(score) {
            if (score >= 85) return 'A+'
            if (score >= 75) return 'A'
            if (score >= 65) return 'B+'
            if (score >= 60) return 'B'
            if (score >= 55) return 'C+'
            if (score >= 50) return 'C'
            return 'D';

            computeGradebtn.addEventListener('click', function () {

                clearGradeUi();

                const raw = scoreInput.value.trim();

                if (raw ==="") {
                    scoreError.textContent = 'Please enter a score (0-100).';
                    return;
                }

                const num = Number(raw);
                if (Number.isNaN(num)){
                    scoreError.textContent = 'Enter a valid number';
                    return;

                    
                }
                if (!Number.isInteger(num)){
                    scoreError.textContent = 'Score must be an integer (no decimals).';
                    return;
                }
                if (num < 0 || num > 100){
                    scoreError.textContent = 'Score must be between 0 and 100.';
                    return;

                }
                const grade = getgradeFromScore(num);
                gradeResultDiv.textContent = 'Score ${num} - Grade ${grade}';

            });
            resetGradeBtn.addEventListener('click', function (){
                scoreInput.value = "";
                cleargradeUI();
            
        })};
    </script>
</body>
</html>
