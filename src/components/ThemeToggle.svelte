<!-- Totally not stolen from vencord.dev -->
<!-- https://github.com/Vencord/vencord.dev/blob/main/src/components/ThemeToggle.svelte -->
<script lang="ts">
    import { writable } from "svelte/store";
    import { IS_SERVER } from "../consts";

    const options = ["Light", "Dark"] as const;

    const initialValue = IS_SERVER
        ? "Dark"
        : (() => {
              const stored = localStorage.theme;
              if (stored && options.includes(stored)) return stored;

              if (window.matchMedia("(prefers-color-scheme: light)").matches)
                  return "Light";
              else return "Dark";
          })();

    const selected = writable(initialValue);
    if (!IS_SERVER) {
        selected.subscribe(v => {
            localStorage.theme = v;
            v = v.toLowerCase();
            const prev = v === "light" ? "dark" : "light";
            document.body.classList.remove(prev);
            document.body.classList.add(v);
        });
    }

    function themeSwitch() {
        selected.update(x => (x = x === "Light" ? "Dark" : "Light"));
    }
</script>

<button on:click={themeSwitch} class="theme-toggle">
    <!-- {#if $selected === "Light"}
        <slot name="dark" />
    {:else if $selected === "Dark"}
        <slot name="light" />
    {/if} -->
    <slot />
</button>

<style>
    button {
        all: unset;
        cursor: pointer;
    }
    button slot {
        display: inline-block;
        width: 2em;
        height: 2em;
        color: var(--clr-text);
    }
    button slot {
        color: var(--clr-accent);
    }
</style>