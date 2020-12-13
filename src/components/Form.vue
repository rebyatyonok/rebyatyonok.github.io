<template>
  <form class="form" method="POST" @submit="submit($event)">
    <fieldset class="form__fieldset">
      <legend class="form__fieldset__legend">Login to continue</legend>

      <div class="form__field">
        <label for="email" class="form__field__label">Email</label>
        <input
          :value="formFieldStates.email.value"
          class="from__field__input"
          type="email"
          name="email"
          id="email"
          placeholder="example@yahoo.com"
          required
          aria-required="true"
          :disabled="formState === 'loading'"
          @input="onFieldInput('email', $event)"
          @blur="onFieldBlur('email')"
        />
        <span
          v-if="formFieldStates.email.isError"
          class="form__field__error"
          role="alert"
        >Please provide a correct email</span>
      </div>

      <div class="form__field">
        <label for="password" class="form__field__label">Password</label>
        <input
          :value="formFieldStates.password.value"
          class="from__field__input"
          type="password"
          name="password"
          id="password"
          placeholder="********"
          required
          aria-required="true"
          :disabled="formState === 'loading'"
          @input="onFieldInput('password', $event)"
          @blur="onFieldBlur('password')"
        />
        <span
          v-if="formFieldStates.password.isError"
          class="form__field__error"
          role="alert"
        >Please provide a password</span>
      </div>

      <div class="form__field form__field--line">
        <input
          for="from__field__input"
          type="checkbox"
          id="remember"
          aria-requried="false"
          :disabled="formState ==='loading'"
        />
        <label for="remember" class="form__field__label">Remember me</label>
      </div>
    </fieldset>

    <button
      class="form__submit-btn"
      type="submit"
      :disabled="isButtonDisabled"
      :aria-disabled="isButtonDisabled"
    >Login</button>
  </form>
</template>

<script lang="ts">
import { Vue } from 'vue-class-component';

enum FormState {
  incomplete = 'incomplete',
  invalid = 'invalid',
  valid = 'valid',
  loading = 'loading',
}

enum FieldState { invalid = 'invalid', valid = 'valid' }

enum FieldType { email = 'email', password = 'password' }

function getTextFieldState() {
  return { state: FieldState.invalid, value: '', isError: false };
}

export default class From extends Vue {
  formFieldStates = {
    [FieldType.email]: getTextFieldState(),
    [FieldType.password]: getTextFieldState(),
  }

  formState: FormState = FormState.incomplete;

  get isButtonDisabled() {
    return this.formState !== FormState.valid;
  }

  onFieldInput(type: FieldType, event: KeyboardEvent) {
    const target = event.target as HTMLInputElement;

    this.formFieldStates[type].value = target ? target.value : '';

    this.formFieldStates[type].state = target.validity.valid
      ? FieldState.valid
      : FieldState.invalid;

    if (this.formFieldStates[type].state === FieldState.valid) {
      this.formFieldStates[type].isError = false;
    }

    this.validateForm();
  }

  onFieldBlur(type: FieldType) {
    this.formFieldStates[type].isError = this.formFieldStates[type].state === FieldState.invalid;
  }

  validateForm() {
    this.formState = this.formFieldStates.email.state === FieldState.valid
      && this.formFieldStates.password.state === FieldState.valid
      ? FormState.valid
      : FormState.invalid;
  }

  // eslint-disable-next-line
  submit(e: any) {
    e.preventDefault();
    this.formState = FormState.loading;

    setTimeout(() => {
      alert('All done!');
      this.formState = FormState.valid;
    }, 1000);
  }
}
</script>

<style lang="scss" scoped>
.form {
  display: flex;
  flex-direction: column;

  &__fieldset {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    margin-bottom: 1rem;
    border: 0;
    padding: 0;

    &__legend {
      margin-bottom: 1.5rem;
      font-size: 1.2rem;
    }
  }

  &__field {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    margin-bottom: 1rem;

    &:last-child {
      margin-bottom: 0;
    }

    &--line {
      flex-direction: row;
      align-items: flex-start;
    }

    &__label {
      display: block;
      margin-bottom: 0.5rem;
    }

    &__error {
      font-size: 0.9rem;
      color: var(--red);
    }
  }

  &__submit-btn {
    margin: 0 0 0.5rem auto;
  }
}
</style>
