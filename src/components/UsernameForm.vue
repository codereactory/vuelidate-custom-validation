<template>
  <form @submit.prevent="onSubmit" class="username-form">
    <div class="form-group" :class="{ 'form-group--error': $v.username.$error }">
      <label class="form__label">Username</label>
      <input class="form__input" v-model.trim="$v.username.$model"/>
    </div>
    <div class="error" v-if="$v.username.$error">{{ usernameError }}</div>
    <button type="submit">Send</button>
  </form>
</template>

<script>
import { required, minLength } from 'vuelidate/lib/validators'

const specialSymbol = 'a'
const whiteListedUsernames = ['Aivon', 'Aloha', 'Alexander', 'Aaa']

export default {
  name: 'username-form',

  data() {
    return {
      username: '',
      networking: false,
      networkingError: '',
    }
  },

  validations: {
    username: {
      required,
      minLength: minLength(4),
      containsSpecialChar: function(val) {
        return val.indexOf(specialSymbol) !== -1
      },
      whiteListed: function(val, vm) {
        return vm.networkingError === ''
      },
    }
  },

  watch: {
    username(val) {
      if (!this.networking && this.networkingError !== '') {
        this.networkingError = ''
      }
    },
  },

  computed: {
    usernameError() {
      let err = 'Invalid input.' 
      
      const state = this.$v.username
      if (!state.required) {
        err = 'Username is required.'
      } else if (!state.minLength) {
        err = 'Username must be at least 4 symbols.'
      } else if (!state.containsSpecialChar) {
        err = `Username must contain ${specialSymbol}`
      } else if (!state.whiteListed) {
        err = this.networkingError
      }

      return err 
    },
  },

  methods: {
    onSubmit(){
      if (this.networking) {
        return
      }
      this.networking = true
      
      new Promise(resolve => setTimeout(resolve, 2000))
        .then(() => {
          this.networking = false

          if (whiteListedUsernames.indexOf(this.username) === -1) {
            this.networkingError = 'Username is not allowed.'
          }
        })
    },
  },
}
</script>

<style>
.username-form {
  max-width: 320px;
  margin: auto;
}

.form-group {
  margin-bottom: 10px;
}

.form__label {
  font-size: 0.8125rem;
  color: #4b6372;
  margin-bottom: 0.3125rem;
  margin-left: 0.875rem;
  display: block;
}

.form__input {
  font-size: 0.875rem;
  font-weight: 300;
  color: #374853;
  line-height: 2.375rem;
  min-height: 2.375rem;
  position: relative;
  border: 1px solid #E8E8E8;
  border-radius: 5px;
  background: #fff;
  padding: 0 0.8125rem;
  width: 100%;
  transition: border .1s ease;
  box-sizing: border-box;
}

.error {
  color: #f57f6c;
}

.form-group--error {
  border-color: #f79483;
}

.form-group--error .form__label {
  color: #f57f6c;
}

.form-group--error input {
  border-color: #f79483;
}
</style>