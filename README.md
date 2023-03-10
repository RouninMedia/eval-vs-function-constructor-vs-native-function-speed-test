# Speed Test: `eval()` vs `Function()` constructors vs native

## HTML

```html
<table>
<thead><th>Script Type</th><th>Not stored</th><th>Stored in variable</th></thead>
<tr><td>Native Function</td><td class="nativeNotStored result"></td><td class="nativeStored result"></td></tr>
<tr><td>eval()</td><td class="evalNotStored result inactive"></td><td class="evalStored result"></td></tr>
<tr><td>Function Constructor</td><td class="functionNotStored result inactive"></td><td class="functionStored result"></td></tr>
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

let nativeNotStored = document.querySelector('.nativeNotStored');
let nativeStored = document.querySelector('.nativeStored');
let evalNotStored = document.querySelector('.evalNotStored');
let evalStored = document.querySelector('.evalStored');
let functionNotStored = document.querySelector('.functionNotStored');
let functionStored = document.querySelector('.functionStored');

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

  let nativeNotStoredScriptDuration = (nativeNotStored.classList.contains('inactive')) ? null : runNativeNotStoredScript() + 'ms';
  document.querySelector('.nativeNotStored').textContent = nativeNotStoredScriptDuration;
  
  let nativeStoredScriptDuration = (nativeStored.classList.contains('inactive')) ? null : runNativeStoredScript() + 'ms';
  document.querySelector('.nativeStored').textContent = nativeStoredScriptDuration;
    
  let evalNotStoredScriptDuration = (evalNotStored.classList.contains('inactive')) ? null : runEvalNotStoredScript() + 'ms';
  document.querySelector('.evalNotStored').textContent = evalNotStoredScriptDuration;
    
  let evalStoredScriptDuration = (evalStored.classList.contains('inactive')) ? null : runEvalStoredScript() + 'ms';
  document.querySelector('.evalStored').textContent = evalStoredScriptDuration;
    
  let functionNotStoredScriptDuration = (functionNotStored.classList.contains('inactive')) ? null : runFunctionNotStoredScript() + 'ms';
  document.querySelector('.functionNotStored').textContent = functionNotStoredScriptDuration;
    
  let functionStoredScriptDuration = (functionStored.classList.contains('inactive')) ? null : runFunctionStoredScript() + 'ms';
  document.querySelector('.functionStored').textContent = functionStoredScriptDuration;
  
  button.textContent = 'Re-run Script';
}

const toggleActive = (e) => {
  e.target.classList.toggle('inactive');
}
  
button.addEventListener('click', runTimers, false);

[... document.querySelectorAll('.result')].forEach((tableCell) => tableCell.addEventListener('click', toggleActive, false));
```
