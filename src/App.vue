<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue';

// Estado
const length = ref(16);
const rawKey = ref('');
const copiedField = ref<string | null>(null);

// Configurações
const useSymbols = ref(false);
const useNumbers = ref(true);

// Lógica de Geração
const generate = () => {
  const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz';
  const numbers = '0123456789';
  const symbols = '!@#$%^&*()_+~`|}{[]:;?><,./-=';

  let chars = letters;
  if (useNumbers.value) chars += numbers;
  if (useSymbols.value) chars += symbols;

  // 1. Congelamos o valor em uma variável local (primitiva)
  let currentLength = length.value;

  // Validações de segurança
  if (currentLength < 1) currentLength = 1;
  if (currentLength > 128) currentLength = 128;
  
  // Atualiza o ref visualmente caso tenha sido corrigido pelos limites acima
  length.value = currentLength;

  let result = '';
  // 2. Usamos a variável local para criar o array
  const array = new Uint32Array(currentLength);
  crypto.getRandomValues(array);

  // 3. Usamos a MESMA variável local no loop
  for (let i = 0; i < currentLength; i++) {
    // Agora o TS sabe que 'i' nunca será maior que o tamanho do array
    // O operador '?? 0' é uma segurança extra caso o TS seja muito estrito
    const randomIndex = array[i] ?? 0; 
    result += chars.charAt(randomIndex % chars.length);
  }

  rawKey.value = result;
};

// Computed Properties para as variações
const keyMixed = computed(() => rawKey.value);
const keyUpper = computed(() => rawKey.value.toUpperCase());
const keyLower = computed(() => rawKey.value.toLowerCase());

// Função de Copiar (API Nativa)
const copyToClipboard = async (text: string, id: string) => {
  try {
    await navigator.clipboard.writeText(text);
    copiedField.value = id;
    
    // Resetar o feedback visual após 2 segundos
    setTimeout(() => {
      copiedField.value = null;
    }, 2000);
  } catch (err) {
    console.error('Falha ao copiar', err);
  }
};

// Observa mudanças no tamanho ou configs para regerar automaticamente
watch([length, useSymbols, useNumbers], () => {
  generate();
});

// Gera ao carregar
onMounted(() => {
  generate();
});
</script>

<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center p-4 sm:p-6 font-sans">
    
    <main class="w-full max-w-4xl grid grid-cols-1 lg:grid-cols-5 bg-white shadow-2xl rounded-2xl overflow-hidden min-h-[500px]">
      
      <aside class="bg-gray-900 p-8 text-white lg:col-span-2 flex flex-col justify-center relative overflow-hidden">
        <div class="absolute top-0 left-0 w-full h-full opacity-10 pointer-events-none">
           <svg class="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
             <path d="M0 100 C 20 0 50 0 100 100 Z" fill="white" />
           </svg>
        </div>

        <div class="relative z-10">
          <h1 class="text-3xl font-bold mb-2">Random Key</h1>
          <p class="text-gray-400 mb-8 text-sm">Gerador de chaves seguras e rápidas.</p>

          <div class="space-y-6">
            <div>
              <label class="block text-sm font-medium text-gray-300 mb-2">Tamanho da Chave</label>
              <div class="flex items-center gap-3">
                <input 
                  type="range" 
                  v-model.number="length" 
                  min="4" 
                  max="64" 
                  class="w-full h-2 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-green-500"
                >
                <input 
                  type="number" 
                  v-model.number="length" 
                  class="w-16 bg-gray-800 border border-gray-700 rounded px-2 py-1 text-center focus:outline-none focus:border-green-500 transition-colors"
                >
              </div>
            </div>

            <div class="space-y-2">
              <label class="flex items-center space-x-3 cursor-pointer">
                <input type="checkbox" v-model="useNumbers" class="form-checkbox h-5 w-5 text-green-500 rounded bg-gray-800 border-gray-600 focus:ring-green-500 focus:ring-offset-gray-900">
                <span class="text-gray-300">Incluir Números</span>
              </label>
              <label class="flex items-center space-x-3 cursor-pointer">
                <input type="checkbox" v-model="useSymbols" class="form-checkbox h-5 w-5 text-green-500 rounded bg-gray-800 border-gray-600 focus:ring-green-500 focus:ring-offset-gray-900">
                <span class="text-gray-300">Incluir Símbolos</span>
              </label>
            </div>

            <button 
              @click="generate" 
              class="w-full mt-4 bg-green-600 hover:bg-green-500 text-white font-bold py-3 px-4 rounded-lg transition-all flex items-center justify-center gap-2 shadow-lg shadow-green-900/50"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
              </svg>
              Gerar Nova Chave
            </button>
          </div>
        </div>
      </aside>

      <section class="p-8 lg:p-12 lg:col-span-3 flex flex-col justify-center space-y-6 bg-white">
        
        <div v-for="(val, label) in { 'Mista': keyMixed, 'Maiúscula': keyUpper, 'Minúscula': keyLower }" :key="label">
          <label class="block text-xs font-bold text-gray-500 uppercase tracking-wide mb-1 ml-1">{{ label }}</label>
          <div class="relative group">
            <input 
              type="text" 
              readonly 
              :value="val" 
              class="w-full bg-gray-50 text-gray-700 border border-gray-200 rounded-lg py-3 pl-4 pr-24 font-mono text-sm focus:outline-none focus:bg-white focus:border-green-500 focus:ring-1 focus:ring-green-500 transition-all"
            >
            <div class="absolute top-1 right-1 bottom-1">
              <button 
                @click="copyToClipboard(val as string, label)"
                :class="[
                  'h-full px-4 rounded-md text-sm font-medium transition-all flex items-center gap-2',
                  copiedField === label 
                    ? 'bg-green-100 text-green-700 w-24 justify-center' 
                    : 'bg-gray-100 text-gray-600 hover:bg-green-500 hover:text-white w-20 justify-center'
                ]"
              >
                <span v-if="copiedField === label">Copiado!</span>
                <span v-else class="flex items-center gap-1">
                   <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z" />
                  </svg>
                  Copiar
                </span>
              </button>
            </div>
          </div>
        </div>

      </section>
    </main>

    <footer class="fixed bottom-4 text-center w-full text-gray-400 text-xs pointer-events-none">
      <p>Desenvolvido com Vue 3 & Tailwind</p>
    </footer>
  </div>
</template>