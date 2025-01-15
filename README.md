<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Score Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
        }
        label, input, button, select {
            margin: 10px 0;
            display: block;
            width: 100%;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid #ccc;
        }
        th, td {
            padding: 8px;
            text-align: center;
        }
        .manual {
            margin-top: 30px;
            font-size: 1.1em;
        }
        .manual h2 {
            color: #333;
        }
        .manual ul {
            list-style-type: none;
            padding-left: 0;
        }
        .manual li {
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Student Score Calculator</h1>

        <label for="numActivities">Number of Activities:</label>
        <select id="numActivities">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
            <option value="4">4</option>
            <option value="5">5</option>
        </select>

        <div id="activityInputs"></div>

        <button onclick="calculateScore()">Calculate Total Score</button>

        <h2>Results</h2>
        <table>
            <thead>
                <tr>
                    <th>Activity</th>
                    <th>Raw Score</th>
                    <th>Max Score</th>
                    <th>Weighted Score</th>
                </tr>
            </thead>
            <tbody id="resultsTable"></tbody>
        </table>

        <h3>Total Score: <span id="totalScore">0</span> / <span id="totalMaxScore">0</span></h3>

        <div class="manual">
            <h2>How to Use the Calculator</h2>
            <ul>
                <li><strong>Select the Number of Activities:</strong> Choose the number of activities you want to calculate scores for (1-5).</li>
                <li><strong>Input Raw Scores:</strong> For each activity, enter the score you received.</li>
                <li><strong>Input Max Scores:</strong> For each activity, enter the maximum possible score.</li>
                <li><strong>Click "Calculate Total Score":</strong> Once all scores are entered, click the button to calculate the total weighted score.</li>
                <li><strong>View the Results:</strong> A table will show the raw score, max score, and weighted score for each activity. The total score will be displayed at the bottom.</li>
            </ul>
            <h3>Created by: Sebastian de Leon</h3>
        </div>
    </div>

    <script>
        const numActivitiesDropdown = document.getElementById('numActivities');
        const activityInputsContainer = document.getElementById('activityInputs');
        const resultsTable = document.getElementById('resultsTable');
        const totalScoreElement = document.getElementById('totalScore');
        const totalMaxScoreElement = document.getElementById('totalMaxScore');

        numActivitiesDropdown.addEventListener('change', generateActivityInputs);

        function generateActivityInputs() {
            const numActivities = parseInt(numActivitiesDropdown.value);
            activityInputsContainer.innerHTML = '';

            for (let i = 1; i <= numActivities; i++) {
                const label1 = document.createElement('label');
                label1.textContent = `Activity ${i} Score:`;

                const input1 = document.createElement('input');
                input1.type = 'number';
                input1.id = `activity${i}Score`;
                input1.min = 0;

                const label2 = document.createElement('label');
                label2.textContent = `Activity ${i} Max Score (out of):`;

                const input2 = document.createElement('input');
                input2.type = 'number';
                input2.id = `activity${i}MaxScore`;
                input2.min = 1;

                activityInputsContainer.appendChild(label1);
                activityInputsContainer.appendChild(input1);
                activityInputsContainer.appendChild(label2);
                activityInputsContainer.appendChild(input2);
            }
        }

        function calculateScore() {
            const numActivities = parseInt(numActivitiesDropdown.value);
            let totalScore = 0;
            let totalMaxScore = 0;

            resultsTable.innerHTML = '';

            for (let i = 1; i <= numActivities; i++) {
                const rawScore = parseFloat(document.getElementById(`activity${i}Score`).value) || 0;
                const maxScore = parseFloat(document.getElementById(`activity${i}MaxScore`).value) || 1;
                const weightedScore = (rawScore / maxScore * 10).toFixed(2);

                totalScore += parseFloat(weightedScore);
                totalMaxScore += 10; // Always considering the total as 10 for normalized score

                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>Activity ${i}</td>
                    <td>${rawScore}</td>
                    <td>${maxScore}</td>
                    <td>${weightedScore}</td>
                `;
                resultsTable.appendChild(row);
            }

            totalScoreElement.textContent = totalScore.toFixed(2);
            totalMaxScoreElement.textContent = totalMaxScore;
        }

        // Initialize with 1 activity
        generateActivityInputs();
    </script>
</body>
</html>
