<script lang="ts">
    import type { InternalItemCardProps } from "./ItemCard.svelte";
    import ItemAttributes from "./components/ItemAttributes.svelte";
    import ItemFooter from "./components/ItemFooter.svelte";
    import ItemImage from "./components/ItemImage.svelte";
    import { goto } from "$app/navigation";
    import { getFormatter } from "$lib/i18n";

    const { defaultImage: _defaultImage, ...props }: InternalItemCardProps = $props();
    const t = getFormatter();
</script>

<div class="compact-wish">
    {#if props.item.imageUrl}
        <div class="compact-wish-thumbnail">
            <ItemImage class="size-14 rounded-lg object-contain" {...props} shrinkBadge badgeClass="sr-only">
                {#snippet defaultImage(t)}
                    {@render _defaultImage(t, "size-14 rounded-lg")}
                {/snippet}
            </ItemImage>
        </div>
    {/if}
    <div class="compact-wish-content">
        <header class="compact-wish-title">
            {#if props.reorderActions}
                <span id={`${props.id}-name`} data-testid="name">{props.item.name}</span>
            {:else}
                <button
                    id={`${props.id}-name`}
                    data-testid="name"
                    onclick={(e) => {
                        e.stopPropagation();
                        goto(`?item-id=${props.item.id}`, { replaceState: true, noScroll: true });
                    }}
                >{props.item.name}</button>
            {/if}
            {#if props.item.mostWanted}
                <span class="compact-wish-badge"><iconify-icon icon="ion:star"></iconify-icon>{$t("wishes.most-wanted")}</span>
            {/if}
        </header>
        <div class="compact-wish-meta"><ItemAttributes {...props} /></div>
    </div>
    <div class="compact-wish-actions"><ItemFooter {...props} /></div>
</div>
