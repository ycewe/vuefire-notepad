<template>
  <form class="auth-form" v-on:submit.prevent="wantsToSignUp ? signUpWithPassword() : signInWithPassword()">
    <h1>{{wantsToSignUp ? 'Sign up' : 'Sign in'}}</h1>
    <div>
      <label for="email">Email</label>
      <input type="email" name="email" id="email" placeholder="Email" required v-model="email">
    </div>
    <div>
      <label for="password">Password</label>
      <input type="password" name="password" id="password" placeholder="Password" required v-model="password">
    </div>
    <div v-show="wantsToSignUp">
      <label for="confirm-password">Confirm Password</label>
      <input type="password" name="confirm-password" id="confirm-password" v-model="confirmPassword">
    </div>
    <div v-show="!wantsToSignUp" class="clearfix btn-group">
      <button type="submit">Sign in</button>
      <button type="button" v-on:click="wantsToSignUp = true">Sign up</button>
    </div>
    <div v-show="wantsToSignUp">
      <button type="submit" class="signup-submit">Sign up</button>
    </div>
  </form>
</template>

<script>
  import Auth from '../../data/Auth';
  export default {
    data() {
      return {
        email: '',
        password: '',
        confirmPassword: '',
        wantsToSignUp: false,
      };
    },
    methods: {
      signUpWithPassword() {
        if (this.password === this.confirmPassword) {
          Auth.signUpWithPassword(this.email, this.password)
          .then(() => this.signInWithPassword())
          .then(() => this.$dispatch('alert',
          { type: 'success', message: 'Signed up successfully' }))
          .catch((error) => this.$dispatch('alert',
          { type: 'error', message: error.message }));
        }
      },
      signInWithPassword() {
        return Auth.signInWithPassword(this.email, this.password)
        .then((userData) => {
          this.$dispatch('alert',
            { type: 'success', message: 'Signed in successfully' });
          this.onSignedIn();
          return userData;
        })
        .catch((error) => this.$dispatch('alert',
          { type: 'error', message: error.message }));
      },
      onSignedIn() {
        this.$router.go({ name: 'notes' });
      },
    },
  };
</script>

<style>
  .auth-form {
    width: 480px;
    max-width: 100%;
    margin: 25vh auto 15px;
    background: #fff;
    padding: 15px;
    border-radius: 2px;
    box-shadow: 0 1px 5px #ccc;
  }
  .auth-form h1 {
    font-size: 32px;
    padding: 10px 0;
  }
  .auth-form > div {
    margin-top: 15px;
  }
  .auth-form input {
    height: 32px;
    border: none;
    border-bottom: 2px solid #bbb;
    padding: 10px;
    margin: 10px 0;
    border-radius: 4px;
  }
  .auth-form input:focus {
    border-bottom: 2px solid cyan;
    outline: none;
  }
  .auth-form label, .auth-form input {
    display: block;
    font-size: 20px;
    width: 100%;
  }
  .auth-form button {
    font-size: 18px;
    background: #fff;
    border: 1px solid #41b883;
    padding: 4px 6px;
    margin: 0;
    border-radius: 3px;
  }
  .auth-form .btn-group button {
    border-radius: 3px 0 0 3px;
    width: 50%;
    float: left;
  }
  .auth-form .btn-group button + button {
    border-radius: 0px 3px 3px 0;
    border-left: none;
  }
  .auth-form .signup-submit {
    width: 100%;
  }
</style>
