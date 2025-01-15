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
                    <th>Weighted Score</th>
                </tr>
            </thead>
            <tbody id="resultsTable"></tbody>
        </table>

        <h3>Total Score: <span id="totalScore">0</span> / 10</h3>
    </div>

    <script>
        const numActivitiesDropdown = document.getElementById('numActivities');
        const activityInputsContainer = document.getElementById('activityInputs');
        const resultsTable = document.getElementById('resultsTable');
        const totalScoreElement = document.getElementById('totalScore');

        numActivitiesDropdown.addEventListener('change', generateActivityInputs);

        function generateActivityInputs() {
            const numActivities = parseInt(numActivitiesDropdown.value);
            activityInputsContainer.innerHTML = '';

            for (let i = 1; i <= numActivities; i++) {
                const label = document.createElement('label');
                label.textContent = `Activity ${i} Score (out of 10):`;

                const input = document.createElement('input');
                input.type = 'number';
                input.id = `activity${i}`;
                input.min = 0;
                input.max = 10;

                activityInputsContainer.appendChild(label);
                activityInputsContainer.appendChild(input);
            }
        }

        function calculateScore() {
            const numActivities = parseInt(numActivitiesDropdown.value);
            const pointsPerActivity = (10 / numActivities).toFixed(2);
            let totalScore = 0;

            resultsTable.innerHTML = '';

            for (let i = 1; i <= numActivities; i++) {
                const rawScore = parseFloat(document.getElementById(`activity${i}`).value) || 0;
                const weightedScore = (rawScore / 10 * pointsPerActivity).toFixed(2);

                totalScore += parseFloat(weightedScore);

                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>Activity ${i}</td>
                    <td>${rawScore}</td>
                    <td>${weightedScore}</td>
                `;
                resultsTable.appendChild(row);
            }

            totalScoreElement.textContent = totalScore.toFixed(2);
        }

        // Initialize with 1 activity
        generateActivityInputs();
    </script>
</body>
</html>
