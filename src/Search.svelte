<script>
    import { createEventDispatcher } from 'svelte'
    // import { formatDate } from 'timeUtils'
    import Datepicker from 'svelte-calendar'
    const dispatch = createEventDispatcher()
    
    export let mails
    let dateFormat = "#{Y}/#{n}/#{j}"

    let resultsCount = mails.length
    let userInput = ''

    const defaultStartDate = '2019/12/31'
    const defaultEndDate = '2020/01/03'

    let dateStartChosen = false
    let dateStartSelected = new Date(defaultStartDate + ' 00:00:00')
    let dateStartFormatted
    let dateEndChosen = false
    let dateEndSelected = new Date(defaultEndDate + ' 23:59:59')
    let dateEndFormatted

    //TODO: 
    const handleSearch = () => {
        dateStartSelected.setHours(0)
        dateStartSelected.setMinutes(0)
        dateStartSelected.setSeconds(0)
        dateStartSelected.setMilliseconds(0)
        dateEndSelected.setHours(23)
        dateEndSelected.setMinutes(59)
        dateEndSelected.setSeconds(59)
        dateEndSelected.setMilliseconds(999)

        const filteredMails = mails.filter(mail => new Date(mail.date) >= dateStartSelected && new Date(mail.date) <= dateEndSelected)
        resultsCount = filteredMails.length
        dispatch('filter', filteredMails)
    }

    const handleKeyUp = (evt) => {
        if (evt.keyCode === 13) {
            handleSearch()
        }
    }

</script>

<section>
    <div class='search'>
        <div class='search-box'>
            <div class='search-date-pickers'>
                <Datepicker format={dateFormat} 
                            bind:formattedSelected={dateStartFormatted} 
                            bind:dateChosen={dateStartChosen}
                            bind:selected={dateStartSelected}>
                    <button class="text-button">
                        {#if dateStartChosen}
                            {dateStartFormatted}
                        {:else} 
                            <span class='placeholder'>
                                {defaultStartDate}
                            </span>
                        {/if}
                    </button>
                </Datepicker>
                <span class={dateStartChosen && dateEndChosen ? '' : 'placeholder'}>&nbsp;-&nbsp;</span>
                <Datepicker format={dateFormat} 
                            bind:formattedSelected={dateEndFormatted} 
                            bind:dateChosen={dateEndChosen} 
                            bind:selected={dateEndSelected}
                            start={dateStartSelected}>
                    <button class="text-button">
                        {#if dateEndChosen}
                            {dateEndFormatted}
                        {:else} 
                            <span class='placeholder'>
                                {defaultEndDate}
                            </span>
                        {/if}
                    </button>
                </Datepicker>
            </div>
            <button on:click={handleSearch} class='submit-search'>
                <img src='/images/icon_search.svg' alt='search icon'/>
            </button>
        </div>
        
<!-- 
        <input bind:value={userInput} on:keyup={handleKeyUp} type='search' placeholder='2020/1/1 - 2020/1/1'>
        <button on:click={handleSearch} class='submit-search'>
            <img src='/images/icon_search.svg' alt='search icon'/>
        </button> -->
    </div>

    <div>
        <span class='results'>Results: <span class='results-count'>{resultsCount}</span> mail(s)</span>
    </div>
</section>

<style>
    section {
        padding: 1em 1em 0.5em 1em;
        border-bottom: solid 1px;
        border-bottom-color: #DEDEDE;
    }

    .search {
        display: flex;
    }

    .search-box {
        display: flex;
    }

    .search-date-pickers {
        display: flex;
        height: calc(2.5em - 2px);
        border-radius: 0.5em 0em 0em 0.5em;
        border: 1px solid #D0D0D0;
        border-right: inherit;
        align-items: center;
        padding: 0 1em 0 2.5em;
        background: url(/images/icon_calender.svg) no-repeat scroll 7px 7px;
        background-size: 1em 1em;
        background-position-y: center;
        background-position-x: 1em;
        
    }

    .placeholder {
        color: #D8D8D8;
    }

    .text-button {
        display: inline;
        cursor: pointer;
        box-shadow: inherit;
        border: inherit;
        background-color: inherit;
        padding: 0;
        margin: 0;
    }

    :global(.text-button button) {
        margin-bottom: 0;
    }

    button.submit-search {
        /* background-color: #F5F5F5; */
        border-color: #DEDEDE;
        border-radius: 0 .5em .5em 0;
        height: 2.5em;
        width: 3em;
    }

    button img {
        width: 1em;
    }

    .results {
        color: #666666;
        font-weight: bold;
    }

    .results-count {
        font-size: 1.2em;
    }
</style>