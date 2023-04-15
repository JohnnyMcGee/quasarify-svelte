<script>
    export let history = [];
  </script>
  
  <div class="chat-history">
    {#each history as message, i}
      <p class="message {i % 2 !== 0 ? 'message--left' : ''}">{message}</p>
    {/each}
  </div>
  
  <style>
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
  