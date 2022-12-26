# Speed Test: `eval()` vs `Function()` constructors vs native

## HTML

```html
<table>
<thead><th>Script Type</th><th>Time 1</th><th>Time 2</th></thead>
<tr><td>Native Function</td><td class="native1 result"></td><td class="native2 result"></td></tr>
<tr><td>eval()</td><td class="eval1 result inactive"></td><td class="eval2 result"></td></tr>
<tr><td>Function Constructor</td><td class="function1 result inactive"></td><td class="function2 result"></td></tr>
</table>

<button type="button">Run Script</button>
<em>(N.B. script takes up to 15 seconds to run)</em>

<p></p>
```

## CSS

```css
button {
  display: inline-block;
  cursor: pointer;
}

table {
  margin: 12px 0;
  border-collapse: collapse;
}

th,
td {
  padding: 6px;
  border: 1px solid rgb(0, 0, 0);
}

td.result {
  cursor: pointer;
}

td.inactive {
  color: rgba(0, 0, 0, 0);
  background-color: rgb(127, 127, 127);
}

button {
  display: inline-block;
  margin-right: 6px;
}
```

## JavaScript

```js
let paragraph = document.querySelector('p');
let button = document.querySelector('button');

let native1 = document.querySelector('.native1');
let native2 = document.querySelector('.native2');
let eval1 = document.querySelector('.eval1');
let eval2 = document.querySelector('.eval2');
let function1 = document.querySelector('.function1');
let function2 = document.querySelector('.function2');

const runNative1Script = () => {

  let scriptDurations = [];
  
  for (let h = 0; h < 100; h++) {
    
    let scriptStart = window.performance.now();
    
    for (let i = 0; i < 10000; i++) {
      ((i) => paragraph.textContent = 'Loop iteration ' + (i + 1))();
    }
    
    let scriptEnd = window.performance.now();
    
    scriptDurations.push((scriptEnd - scriptStart));
  }
    
  return ((scriptDurations.reduce((a, b) => a + b, 0)) / 100);
}

const runNative2Script = () => {

  let scriptDurations = [];
  
  for (let h = 0; h < 100; h++) {

    let scriptStart = window.performance.now();

    const nativeFunction = (i) => paragraph.textContent = 'Loop iteration ' + (i + 1);
  
    for (let i = 0; i < 10000; i++) {
      nativeFunction(i);
    }

    let scriptEnd = window.performance.now();
    
    scriptDurations.push((scriptEnd - scriptStart));
  }
    
  return ((scriptDurations.reduce((a, b) => a + b, 0)) / 100);
}

const runEval1Script = () => {

  let scriptDurations = [];
  
  for (let h = 0; h < 10; h++) {

    let scriptStart = window.performance.now();

    for (let i = 0; i < 10000; i++) {
      eval("(i) => paragraph.textContent = 'Loop iteration ' + (i + 1)");
    }

    let scriptEnd = window.performance.now();
    
    scriptDurations.push((scriptEnd - scriptStart));
  }
  
  return ((scriptDurations.reduce((a, b) => a + b, 0)) / 10);
}

const runEval2Script = () => {

  let scriptDurations = [];
  
  for (let h = 0; h < 100; h++) {

    let scriptStart = window.performance.now();
  
    const evalFunction = eval("(i) => paragraph.textContent = 'Loop iteration ' + (i + 1)");

    for (let i = 0; i < 10000; i++) {
      evalFunction(i);
    }

    let scriptEnd = window.performance.now();
    
    scriptDurations.push((scriptEnd - scriptStart));
  }
  
  return ((scriptDurations.reduce((a, b) => a + b, 0)) / 100);
}

const runFunction1Script = () => {

  let scriptDurations = [];
  
  for (let h = 0; h < 10; h++) {

    let scriptStart = window.performance.now();

    for (let i = 0; i < 10000; i++) {
      Function("i", "paragraph.textContent = 'Loop iteration ' + (i + 1)")(i);
    }

    let scriptEnd = window.performance.now();
    
    scriptDurations.push((scriptEnd - scriptStart));
  }
  
  return ((scriptDurations.reduce((a, b) => a + b, 0)) / 10);
}

const runFunction2Script = () => {

  let scriptDurations = [];
  
  for (let h = 0; h < 100; h++) {

    let scriptStart = window.performance.now();
  
    const myFunction = Function("i", "paragraph.textContent = 'Loop iteration ' + (i + 1)");

    for (let i = 0; i < 10000; i++) {
      myFunction(i);
    }

    let scriptEnd = window.performance.now();
    
    scriptDurations.push((scriptEnd - scriptStart));
  }
  
  return ((scriptDurations.reduce((a, b) => a + b, 0)) / 100);
}

const runTimers = () => {

  let native1ScriptDuration = (native1.classList.contains('inactive')) ? null : runNative1Script() + 'ms';
  document.querySelector('.native1').textContent = native1ScriptDuration;
  
  let native2ScriptDuration = (native2.classList.contains('inactive')) ? null : runNative2Script() + 'ms';
  document.querySelector('.native2').textContent = native2ScriptDuration;
    
  let eval1ScriptDuration = (eval1.classList.contains('inactive')) ? null : runEval1Script() + 'ms';
  document.querySelector('.eval1').textContent = eval1ScriptDuration;
    
  let eval2ScriptDuration = (eval2.classList.contains('inactive')) ? null : runEval2Script() + 'ms';
  document.querySelector('.eval2').textContent = eval2ScriptDuration;
    
  let function1ScriptDuration = (function1.classList.contains('inactive')) ? null : runFunction1Script() + 'ms';
  document.querySelector('.function1').textContent = function1ScriptDuration;
    
  let function2ScriptDuration = (function2.classList.contains('inactive')) ? null : runFunction2Script() + 'ms';
  document.querySelector('.function2').textContent = function2ScriptDuration;
  
  button.textContent = 'Re-run Script';
}

const toggleActive = (e) => {
  e.target.classList.toggle('inactive');
}
  
button.addEventListener('click', runTimers, false);

[... document.querySelectorAll('.result')].forEach((tableCell) => tableCell.addEventListener('click', toggleActive, false));

```
