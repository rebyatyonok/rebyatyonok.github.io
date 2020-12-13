<template>
  <div class="login-page">
    <div class="login-page__content">
      <Form />
      <a class="login-page__content__link" href="https://google.com">Forgot your password?</a>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import Form from './components/Form.vue';

@Options({
  components: {
    Form,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">
:root {
  --white: #ffffff;
  --whitesmoke: #f5f5f5;
  --lightblue: #add8e6;
  --red: #ff0000;
}
body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}
input,
button {
  font-size: inherit;
  font-family: inherit;
  padding: 5px 7px;
  border-radius: 3px;
}
a {
  color: inherit;
}

.login-page {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100%;
  background-color: var(--whitesmoke);

  &__content {
    width: 100%;
    max-width: 300px;
    background-color: var(--white);
    padding: 1rem;
    border-radius: 3px;

    &__link {
      font-size: 0.8rem;
    }
  }
}
</style>
