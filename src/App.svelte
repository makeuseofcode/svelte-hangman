<script>
  import { hangmanArt } from "./hangmanArt";
  let words = [
    "appetizer",
    "roommates",
    "shrinking",
    "freedom",
    "happiness",
    "development",
  ];
  let userInput;
  let randomNum = Math.floor(Math.random() * (words.length - 1));
  let selectedWord = words[randomNum].toUpperCase();
  let initial = selectedWord;
  let match;
  let message;
  let hangmanStages = hangmanArt;
  let output = "";
  [...selectedWord].forEach(() => (output += "-"));
  match = output;


  function generateOutput(input1, input2) {
    output = "";
    for (let i = 0; i < input1.length; i++) {
      if (input2[i] === "-") {
        output += input1[i];
      } else {
        output += "-";
      }
    }
  }
  
  function evaluate() {
    let guess = userInput.toUpperCase().trim();
    if (!guess) {
      return;
    }
    if (selectedWord.includes(guess)) {
      selectedWord = selectedWord.replaceAll(guess, "-");
      generateOutput(initial, selectedWord);
    } else {
      hangmanStages.shift();
      hangmanStages = hangmanStages;
    }
    userInput = "";
  }
</script>

<main>
  <h1 class="title">
    Hangman
  </h1>
  <div class="tagline">
    I'm thinking of a word. Could you guess the letters in that word?
  </div>
  {#if hangmanStages.length > 0}
    <pre class="hangman">
    {hangmanStages[0]}</pre>
  {/if}
  {#if hangmanStages.length === 1}
    <div class="message" bind:this={message}>You Lose...</div>
  {/if}
  {#if selectedWord === match}
    <div class="message" bind:this={message}>You Win...</div>
  {/if}

  {#if !message}
    <div class="output">
      {#each output as letter}
        {#if letter !== "-"}
          <span class="complete box">{letter}</span>
        {:else}
          <span class="incomplete box" />
        {/if}
      {/each}
    </div>
    <form on:submit|preventDefault={() => evaluate()}>
      <input
        type="text"
        placeholder="Enter a letter"
        maxlength="1"
        bind:value={userInput}
      />
      <button type="submit">Submit</button>
    </form>
  {/if}
</main>

<style>
  * {
    color: green;
    text-align: center;
  }

  main {
    display: flex;
    width: 100%;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  input,
  button {
    text-transform: uppercase;
    background-color: transparent;
    border: solid 1.2px green;
    height:40px;
    font-size: 15px;
  }

  .box {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 45px;
    height: inherit;
    border: dotted 5.2px green;
  }

  .output {
    display: flex;
    font-size: 23px;
    font-weight: 600;
    height: 45px;
    gap: 10px;
    justify-content: center;
  }

  .hangman {
    font-size: 32px;
  }

  form {
    margin-top: 50px;
  }
  .tagline,
  .message {
    font-size: 20px;
  }
</style>
