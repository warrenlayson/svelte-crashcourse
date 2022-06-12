<script lang="ts">
  import { v4 as uuidv4 } from 'uuid';
  import { FeedbackStore } from '../stores';
  import Button from './Button.svelte';
  import Card from './Card.svelte';
  import RatingSelect from './RatingSelect.svelte';

  let text: string;
  let btnDisabled = true;
  let min = 10;
  let errorMessage: string | null = null;
  let rating = 10;

  const handleSubmit = () => {
    if (text.trim().length > min) {
      const newFeedback = {
        id: uuidv4(),
        text,
        rating,
      };
      FeedbackStore.update((state) => [...state, newFeedback]);

      text = '';
    }
  };

  const handleSelect = (e: CustomEvent<number>) => (rating = e.detail);

  const handleInput = () => {
    if (text.trim().length <= min) {
      errorMessage = `Text must be at least ${min} characters long`;
      btnDisabled = true;
    } else {
      btnDisabled = false;
      errorMessage = null;
    }
  };
</script>

<Card>
  <header>
    <h2>How would you rate your service with us?</h2>
  </header>

  <form on:submit|preventDefault={handleSubmit}>
    <RatingSelect on:rating-select={handleSelect} />
    <div class="input-group">
      <input
        type="text"
        placeholder="Tell us something that keeps you coming back"
        bind:value={text}
        on:input={handleInput}
      />
      <Button type="submit" disabled={btnDisabled}>Send</Button>
    </div>
    {#if errorMessage}
      <div class="message">
        {errorMessage}
      </div>
    {/if}
  </form>
</Card>

<style>
  header {
    max-width: 400px;
    margin: auto;
  }

  header h2 {
    font-size: 22px;
    font-weight: 600px;
    text-align: center;
  }

  .input-group {
    display: flex;
    flex-direction: row;
    border: 1px solid #ccc;
    padding: 8px 10px;
    border-radius: 8px;
    margin-top: 15px;
  }

  input {
    flex-grow: 2;
    border: none;
    font-size: 16px;
  }

  input:focus {
    outline: none;
  }

  .message {
    padding-top: 10px;
    text-align: center;
    color: rebeccapurple;
  }
</style>
