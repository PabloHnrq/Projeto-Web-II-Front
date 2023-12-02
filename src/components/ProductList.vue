<template>
  <div class="container mt-5">
    <h2>Lista de Produtos</h2>
    <RouterButton @navigate="navigateTo" routePath="/product/add" buttonText="Adicionar produto" />
    <LogoutButton/>
    <table class="table mt-3">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Descrição</th>
          <th>Preço</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td><router-link :to="{ name: 'ProductDetails', params: { id: product.id } }">{{ product.id }}</router-link></td>
          <td>{{ product.name }}</td>
          <td>{{ product.description }}</td>
          <td>US$ {{ product.price.toFixed(2) }}</td>
          <td>
            <router-link :to="{ name: 'ProductEdit', params: { id: product.id },query: { editing: true } }"
              class="btn btn-primary btn-sm mr-2">Editar</router-link>
            <button @click="deleteProduct(product.id)" class="btn btn-danger btn-sm">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
        <div class="pagination justify-content-end mt-3">
        <button @click="prevPage" :disabled="currentPage === 1" class="btn btn-secondary mr-2">Anterior</button>
        <button @click="nextPage" :disabled="currentPage * pageSize >= totalProducts" class="btn btn-secondary">Próximo</button>
      </div>
  </div>
</template>

<script>
import api from '@/api'

import RouterButton from "@/components/RouterButton.vue"
import LogoutButton from "@/components/LogoutButton.vue"

export default {
  components: {
    RouterButton,
    LogoutButton
  },
  data() {
    return {
      products: [],
      currentPage: 1,
      pageSize: 3, // Número de itens por página
      totalProducts: 0
    }
  },
  beforeMount() {
    this.fetchProducts()
  },
  methods: {
    navigateTo(routePath) {
      this.$router.push(routePath)
    },
    async fetchProducts() {
      try {
        const response = await api.get(`/products?page=${this.currentPage}&size=${this.pageSize}&sort=price&direction=desc`)
        this.products = response.data.content
        this.totalProducts = response.data.totalElements;
      } catch (error) {
        console.error(error)
      }
    },
    async deleteProduct(productId) {
      if (confirm('Tem certeza de que deseja excluir este produto?')) {
        try {
          await api.delete(`/products/${productId}`)
          this.products = this.products.filter(product => product.id !== productId)
        } catch (error) {
          console.error(error)
        }
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.fetchProducts();
      }
    },
    nextPage() {
      if (this.currentPage * this.pageSize < this.totalProducts) {
        this.currentPage++;
        this.fetchProducts();
      }
    }
  },
}
</script>
<style scoped>

.mt-5{
  margin-top: 0rem !important;
}
.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 100px;
}

.table th,
.table td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}

.table th {
  background-color: #f2f2f2;
  font-weight: bold;
}

/* Estilos para os botões de ação */
.btn-primary,
.btn-danger {
  color: white;
  border: none;
  padding: 6px 12px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  margin-bottom: 0;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-primary {
  background-color: #007bff;
}

.btn-danger {
  background-color: #dc3545;
}

.btn-primary:hover,
.btn-danger:hover {
  opacity: 0.8;
}

/* Estilos para a paginação */
.pagination {
  display: flex;
  justify-content: flex-end;
  margin-top: 20px;
}

.pagination button {
  background-color: #6c757d;
  color: white;
  border: none;
  padding: 6px 12px;
  margin-left: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.pagination button:hover {
  background-color: #5a6268;
}

/* Estilos para os links da tabela */
a {
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>