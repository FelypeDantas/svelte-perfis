<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import type IUsuario from "../interfaces/IUsuario";
  import { buscaRepositorios, buscaUsuario } from "../requisicoes";
  import montaUsuario from "../utils/montaUsuario";

  let valorInput = "";
  let statusDeErro: number | null = null;
  let carregando = false;

  const dispatch = createEventDispatcher<{
    aoAlterarUsuario: IUsuario | null;
  }>();

  async function aoSubmeter() {
    if (!valorInput.trim()) return;

    carregando = true;
    statusDeErro = null;

    try {
      // 🔥 roda em paralelo (mais rápido)
      const [resUsuario, resRepos] = await Promise.all([
        buscaUsuario(valorInput),
        buscaRepositorios(valorInput)
      ]);

      if (!resUsuario.ok) {
        statusDeErro = resUsuario.status;
        dispatch("aoAlterarUsuario", null);
        return;
      }

      const [dadosUsuario, dadosRepos] = await Promise.all([
        resUsuario.json(),
        resRepos.ok ? resRepos.json() : []
      ]);

      dispatch("aoAlterarUsuario", montaUsuario(dadosUsuario, dadosRepos));

    } catch (erro) {
      console.error("Erro inesperado:", erro);
      statusDeErro = 500;
      dispatch("aoAlterarUsuario", null);
    } finally {
      carregando = false;
    }
  }
</script>

<form on:submit|preventDefault={aoSubmeter} class="form">
  <input
    type="text"
    placeholder="Pesquise um usuário..."
    class="input"
    class:erro-input={statusDeErro === 404}
    bind:value={valorInput}
  />

  {#if statusDeErro === 404}
    <span class="erro">Usuário não encontrado!</span>
  {:else if statusDeErro === 500}
    <span class="erro">Erro inesperado... tenta de novo 👀</span>
  {/if}

  <div class="botao-container">
    <button type="submit" class="botao" disabled={carregando}>
      {#if carregando}
        Buscando...
      {:else}
        Buscar <img src="/assets/lupa.svg" alt="icone de lupa" />
      {/if}
    </button>
  </div>
</form>

<style>
  .form {
    position: relative;
  }

  .input {
    padding: 15px 25px;
    width: calc(100% - 9.625rem);
    font-size: 1rem;
    border-radius: 8px;
    border: 1px solid #2e80fa;
    box-shadow: 0px 17px 52px rgba(222, 231, 247, 0.4);
    outline: 0;
  }

  .input::placeholder {
    font-family: "Roboto";
    font-style: italic;
    font-weight: 300;
    font-size: 1rem;
    color: #6e8cba;
  }

  .erro {
    position: absolute;
    bottom: -22px;
    left: 0;
    font-size: 0.9rem;
    color: #ff003e;
  }

  .erro-input {
    border-color: #ff003e;
  }

  .botao-container {
    position: absolute;
    right: 0;
    top: 0;
    bottom: 0;
    display: flex;
  }

  .botao {
    padding: 0 20px;
    border-radius: 8px;
    border: none;
    background: #2e80fa;
    color: #fff;
    font-size: 1.2rem;
    cursor: pointer;

    display: flex;
    align-items: center;
    gap: 10px;

    transition: background-color 0.2s;
  }

  .botao:hover:not(:disabled) {
    background: #4590ff;
  }

  .botao:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
</style>
