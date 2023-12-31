<script>
    import { getContext } from "svelte";
    import { localize } from "#runtime/svelte/helper";

    import ImportButton from "../ImportButton.svelte";

    export let document;

    function getAttunementLabel(object) {
        return object.system.attunement
            ? localize("DND5E.AttunementRequired")
            : null;
    }

    function getObjectDetailsLabel(item) {
        const attunement = getAttunementLabel(item);
        const { price } = item.system;
        const rarity = getRarityLabel(item);

        if (rarity) {
            if (price.value && attunement)
                return `${rarity} (${attunement}; Cost ${price.value} ${price.denomination})`;
            if (price.value)
                return `${rarity} (Cost ${price.value} ${price.denomination})`;
            if (attunement) return `${rarity} (${attunement})`;

            return rarity;
        }

        if (price.value && attunement)
            return `${attunement}; Cost ${price} ${price.denomination}`;
        if (price.value) return `Cost ${price.value} ${price.denomination}`;
        if (attunement) return attunement;

        return null;
    }

    function getRarityLabel(object) {
        const { rarity } = object.system;

        if (!rarity) return null;

        return (itemRarity[rarity] ?? rarity).capitalize();
    }

    function onDragStart(event) {
        const data = {
            type: collection.documentName,
            uuid: collection.getUuid(document._id),
        };
        return event.dataTransfer.setData("text/plain", JSON.stringify(data));
    }

    const collection = getContext("collection");
    const { itemRarity } = CONFIG.DND5E;

    $: objectDetails = getObjectDetailsLabel(document);
</script>

<!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
<li
    class="fc-document"
    draggable="true"
    on:click={async () => {
        const doc =
            collection.get(document._id) ??
            (await collection.getDocument(document._id));
        doc.sheet?.render(true);
    }}
    on:dragstart={onDragStart}
>
    <img class="fc-document__image" src={document.img} alt={document.name} />

    <h3 class="fc-document__name">
        {document.name}

        {#if document.system?.requiresAttunement}
            <i
                class="fc-document__icon fa-solid fa-link"
                data-tooltip="Requires Attunement"
                data-tooltip-direction="UP"
            />
        {/if}
    </h3>

    <span class="fc-document__details">
        {objectDetails}
    </span>

    <ImportButton {document} />
</li>
