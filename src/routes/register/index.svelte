<script>
  import { goto, stores } from "@sapper/app";
  import ListErrors from "../_components/ListErrors.svelte";
  import { post } from "utils.js";

  const { session } = stores();

  let username = "";
  let email = "";
  let password = "";
  let cnfpassword = "";
  let learningArea = "";
  let guidingArea = "";
  let phoneNo = null;
  let errors = null;
  let isLearner = false;
  let isGuide = false;
  let isCoLearner = false;
  let agreePolicy = false;
  let errorMessage = "";

	async function submit(event) {
		const response = await post(
			`http://localhost:3100/register`, 
			{
				username: username,
				password: password,
				email: email,
				terms_and_conditions_checked: agreePolicy,
				need_mentoring: isLearner,
				available_to_mentor: isGuide,
				need_colearner: isCoLearner,
				skill_to_guide: guidingArea,
				skill_to_learn: learningArea,
				phonenumber: phoneNo
			}
		);

		// TODO handle network errors
		if (response.errors != undefined) {
			errors = response.errors;
		} else {
			goto("/register/success");
		}
	}
</script>

<style>
  /* Chrome, Safari, Edge, Opera */
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  /* Firefox */
  input[type="number"] {
    -moz-appearance: textfield;
  }

  input[type="checkbox"] {
    cursor: pointer;
    margin: 2px;
  }
  label {
    cursor: pointer;
    font-size: 1.25rem;
    color: #55595c;
  }
  .policy-label {
    padding-top: 20px;
  }
</style>

<svelte:head>
  <title>Sign up • Learning Web</title>
</svelte:head>

<div class="auth-page">
  <div class="container page">
    <div class="row">
      <div class="col-md-6 offset-md-3 col-xs-12">
        <h1 class="text-xs-center">Sign up</h1>
        <p class="text-xs-center">
          <a href="/login">Have an account?</a>
        </p>

        <ListErrors {errors} />

        <label class="form-check-label policy-label">{errorMessage}</label>
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
              type="email"
              placeholder="Email"
              bind:value={email} />
          </fieldset>
          <fieldset class="form-group">
            <input
              class="form-control form-control-lg"
              type="password"
              placeholder="Password"
              bind:value={password} />
          </fieldset>
          <fieldset class="form-group">
            <input
              class="form-control form-control-lg"
              type="password"
              placeholder="Confirm Password"
              bind:value={cnfpassword} />
          </fieldset>
          <fieldset class="form-group">
            <input
              class="form-control form-control-lg"
              type="number"
              placeholder="Phone Number"
              bind:value={phoneNo} />
          </fieldset>
          <fieldset>
            <div class="row">
              <div class="col-xs-3">
                <label class="form-check-label">
                  <input type="checkbox" bind:checked={isLearner} />
                  Learner
                </label>
              </div>
              <div class="col-xs-9">
                <input
                  class="form-control form-control-lg"
                  type="text"
                  placeholder="Main area of Learning"
                  bind:value={learningArea}
                  disabled={!isLearner} />
              </div>
            </div>
            <div class="row">
              <div class="col-xs-3">
                <label class="form-check-label">
                  <input type="checkbox" bind:checked={isGuide} />
                  Guide
                </label>
              </div>
              <div class="col-xs-9">
                <input
                  class="form-control form-control-lg"
                  type="text"
                  placeholder="Main guiding area/skill"
                  bind:value={guidingArea}
                  disabled={!isGuide} />
              </div>
            </div>
            <div class="row col-xs-12">
              <label class="form-check-label">
                <input type="checkbox" bind:checked={isCoLearner} />
                Co-Learner
              </label>

            </div>
          </fieldset>

          <fieldset>
            <label class="form-check-label policy-label">
              <input type="checkbox" bind:checked={agreePolicy} />
              I agree the policies and privacies of the system.
            </label>
          </fieldset>
          <button
            disabled={!agreePolicy}
            class="btn btn-lg btn-primary pull-xs-right">
            Sign up
          </button>
        </form>
      </div>
    </div>
  </div>
</div>
