<template>
  <section>
    <h4 class="font-normal mt-3 md:mt-12">Client-side Encryption</h4>
    <hr>
    <div class="mb-4">
      <label for="tags-search">Documents are encrypted using a <a href="https://en.wikipedia.org/wiki/Hybrid_cryptosystem" target="_blank" rel="noopener noreferrer">hybrid cryptosystem</a>. You may provide your own keys or generate a new set here. If you choose to generate a new set, be sure to <strong>make a secure backup of your keys</strong>. If you lose your private key, you will not be able to recover any data that is encrypted.</label>
    </div>
    <div class="mb-4">
      <div>
        <label class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-blue-600 text-white hover:bg-blue-600 btn-toggle">
          <div class="custom-control custom-checkbox flex items-center">
            <input v-model="toggleCrypto" :disabled="!allowCrypto || togglingCrypto" type="checkbox" class="custom-control-input flex">
            <span class="custom-control-label flex">Enable Encryption</span>
          </div>
        </label>
      </div>
      <small class="text-gray-700">Note: Toggling encryption will encrypt/decrypt all existing documents. <span v-if="!allowCrypto">Enabling encryption <strong>requires</strong> private/public keys. Generate or supply them below to enable.</span></small>
    </div>
    <div class="mb-4">
      <label for="tags-search">Private Key</label>
      <textarea v-model="privateKey" class="block appearance-none w-full py-1 px-2 mb-1 text-base leading-normal bg-white text-gray-800 border border-gray-200 rounded" rows="5" placeholder="Private key" autocomplete="off"></textarea>
      <small class="text-gray-700">This key is used to <em>decrypt</em> documents. It <strong>will not</strong> be synced across devices when signed in.</small>
    </div>
    <div class="mb-4">
      <label for="tags-search">Public Key</label>
      <textarea v-model="publicKey" class="block appearance-none w-full py-1 px-2 mb-1 text-base leading-normal bg-white text-gray-800 border border-gray-200 rounded" rows="5" placeholder="Public key" autocomplete="off"></textarea>
      <small class="text-gray-700">This key is used to <em>encrypt</em> documents. It <strong>will</strong> be synced across devices when signed in.</small>
    </div>
    <div class="mb-4">
      <button @click="generateKeys" class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-gray-600 text-white hover:bg-gray-700">Generate Keys</button>
    </div>
  </section>
</template>

<script>
import { exportKeys, generateKeys } from '@/common/crypto/asymmetric';

import { TOUCH_DOCUMENT } from '@/store/actions';

import {
  SET_CRYPTO_ENABLED,
  SET_CRYPTO_KEYS,
} from '@/store/modules/settings';

export default {
  name: 'Encryption',
  data() {
    return {
      togglingCrypto: false,
    };
  },
  computed: {
    allowCrypto() {
      return this.privateKey && this.publicKey;
    },
    privateKey: {
      get() {
        return this.$store.state.settings.crypto.privateKey;
      },
      set(value) {
        this.$store.dispatch(SET_CRYPTO_KEYS, {
          privateKey: value,
        });
      },
    },
    publicKey: {
      get() {
        return this.$store.state.settings.crypto.publicKey;
      },
      set(value) {
        this.$store.dispatch(SET_CRYPTO_KEYS, {
          publicKey: value,
        });
      },
    },
    toggleCrypto: {
      get() {
        return this.$store.state.settings.crypto.enabled;
      },
      async set(value) {
        this.togglingCrypto = true;

        await this.$store.dispatch(SET_CRYPTO_ENABLED, value);
        await Promise.all(
          this.$store.getters.decrypted.map((doc) => {
            return this.$store.dispatch(TOUCH_DOCUMENT, doc);
          })
        );

        this.togglingCrypto = false;
      },
    },
  },
  methods: {
    async generateKeys() {
      const keys = await generateKeys();
      const { privateKey, publicKey } = await exportKeys(keys);

      this.privateKey = privateKey;
      this.publicKey = publicKey;
    },
  },
};
</script>
