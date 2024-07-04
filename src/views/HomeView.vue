<template>
  <div id="app">
    <div class="container">
      <h2>Encryption/Decryption Demo</h2>
      <div class="input-group">
        <label for="message">Message:</label>
        <input v-model="state.message" id="message" placeholder="Enter message here" autocomplete="off">
      </div>
      <div class="input-group">
        <label for="password">Password:</label>
        <input v-model="state.password" type="text" id="password" placeholder="Enter password here" autocomplete="off">
      </div>
      <div class="button-group">
        <button @click="encryptMessage">Encrypt</button>
        <button @click="decryptMessage">Decrypt</button>
      </div>
      <div class="output-group">
        <h3>Encrypted Message:</h3>
        <p>{{ state.encryptedMessage }}</p>
      </div>
      <div class="output-group">
        <h3>Decrypted Message:</h3>
        <p>{{ state.decryptedMessage }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from 'vue';
import CryptoJS from 'crypto-js';

export default {
  setup() {
    const state = reactive({
      message: '',
      password: '',
      encryptedMessage: '',
      decryptedMessage: ''
    });

    const encryptMessage = () => {
      if (state.message !== '') {
        state.encryptedMessage = CryptoJS.AES.encrypt(state.message, state.password).toString();
        state.message = '';
        state.password = '';
        state.decryptedMessage = '';
      } else {
        alert('Messagenya diisi dulu ya');
      }
    };

    const decryptMessage = () => {
      try {
        const bytes = CryptoJS.AES.decrypt(state.encryptedMessage, state.password);
        const decrypted = bytes.toString(CryptoJS.enc.Utf8);

        if (decrypted) {
          state.decryptedMessage = decrypted;
        } else {
          state.decryptedMessage = 'Password Salah';
        }
      } catch (error) {
        state.decryptedMessage = 'Password Salah';
      }
    };

    return {
      state,
      encryptMessage,
      decryptMessage
    };
  }
};
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  background-color: #f4f4f9;
  padding: 20px;
}

.container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #333;
  margin-bottom: 20px;
}

.input-group,
.button-group,
.output-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
}

input {
  width: calc(100% - 20px);
  padding: 10px;
  margin: 0 auto;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 10px 20px;
  margin: 5px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0056b3;
}

p {
  font-size: 1.2em;
  word-break: break-word;
  background: #e9ecef;
  padding: 10px;
  border-radius: 5px;
  margin: 10px 0;
}
</style>