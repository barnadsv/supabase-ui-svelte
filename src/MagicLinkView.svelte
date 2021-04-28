<script>
  import LinkButton from './LinkButton.svelte'
  import Text from './Text.svelte'
  import Button from './Button.svelte'
  import Input from './Input.svelte'

  export let supabaseClient
  export let setView

  let error = '', message = '', loading = false, email = ''

  async function submit() {
    error = ''
    message = ''
    loading = true

    const { error: err } = await supabaseClient.auth.signIn({ email })

    if (err)
      error = err.message
    else
      message = 'Procure em sua caixa de e-mail pelo link mágico de autenticação.'

    loading = false
  }
</script>

<form on:submit|preventDefault={submit}>
  <Input name="email" type="email" label="E-mail" placeholder="Seu endereço de e-mail" icon="mail" bind:value={email}/>
  <Button block primary size="large" {loading} icon="inbox">Enviar link mágico de autenticação</Button>

  <LinkButton on:click={() => setView('sign_in')}>Login com senha</LinkButton>

  {#if message}
    <Text>{message}</Text>
  {/if}

  {#if error}
    <Text type="danger">{error}</Text>
  {/if}
</form>
