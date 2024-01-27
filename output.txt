const fs = require('fs');

function analyzeConsecutiveDays(data, consecutiveDays = 7) {
  const employeeShifts = {};

  data.forEach(entry => {
    const name = entry["Employee Name"];
    const date = new Date(entry["Time In"]);

    if (!employeeShifts[name]) {
      employeeShifts[name] = [];
    }

    employeeShifts[name].push(date);
  });

  for (const [name, shifts] of Object.entries(employeeShifts)) {
    for (let i = 0; i < shifts.length - consecutiveDays + 1; i++) {
      const consecutiveShifts = shifts.slice(i, i + consecutiveDays);
      const isConsecutive = consecutiveShifts.every((shift, index) =>
        index === 0 || (shift - consecutiveShifts[index - 1]) / (1000 * 60 * 60 * 24) === 1
      );

      if (isConsecutive) {
        console.log(`${name} has worked for ${consecutiveDays} consecutive days.`);
        break; // No need to check further for this employee
      }
    }
  }
}

// Read JSON file
const filePath = 'path/to/employeeData.json'; // Provide the correct file path
const rawData = fs.readFileSync(filePath);
const employeeData = JSON.parse(rawData);

// Example usage
analyzeConsecutiveDays(employeeData);
