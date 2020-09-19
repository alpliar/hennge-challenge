<script>
    import Mail from './Mail.svelte'
    import Search from './Search.svelte'
    import Sorting from './Sorting.svelte'
    
    //TODO: update mails from mockups
    let mails = [
        {
            from: 'aaa@example.com',
            to: ['zzz.zzz@example.com'],
            subject: '[ HR-888 ] Notice of official announcement',
            date: '2020-01-02T00:20:10.509Z',
            body: `The English word company has its origins in the Old French term compagnie (first recorded in 1150), meaning a "society, friendship, intimacy; body of soldiers", which came from the Late Latin word companio ("one who eats bread with you"), first attested in the Lex Salica (English: Salic Law) (c. 500 CE) as a calque of the  Germanic expression gahlaibo (literally, "with bread"), related to Old High German galeipo ("companion") and to Gothic gahlaiba ("messmate").`,
            hasAttachment: false
        },
        {
            from: 'bbb.bbbb@example.com',
            to: ['yyy@example.com'],
            subject: '[web:333] "Web Contact"',
            date: '2020-01-02T00:10:43.511Z',
            body: `Nokia launched mobile TV trials in 2005 in Finland with content provided by public broadcaster Yle. The services are based on the DVB-H standard. It could be viewed with the widescreen Nokia 7710 smartphone with a special accessory enabling it to receive DVB-H signals.`,
            hasAttachment: false
        },
        {
            from: 'ccc@example.com',
            to: ['xxx@example.com', 'www.www@example.com'],
            subject: 'Happy New Year! Greetings for the New Year.',
            date: '2020-01-02T00:00:43.509Z',
            body: `The Galactic year is the time it takes Earth's Solar System to revolve once around the galactic center. It comprises roughly 230 million Earth years.`,
            hasAttachment: true
        },
        {
            from: 'ddd.dddd@example.com',
            to: ['vvv.vvv@example.com', 'www.www@example.com'],
            subject: '[HR-887(Revised: Office Expansion Project Team)] Notice of off...',
            date: '2020-01-01T00:10:43.511Z',
            body: `In geometry, expansion is a polytope operation where facets are separated and moved radially apart, and new facets are formed at separated elements (vertices, edges, etc.). Equivalently this operation can be imagined by keeping facets in the same position but reducing their size.`,
            hasAttachment: false
        },
        {
            from: 'eee@example.com',
            to: ['sss@example.com', 'www.www@example.com', 'ttt@example.com'],
            subject: '[Github] Logout page',
            date: '2020-01-01T00:10:43.511Z',
            body: `Logging out of a computer, when leaving it, is a common security practice preventing unauthorised  users from tampering with it. There are also people who choose to have a password-protected screensaver set to activate after some period of inactivity, thereby requiring the user to re-enter his or her login credentials to unlock the screensaver and gain access to the system. There can be different methods of logging in that may be via image, fingerprints, eye scan, password (oral or textual input), etc.`,
            hasAttachment: false
        },
        {
            from: 'fff.ffff@example.com',
            to: ['qqq.qqq@example.com'],
            subject: '[dev] Postfix 3.1.12 / 3.2.9 / 3.3.4 / 3.4.5',
            date: '2020-01-01T00:10:43.511Z',
            body: `The size of patches may vary from a few bytes to hundreds of megabytes; thus, more significant changes imply a larger size, though this also depends on whether the patch includes entire files or only the changed portion(s) of files. In particular, patches can become quite large when the changes add or replace non-program data, such as graphics and sounds files. Such situations commonly occur in the patching of computer games.  Compared with the initial installation of software, patches usually do not take long to apply.`,
            hasAttachment: false
        },
        {
            from: 'ggg@example.com',
            to: ['ppp@example.com'],
            subject: 'Re: [Github] Brush-up on loading animation',
            date: '2020-01-01T00:10:43.511Z',
            body: `Rotoscoping is a technique patented by Max Fleischer in 1917 where animators trace live-action movement, frame by frame. as in The Lord of the Rings (US, 1978), or used in a stylized and expressive manner, as in Waking Life (US, 2001) and A Scanner Darkly (US, 2006). Some other examples are Fire and Ice (US, 1983), Heavy Metal (1981), and Aku no Hana (2013).`,
            hasAttachment: false
        },
        {
            from: 'hhh.hhh@example.com',
            to: ['ooo.ooo@example.com'],
            subject: 'Workplace Summary for sample, Inc.: Jun 2 - Jun 9',
            date: '2020-01-01T00:10:43.511Z',
            body: `A workplace is a location where someone works for his or her employer, a place of employment. Such a place can range from a home office to a large office building or factory. For industrialized societies, the workplace is one of the most important social spaces other than the home, constituting "a central concept for several entities: the worker and his/her family, the employing organization, the customers of the organization, and the society as a whole".[1] The development of new communication technologies have led to the development of the virtual workplace, a workplace that is not located in any one physical space.`,
            hasAttachment: true
        },
        {
            from: 'iii@example.com',
            to: ['nnn@example.com'],
            subject: 'I love you',
            date: '2019-12-31T00:10:43.511Z',
            body: `Agape (ἀγάπη agápē) means love in modern-day Greek. The term s'agapo means I love you in Greek. The word agapo is the verb I love. It generally refers to a "pure," ideal type of love, rather than the physical attraction suggested by eros. However, there are some examples of agape used to mean the same as eros. It has also been translated as "love of the soul."`,
            hasAttachment: true
        },
        {
            from: 'Pablo-Diego-José-Francisco@example.com',
            to: ['Pablo-Diego-José-Francisco@example.com'],
            subject: '[info:888] ABC EQUIPMENT COMPANY',
            date: '2019-12-31T00:10:43.511Z',
            body: `Electrical equipment includes any machine powered by electricity. It usually consists of an enclosure, a variety of electrical components, and often a power switch.`,
            hasAttachment: false
        }
    ]

    //TOOD: move this sorting code elsewhere (Sorting.svelte ?)
    let sortingProperty = 'date'
    let sortingDirection = 'asc'

    const sortBy = (Array, sortOn, sortDir) => {
        let sortAttr1
        let sortAttr2

        sortAttr1 = sortDir == 'asc' ? 1 : -1
        sortAttr2 = sortDir == 'asc' ? -1 : 1

        switch(sortOn) {
            case 'date':
                return mails.sort((a, b) => (a.date > b.date) ? sortAttr1 : sortAttr2)
                break;
            case 'from':
                return mails.sort((a, b) => (a.from.toLowerCase() > b.from.toLowerCase()) ? sortAttr1 : sortAttr2)
                break;
            case 'to':
                return mails.sort((a, b) => (a.to[0].toLowerCase() > b.to[0].toLowerCase()) ? sortAttr1 : sortAttr2)
                break;
            case 'subject':
                return mails.sort((a, b) => (a.subject.toLowerCase() > b.subject.toLowerCase()) ? sortAttr1 : sortAttr2)
                break;
        }
    }

    
    let filteredMails = null
    $: sortedMails = sortBy(mails, sortingProperty, sortingDirection)
    $: displayedMails = filteredMails != null ? filteredMails : sortedMails
    
    const handleSorting = (sort) => {
        sortingProperty = sort.detail.sortOn
        sortingDirection = sort.detail.sortDir
    }

    const handleFilter = (filter) => {
        // console.log("filter.detail")
        // console.log(filter.detail)
        filteredMails = filter.detail
    }

</script>

<Search {mails} on:filter={handleFilter} />
<div class={displayedMails.length == 0 ? "emails-list" : ""}>
    {#if displayedMails.length > 0}
        <Sorting on:update={handleSorting} />
    {/if}
    <div class="email-container">
        {#each displayedMails as mail}
            <Mail {mail} />
        {/each}
    </div>
</div>

<style>

.emails-list {
    min-height: 50vw;
    background-image: url(/images/logo.png);
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-position: center; 
}

.email-container {
    padding: 0em 0.75em;
}

</style>