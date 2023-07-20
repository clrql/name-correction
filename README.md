# Name Correction
Contents:
- `levenshteinDistance`: A function that calculates the Levenshtein Distance between two strings using dynamic programming.
- `findMostSimilarName`: A function that takes an incorrect name and a list of names, and returns the most similar name from the list based on the Levenshtein Distance.
- An example of how to use the functions with sample names and output.

Levenshtein Distance implementation usign a dynammic programming approach.
```javascript
function levenshteinDistance(str1, str2) {
  const m = str1.length;
  const n = str2.length;
  const dp = [];

  for (let i = 0; i <= m; i++) {
    dp[i] = [];
    for (let j = 0; j <= n; j++) {
      // Base cases: If one of the strings is empty, the distance is the length of the other string.
      if (i === 0) {
        dp[i][j] = j;
      } else if (j === 0) {
        dp[i][j] = i;
      } else {
        // Calculate the minimum distance for the current position based on three possible operations:
        // 1. Replace: If the characters at the current positions are different, add 1 to the previous diagonal value.
        // 2. Insert: Add 1 to the value on the previous row at the same column.
        // 3. Delete: Add 1 to the value on the same row at the previous column.
        dp[i][j] = Math.min(
          dp[i - 1][j - 1] + (str1[i - 1] === str2[j - 1] ? 0 : 1),
          dp[i - 1][j] + 1,
          dp[i][j - 1] + 1
        );
      }
    }
  }
  return dp[m][n]; // The last cell of the matrix holds the Levenshtein distance.
}
```

Function to look for the most similar name.
```javascript
// Function to find the most similar name from a list of names to the given incorrect name.
function findMostSimilarName(incorrectName, nameList) {
  const incorrectNameComponents = incorrectName.split(/\s+/);
  let bestMatch = null;
  let minDistance = Infinity;

  for (const fullName of nameList) {
    const fullNameComponents = fullName.split(/\s+/);
    let totalDistance = 0;
    for (const incorrectComponent of incorrectNameComponents) {
      let minComponentDistance = Infinity;
      for (const nameComponent of fullNameComponents) {
        // Calculate the Levenshtein distance between each component of the incorrect name and each component of the full name.
        const distance = levenshteinDistance(
          incorrectComponent.toLowerCase(),
          nameComponent.toLowerCase()
        );

        if (distance < minComponentDistance) {
          minComponentDistance = distance;
        }
      }
      totalDistance += minComponentDistance;
    }
    // Update the best match if the current full name has a smaller total distance than the previous best match.
    if (totalDistance < minDistance) {
      minDistance = totalDistance;
      bestMatch = fullName;
    }
  }
  return bestMatch; // Return the most similar name found.
}
```

An example of use:
```javascript
const names = [
  "Juan Pérez",
  "María González",
  "Pedro López",
  "Luisa Martínez",
  "Ana Sánchez",
  "Roberto Gómez",
];
const incorrectName = "Hernandez Velazquez Gabriel";
const mostSimilarName = findMostSimilarName(incorrectName, names);
console.log(
  `The most similar name to '${incorrectName}' is: '${mostSimilarName}'.`
);
```
