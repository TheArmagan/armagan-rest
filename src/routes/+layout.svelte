<script lang="ts">
  import { onNavigate } from "$app/navigation";
  import { sleep } from "$lib/utils";
  import gsap from "gsap";

  let slideElm: HTMLDivElement;

  onNavigate(async (navigation) => {
    slideElm.style.display = "block";
    if (navigation.delta === 1) {
      gsap
        .fromTo(
          "#slide",
          { transform: "translate(-100%)" },
          { transform: "translate(100%)", duration: 1, ease: "power2.inOut" }
        )
        .then(() => {
          slideElm.style.display = "none";
        });
    } else {
      gsap
        .fromTo(
          "#slide",
          { transform: "translate(100%)" },
          { transform: "translate(-100%)", duration: 1, ease: "power2.inOut" }
        )
        .then(() => {
          slideElm.style.display = "none";
        });
    }
    await sleep(500);
  });
</script>

<div class="layout">
  <div id="slide" bind:this={slideElm} />
  <div class="page">
    <div class="nav">
      <div class="container">
        <div class="title">
          <a class="content" href="/">
            <div class="name">armagan</div>
            <div class="domain">.rest</div>
          </a>
        </div>
        <div class="buttons">
          <a class="button" href="/about-me">Hakkımda</a>
          <a class="button" href="/projects">Projelerim</a>
          <a class="button" href="/links">Linkler</a>
        </div>
      </div>
    </div>
    <div class="slot">
      <slot />
    </div>
  </div>
</div>

<style lang="scss">
  .layout {
    position: relative;
    width: 100vw;
    height: 100vh;
    background-color: var(--theme-white);
    color: var(--theme-black);
    overflow: hidden;

    #slide {
      position: absolute;
      background-color: var(--theme-color);
      width: 100vw;
      height: 100vh;
      z-index: 9999;
      pointer-events: none;
      transform: translateX(-100%);
    }

    & > .page {
      overflow: auto;
      position: absolute;
      inset: 0;
      z-index: 1;
      display: flex;
      flex-direction: column;
      gap: 0.5rem;

      & > .nav {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 50px;

        & > .container {
          padding: 0 1rem;
          max-width: 720px;
          width: 100%;
          display: flex;
          align-items: center;
          justify-content: space-between;
          gap: 0.5rem;

          & > .title {
            display: flex;
            font-size: 2rem;

            & > .content {
              display: flex;
              position: relative;
              cursor: pointer;

              & > .name {
                font-weight: 800;
              }

              & > .domain {
                font-weight: 300;
              }

              &::before {
                content: "";
                display: block;
                border-bottom: 2px solid var(--theme-color);
                position: absolute;
                width: 0;
                right: 0;
                transition: 1s ease;
                bottom: -4px;
              }

              &:hover {
                &::before {
                  width: 100%;
                  left: 0;
                }
              }
            }
          }

          & > .buttons {
            display: flex;
            gap: 0.5rem;
            align-items: center;

            .button {
              font-weight: 400;
              padding: 0.25rem 0.5rem;
              border-bottom: 2px solid var(--theme-color);
              transition: all 0.1s ease;
              font-size: 1rem;

              &:hover {
                color: var(--theme-white);
                background-color: var(--theme-black);
              }
            }
          }
        }

        @media screen and (max-width: 720px) {
          height: 75px;

          & > .container {
            flex-direction: column;
            gap: 0.25rem;
          }
        }
      }
    }
  }
</style>
