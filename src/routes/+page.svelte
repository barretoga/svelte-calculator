<script lang="ts">
import { onMount } from "svelte";

let display: string = '';
let result: number | string = 0;
let funMode: boolean = false;
let focusedButtonIndex: number = -1;

let buttonPositions = Array(16).fill({ top: '0px', left: '0px' });

function appendToDisplay(value: string) {
  display += value;
}

function clearDisplay() {
  display = '';
  result = 0;
}

function toggleFunMode() {
  funMode = !funMode;
}

function calculate() {
  try {
    result = Number(eval(display));
    if (isNaN(result)) {
      throw new Error('Resultado não é um número');
    }
  } catch (e) {
    result = 'Erro';
  }
}

function moveButton(index: number) {
  if (funMode && index !== focusedButtonIndex) {
    focusedButtonIndex = index;
    const maxTop = 200;
    const maxLeft = 200;

    const newTop = Math.floor(Math.random() * maxTop) + 'px';
    const newLeft = Math.floor(Math.random() * maxLeft) + 'px';

    buttonPositions = buttonPositions.map((pos, i) =>
      i === index ? { top: newTop, left: newLeft } : pos
    );
  }
}

function clearFocusedIndex() {
  focusedButtonIndex = -1;
}

onMount(() => {
  buttonPositions = [
    { top: '0px', left: '0px' },
    { top: '0px', left: '80px' },
    { top: '0px', left: '160px' },
    { top: '0px', left: '240px' },
    { top: '80px', left: '0px' },
    { top: '80px', left: '80px' },
    { top: '80px', left: '160px' },
    { top: '80px', left: '240px' },
    { top: '160px', left: '0px' },
    { top: '160px', left: '80px' },
    { top: '160px', left: '160px' },
    { top: '160px', left: '240px' },
    { top: '240px', left: '0px' },
    { top: '240px', left: '80px' },
    { top: '240px', left: '160px' },
    { top: '240px', left: '240px' }
  ];
});
</script>

<style>
.container {
  font-family: 'Roboto', sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.calculator {
  width: 300px;
  height: 450px;
  margin: 2rem auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  position: relative;
  background-color: #FEFAE0;
}

.display {
  font-size: 2rem;
  padding: 0.5rem;
  background: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 1rem;
  text-align: right;
  position: relative;
  z-index: 1;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.buttons {
  position: relative;
  height: 240px;
}

.calculator button {
  font-size: 1.8rem;
  width: 60px;
  height: 60px;
  padding: 1rem;
  cursor: pointer;
  position: absolute;
  transition: top 0.3s, left 0.3s, transform 0.3s;
  border: none;
  background-color: #f0f0f0;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.calculator button:hover {
  transform: scale(1.1);
}

.calculator button.number {
  background-color: #F9D689;
  color: white;
}

.calculator button.operator {
  background-color: #E0A75E;
  color: white;
}

.calculator button.clear {
  background-color: #973131;
  color: white;
}

.calculator button.equal {
  background-color: #973131;
  color: white;
}

.fun-mode-toggle {
  background-color: #973131;
  color: white;
  font-size: 1.5rem;
  width: 300px;
  padding: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
  transition: top 0.3s, left 0.3s, transform 0.3s;
  border: 1px solid #ccc;
  border-radius: 8px;
  margin-top: 1rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.fun-mode-toggle:hover {
  transform: scale(1.1);
  background-color: #E0A75E;
}
</style>

<div class="container">
  <div class="calculator">
    <div class="display">{display || '0'}</div>
    <div class="display">{result}</div>
    <div class="buttons">
      {#each ['1', '2', '3', '+', '4', '5', '6', '-', '7', '8', '9', '*', 'C', '0', '=', '/'] as btn, i}
        <button
          class="{btn === 'C' ? 'clear' : btn === '=' ? 'equal' : isNaN(parseInt(btn)) ? 'operator' : 'number'}"
          on:click={() => btn === 'C' ? clearDisplay() : btn === '=' ? calculate() : appendToDisplay(btn)}
          on:mouseover={() => moveButton(i)}
          on:focus={() => moveButton(i)}
          on:blur={clearFocusedIndex}
          style="top: {buttonPositions[i].top}; left: {buttonPositions[i].left};"
        >
          {btn}
        </button>
      {/each}
    </div>
  </div>
  <button class="fun-mode-toggle" on:click={toggleFunMode}>
    {funMode ? 'Desativar inovação' : 'Ativar inovação'}
  </button>
</div>

