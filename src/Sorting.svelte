<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();
    
    let sortings = [
        { title : 'from'},
        { title : 'to'},
        { title : 'subject'},
        { title : 'date'}
    ]
    
    $: appliedSorting = 'date'
    $: ascendingSorting = true
    $: sortingDirection = ascendingSorting ? 'ascending' : 'descending'

    const applySorting = (evt) => {
        const newSorting = evt.srcElement.textContent
        let sortedMails = []
        if (newSorting == appliedSorting) {
            ascendingSorting = !ascendingSorting
        } else {
            appliedSorting = newSorting
            ascendingSorting = true
        }
        
        dispatch('update', {'sortOn': newSorting, 'sortDir': ascendingSorting ? 'asc' : 'desc'})
    }
</script>

<section role="row">
    {#each sortings as sorting, key}
        <button on:click={applySorting} role="columnheader" aria-sort="{appliedSorting === sorting.title ? sortingDirection : undefined}" class={sorting.title}>{sorting.title}</button>
        {#if key != sortings.length -1}
            <span>|</span>
        {/if}
    {/each}
</section>

<style>

    button {
        border: inherit;
        color: inherit;
        font-weight: bold;
        color: #666666;
        margin-bottom: 0;
    }
    button::first-letter {
        text-transform: uppercase;
    }

    section {
        background-color: #F5F5F5;
        padding: .75em;
    }

    [aria-sort] {
        color: #000000;
    }

    /* [aria-sort='ascending']::after {
    } */

    [aria-sort='descending']::after {
        transform: rotateX(180deg);
        transition: transform .25s;
    }

    [aria-sort]::after {
        display: inline-block;
        content: '';
        width: 1em;
        height: .7em;
        background: url(/images/icon_arrow01.svg) no-repeat;
        background-size: .75em 1em;
        background-position-y: 60%;
        background-position-x: center;
        margin-left: .1em;
    }

    @media only screen and (min-width: 768px) {
        section {
            display: flex;
            height: 2em;
        }

        section>span {display: none;}
        section button {
            /* position: absolute; */
            text-align: left;
            padding-left: 0;
            padding-right: 0;
        }
        section button.from,
        section button.to,
        section button.subject {
            padding-right: 1em;
        }

        section button.from { width: calc(40% * 4/10); }
        section button.to { width: calc(40% * 6/10); }
        section button.subject { width: 45%;}
        section button.date { width: 15%;}
    }
</style>

