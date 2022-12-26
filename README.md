# Speed Test: `eval()` vs `Function()` constructors vs native

## HTML

```html
<table>
<thead><th>Script Type</th><th>Not stored</th><th>Stored in variable</th></thead>
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

const runNativeNotStoredScript = () => {

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

const runNativeStoredScript = () => {

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

const runEvalNotStoredScript = () => {

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

const runEvalStoredScript = () => {

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

const runFunctionNotStoredScript = () => {

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

const runFunctionStoredScript = () => {

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

  let nativeNotStoredScriptDuration = (native1.classList.contains('inactive')) ? null : runNativeNotStoredScript() + 'ms';
  document.querySelector('.native1').textContent = nativeNotStoredScriptDuration;
  
  let nativeStoredScriptDuration = (native2.classList.contains('inactive')) ? null : runNativeStoredScript() + 'ms';
  document.querySelector('.native2').textContent = nativeStoredScriptDuration;
    
  let evalNotStoredScriptDuration = (eval1.classList.contains('inactive')) ? null : runEvalNotStoredScript() + 'ms';
  document.querySelector('.eval1').textContent = evalNotStoredScriptDuration;
    
  let evalStoredScriptDuration = (eval2.classList.contains('inactive')) ? null : runEvalStoredScript() + 'ms';
  document.querySelector('.eval2').textContent = evalStoredScriptDuration;
    
  let functionNotStoredScriptDuration = (function1.classList.contains('inactive')) ? null : runFunctionNotStoredScript() + 'ms';
  document.querySelector('.function1').textContent = functionNotStoredScriptDuration;
    
  let functionStoredScriptDuration = (function2.classList.contains('inactive')) ? null : runFunctionStoredScript() + 'ms';
  document.querySelector('.function2').textContent = functionStoredScriptDuration;
  
  button.textContent = 'Re-run Script';
}

const toggleActive = (e) => {
  e.target.classList.toggle('inactive');
}
  
button.addEventListener('click', runTimers, false);

[... document.querySelectorAll('.result')].forEach((tableCell) => tableCell.addEventListener('click', toggleActive, false));
```
