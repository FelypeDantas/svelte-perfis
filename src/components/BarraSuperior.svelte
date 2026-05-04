<div class="window-bar">
  <span class="control close"></span>
  <span class="control minimize"></span>
  <span class="control maximize"></span>
</div>

<style>
  .window-bar {
    --control-size: 8px;
    --gap: 6px;

    display: flex;
    align-items: center;
    gap: var(--gap);

    padding: 10px;
    border-radius: 8px 8px 0 0;

    background: rgba(255, 255, 255, 0.5);
    backdrop-filter: blur(8px);
  }

  .control {
    width: var(--control-size);
    height: var(--control-size);

    border-radius: 50%;
    display: inline-block;

    transition: transform 0.15s ease, filter 0.15s ease;
  }

  /* Cores sem depender de nth-child */
  .control.close {
    background: #d8695b;
  }

  .control.minimize {
    background: #eac213;
  }

  .control.maximize {
    background: #79c743;
  }

  /* Interação */
  .control:hover {
    transform: scale(1.15);
    filter: brightness(0.9);
    cursor: pointer;
  }
</style>
