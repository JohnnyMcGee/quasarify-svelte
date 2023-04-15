<script>
  import SendIcon from "../components/SendIcon.svelte";

  const sendMessage = async () => {
    if (currentMessage) {
      history = [...history, currentMessage];
      const m = currentMessage;
      currentMessage = "";
      const qm = await quasarifyMessage(m);
      typewriterEffect(qm);
    }
  };

  const typewriterEffect = async (message) => {
    let i = 0;
    let currentHistory = history;
    while (i < message.length) {
      i++;
      history = [...currentHistory, message.slice(0, i)];
      await new Promise((resolve) => setTimeout(resolve, 60));
    }
  };

  let currentMessage = "";

  let history = [];

  const quasarifyMessage = async (message) => {
    const url = "https://us-central1-quasarify.cloudfunctions.net/api";
    const query = new URLSearchParams({ message }).toString();
    const response = await fetch(url + "?" + query);
    const qm = await response.text();
    return qm;
  };
</script>

<h1>Quasarify Your Message</h1>
<div class="chat-history">
  {#each history as message, i}
    <p class="message {i % 2 !== 0 ? 'message--left' : ''}">{message}</p>
  {/each}
</div>
<form on:submit|preventDefault={sendMessage}>
  <input
    bind:value={currentMessage}
    type="text"
    placeholder="Enter your message"
    autofocus
  />
  <button type="submit"><SendIcon /></button>
</form>

<style>
  h1 {
    font-size: 3rem;
    text-align: center;
  }

  form {
    display: flex;
    position: fixed;
    bottom: 0;
    right: 0;
    width: 100%;
    box-sizing: border-box;
    padding: 36px;
    column-gap: 12px;
  }

  input {
    flex: 1;
    font-size: 1rem;
    padding: 16px;
    border: 2px solid var(--color-light-purple);
    border-radius: 100px;
    box-sizing: border-box;
    background-color: var(--color-midnight);
    color: white;
  }

  input::placeholder {
    color: var(--color-light-green);
  }

  button {
    font-size: 1.25rem;
    padding: 12px;
    border: 2px solid var(--color-light-purple);
    border-radius: 48px;
    box-sizing: border-box;
    background-color: var(--color-light-purple);
    color: white;
    transition: all 150ms ease;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  button:hover {
    background-color: #8d91fd;
  }
  button:active {
    background-color: #777ae9;
  }

  div.chat-history {
    display: flex;
    flex-direction: column;
    height: calc(100vh - 200px);
    overflow-y: scroll;
    padding: 36px;
    gap: 24px;
    margin-bottom: 48px;
  }

  .message {
    text-align: right;
    display: block;
    margin: 0;
  }

  .message.message--left {
    text-align: left;
    color: var(--color-light-green);
  }
</style>
