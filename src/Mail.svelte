<script>
    import { afterUpdate } from 'svelte'
    import { getContext } from 'svelte';

    import { watchResize } from "svelte-watch-resize"

    export let mail

    $: from = mail.from
    $: to = mail.to //.join(', ')
    $: date = mail.date
    $: subject = mail.subject
    $: body = mail.body
    $: hasAttachment = mail.hasAttachment
    
    let displayedDate = ''
    let toMailsContainer
    let bodyContainer
    let withDateClass

    //TODO: ellipsis + badge : count emails that are not visible
    //TODO: move all date format related functions elsewhere (js module)
    const getFormattedMonth = (monthIndex) => {
        
        const formattedMonths = [
            "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
        ]

        return formattedMonths[monthIndex]
    }

    const getWithDateClass = () => {

        let cssClass = ''
        if(displayedDate.length < 6) {
            cssClass = 'with-small-date'
        } else if (displayedDate.length < 8) {
            cssClass = 'with-medium-date'
        } else {
            cssClass = 'with-large-date'
        }

        if(hasAttachment) {
            cssClass += ' with-attachment'
        }

        return cssClass
    }

    const getFormattedDate = () => {
        const dateSent = new Date(date)
        const today = new Date('2020-01-02T22:20:10.509Z')
    
        if(dateSent.getFullYear() === today.getFullYear() &&
            dateSent.getMonth() === today.getMonth() &&
            dateSent.getDay() === today.getDay()
        ) {
            // if mail was sent today, display hour & minutes
            return `${date.substring(12,13)}:${date.substring(14,16)}`
        } else if (dateSent.getFullYear() === today.getFullYear()) {
            // if mail was sent earlier this year, display month & day
            return `${getFormattedMonth(dateSent.getMonth())} ${date.substring(8,10)}`
        } else {
            // if mail is older, display full date
            return `${dateSent.getFullYear()}/${date.substring(5,7)}/${date.substring(8,10)}`
        }
    }

    const countInvisibleEmails = (parentNode) => {
        let invisibleCount = 0
        // console.log('.to offsetWidth: ' + parentNode.offsetWidth)
        // console.log('.to offsetLeft: ' + parentNode.offsetLeft)

        //parentNode.querySelectorAll('span').forEach(email, key => {
        let emails = parentNode.querySelectorAll('span.email')
        for(let key in emails) {
            // console.log('email offsetWidth: ' + email.offsetWidth)
            // console.log('email offsetLeft: ' + email.offsetLeft)
            const email = emails[key]
            if(key > 0) {
                if(email.offsetLeft + email.offsetWidth > parentNode.offsetWidth) {
                    invisibleCount++
                    // console.log(email)
                    email.style.color = 'white'
                } else {
                    if(email.style.color == 'white') {
                        email.style.color = 'inherit'
                    }
                }
            }
        }

        // if(invisibleCount > 0) {
        //     parentNode.parentNode.parentNode.querySelector('.count-hidden-emails').textContent = '+' + invisibleCount
        // } else {
        //     parentNode.parentNode.parentNode.querySelector('.count-hidden-emails').textContent = ''
        // }
        
        to.hidden = invisibleCount

        // console.log(invisibleCount)
    }

    const handleToggleBodyDisplay = () => {
        if(bodyContainer.style.display != 'block') {
            bodyContainer.style.display = 'block'
        } else {
            bodyContainer.style.display = 'none'
        }

    }

    afterUpdate(() => {
        displayedDate = getFormattedDate()
        countInvisibleEmails(toMailsContainer)
        withDateClass = getWithDateClass()
    })

</script>

<!-- <svelte:window on:resize={countInvisibleEmails} /> -->

