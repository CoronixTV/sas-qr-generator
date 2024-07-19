<script>
  import {
    Input,
    Label,
    Helper,
    Button,
    Checkbox,
    A,
    Modal,
  } from "flowbite-svelte";

  let downloadUrl = "";

  const handleDownloadUrlGenerated = (url = "") => {
    downloadUrl = url;
  };
  var reciever = "";
  var amount = "";
  var showModal = true;
  import QRCode from "@castlenine/svelte-qrcode";
</script>

<h1 class="mb-6">Bezahl-Code Generieren</h1>

<Input
  bind:value={reciever}
  placeholder="Empfänger"
  required
  class="mr-2 custom-height mb-6"
  autocomplete="one-time-code"
/>
<Input
  bind:value={amount}
  placeholder="Betrag"
  required
  class="mb-6 mr-2 custom-height"
  autocomplete="one-time-code"
/>

<Button
  class="mb-6"
  on:click={() => (showModal = true)}
  disabled={reciever == "" || amount == "" || isNaN(Number(amount))}
  >Generieren</Button
>
<Modal title="QR-Code" bind:open={showModal}>
  <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
    Mit dem Scannen des QR-Codes werden Empfänger und Betrag automatisch
    ausgefüllt.
  </p>
  <div class="container flex justify-center items-center">
    <QRCode
      errorCorrectionLevel="H"
      data={"k" + ":" + reciever + ";" + amount}
      downloadUrlFileFormat="png"
      dispatchDownloadUrl
      on:downloadUrlGenerated={(event) =>
        handleDownloadUrlGenerated(event.detail.url)}
    />
  </div>
  <div class="flex justify-center">
    <Button href={downloadUrl} download={reciever}>Download</Button>
  </div>
  <svelte:fragment slot="footer">
    <Button on:click={() => (showModal = false)}>Schließen</Button>
  </svelte:fragment>
</Modal>
