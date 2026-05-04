<script lang="ts">
  import type IUsuario from "../interfaces/IUsuario";
  import BarraSuperior from "./BarraSuperior.svelte";

  export let usuario: IUsuario;

  $: temRepositorios = usuario?.repositorios_recentes?.length > 0;
</script>

<div class="card">
  <BarraSuperior />

  <div class="conteudo">
    <a
      href={usuario.perfil_url}
      target="_blank"
      rel="noopener noreferrer"
      class="avatar-link"
      aria-label="Abrir perfil"
    >
      <div
        class="avatar"
        style="background-image: url({usuario.avatar_url})"
      ></div>
    </a>

    <div class="info">
      {#if usuario.nome}
        <p><strong>Nome:</strong> <span>{usuario.nome}</span></p>
      {/if}

      <p><strong>Usuário:</strong> <span>{usuario.login}</span></p>
      <p><strong>Seguidores:</strong> <span>{usuario.seguidores}</span></p>
      <p><strong>Repositórios:</strong> <span>{usuario.repositorios_publicos}</span></p>
    </div>

    {#if temRepositorios}
      <div class="repos">
        <h2>Repositórios recentes</h2>

        <ul>
          {#each usuario.repositorios_recentes as repo (repo.id)}
            <li>
              <a
                href={repo.url}
                target="_blank"
                rel="noopener noreferrer"
              >
                {repo.nome}
              </a>
            </li>
          {/each}
        </ul>
      </div>
    {/if}
  </div>
</div>

<style>
  .card {
    margin-top: 4rem;
  }

  .conteudo {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    align-items: center;

    padding: 2rem;
    border-radius: 0 0 12px 12px;

    background: rgba(255, 255, 255, 0.6);
    backdrop-filter: blur(8px);
    box-shadow: -12px 37px 45px rgba(133, 127, 201, 0.18);
  }

  .avatar-link {
    flex-shrink: 0;
  }

  .avatar {
    width: 12rem;
    height: 12rem;
    border-radius: 50%;
    border: 4px solid #2e80fa;

    background-size: cover;
    background-position: center;

    transition: transform 0.2s ease;
  }

  .avatar-link:hover .avatar {
    transform: scale(1.05);
  }

  .info {
    display: flex;
    flex-direction: column;
    gap: 0.4rem;

    font-size: 1.1rem;
    color: #395278;
  }

  .info span {
    color: #6781a8;
    font-weight: 400;
  }

  .repos {
    flex: 1 1 100%;
  }

  .repos h2 {
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
    color: #395278;
  }

  .repos ul {
    padding-left: 1rem;
  }

  .repos a {
    color: #6781a8;
    text-decoration: none;
    transition: color 0.2s;
  }

  .repos a:hover {
    color: #2e80fa;
  }
</style>
