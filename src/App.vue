<template>
  <div id="app">
    <h1>RSA АЛГОРИТМ</h1>
    <div class="main">
      <div class="encrypt">
        <input
          v-model="m"
          @input="inputMessage"
          type="text"
          placeholder="Введите текст"
        />
        <input v-model="e" @input="eKeys" type="text" placeholder="e" />
        <input v-model="n" @input="nKeys" type="text" placeholder="n" />
        <input
          v-model="encryptedMessage"
          type="text"
          placeholder="Зашифрованный текст"
          readonly
        />
      </div>

      <div class="decrypt">
        <input
          v-model="m1"
          @input="destMessage"
          type="text"
          placeholder="Введите шифрованный текст"
        />
        <input v-model="d" @input="dKeys" type="text" placeholder="d" />
        <input v-model="n" @input="nKeys" type="text" placeholder="n" />
        <input
          :value="decryptedMessage"
          type="text"
          placeholder="Расшифрованный текст"
          readonly
        />
      </div>
    </div>
  </div>
</template>

<script>
import { gcd } from 'mathjs';

const SimpleNumbers = [
  2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71,
  73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151,
  157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233,
  239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293,
];

export default {
  data() {
    return {
      m: '',
      e: '',
      n: '',
      m1: '',
      d: '',
      encryptedMessage: '',
      decryptedMessage: '',
    };
  },

  created() {
    this.initializeKeys();
  },

  methods: {
    initializeKeys() {
      let p = SimpleNumbers[Math.floor(Math.random() * SimpleNumbers.length)];
      let q = SimpleNumbers[Math.floor(Math.random() * SimpleNumbers.length)];

      this.n = p * q;
      let Fn = (p - 1) * (q - 1);

      this.e = this.eFunc(Fn);
      this.d = this.modInverse(this.e, Fn);
    },

    eFunc(Fn) {
      while (true) {
        let y = SimpleNumbers[Math.floor(Math.random() * SimpleNumbers.length)];
        let x = gcd(y, Fn);
        if (x == 1) {
          return y;
        }
      }
    },

    modInverse(a, m) {
      for (let i = 0; i < m; i++) {
        if ((a * i) % m === 1) {
          return i;
        }
      }
      return 1;
    },

    inputMessage() {
      this.updateEncryptedMessage();
    },

    destMessage() {
      this.updateDescryptedMessage();
    },

    updateEncryptedMessage() {
      let result1 = '';
      for (let i = 0; i < this.m.length; i++) {
        let x = BigInt(this.m.charCodeAt(i));
        let result = x ** BigInt(this.e) % BigInt(this.n);
        result1 += result.toString() + ' ';
      }
      this.encryptedMessage = result1;
    },

    updateDescryptedMessage() {
      let result2 = '';
      let res = this.encryptedMessage.split(' ');
      for (let i = 0; i < res.length; i++) {
        let result = BigInt(res[i]) ** BigInt(this.d) % BigInt(this.n);
        result2 += String.fromCharCode(Number(result));
      }
      this.decryptedMessage = result2;
    },
  },
};
</script>

<style scoped>
.main {
  /* border: 1px solid red; */
  height: 35rem;
  box-shadow: none !important;
  display: flex;
  align-items: center;
  justify-content: center;
}

.encrypt {
  display: flex;
  flex-direction: column;
  width: 50%;
  margin-right: 1rem;
}

.encrypt input {
  margin: 10px 0;
}

.decrypt {
  margin-right: -1rem;
  display: flex;
  flex-direction: column;
  width: 50%;
}

.decrypt input {
  margin: 10px 0;
}
</style>
