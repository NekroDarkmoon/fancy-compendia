<script>
    import { createEventDispatcher } from "svelte";
    import { localize } from "#runtime/svelte/helper";

    export let active = false;
    export let disabled = false;
    export let label;
    export let orange = false;
    export let optionStyles = "";
    export let red = false;
    export let tight = false;
    export let value;

    const dispatch = createEventDispatcher();
</script>

<li>
    <button
        class="tag"
        style={optionStyles}
        class:tag--active={active}
        class:tag--orange={orange}
        class:tag--red={red}
        class:tag--tight={tight}
        {disabled}
        {value}
        on:click|preventDefault={() => dispatch("tagToggle", value)}
        on:auxclick|preventDefault={() => dispatch("tagToggleAux", value)}
    >
        {localize(label)}
    </button>
</li>

<style lang="scss">
    .tag {
        all: unset;
        background: transparent;
        display: inline;
        padding: 0.15rem 0.4rem;
        border: 1px solid #b5b3a4;
        border-radius: 3px;
        color: inherit;
        transition: all 0.15s ease-in-out;
        white-space: normal;
        cursor: pointer;

        &:hover,
        &:focus {
            background: darken(#dddace, 2.5%);
            color: var(--color-hover, inherit);
        }

        &:disabled,
        &[disabled] {
            background: #aaa;
            border: 1px solid #999;
            color: white;
            cursor: auto;

            &:hover,
            &:focus {
                background: #aaa;
            }
        }

        &--red {
            border-color: darken(#8b2525, 5%);
            background: #8b2525;
            color: lighten(#8b2525, 95%);

            &:hover,
            &:focus {
                background: #8b2525;
                color: lighten(#8b2525, 80%);
            }

            &:disabled,
            &[disabled],
            &:disabled:hover,
            &[disabled]:hover {
                background: #8b2525;
                color: lighten(#8b2525, 95%);
            }
        }

        &--orange {
            border-color: darken(#8b6225, 5%);
            background: #8b6225;
            color: lighten(#8b6225, 95%);

            &:hover,
            &:focus {
                background: #8b6225;
                color: lighten(#8b6225, 80%);
            }

            &:disabled,
            &[disabled],
            &:disabled:hover,
            &[disabled]:hover {
                background: #8b6225;
                color: lighten(#8b6225, 95%);
            }
        }

        &--active {
            border-color: #425f65;
            background: #425f65;
            color: lighten(#425f65, 80%);

            &:hover,
            &:focus {
                background: #425f65;
                color: lighten(#425f65, 80%);
            }

            &:disabled,
            &[disabled] {
                color: lighten(#425f65, 95%);
            }
        }

        &--tight {
            padding: 0.1rem 0.375rem;
        }
    }
</style>