<div class='mail-container' on:click={handleToggleBodyDisplay}>
    <div class='fromto-container'>
        <div class={'from ' + withDateClass}>
            <div>
                <span>
                    {from}
                </span>
            </div>
        </div>
        <div class='to'>
            <div on:load={countInvisibleEmails} 
                use:watchResize={countInvisibleEmails}
                bind:this={toMailsContainer}>
                {#each to as email, key}
                    <span class='email'>{email}{#if key != to.length - 1},&nbsp;{/if}</span>
                {/each}
                
            </div>
            <span contenteditable='true' class='count-hidden-emails'>{to.hidden > 0 ? '+' + to.hidden : ''}</span> 
        </div>
    </div>
    <div class='subject'>
        <div>
            <span>{subject}</span>
        </div>
    </div>
    <div class="aside">
        <!-- {#if hasAttachment} -->
            <div class="attachment">
                {#if hasAttachment}
                    <img src="/images/icon_clip.svg" alt="attachment icon"/>
                {/if}
            </div>    
        <!-- {/if} -->
        <span class="when">
            {displayedDate}
        </span>
    </div>
</div>
<div bind:this={bodyContainer} class="body">
    {body}
</div>

<style>
    .body {
        background: #dedede;
        padding: 1em;
        display: none;
        
    }
    .mail-container {
        cursor: pointer;
        padding: 0.75em 0em;
        border-bottom: solid 1px;
        border-bottom-color: #DEDEDE;
    }

    .attachment {
        width: 1em;
        margin-right: 0.25em;
        margin-top: 0.125em;
    }

    .aside {
        display: flex;
    }

    .count-hidden-emails {
        color: white;
        background-color: #666;
        padding: 0 0.25em;
        border-radius: 0.25em;
        position: absolute;
    }

    .count-hidden-emails {
        right: 0;
        top: 0;
    }
    .count-hidden-emails:empty {
        display: none;
    }

    .subject {
        font-size: 1.1em;
    }

    @media only screen and (min-width: 768px) {
        /* .fromto-container {
        } */

        .mail-container {
            display: flex;
        }

        .mail-container:hover {
            color: #0033DD;
            background-color: #F6F8FA;
        }

        .mail-container:hover .when {
            color: #333;
        }
        .mail-container:hover img {
            /* fill: #0033DD; */
            filter: invert(13%) sepia(77%) saturate(7302%) hue-rotate(230deg) brightness(85%) contrast(108%);
        }

        .from,
        .to {
            padding-right: 1em;
        }

        .fromto-container {
            display: flex;
            width: 40%;
            min-width: 40%;
        }

        .from {
            width: 40%;
        }

        .to {
            width: 60%;
            position: relative;
        }

        .subject {
            width: 45%;
            /* padding-right: 2.5em; */
            /* flex-grow: 1; */
        }

        .to > div,
        .subject > div {
            width: calc(100% - 2.5em);
        }

        .to > div {
            overflow: hidden;
            text-overflow: ellipsis;
        }

        /* .subject span {
            padding-right: 1.25em;
        } */

        .aside {
            width: 15%;
            position: relative;
        }

        .attachment {
            position: absolute;
            left: -1.25em;
            margin-right: inherit;
        }
        .when {
            width: 100%;
            font-weight: bold;
        }

        .count-hidden-emails {
            right: 1em;
            top: 0;
        }
    }

    /* note: it max-width: 767px, and screen-width = 767px, this media rule is not applied... strange ! */
    /* don't want to use max-width: 768px because it would merge with 'desktop css' rules above when screen-width=768px ... :D */
    @media only screen and (max-width: 767.5px) {
        .mail-container {
            position: relative;
        }

        .aside {
            position: absolute;
            top: 0.75em;
            right: 0;
        }

        .from {
            font-weight: bold;
        }

        .from.with-large-date {
            width: calc(100% - 6em);
        }
        .from.with-large-date.with-attachment {
            width: calc(100% - 6em - 1em);
        }

        .from.with-medium-date {
            width: calc(100% - 4em);
        }
        .from.with-medium-date.with-attachment {
            width: calc(100% - 4em - 1em);
        }

        .from.with-small-date {
            width: calc(100% - 3em);
        }
        .from.with-small-date.with-attachment {
            width: calc(100% - 3em - 1em);
        }
        
        .to { 
            margin-top: 0.125em;
            position: relative;
        }

        .to > div {
            width: calc(100% - 0.5em);
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .fromto-container {
            background: url(/images/icon_mail_sp.svg) no-repeat left 60%;
            background-size: 0.86em;
            padding-left: 1.125em;
            display: flex;
            flex-direction: column;
            margin-bottom: 0.25em;
            /*padding-right: 4em;*/ /* padding to prevent overlay with "when" info */
        }

        .when::after {
            content: '';
            background: url(/images/icon_arrow02.svg) no-repeat right 80%;
            display: inline-block;
            background-size: 0.25em;
            width: 0.5em;
            height: 1em;
        }
    }

    .from, .from > div, .to, .subject, .subject > div {
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }

</style>