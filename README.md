# Speed Test: `eval()` vs `Function()` constructors vs native

## HTML

```html
<p></p>
<div class="checkboxes">
<label><input type="checkbox">Native</label>
<label><input type="checkbox">eval()</label>
<label><input type="checkbox">Function Constructor 1</label>
<label><input type="checkbox">Function Constructor 2</label>
</div>

<table>
<thead><th>Script Type</th><th>Time 1</th><th>Time 2</th></thead>
<tr><td>Native Function</td><td class="script native1"></td><td class="script native2"></td></tr>
<tr><td>eval()</td><td class="script eval1"></td><td class="script eval2"></td></tr>
<tr><td>Function Constructor 1</td><td class="script function1"></td><td class="script function2"></td></tr>
</table>

<button type="button">Run Script</button>
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

td.script {
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

const runNativeScript = () => {

  let nativeScriptStart = window.performance.now();

  for (let i = 0; i < 10000; i++) {
    paragraph.textContent = 'Loop iteration ' + (i + 1);
  }

  let nativeScriptEnd = window.performance.now();

  let nativeScriptDuration = (nativeScriptEnd - nativeScriptStart);
  
  return nativeScriptDuration;
}

const runEvalScript = () => {

  let evalScriptStart = window.performance.now();

  for (let i = 0; i < 10000; i++) {
    eval("paragraph.textContent = 'Loop iteration ' + (i + 1)");
  }

  let evalScriptEnd = window.performance.now();

  let evalScriptDuration = (evalScriptEnd - evalScriptStart);
  
  return evalScriptDuration;
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

  for (let i = 0; i < 12000; i++) {
    myFunction(i);
  }

  let function2ScriptEnd = window.performance.now();

  let function2ScriptDuration = (function2ScriptEnd - function2ScriptStart);
  
  return function2ScriptDuration;
}

const runTimers = () => {

  let nativeScriptDuration = runNativeScript();
  let evalScriptDuration = runEvalScript();
  let function1ScriptDuration = runFunction1Script();
  let function2ScriptDuration = runFunction2Script();
  
  document.querySelector('.native1').textContent = nativeScriptDuration + 'ms';
  document.querySelector('.eval1').textContent = evalScriptDuration + 'ms';
  document.querySelector('.function1').textContent = function1ScriptDuration + 'ms';
  document.querySelector('.function2').textContent = function2ScriptDuration + 'ms';
  
  button.textContent = 'Re-run Script';
}

const toggleActive = (e) => {
  e.target.classList.toggle('inactive');
}
  
button.addEventListener('click', runTimers, false);

[... document.querySelectorAll('.script')].forEach((tableCell) => tableCell.addEventListener('click', toggleActive, false));

```
