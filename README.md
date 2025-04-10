<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Registration and Sports Survey</title>
  <style>
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(-30px); }
      to { opacity: 1; transform: translateY(0); }
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to right, #83a4d4, #b6fbff);
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
      padding: 40px;
      margin: 0;
    }

    .container, .survey-container, .report-container {
      background: white;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
      animation: fadeIn 0.8s ease-in-out;
      max-width: 700px;
      width: 100%;
      margin-bottom: 30px;
    }

    h2, h3 {
      text-align: center;
      color: #333;
      margin-bottom: 20px;
    }

    label {
      display: block;
      margin-top: 15px;
      font-weight: bold;
      color: #444;
    }

    input, select, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 6px;
      border: 1px solid #ccc;
      border-radius: 6px;
      box-sizing: border-box;
    }

    .error {
      color: #d8000c;
      background-color: #ffbaba;
      padding: 6px;
      border-radius: 4px;
      font-size: 12px;
      margin-top: 5px;
      display: none;
    }

    button {
      width: 100%;
      padding: 12px;
      margin-top: 25px;
      background: gray;
      color: white;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      cursor: not-allowed;
    }

    button.active {
      background: #007bff;
      cursor: pointer;
    }

    button.active:hover {
      background: #0056b3;
    }

    .checkbox-group label {
      display: inline-block;
      margin-right: 10px;
    }

    .student-data {
      background: #f4f4f4;
      padding: 20px;
      margin-top: 20px;
      border-radius: 10px;
      animation: fadeIn 0.5s ease-in-out;
    }

    .student-data p {
      margin: 6px 0;
      font-size: 14px;
    }

    .report-container {
      animation: fadeIn 0.8s ease-in-out;
    }

    .no-data {
      text-align: center;
      font-style: italic;
      color: #666;
    }
  </style>
</head>
<body>

<div id="mainContent"></div>

