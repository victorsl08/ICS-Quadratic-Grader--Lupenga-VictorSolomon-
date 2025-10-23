# ICS-Quadratic-Grader--Lupenga-VictorSolomon-
Quadratic solver and Grade converter for ICT251
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>ICS Quadratic Solver & Grader</title>
  
</head>
<body>
  <h1>Quadratic Solver & Grade Converter</h1>
  <p class="lead">Single-file page: compute roots for ax² + bx + c = 0 and convert numeric scores (0–100) to letter grades. Double-click the index.html to run offline.</p>

  <section class="card" aria-labelledby="quad-title">
    <h2 id="quad-title">Quadratic Solver</h2>
    <form id="quad-form" novalidate>
      <div>
        <label for="a">a (must not be 0)</label>
        <input id="a" name="a" type="number" step="any" placeholder="e.g., 1" required>
        <div id="a-error" class="errors" aria-live="polite"></div>
      </div>

      <div>
        <label for="b">b</label>
        <input id="b" name="b" type="number" step="any" placeholder="e.g., -3" required>
        <div id="b-error" class="errors" aria-live="polite"></div>
      </div>

      <div>
        <label for="c">c</label>
        <input id="c" name="c" type="number" step="any" placeholder="e.g., 2" required>
        <div id="c-error" class="errors" aria-live="polite"></div>
      </div>

      <div class="full row">
        <button type="button" id="quad-calc" class="btn btn-primary">Compute Roots</button>
        <button type="button" id="quad-reset" class="btn btn-muted">Reset</button>
      </div>

      <div class="full">
        <div id="quad-result" class="result" aria-live="polite">Enter coefficients and click <strong>Compute Roots</strong>.</div>
      </div>
    </form>
  </section>

  <section class="card" aria-labelledby="grade-title">
    <h2 id="grade-title">Grade Converter</h2>
    <form id="grade-form" novalidate>
      <div style="min-width:140px;">
        <label for="score">Score (0–100)</label>
        <input id="score" name="score" type="number" step="1" min="0" max="100" placeholder="e.g., 82" required>
        <div id="score-error" class="errors" aria-live="polite"></div>
      </div>

      <div class="full row">
        <button type="button" id="grade-calc" class="btn btn-primary">Get Grade</button>
        <button type="button" id="grade-reset" class="btn btn-muted">Reset</button>
      </div>

      <div class="full">
        <div id="grade-result" class="result" aria-live="polite">Enter a score and click <strong>Get Grade</strong>.</div>
      </div>
    </form>
    <p class="small">Grade scale (inclusive): A+ 85–100, A 75–84, B+ 65–74, B 60–64, C+ 55–59, C 50–54, D 0–49.</p>
  </section>

  <script>
    // Utility: format numbers sensibly (up to 6 significant digits, trim trailing zeros)
    function fmt(n){
      if (!isFinite(n)) return String(n);
      // show up to 6 significant digits but keep integers clean
      let s = Number.isInteger(n) ? n.toString() : Number.parseFloat(n.toFixed(6)).toString();
      return s;
    }

    // Quadratic solver
    (function(){
      const aIn = document.getElementById('a');
      const bIn = document.getElementById('b');
      const cIn = document.getElementById('c');
      const calc = document.getElementById('quad-calc');
      const reset = document.getElementById('quad-reset');
      const result = document.getElementById('quad-result');
      const aErr = document.getElementById('a-error');
      const bErr = document.getElementById('b-error');
      const cErr = document.getElementById('c-error');

      function clearErrors(){ aErr.textContent=''; bErr.textContent=''; cErr.textContent=''; }

      calc.addEventListener('click', ()=>{
        clearErrors();
        const aVal = aIn.value.trim();
        const bVal = bIn.value.trim();
        const cVal = cIn.value.trim();
        let hasError = false;

        if (aVal === ''){ aErr.textContent = 'Please enter a value for a.'; hasError = true; }
        if (bVal === ''){ bErr.textContent = 'Please enter a value for b.'; hasError = true; }
        if (cVal === ''){ cErr.textContent = 'Please enter a value for c.'; hasError = true; }
        const a = parseFloat(aVal);
        const b = parseFloat(bVal);
        const c = parseFloat(cVal);
        if (!hasError){
          if (Number.isNaN(a)) { aErr.textContent = 'a must be a number.'; hasError = true; }
          if (Number.isNaN(b)) { bErr.textContent = 'b must be a number.'; hasError = true; }
          if (Number.isNaN(c)) { cErr.textContent = 'c must be a number.'; hasError = true; }
        }
        if (!hasError && a === 0){ aErr.textContent = 'Coefficient a cannot be 0 for a quadratic equation.'; hasError = true; }

        if (hasError){ result.innerHTML = '<strong>Cannot compute:</strong> fix the errors above.'; return; }

        // compute discriminant
        const D = b*b - 4*a*c;
        let out = '';
        out += '<div><strong>Discriminant (D):</strong> ' + fmt(D) + '</div>';

        if (D > 0){
          const sqrtD = Math.sqrt(D);
          const x1 = (-b + sqrtD) / (2*a);
          const x2 = (-b - sqrtD) / (2*a);
          out += '<div><strong>Nature:</strong> Two distinct real roots (D &gt; 0)</div>';
          out += `<div><strong>Roots:</strong> x₁ = ${fmt(x1)}, x₂ = ${fmt(x2)}</div>`;
        } else if (D === 0){
          const x = -b / (2*a);
          out += '<div><strong>Nature:</strong> One real repeated root (D = 0)</div>';
          out += `<div><strong>Root:</strong> x = ${fmt(x)}</div>`;
        } else {
          // complex conjugate roots
          const real = -b / (2*a);
          const imag = Math.sqrt(Math.abs(D)) / (2*a);
          out += '<div><strong>Nature:</strong> Two complex conjugate roots (D &lt; 0)</div>';
          out += `<div><strong>Roots:</strong> x₁ = ${fmt(real)} + ${fmt(imag)}i, x₂ = ${fmt(real)} - ${fmt(imag)}i</div>`;
        }

        result.innerHTML = out;
      });

      reset.addEventListener('click', ()=>{
        aIn.value = '';
        bIn.value = '';
        cIn.value = '';
        clearErrors();
        result.innerHTML = 'Enter coefficients and click <strong>Compute Roots</strong>.';
      });
    })();

    // Grading system
    (function(){
      const scoreIn = document.getElementById('score');
      const calc = document.getElementById('grade-calc');
      const reset = document.getElementById('grade-reset');
      const result = document.getElementById('grade-result');
      const scoreErr = document.getElementById('score-error');

      function clearError(){ scoreErr.textContent = ''; }

      function gradeFromScore(n){
        // inclusive ranges exactly as specified
        if (n >= 85 && n <= 100) return 'A+';
        if (n >= 75 && n <= 84) return 'A';
        if (n >= 65 && n <= 74) return 'B+';
        if (n >= 60 && n <= 64) return 'B';
        if (n >= 55 && n <= 59) return 'C+';
        if (n >= 50 && n <= 54) return 'C';
        if (n >= 0 && n <= 49) return 'D';
        return null;
      }

      calc.addEventListener('click', ()=>{
        clearError();
        const val = scoreIn.value.trim();
        if (val === ''){ scoreErr.textContent = 'Please enter a score between 0 and 100.'; result.innerHTML = 'Cannot compute grade.'; return; }
        const n = Number(val);
        if (!Number.isFinite(n) || !Number.isInteger(n)){
          scoreErr.textContent = 'Score must be an integer between 0 and 100.'; result.innerHTML = 'Cannot compute grade.'; return; }
        if (n < 0 || n > 100){ scoreErr.textContent = 'Score must be within 0 and 100.'; result.innerHTML = 'Cannot compute grade.'; return; }
        const g = gradeFromScore(n);
        if (g === null){ result.innerHTML = 'Score outside valid ranges.'; }
        else { result.innerHTML = `<strong>Score ${n} → Grade ${g}</strong>`; }
      });

      reset.addEventListener('click', ()=>{
        scoreIn.value = '';
        clearError();
        result.innerHTML = 'Enter a score and click <strong>Get Grade</strong>.';
      });
    })();
  </script>
</body>
</html>
