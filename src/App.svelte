<script>
  import { slide } from 'svelte/transition';
  import vest from 'vest';
  // import { create, enforce, test } from 'vest';
  import isEmail from 'validator/es/lib/isEmail';

  // destructing imports does not work at the top import level
  const { create, enforce, test } = vest;

  enforce.extend({ isEmail });

  const initial = {
    email: '',
    password: ''
  };

  let errors = [];

  const validate = create('login_form', (data = {}) => {
    test('email', 'Email address is required', () =>
      enforce(data.email).isNotEmpty()
    );
    test('email', 'Email address is not valid', () => {
      enforce(data.email).isEmail();
    });
    test('password', 'Password is required', () =>
      enforce(data.password).isNotEmpty()
    );
    test('password', 'Password must be at least 5 chars long', () =>
      enforce(data.password).longerThanOrEquals(6)
    );
  });

  let values = { ...initial };

  const submit = () => {
    const res = validate(values);
    if (res.hasErrors()) {
      errors = Object.values(res.getErrors()).flat();
      console.log(errors);
      return;
    }
    console.log(values);
  };

  const reset = () => {
    values = { ...initial };
    errors = [];
  };
</script>

<div class="container">
  <form class="generic" on:submit|preventDefault={submit}>
    <div>
      <label>
        <span>Email</span>
        <input
          type="text"
          name="email"
          placeholder="name@company.com"
          bind:value={values.email}
        />
      </label>
    </div>
    <div>
      <label>
        <span>Password</span>
        <input
          type="password"
          name="password"
          placeholder="************"
          bind:value={values.password}
        />
      </label>
    </div>

    {#if errors.length}
      <div transition:slide|local>
        <ul class="text-red-500">
          {#each errors as err}
            <li>{err}</li>
          {/each}
        </ul>
      </div>
    {/if}

    <div>
      <button type="submit">Login</button>
      <button type="button" on:click={reset}>Reset</button>
    </div>
  </form>
</div>

<style>
  :global(body) {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
  }
  .container {
    width: 600px;
    margin: 0 auto;
    margin: 4em auto;
    max-width: 64em;
  }
  .generic span {
    display: block;
    font-weight: 600;
    font-size: 0.8em;
    padding-bottom: 5px;
  }
  .generic input {
    padding: 5px;
    min-width: 200px;
  }
  .generic div + div {
    margin-top: 12px;
  }
  .generic ul {
    color: red;
    list-style: inside;
    padding-left: 0;
  }
</style>
