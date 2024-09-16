const fs = require('fs');

// Function to decode the value from a specific base to decimal function decodeValue(base, value) { return parseInt(value, base); }

// Function to perform Gaussian Elimination function gaussianElimination(A, B) { const n = B.length; for (let i = 0; i < n; i++) { let maxRow = i; for (let k = i + 1; k < n; k++) { if (Math.abs(A[k][i]) > Math.abs(A[maxRow][i])) { maxRow = k; } }

    // Swap rows
    [A[i], A[maxRow]] = [A[maxRow], A[i]];
    [B[i], B[maxRow]] = [B[maxRow], B[i]];

    for (let k = i + 1; k < n; k++) {
        const factor = A[k][i] / A[i][i];
        for (let j = i; j < n; j++) {
            A[k][j] -= factor * A[i][j];
        }
        B[k] -= factor * B[i];
    }
}

const X = new Array(n).fill(0);
for (let i = n - 1; i >= 0; i--) {
    X[i] = B[i] / A[i][i];
    for (let k = i - 1; k >= 0; k--) {
        B[k] -= A[k][i] * X[i];
    }
}

return X;
}

// Function to find the constant term from the test case function findConstantTerm(testCase) { const n = testCase.keys.n; const k = testCase.keys.k;

let roots = [];
for (let key in testCase) {
    if (key !== "keys") {
        const base = parseInt(testCase[key].base);
        const value = testCase[key].value;
        const decodedValue = decodeValue(base, value);
        roots.push([parseInt(key), decodedValue]);
    }
}

// Sort roots by x value and use the first k roots
roots.sort((a, b) => a[0] - b[0]);
roots = roots.slice(0, k);

// Create matrix A and vector B
let A = [];
let B = [];

for (let i = 0; i < k; i++) {
    let row = [];
    for (let j = k - 1; j >= 0; j--) {
        row.push(Math.pow(roots[i][0], j));
    }
    A.push(row);
    B.push(roots[i][1]);
}

// Solve the system of linear equations
const coefficients = gaussianElimination(A, B);

// The constant term is the last coefficient
return coefficients[coefficients.length - 1];
}

// Read the input JSON file fs.readFile('input.json', 'utf8', (err, data) => { if (err) { console.error('Error reading the file:', err); return; }

try {
    const testCase = JSON.parse(data);
    const constantTerm = findConstantTerm(testCase);
    console.log(`Constant term c: ${constantTerm}`);
} catch (e) {
    console.error('Error parsing JSON:', e);
}
});

About
No description, website, or topics provided.
Resources
 Readme
 Activity
Stars
 0 stars
Watchers
 1 watching
Forks
 0 forks
Report repository
Releases
No releases published
Packages
No packages published
Footer
