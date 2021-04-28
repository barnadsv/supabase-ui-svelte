<script>
  import LinkButton from './LinkButton.svelte'
  import Text from './Text.svelte'
  import Button from './Button.svelte'
  import Input from './Input.svelte'

  export let supabaseClient
  export let view
  export let setView

  let error = '', message = '', loading = false, email = '', password = ''

  async function submit() {
    error = ''
    message = ''
    loading = true

    if (view == 'sign_up') {
      const { error: signUpError } = await supabaseClient.auth.signUp({
        email, password
      })

      if (signUpError) error = signUpError.message
    } else if (view == 'sign_in') {
      const { error: signInError } = await supabaseClient.auth.signIn({
        email, password
      })

      if (signInError) error = signInError.message
    }

    loading = false
  }
</script>

<form on:submit|preventDefault={submit}>
  <Input name="email" type="email" label="E-mail" icon="mail" bind:value={email}/>
  <Input name="password" type="password" label="Senha" icon="key" bind:value={password}/>

  {#if view == 'sign_up'}
    <Button block primary size="large" {loading} icon="inbox">Cadastro</Button>
    <div class="links">
      <LinkButton on:click={() => setView('magic_link')}>Login com link mágico de autenticação</LinkButton>
      <LinkButton on:click={() => setView('sign_in')}>Você tem uma conta? Faça Login</LinkButton>
    </div>
  {:else}
    <Button block primary size="large" {loading} icon="inbox">Login</Button>
    <div class="links">
      <LinkButton on:click={() => setView('sign_up')}>Não tem uma conta? Cadastre-se</LinkButton>
    </div>
  {/if}

  {#if message}
    <Text>{message}</Text>
  {/if}

  {#if error}
    <Text type="danger">{error}</Text>
  {/if}
</form>

<style>
  form {
    display: flex;
    flex-direction: column;
  }

  .links {
    display: flex;
    flex-direction: column;
    margin: 1rem 0;
    gap: 0.5rem;
  }
</style>
