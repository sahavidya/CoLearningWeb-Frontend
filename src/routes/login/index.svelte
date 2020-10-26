<script>
  import { goto, stores } from "@sapper/app";
  import ListErrors from "../_components/ListErrors.svelte";
  import { post } from "utils.js";
  import * as api from 'api.js';

  const { session } = stores();

  let username = "";
  let password = "";
  let errors = null;

  async function submit(event) {
    const response = await api.post(`login`, {
      username: username,
      password: password
    });

    // TODO handle network errors
    if (response.message != undefined) {
      errors = response.message;
      console.log(errors);
    } else {
      $session.user = response;
      $session.username = username;
      goto('/Home');
    }
  }
</script>

<svelte:head>
  <title>Sign in • Learning Web</title>
</svelte:head>

<div class="auth-page">
  <div class="container page">
    <div class="row">
      <div class="col-md-6 offset-md-3 col-xs-12">
        <h1 class="text-xs-center">Sign In</h1>
        <p class="text-xs-center">
          <a href="/register">Need an account?</a>
        </p>

        <ListErrors {errors} />

        <form on:submit|preventDefault={submit}>
          <fieldset class="form-group">
            <input
              class="form-control form-control-lg"
              type="text"
              placeholder="User name"
              bind:value={username} />
          </fieldset>
          <fieldset class="form-group">
            <input
              class="form-control form-control-lg"
              type="password"
              placeholder="Password"
              bind:value={password} />
          </fieldset>
          <button
            class="btn btn-lg btn-primary pull-xs-right"
            type="submit"
            disabled={!username || !password}>
            Sign in
          </button>
        </form>
      </div>
    </div>
  </div>
</div>
