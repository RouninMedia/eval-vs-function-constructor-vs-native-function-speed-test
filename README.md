# Speed Test: `eval()` vs `Function()` constructors vs native

## HTML

```html
<table>
<thead><th>Script Type</th><th>Time 1</th><th>Time 2</th></thead>
<tr><td>Native Function</td><td class="native1 result"></td><td class="native2 result"></td></tr>
<tr><td>eval()</td><td class="eval1 result"></td><td class="eval2 result"></td></tr>
<tr><td>Function Constructor 1</td><td class="function1 result"></td><td class="function2 result"></td></tr>
</table>

<button type="button">Run Script</button>

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
```

## JavaScript

```js
let paragraph = document.querySelector('p');
let button = document.querySelector('button');

const runNative1Script = () => {

  let nativeScript1Start = window.performance.now();

  for (let i = 0; i < 10000; i++) {
    (i) => paragraph.textContent = 'Loop iteration ' + (i + 1);
  }

  let nativeScript1End = window.performance.now();

  let nativeScript1Duration = (nativeScript1End - nativeScript1Start);
  
  return nativeScript1Duration;
}

const runNative2Script = () => {

  let nativeScript2Start = window.performance.now();

  const nativeFunction = (i) => paragraph.textContent = 'Loop iteration ' + (i + 1);
  
  for (let i = 0; i < 10000; i++) {
    nativeFunction(i);
  }

  let nativeScript2End = window.performance.now();

  let nativeScript2Duration = (nativeScript2End - nativeScript2Start);
  
  return nativeScript2Duration;
}

const runEval1Script = () => {

  let evalScript1Start = window.performance.now();

  for (let i = 0; i < 10000; i++) {
    eval("(i) => paragraph.textContent = 'Loop iteration ' + (i + 1)");
  }

  let evalScript1End = window.performance.now();

  let evalScript1Duration = (evalScript1End - evalScript1Start);
  
  return evalScript1Duration;
}

const runEval2Script = () => {

  let evalScript2Start = window.performance.now();
  
  const evalFunction = eval("(i) => paragraph.textContent = 'Loop iteration ' + (i + 1)");

  for (let i = 0; i < 10000; i++) {
    evalFunction(i);
  }

  let evalScript2End = window.performance.now();

  let evalScript2Duration = (evalScript2End - evalScript2Start);
  
  return evalScript2Duration;
}

const runFunction1Script = () => {

  let function1ScriptStart = window.performance.now();

  for (let i = 0; i < 10000; i++) {
    Function("i", "paragraph.textContent = 'Loop iteration ' + (i + 1)")(i);
  }

  let function1ScriptEnd = window.performance.now();

  let function1ScriptDuration = (function1ScriptEnd - function1ScriptStart);
  
  return function1ScriptDuration;
}

const runFunction2Script = () => {

  let function2ScriptStart = window.performance.now();
  
  const myFunction = Function("i", "paragraph.textContent = 'Loop iteration ' + (i + 1)");

  for (let i = 0; i < 10000; i++) {
    myFunction(i);
  }

  let function2ScriptEnd = window.performance.now();

  let function2ScriptDuration = (function2ScriptEnd - function2ScriptStart);
  
  return function2ScriptDuration;
}

const runTimers = () => {

  let native1ScriptDuration = (document.querySelector('.native1').classList.contains('inactive')) ? null : runNative1Script();
  let native2ScriptDuration = runNative2Script();
  let eval1ScriptDuration = runEval1Script();
  let eval2ScriptDuration = runEval2Script();
  let function1ScriptDuration = runFunction1Script();
  let function2ScriptDuration = runFunction2Script();
  
  document.querySelector('.native1').textContent = native1ScriptDuration + 'ms';  document.querySelector('.native2').textContent = native2ScriptDuration + 'ms';
  document.querySelector('.eval1').textContent = eval1ScriptDuration + 'ms';
  document.querySelector('.eval2').textContent = eval2ScriptDuration + 'ms';
  document.querySelector('.function1').textContent = function1ScriptDuration + 'ms';
  document.querySelector('.function2').textContent = function2ScriptDuration + 'ms';
  
  button.textContent = 'Re-run Script';
}

const toggleActive = (e) => {
  e.target.classList.toggle('inactive');
}
  
button.addEventListener('click', runTimers, false);

[... document.querySelectorAll('.result')].forEach((tableCell) => tableCell.addEventListener('click', toggleActive, false));

```
