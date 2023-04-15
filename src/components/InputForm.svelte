<script>
  import SendIcon from "./SendIcon.svelte";
  export let sendMessage;
  let currentMessage = "";

  const onSubmit = (e) => {
    e.preventDefault();
    sendMessage(currentMessage);
    currentMessage = "";
  };
</script>

<form on:submit|preventDefault={onSubmit}>
  <input
    bind:value={currentMessage}
    type="text"
    placeholder="Enter your message"
    autofocus
  />
  <button type="submit"><SendIcon /></button>
</form>

<style>
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
</style>
