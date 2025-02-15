<script>
  export let question = "";
  export let answer = "";
  let flipped = false;

  function flipCard() {
    flipped = !flipped;
  }
</script>

<style>
  .flashcard {
    margin: 0.5rem auto;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #fff;
    cursor: pointer;
    transition: transform 0.6s;
    transform-style: preserve-3d;
    position: relative;
    width: 120%;
    max-width: 800px;
    height: 400px;
    padding: 2rem;
    left: 50%;
    transform: translateX(-50%);
  }

  .flashcard.flipped {
    transform: translateX(-50%) rotateY(180deg); /* Combine transformations */
  }

  .flashcard-content {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    display: flex;
    flex-direction: column;
    padding: 2rem;
    box-sizing: border-box;
    transform-style: preserve-3d; /* Add this */
  }

  .flashcard-front p,
  .flashcard-back p {
    margin: 0;
    padding: 1rem;
    width: 100%;
    flex: 1;
    text-align: center;
    box-sizing: border-box;
    font-size: 1.5rem;
    margin-bottom: 2rem;
    pointer-events: none; /* Disable interaction by default */
  }

  /* Enable scrolling only on visible side */
  .flashcard:not(.flipped) .flashcard-front p {
    pointer-events: auto;
    overflow-y: auto;
  }

  .flashcard.flipped .flashcard-back p {
    pointer-events: auto;
    overflow-y: auto;
  }

  /* Hide scrollbar on inactive side */
  .flashcard.flipped .flashcard-front p,
  .flashcard:not(.flipped) .flashcard-back p {
    overflow: hidden;
  }

  .flashcard-front,
  .flashcard-back {
    width: 100%;
    height: 100%; /* Added explicit height */
    position: absolute;
    backface-visibility: hidden;
    display: flex;
    flex-direction: column;
    pointer-events: none; /* Disable interaction by default */
  }

  /* Enable interaction only on visible side */
  .flashcard:not(.flipped) .flashcard-front,
  .flashcard.flipped .flashcard-back {
    pointer-events: auto;
  }

  .flashcard-back {
    transform: rotateY(180deg);
  }

  .flashcard-front p::-webkit-scrollbar,
  .flashcard-back p::-webkit-scrollbar {
    width: 6px;
  }

  .flashcard-front p::-webkit-scrollbar-track,
  .flashcard-back p::-webkit-scrollbar-track {
    background: transparent;
  }

  .flashcard-front p::-webkit-scrollbar-thumb,
  .flashcard-back p::-webkit-scrollbar-thumb {
    background-color: #ccc;
    border-radius: 3px;
  }

  .flip-indicator {
    position: absolute;
    bottom: 1rem;
    right: 1rem;
    font-size: 1rem;
    color: #888;
    pointer-events: none; /* Prevent indicator from interfering with scroll */
  }
</style>

<button type="button" class="flashcard {flipped ? 'flipped' : ''}" on:click={flipCard} aria-label="Flashcard">
  <div class="flashcard-content flashcard-front">
    <p>{question}</p>
    <div class="flip-indicator">Flip</div>
  </div>
  <div class="flashcard-content flashcard-back">
    <p>{answer}</p>
    <div class="flip-indicator">Flip</div>
  </div>
</button>