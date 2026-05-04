<h1 class="titulo">Svelte Perfis</h1>

<style>
  .titulo {
    --cor-inicial: #3680f4;
    --cor-final: #c14fac;

    font-family: "Roboto", sans-serif;
    font-weight: 800;
    font-size: clamp(1.8rem, 4vw, 2.4rem);
    line-height: 1.2;

    background: linear-gradient(90deg, var(--cor-inicial), var(--cor-final));
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;

    letter-spacing: 0.5px;

    background-size: 200% auto;
    animation: brilho 3s linear infinite;
  }

      @keyframes brilho {
        to {
          background-position: 200% center;
        }
      }
</style>
