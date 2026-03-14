<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mortgage Renewal Payment Shock Calculator</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      padding: 24px;
      font-family: Arial, Helvetica, sans-serif;
      background: #f4f7fb;
      color: #1f2937;
    }

    .wrap {
      max-width: 760px;
      margin: 0 auto;
      background: #ffffff;
      border-radius: 20px;
      box-shadow: 0 12px 35px rgba(0, 0, 0, 0.08);
      overflow: hidden;
    }

    .header {
      padding: 32px 28px 20px;
      background: linear-gradient(180deg, #f8fbff 0%, #ffffff 100%);
      border-bottom: 1px solid #e5e7eb;
    }

    .header h1 {
      margin: 0 0 10px;
      font-size: 30px;
      line-height: 1.2;
      color: #111827;
    }

    .header p {
      margin: 0;
      font-size: 17px;
      color: #4b5563;
      line-height: 1.6;
    }

    .body {
      padding: 28px;
    }

    .lead-box,
    .calculator-box {
      margin-top: 0;
    }

    .calculator-box {
      display: none;
    }

    .input-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 18px;
    }

    .field {
      display: flex;
      flex-direction: column;
    }

    .field.full-width {
      grid-column: 1 / -1;
    }

    .field label {
      font-size: 15px;
      font-weight: 700;
      margin-bottom: 8px;
      color: #374151;
    }

    .field input {
      width: 100%;
      padding: 16px;
      font-size: 18px;
      border: 1px solid #d1d5db;
      border-radius: 12px;
      outline: none;
      transition: 0.2s ease;
      background: #fff;
    }

    .field input:focus {
      border-color: #2563eb;
      box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.12);
    }

    .helper-text {
      margin-top: 6px;
      font-size: 13px;
      color: #6b7280;
      line-height: 1.5;
    }

    .button-row {
      margin-top: 22px;
    }

    .btn {
      width: 100%;
      border: none;
      border-radius: 14px;
      background: #2563eb;
      color: #ffffff;
      padding: 17px 20px;
      font-size: 18px;
      font-weight: 700;
      cursor: pointer;
      transition: 0.2s ease;
    }

    .btn:hover {
      background: #1d4ed8;
    }

    .error-message,
    .success-message {
      display: none;
      margin-top: 14px;
      font-size: 14px;
      font-weight: 600;
      line-height: 1.5;
    }

    .error-message {
      color: #b91c1c;
    }

    .success-message {
      color: #166534;
    }

    .results-box {
      display: none;
      margin-top: 26px;
      padding: 24px;
      background: #eff6ff;
      border: 1px solid #bfdbfe;
      border-radius: 18px;
    }

    .results-box h2 {
      margin: 0 0 18px;
      font-size: 24px;
      color: #1e3a8a;
    }

    .result-row {
      display: flex;
      justify-content: space-between;
      gap: 20px;
      padding: 12px 0;
      border-bottom: 1px solid #dbeafe;
    }

    .result-row:last-child {
      border-bottom: none;
    }

    .result-label {
      font-size: 16px;
      font-weight: 600;
      color: #374151;
    }

    .result-value {
      font-size: 18px;
      font-weight: 700;
      color: #111827;
      text-align: right;
    }

    .result-highlight {
      margin-top: 18px;
      padding: 16px;
      background: #ffffff;
      border: 1px solid #dbeafe;
      border-radius: 14px;
    }

    .result-highlight strong {
      display: block;
      margin-bottom: 6px;
      font-size: 16px;
      color: #1e3a8a;
    }

    .message {
      margin-top: 20px;
      font-size: 15px;
      color: #374151;
      line-height: 1.6;
    }

    .cta-row {
      margin-top: 18px;
    }

    .cta-btn {
      display: inline-block;
      text-decoration: none;
      background: #111827;
      color: #ffffff;
      padding: 14px 18px;
      border-radius: 12px;
      font-weight: 700;
      font-size: 15px;
    }

    .disclaimer {
      margin-top: 22px;
      padding: 14px 16px;
      border-radius: 12px;
      background: #f9fafb;
      border: 1px solid #e5e7eb;
      font-size: 13px;
      color: #6b7280;
      line-height: 1.6;
    }

    @media (max-width: 640px) {
      body {
        padding: 14px;
      }

      .header,
      .body {
        padding: 20px;
      }

      .header h1 {
        font-size: 25px;
      }

      .input-grid {
        grid-template-columns: 1fr;
      }

      .result-row {
        flex-direction: column;
        gap: 6px;
      }

      .result-value {
        text-align: left;
      }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="header">
      <h1>See What Your Mortgage Payment Could Jump To At Renewal</h1>
      <p>
        Enter your name and email to access the calculator. Then estimate how much your mortgage
        payment could increase when you renew at a new rate.
      </p>
    </div>

    <div class="body">
      <!-- Lead gate -->
      <div class="lead-box" id="leadBox">
        <div class="input-grid">
          <div class="field">
            <label for="leadName">Name</label>
            <input type="text" id="leadName" placeholder="Your name" />
          </div>

          <div class="field">
            <label for="leadEmail">Email Address</label>
            <input type="email" id="leadEmail" placeholder="you@example.com" />
          </div>
        </div>

        <div class="button-row">
          <button class="btn" id="accessBtn">Access Calculator</button>
        </div>

        <div class="error-message" id="leadError">
          Please enter a valid name and email address.
        </div>

        <div class="success-message" id="leadSuccess">
          Thanks. Your calculator is ready below.
        </div>
      </div>

      <!-- Calculator -->
      <div class="calculator-box" id="calculatorBox">
        <div class="input-grid">
          <div class="field">
            <label for="balance">Mortgage Balance ($)</label>
            <input type="number" id="balance" placeholder="Example: 850000" min="0" step="0.01" />
          </div>

          <div class="field">
            <label for="currentRate">Current Interest Rate (%)</label>
            <input type="number" id="currentRate" placeholder="Example: 2.49" min="0" step="0.01" />
          </div>

          <div class="field full-width">
            <label for="amortization">Remaining Amortization (years)</label>
            <input type="number" id="amortization" placeholder="Example: 22" min="1" step="1" />
            <div class="helper-text">
              This sample uses a renewal rate of 4.00% for illustration.
            </div>
          </div>
        </div>

        <div class="button-row">
          <button class="btn" id="calculateBtn">Calculate Payment Change</button>
        </div>

        <div class="error-message" id="calcError">
          Please enter valid numbers in all calculator fields.
        </div>

        <div class="results-box" id="resultsBox">
          <h2>Your Estimated Payment Change</h2>

          <div class="result-row">
            <div class="result-label">Current Monthly Payment</div>
            <div class="result-value" id="currentPayment">$0.00</div>
          </div>

          <div class="result-row">
            <div class="result-label">New Monthly Payment at 4.00%</div>
            <div class="result-value" id="renewalPayment">$0.00</div>
          </div>

          <div class="result-row">
            <div class="result-label">Monthly Payment Increase</div>
            <div class="result-value" id="monthlyIncrease">$0.00</div>
          </div>

          <div class="result-row">
            <div class="result-label">Annual Payment Increase</div>
            <div class="result-value" id="annualIncrease">$0.00</div>
          </div>

          <div class="result-highlight">
            <strong>What this means</strong>
            <div id="summaryText">Your estimated payment change will appear here.</div>
          </div>

          <div class="message">
            If your payment increase looks uncomfortable, it may not just be a rate problem.
            Mortgage structure can sometimes reduce monthly pressure. A strategy review can show
            you the best options.
          </div>

          <div class="cta-row">
            <a
              href="https://calendly.com/jtmortgages/discovery-call-1"
              class="cta-btn"
              target="_blank"
              rel="noopener noreferrer"
            >
              Book a Discovery Call
            </a>
          </div>

          <div class="disclaimer">
            Disclaimer: This calculator provides estimates for general illustration only. It uses
            a sample renewal rate of 4.00% and assumes standard monthly payments over the remaining
            amortization shown. Actual renewal payments may vary based on your lender, mortgage
            product, payment frequency, compounding method, fees, and any changes made at renewal.
            This is not a rate quote or financial advice.
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    // Google Apps Script web app URL that stores leads in your Google Sheet.
    const SCRIPT_URL =
      "https://script.google.com/macros/s/AKfycbz3Oe4sg-TrmaZ28kacvh_xuDOPUAvDCB9O0c9furY4XkCB0uQYS_Q1lt6Y8CD97YKY/exec";

    // Sample renewal rate used for comparison.
    const SAMPLE_RENEWAL_RATE = 4.0;

    // Formats numbers as Canadian currency.
    function formatCurrency(value) {
      return new Intl.NumberFormat("en-CA", {
        style: "currency",
        currency: "CAD",
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(value);
    }

    // Basic email validation.
    function isValidEmail(email) {
      return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
    }

    /*
      Standard mortgage payment formula:

      M = P * [ r(1+r)^n ] / [ (1+r)^n - 1 ]

      Where:
      M = monthly payment
      P = mortgage principal / balance
      r = monthly interest rate
      n = total number of monthly payments

      If the rate is 0, payment is principal / months.
    */
    function calculateMonthlyPayment(principal, annualRatePercent, years) {
      const monthlyRate = (annualRatePercent / 100) / 12;
      const totalPayments = years * 12;

      if (monthlyRate === 0) {
        return principal / totalPayments;
      }

      return principal * (
        (monthlyRate * Math.pow(1 + monthlyRate, totalPayments)) /
        (Math.pow(1 + monthlyRate, totalPayments) - 1)
      );
    }

    function buildSummary(monthlyIncrease, annualIncrease) {
      if (monthlyIncrease <= 0) {
        return "Based on these numbers, your monthly payment does not increase at the sample 4.00% renewal rate.";
      }

      return "At a 4.00% sample renewal rate, your payment could increase by " +
        formatCurrency(monthlyIncrease) +
        " per month, or about " +
        formatCurrency(annualIncrease) +
        " per year.";
    }

    async function storeLead(name, email) {
      try {
        await fetch(SCRIPT_URL, {
          method: "POST",
          body: JSON.stringify({ name, email }),
          headers: {
            "Content-Type": "text/plain;charset=utf-8"
          },
          mode: "no-cors"
        });
        return true;
      } catch (error) {
        console.error("Lead submission failed:", error);
        return false;
      }
    }

    document.getElementById("accessBtn").addEventListener("click", async function () {
      const name = document.getElementById("leadName").value.trim();
      const email = document.getElementById("leadEmail").value.trim();
      const leadError = document.getElementById("leadError");
      const leadSuccess = document.getElementById("leadSuccess");
      const calculatorBox = document.getElementById("calculatorBox");

      leadError.style.display = "none";
      leadSuccess.style.display = "none";

      if (!name || !isValidEmail(email)) {
        leadError.style.display = "block";
        return;
      }

      await storeLead(name, email);

      leadSuccess.style.display = "block";
      calculatorBox.style.display = "block";
      calculatorBox.scrollIntoView({ behavior: "smooth", block: "start" });
    });

    document.getElementById("calculateBtn").addEventListener("click", function () {
      const balance = parseFloat(document.getElementById("balance").value);
      const currentRate = parseFloat(document.getElementById("currentRate").value);
      const amortization = parseFloat(document.getElementById("amortization").value);

      const calcError = document.getElementById("calcError");
      const resultsBox = document.getElementById("resultsBox");

      calcError.style.display = "none";

      if (
        isNaN(balance) || balance <= 0 ||
        isNaN(currentRate) || currentRate < 0 ||
        isNaN(amortization) || amortization <= 0
      ) {
        calcError.style.display = "block";
        resultsBox.style.display = "none";
        return;
      }

      const currentPayment = calculateMonthlyPayment(balance, currentRate, amortization);
      const renewalPayment = calculateMonthlyPayment(balance, SAMPLE_RENEWAL_RATE, amortization);
      const monthlyIncrease = renewalPayment - currentPayment;
      const annualIncrease = monthlyIncrease * 12;

      document.getElementById("currentPayment").textContent = formatCurrency(currentPayment);
      document.getElementById("renewalPayment").textContent = formatCurrency(renewalPayment);
      document.getElementById("monthlyIncrease").textContent = formatCurrency(monthlyIncrease);
      document.getElementById("annualIncrease").textContent = formatCurrency(annualIncrease);
      document.getElementById("summaryText").textContent = buildSummary(monthlyIncrease, annualIncrease);

      resultsBox.style.display = "block";
      resultsBox.scrollIntoView({ behavior: "smooth", block: "start" });
    });
  </script>
</body>
</html>
