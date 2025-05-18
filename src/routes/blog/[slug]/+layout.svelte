<script lang="ts">
    import type { Snippet } from 'svelte';
    import type { LayoutData } from './$types';

    let { data, children }: { data: LayoutData, children: Snippet } = $props();
</script>

<div class="layout">
<main>
    {@render children()}
</main>
<aside>
    <h2>More posts</h2>
    <ul>
        {#each data.summaries as { slug, title }}
            <li>
                <a href={`/blog/${slug}`}>{title}</a>
            </li>
        {/each}
    </ul>
</aside>

</div>

<style>
    @media (min-width: 640px) {
        .layout {
            display: grid;
            grid-template-columns: 3fr 1fr;
            gap: 1rem;
        }
    }
</style>