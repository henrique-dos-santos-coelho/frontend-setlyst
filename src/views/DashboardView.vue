<template>
    <div class="flex flex-col items-center justify-center h-screen bg-gray-100 p-4">
      <div class="w-full max-w-2xl p-8 bg-white rounded-lg shadow-lg">
        <h2 class="mb-6 text-3xl font-bold text-center text-gray-800">
          Bem-vindo, {{ user.nome }}!
        </h2>
        <p class="mb-4 text-center text-gray-600">
          Este é o painel de controle. Você está autenticado com sucesso.
        </p>
        <div class="flex flex-col gap-2 p-4 bg-gray-50 rounded-md">
          <p class="text-sm font-medium text-gray-700">Seu ID de Usuário: <span class="font-normal">{{ user.id }}</span></p>
          <p class="text-sm font-medium text-gray-700">Seu Email: <span class="font-normal">{{ user.email }}</span></p>
        </div>
        <div class="mt-6 text-center">
          <button
            @click="handleLogout"
            class="p-2 text-sm text-red-500 transition-colors duration-200 hover:text-red-700"
          >
            Sair
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { useRouter } from 'vue-router';
  import axios from 'axios';
  
  const user = ref({ nome: '', email: '', id: null });
  const router = useRouter();
  
  const fetchUserData = async () => {
    try {
      const token = localStorage.getItem('token');
      if (!token) {
        router.push({ name: 'login' });
        return;
      }
  
      const response = await axios.get('http://localhost:3000/api/me', {
        headers: {
          'Authorization': `Bearer ${token}`
        }
      });
      user.value = response.data;
    } catch (err) {
      console.error("Erro ao buscar dados do usuário:", err);
      // Se o token for inválido ou expirado, redirecione para o login
      localStorage.removeItem('token');
      router.push({ name: 'login' });
    }
  };
  
  const handleLogout = () => {
    localStorage.removeItem('token');
    router.push({ name: 'login' });
  };
  
  onMounted(fetchUserData);
  </script>