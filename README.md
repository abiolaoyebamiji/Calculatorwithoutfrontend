# Calculatorwithoutfrontend
JavaScript Calculator without frontend

/**
 *  In main file
 *  let ZuriboardCalculator = require('./ZuriboardCalculator');
 *  console.log(ZuriboardCalculator.sum(1, 2));
 */
class MyCalculator
{
  constructor(previousOperandTextElement, currentOperandTextElement) {
    this.previousOperandTextElement = previousOperandTextElement
    this.currentOperandTextElement = currentOperandTextElement
    this.clear()
  }

  clear() {
    this.currentOperand = ''
    this.previousOperand = ''
    this.operation = undefined
  }

  delete() {
    this.currentOperand = this.currentOperand.toString().slice(0, -1)
  }

  appendNumber(number)
  {
    if (number === '.' && this.currentOperand.includes('.')) return
    this.currentOperand = this.currentOperand.toString() + number.toString()
  }

  chooseOperation(operation) 
  {
    if (this.currentOperand === '') return
    if (this.previousOperand !== '') 
    {
      this.compute()
    }
    this.operation = operation
    this.previousOperand = this.currentOperand
    this.currentOperand = ''
  }

  compute() 
  {
    let computation
    const prev = parseFloat(this.previousOperand)
    const current = parseFloat(this.currentOperand)
    if (isNaN(prev) || isNaN(current)) return
    switch (this.operation) 
    {
      case '+':
        computation = prev + current
        break
      case '-':
        computation = prev - current
        break
      case '*':
        computation = prev * current
        break
      case '÷':
        computation = prev / current
        break
      default:
        return
    }
    this.currentOperand = computation
    this.operation = undefined
    this.previousOperand = ''
  }

  getDisplayNumber(number) 
  {
    const stringNumber = number.toString()
    const integerDigits = parseFloat(stringNumber.split('.')[0])
    const decimalDigits = stringNumber.split('.')[1]
    let integerDisplay
    if (isNaN(integerDigits)) 
    {
      integerDisplay = ''
    } else {
      integerDisplay = integerDigits.toLocaleString('en', { maximumFractionDigits: 0 })
    }
    if (decimalDigits != null) 
    {
      return `${integerDisplay}.${decimalDigits}`
    } 
    else 
    {
      return integerDisplay
    }
  }

  updateDisplay() 
  {
    this.currentOperandTextElement.innerText =
      this.getDisplayNumber(this.currentOperand)
    if (this.operation != null) 
    {
      this.previousOperandTextElement.innerText =
        `${this.getDisplayNumber(this.previousOperand)} ${this.operation}`
    } 
    else 
    {
      this.previousOperandTextElement.innerText = ''
    }
  }
}

const numberButtons = document.querySelectorAll('[data-number]')
const operationButtons = document.querySelectorAll('[data-operation]')
const equalsButton = document.querySelector('[data-equals]')
const deleteButton = document.querySelector('[data-delete]')
const allClearButton = document.querySelector('[data-all-clear]')
const previousOperandTextElement = document.querySelector('[data-previous-operand]')
const currentOperandTextElement = document.querySelector('[data-current-operand]')

const mycalculator = new MyCalculator(previousOperandTextElement, currentOperandTextElement)

numberButtons.forEach(button => 
{
  button.addEventListener('click', () => 
  {
    mycalculator.appendNumber(button.innerText)
    mycalculator.updateDisplay()
  })
})

operationButtons.forEach(button => 
{
  button.addEventListener('click', () => 
  {
    mycalculator.chooseOperation(button.innerText)
    mycalculator.updateDisplay()
  })
})

equalsButton.addEventListener('click', button => 
{
  mycalculator.compute()
  mycalculator.updateDisplay()
})

allClearButton.addEventListener('click', button => 
{
  mycalculator.clear()
  mycalculator.updateDisplay()
})

deleteButton.addEventListener('click', button => {
  mycalculator.delete()
  mycalculator.updateDisplay()
})

document.addEventListener('keydown', function (event) 
{
  let patternForNumbers = /[0-9]/g;
  let patternForOperators = /[+\-*\/]/g
  if (event.key.match(patternForNumbers)) 
  {
    event.preventDefault();
    mycalculator.appendNumber(event.key)
    mycalculator.updateDisplay()
  }
  if (event.key === '.') {
    event.preventDefault();
    mycalculator.appendNumber(event.key)
    mycalculator.updateDisplay()
  }
  if (event.key.match(patternForOperators)) {
    event.preventDefault();
    mycalculator.chooseOperation(event.key)
    mycalculator.updateDisplay()
  }
  if (event.key === 'Enter' || event.key === '=') 
  {
    event.preventDefault();
    mycalculator.compute()
    mycalculator.updateDisplay()
  }
  if (event.key === "Backspace") 
  {
    event.preventDefault();
    mycalculator.delete()
    mycalculator.updateDisplay()
  }
  if (event.key == 'Delete') 
  {
    event.preventDefault();
    mycalculator.clear()
    mycalculator.updateDisplay()
  }
});
module.exports = { MyCalculator};
