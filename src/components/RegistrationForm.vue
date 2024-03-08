<template>
  <div class="registration-container">
    <div class="registration-form">
      <div class="form-header">
        <h2>Регистрация</h2>
        <hr class="divider" />
      </div>
      <div class="form-title">Заполните Ваши данные</div>
      <div class="form-fields">
        <div class="form-column">
          <div>
            <input
              type="text"
              v-model="username"
              id="username"
              placeholder="Имя"
              required
            />
          </div>
          <div>
            <input
              type="password"
              v-model="password"
              id="password"
              placeholder="Пароль"
              required
            />
          </div>
        </div>
        <div class="form-column">
          <div>
            <input
              type="email"
              v-model="email"
              id="email"
              placeholder="Email"
              required
            />
          </div>
          <div>
            <select v-model="role" id="role" required>
              <option :value="null" disabled>Должность</option>
              <option
                :value="position.value"
                v-for="position in positions"
                :key="position.value"
              >
                {{ position.name }}
              </option>
            </select>
          </div>
          <div>
            <input
              type="password"
              v-model="passwordRepeat"
              id="password_repeat"
              placeholder="Повторите пароль"
              required
            />
          </div>
        </div>
      </div>
      <hr class="divider" />
      <div class="form-footer">
        <div class="switch-container">
          <label class="switch">
            <input type="checkbox" v-model="profileVisibility" />
            <span class="slider"></span>
          </label>
          <label
            >Хотите чтобы Ваш профиль видели другие участники платформы?
            <span
              >Включает профиль для просмотра другими пользователями по
              ссылке</span
            ></label
          >
        </div>
        <div class="agreement-container">
          <div class="agreement-checkbox">
            <input type="checkbox" id="agree" v-model="agree" required />
            <label for="agree">Регистрируясь, Вы соглашаетесь <a href="#">с политикой конфиденциальности</a> и обработкой <a href="#">персональных данных</a></label>
          </div>
          <button type="button" :disabled="!agree" @click="register">
            Зарегистрироваться
          </button>
        </div>
      </div>
      <p v-if="registered">Вы успешно зарегистрированы!</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import MockAdapter from 'axios-mock-adapter';

const mock = new MockAdapter(axios);
mock.onPost('/api/register').reply(200, {
  status: 'success',
  message: 'Пользователь успешно зарегистрирован',
});

export default {
  data() {
    return {
      username: '',
      email: '',
      password: '',
      passwordRepeat: '',
      role: null,
      agree: true,
      profileVisibility: true,
      registered: false,
      positions: [
        { value: 1, name: 'Директор' },
        { value: 2, name: 'Менеджер' },
        { value: 3, name: 'Сотрудник' },
      ],
    };
  },
  methods: {
    register() {
      if (this.password !== this.passwordRepeat) {
        alert('Пароли не совпадают');
        return;
      }
      axios.post('/api/register', {
        public: this.profileVisibility,
        username: this.username,
        role: this.role,
        email: this.email,
        password: this.password,
        password_repeat: this.passwordRepeat
      })
      .then(response => {
        console.log(response.data);
        alert(response.data.message);
        this.registered = true;
      })
      .catch(error => {
        console.error('Ошибка при отправке данных:', error);
        alert('Произошла ошибка при регистрации');
      });
    },
  },
};
</script>

<style scoped>
.registration-container {
  height: 100vh;

  display: flex;
  justify-content: center;
  align-items: center;
}

.registration-form {
  max-width: 960px;
  width: 100%;

  display: flex;
  flex-direction: column;

  border-radius: 15px;
  background-color: #fff;
}

.registration-form h2 {
  margin-bottom: 20px;
  padding-left: 30px;

  font-size: 19px;
  line-height: 27px;
}

.registration-form .divider {
  height: 1px;
  width: 100%;

  margin-bottom: 20px;

  border: none;
  background-color: #d9d9d9;
}

.registration-form label {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 10px;

  font-size: 16px;
  font-weight: 500;
}

.registration-form label > span {
  font-size: 14px;
  font-weight: 400;
  color: #696977;
}

.registration-form input,
.registration-form select {
  box-sizing: border-box;
  width: 100%;
  padding: 10px;
  margin-bottom: 30px;

  font-size: 14px;
  border-radius: 11px;
  border: 1px solid #e6e6eb;
  font-family: 'Montserrat', sans-serif;
  color: #9292a0;
}

.registration-form ::placeholder {
  color: #9292a0;
}

.registration-form button {
  padding: 11px 85px;

  font-size: 16px;
  border: none;
  border-radius: 8px;
  background-color: #DBE4F8;
  color: #497ADA;
  cursor: pointer;
}

.registration-form button:hover {
  background-color: #3586FF;
  color: #fff;
}

.form-title {
  padding-left: 30px;
  margin-bottom: 30px;

  font-size: 16px;
  line-height: 19px;
  font-weight: 500;
}

.form-fields {
  display: flex;
  justify-content: space-between;
  padding: 0 30px;
  gap: 14px;
}

.form-column {
  width: 450px;
}

.form-footer {
  display: flex;
  flex-direction: column;
  padding: 0 30px;
  margin-bottom: 45px;
}

.switch-container {
  display: flex;
  align-items: flex-start;
  margin-bottom: 30px;
}

.switch-container label {
  margin-right: 10px;
}

.switch {
  position: relative;
  display: inline-block;
  width: 39px;
  height: 19px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
  border-radius: 34px;
}

.slider:before {
  position: absolute;
  content: '';
  height: 19px;
  width: 19px;
  left: 0;
  bottom: 0px;
  background-color: white;
  transition: 0.4s;
  border-radius: 50%;
}

input:checked + .slider {
  background-color: #3586FF;
}

input:focus + .slider {
  box-shadow: 0 0 1px #3586FF;
}

input:checked + .slider:before {
  -webkit-transform: translateX(20px);
  -ms-transform: translateX(20px);
  transform: translateX(20px);
}

.checkbox input[type='checkbox'] {
  margin-right: 10px;
}

.checkbox label {
  margin-left: 5px;
}

.agreement-container {
  display: flex;
  justify-content: space-between;
  gap: 36px;
}

.agreement-checkbox {
  display: flex;
  gap: 14px;
}

.agreement-checkbox input {
  width: 19px;
}

.agreement-checkbox label {
  display: block;

  font-size: 14px;
  font-weight: 400;
}

.agreement-checkbox label>a {
  text-decoration: none;
  color: #3586FF;
}
@media only screen and (max-width: 600px) {
  .form-fields {
    flex-direction: column;
  }

  .form-column {
    width: 100%;
  }

  .agreement-container {
    flex-direction: column;
  }
}

</style>
