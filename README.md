const express = require('express');
const app = express();

// History array to store the last 20 operations
let history = [];

// GET request for the root endpoint
app.get('/', (req, res) => {
  res.send('Welcome to the Math Operations API!');
});

// GET request for the /history endpoint
app.get('/history', (req, res) => {
  res.json(history);
});

// GET request for mathematical operations
app.get('/:num1/:operator/:num2/:operator2?/:num3?', (req, res) => {
  let result;
  
  const { num1, operator, num2, operator2, num3 } = req.params;

  if (operator === 'plus') {
    result = parseInt(num1) + parseInt(num2);
  } else if (operator === 'minus') {
    result = parseInt(num1) - parseInt(num2);
  } else if (operator === 'into') {
    result = parseInt(num1) * parseInt(num2);
  } else if (operator === 'by') {
    result = parseInt(num1) / parseInt(num2);
  }

  if (operator2) {
    if (operator2 === 'plus') {
      result += parseInt(num3);
    } else if (operator2 === 'minus') {
      result -= parseInt(num3);
    } else if (operator2 === 'into') {
      result *= parseInt(num3);
    } else if (operator2 === 'by') {
      result /= parseInt(num3);
    }
  }

  const question = `${num1}${operator}${num2}${operator2 ? num3 : ''}`;
  const answer = result;

  const operation = {
    question,
    answer
  };

  // Add the operation to history
  history.push(operation);

  // Keep only the last 20 operations in the history array
  if (history.length > 20) {
    history.shift();
  }

  res.json(operation);
});

// Start the server
app.listen(3000, () => {
  console.log('Server started on port 3000.');
});
