<template>
  <div class="container mt-5">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="card">
          <div class="card-header">LOGIN</div>
          <div class="card-body">
            <form @submit.prevent="login">
              <div class="form-group">
                <label for="username">Username:</label>
                <input type="text" id="username" v-model="username" class="form-control" required>
              </div>
              <div class="form-group">
                <label for="password">Password:</label>
                <input type="password" id="password" v-model="password" class="form-control" required>
              </div>
              <button type="submit" class="btn btn-primary btn-block">LOGIN</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import api from '@/api'

export default {
  data() {
    return {
      username: '',
      password: ''
    };
  },
  methods: {
    async login() {
      try {
        const response = await api.post('/users/login', {
          username: this.username,
          password: this.password
        });
        // Armazena o token de autenticação no localStorage
        localStorage.setItem('token', response.data.token);
        // Redireciona o usuário para outra página após o login, se necessário
        this.$router.push('/products');
      } catch (error) {
        alert('Erro ao fazer login.', error);
        // Trate o erro de acordo com sua necessidade (exibindo uma mensagem de erro, por exemplo)
      }
    }
  }
};
</script>
<style scoped>

.card-body[data-v-8dac4566] {
    padding: 40px;
    background-color: #f9f9f900;
}
.mt-5{
  margin-top: 0rem !important;
}

.login-container {
  max-width: 300px;
  margin: 50px auto; /* Ajuste o valor do margin conforme necessário */
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.card {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: rgba(255, 255, 255, 0);
  margin-bottom: 20px;
  margin-top: 175px;
}


.card-header {
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  font-size: large;
  background-color: #f700ff;
  color: #fff;
  padding: 10px;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}

.form-group {

  padding: 10px; 
  margin-bottom: 20px; 
  font-size: 18px; 
}

.btn-primary {
  background-color: #007bff00;
  color: white;
  border: 1px solid rgb(43, 255, 0);
  padding: 10px 20px;
  cursor: pointer;
}

.btn-primary:hover {
  background-color: #f700ff;
}

.form-control {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

</style>