<script>
  const urlParams = new URLSearchParams(window.location.search);
  const isReportPage = urlParams.get("report") === "true";
  const mainContent = document.getElementById("mainContent");

  if (isReportPage) {
    showReportPage();
  } else {
    showRegistrationForm();
  }

  function showRegistrationForm() {
    mainContent.innerHTML = `
      <div class="container" id="registrationContainer">
        <h2>Register</h2>
        <form id="registrationForm">
          <label for="username">Username</label>
          <input type="text" id="username" />
          <div class="error" id="usernameError">Username must be more than 3 characters</div>

          <label for="email">Email</label>
          <input type="text" id="email" />
          <div class="error" id="emailError">Email is not valid</div>

          <label for="password">Password</label>
          <input type="password" id="password" />
          <div class="error" id="passwordError">Password must be more than 6 characters</div>

          <label for="confirmPassword">Confirm Password</label>
          <input type="password" id="confirmPassword" />
          <div class="error" id="confirmPasswordError">Passwords do not match</div>

          <button type="submit" id="submitBtn" disabled>Submit</button>
        </form>
      </div>
    `;

    const username = document.getElementById("username");
    const email = document.getElementById("email");
    const password = document.getElementById("password");
    const confirmPassword = document.getElementById("confirmPassword");
    const submitBtn = document.getElementById("submitBtn");

    const usernameError = document.getElementById("usernameError");
    const emailError = document.getElementById("emailError");
    const passwordError = document.getElementById("passwordError");
    const confirmPasswordError = document.getElementById("confirmPasswordError");

    function validateForm(callback) {
      let valid = true;

      if (username.value.trim().length < 4) {
        usernameError.style.display = "block";
        valid = false;
      } else {
        usernameError.style.display = "none";
      }

      const emailPattern = /^[^@\s]+@[^@\s]+\.[^@\s]+$/;
      if (!emailPattern.test(email.value.trim())) {
        emailError.style.display = "block";
        valid = false;
      } else {
        emailError.style.display = "none";
      }

      if (password.value.length < 7) {
        passwordError.style.display = "block";
        valid = false;
      } else {
        passwordError.style.display = "none";
      }

      if (confirmPassword.value !== password.value || confirmPassword.value === "") {
        confirmPasswordError.textContent = confirmPassword.value === ""
          ? "Confirm Password is required"
          : "Passwords do not match";
        confirmPasswordError.style.display = "block";
        valid = false;
      } else {
        confirmPasswordError.style.display = "none";
      }

      callback(valid);
    }

    [username, email, password, confirmPassword].forEach(input =>
      input.addEventListener("input", () => {
        validateForm(valid => {
          submitBtn.disabled = !valid;
          submitBtn.classList.toggle("active", valid);
        });
      })
    );

    document.getElementById("registrationForm").addEventListener("submit", function (event) {
      event.preventDefault();
      validateForm(valid => {
        if (valid) {
          loadSurveyForm();
        }
      });
    });
  }

  function loadSurveyForm() {
    mainContent.innerHTML = `
      <div class="survey-container">
        <h2>Sports Survey</h2>
        <form id="surveyForm">
          <label for="full-name">Full Name:</label>
          <input type="text" id="full-name" required />

          <label for="survey-email">Email ID:</label>
          <input type="email" id="survey-email" required />

          <label for="age">Age:</label>
          <input type="number" id="age" required />

          <label for="phone">Phone Number:</label>
          <input type="text" id="phone" required />

          <label>What type of sports do you participate in regularly?</label>
          <div class="checkbox-group">
            <label><input type="checkbox" name="sports" value="Football"> Football</label>
            <label><input type="checkbox" name="sports" value="Basketball"> Basketball</label>
            <label><input type="checkbox" name="sports" value="Swimming"> Swimming</label>
            <label><input type="checkbox" name="sports" value="Other"> Other</label>
          </div>

          <label>How often do you participate in sports?</label>
          <div class="checkbox-group">
            <label><input type="checkbox" name="frequency" value="Daily"> Daily</label>
            <label><input type="checkbox" name="frequency" value="Weekly"> Weekly</label>
            <label><input type="checkbox" name="frequency" value="Monthly"> Monthly</label>
            <label><input type="checkbox" name="frequency" value="Rarely"> Rarely</label>
          </div>

          <label>Primary reason for participating in sports:</label>
          <div class="checkbox-group">
            <label><input type="checkbox" name="reason" value="Physical"> Physical</label>
            <label><input type="checkbox" name="reason" value="Mental"> Mental</label>
            <label><input type="checkbox" name="reason" value="Social"> Social</label>
            <label><input type="checkbox" name="reason" value="Fun"> Fun</label>
          </div>

          <label>Do you think sports events are conducted fairly?</label>
          <select id="fairness" required>
            <option value="">Select</option>
            <option value="Yes">Yes</option>
            <option value="No">No</option>
            <option value="Unsure">Unsure</option>
          </select>

          <label for="suggestions">Suggestions for improvement:</label>
          <textarea id="suggestions" rows="4"></textarea>

          <button type="submit" class="active">Submit Survey</button>
        </form>
      </div>
    `;

    document.getElementById("surveyForm").addEventListener("submit", function (event) {
      event.preventDefault();

      const getCheckedValues = (name) =>
        Array.from(document.querySelectorAll(`input[name="${name}"]:checked`)).map(cb => cb.value);

      const data = {
        fullName: document.getElementById("full-name").value,
        email: document.getElementById("survey-email").value,
        age: document.getElementById("age").value,
        phone: document.getElementById("phone").value,
        sports: getCheckedValues("sports"),
        frequency: getCheckedValues("frequency"),
        reason: getCheckedValues("reason"),
        fairness: document.getElementById("fairness").value,
        suggestions: document.getElementById("suggestions").value
      };

      saveSurveyData(data).then(() => {
        window.location.href = window.location.pathname + "?report=true";
      });
    });
  }

  function saveSurveyData(data) {
    return new Promise((resolve) => {
      setTimeout(() => {
        const students = JSON.parse(localStorage.getItem("students")) || [];
        students.push(data);
        localStorage.setItem("students", JSON.stringify(students));
        resolve(students);
      }, 300);
    });
  }

  function showReportPage() {
    const students = JSON.parse(localStorage.getItem("students")) || [];
    let reportHTML = `
      <div class="report-container">
        <h2>Survey Report</h2>
        ${students.length === 0 ? '<p class="no-data">No records found.</p>' : ''}
        ${students.map((s, i) => `
          <div class="student-data">
            <p><strong>Student ${i + 1}</strong></p>
            <p><strong>Name:</strong> ${s.fullName}</p>
            <p><strong>Email:</strong> ${s.email}</p>
            <p><strong>Age:</strong> ${s.age}</p>
            <p><strong>Phone:</strong> ${s.phone}</p>
            <p><strong>Sports:</strong> ${s.sports.join(', ') || 'None'}</p>
            <p><strong>Frequency:</strong> ${s.frequency.join(', ') || 'None'}</p>
            <p><strong>Reasons:</strong> ${s.reason.join(', ') || 'None'}</p>
            <p><strong>Fairness:</strong> ${s.fairness}</p>
            <p><strong>Suggestions:</strong> ${s.suggestions || 'N/A'}</p>
          </div>
        `).join('')}
      </div>
    `;
    mainContent.innerHTML = reportHTML;
  }
</script>

</body>
</html>
