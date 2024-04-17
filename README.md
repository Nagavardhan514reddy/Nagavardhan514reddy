<form id="tax-form">
  <select id="age">
    <option value="<40">&lt; 40</option>
    <option value=">=40&<60">&ge; 40 &lt; 60</option>
    <option value=">=60">&ge; 60</option>
  </select>
  <input type="number" id="income" placeholder="Enter income">
  <input type="number" id="deductions" placeholder="Enter deductions">
  <button type="submit">Submit</button>
</form>

<div id="error-icons">
  <div class="error-icon" id="age-error-icon" style="display: none;">!</div>
  <div class="error-icon" id="income-error-icon" style="display: none;">!</div>
  <div class="error-icon" id="deductions-error-icon" style="display: none;">!</div>
</div>

<div id="result-modal" style="display: none;">
  <div id="result"></div>
</div>
.error-icon {
  display: inline-block;
  margin-left: 5px;
  color: red;
  cursor: pointer;
}

.tooltip {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 5px;
  border-radius: 3px;
  display: none;
}
$(document).ready(function() {
  $('#tax-form').submit(function(event) {
    event.preventDefault();
    
    // Reset error icons
    $('.error-icon').hide();
    
    // Validate age
    var age = $('#age').val();
    if (!age) {
      $('#age-error-icon').show();
      return;
    }

    // Validate income
    var income = parseFloat($('#income').val());
    if (isNaN(income) || income < 0) {
      $('#income-error-icon').show();
      return;
    }

    // Validate deductions
    var deductions = parseFloat($('#deductions').val());
    if (isNaN(deductions) || deductions < 0) {
      $('#deductions-error-icon').show();
      return;
    }

    // Calculate tax
    var tax = calculateTax(age, income, deductions);

    // Show result modal
    $('#result').text('Tax: ' + tax + ' Lakhs');
    $('#result-modal').show();
  });
});

function calculateTax(age, income, deductions) {
  var grossIncome = income + deductions;
  var taxFreeLimit = 800000; // 8 Lakhs
  var taxableIncome = Math.max(0, grossIncome - taxFreeLimit);

  if (grossIncome <= taxFreeLimit) {
    return 0;
  }

  var taxRate;
  if (age === '<40') {
    taxRate = 0.3;
  } else if (age === '>=40&<60') {
    taxRate = 0.4;
  } else {
    taxRate = 0.1;
  }

  var taxAmount = taxRate * taxableIncome;
  return taxAmount.toFixed(2);
}

