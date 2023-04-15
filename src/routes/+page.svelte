<script>
  import ChatHistory from "../components/ChatHistory.svelte";
  import InputForm from "../components/InputForm.svelte";

  let currentMessage = "";
  let history = [];

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

  const quasarifyMessage = async (message) => {
    const url = "https://us-central1-quasarify.cloudfunctions.net/api";
    const query = new URLSearchParams({ message }).toString();
    const response = await fetch(url + "?" + query);
    const qm = await response.text();
    return qm;
  };
</script>

<h1>Quasarify Your Message</h1>
<ChatHistory {history} />
<InputForm {sendMessage} {currentMessage} />

<style>
  h1 {
    font-size: 3rem;
    text-align: center;
  }
</style>
