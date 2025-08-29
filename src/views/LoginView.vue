<template>
    <div class="flex flex-col items-center justify-center h-screen bg-gray-100">
      <div class="w-full max-w-sm p-8 bg-white rounded-lg border border-gray-300 shadow-lg">
        <h2 class="mb-6 text-2xl font-bold text-center text-gray-800">Entrar</h2>
        <form @submit.prevent="handleLogin" class="flex flex-col gap-4">
          <input 
            v-model="email"
            type="email" 
            placeholder="Email"
            required
            class="p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
          <input 
            v-model="password"
            type="password" 
            placeholder="Senha"
            required
            class="p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
          <button
            type="submit"
            class="p-3 font-semibold transition-colors duration-200 bg-primary rounded-md hover:bg-blue-600"
          >
            Entrar
          </button>
        </form>
        <p v-if="error" class="mt-4 text-sm text-center text-red-500">{{ error }}</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  import axios from 'axios';
  
  const email = ref('');
  const password = ref('');
  const error = ref('');
  const router = useRouter();
  
  const handleLogin = async () => {
    error.value = '';
    try {
      const response = await axios.post('http://localhost:3000/api/login', {
        email: email.value,
        password: password.value,
      });

      console.log(response)
  
      // Se o login for bem-sucedido, guarde o token no armazenamento local do navegador
      localStorage.setItem('token', response.data.token);
  
      // Redireciona o usuário para a página principal
      router.push({ name: 'home' });
  
    } catch (err) {
      error.value = 'Credenciais inválidas. Tente novamente.';
      console.error(err);
    }
  };
  </script>