<script lang="ts">
    import Found from "./Found.svelte";
import Grid from "./Grid.svelte";
    import { levels } from "./levels";
    import type { Level } from "./levels";
    import { shuffle } from "./utils";

    const level = levels[0];

    let size: Number = level.size;
    let grid: string[] = create_grid(level);
    let found: string[] = [];

    function create_grid(level: Level) {
        const copy = level.emojis.slice();
        const paris: string[] = [];
        for (let i = 0; i < level.size ** 2 / 2; i++) {
            const index = Math.floor(Math.random() * copy.length);
            const emoji = copy[index];

            copy.splice(index, 1);
            paris.push(emoji);
        }

        paris.push(...paris);
        return shuffle(paris);
    }
</script>

<div class="game">
    <div class="info" />
    <div class="grid-container">
        <Grid
            {grid}
            on:found={(e) => {
                found = [...found, e.detail.emoji];
            }}
            {found}
        />
    </div>
    <div class="info">
        <Found {found}></Found>
    </div>
</div>

<style>
    .game {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100%;
        font-size: min(1vmin, 0.3rem);
    }
    .info {
        width: 80vmin;
        height: 10vmin;
        background: purple;
    }
    .grid-container {
        width: 80vmin;
        height: 80vmin;
        background: teal;
    }
</style>
