// script.js

function checkCardValidity() {
  const cardNumber = document.getElementById('card-number').value.trim();
  const resultMessage = document.getElementById('result-message');

  // Check if the card number is empty or contains non-numeric characters
  if (!cardNumber || !/^\d+$/.test(cardNumber)) {
    resultMessage.textContent = 'Please enter a valid card number.';
    resultMessage.className = 'invalid';
    return;
  }

  // Luhn Algorithm to check card number validity
  if (!isValidLuhn(cardNumber)) {
    resultMessage.textContent = 'Invalid card number.';
    resultMessage.className = 'invalid';
    return;
  }

  // Check card type based on the first few digits
  const cardType = getCardType(cardNumber);

  if (cardType) {
    resultMessage.textContent = `Valid ${cardType} card.`;
    resultMessage.className = 'valid';
  } else {
    resultMessage.textContent = 'Unknown card type.';
    resultMessage.className = 'invalid';
  }
}

// Luhn Algorithm to validate card number
function isValidLuhn(cardNumber) {
  let sum = 0;
  let shouldDouble = false;

  // Iterate through the digits in reverse order
  for (let i = cardNumber.length - 1; i >= 0; i--) {
    let digit = parseInt(cardNumber.charAt(i), 10);

    if (shouldDouble) {
      digit *= 2;
      if (digit > 9) {
        digit -= 9;
      }
    }

    sum += digit;
    shouldDouble = !shouldDouble;
  }

  return (sum % 10 === 0);
}

// Get card type based on the starting digits
function getCardType(cardNumber) {
  const visaRegex = /^4/;
  const mastercardRegex = /^5[1-5]/;
  const amexRegex = /^3[47]/;
  const discoverRegex = /^6/;

  if (visaRegex.test(cardNumber)) {
    return 'Visa';
  } else if (mastercardRegex.test(cardNumber)) {
    return 'MasterCard';
  } else if (amexRegex.test(cardNumber)) {
    return 'American Express';
  } else if (discoverRegex.test(cardNumber)) {
    return 'Discover';
  } else {
    return null;
  }
}
