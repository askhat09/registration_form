<template>
  <div>
    <h2>Регистрация</h2>
    <form @submit.prevent="register">
      <div>
        <label for="username">Имя пользователя:</label>
        <input type="text" id="username" v-model="username" required>
      </div>
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="email" required>
      </div>
      <div>
        <label for="password">Пароль:</label>
        <input type="password" id="password" v-model="password" required>
      </div>
      <div>
        <button type="submit">Зарегистрироваться</button>
      </div>
    </form>
    <p v-if="registered">Вы успешно зарегистрированы!</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      email: '',
      password: '',
      registered: false
    };
  },
  methods: {
    register() {
      // Здесь обычно происходила бы отправка данных на сервер
      // Но для простоты этот пример просто устанавливает флаг зарегистрированного пользователя
      this.registered = true;
    }
  }
};
</script>

<style scoped>

</style>
