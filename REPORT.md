# REPORT

### Why Svelte ?
I have spent some times during COVID-19 lockdown working on Svelte.js, but nothing work-related / serious

I have enjoyed the time spent to explore this technology, and was impressed by how little code it takes to write reliable and reactive interfaces

## Mission 1: Design the UI using the given images

### Analysis

Identified features : 
* Header 
    * Search input
        * from date
        * end date
        * submit button
        * css
    * Search result count
        * fixed string
        * dynamic count of results
        * css
* Search results
    * Sorting options
        * selected sorting
        * other sortings
        * sorting direction management (asc / desc)
    * Results
        * Mail Archiver logo when results count = 0
        * from / to container
            * from
                * text overflow management
                * bold text
            * to
                * text overflow management
                * hidden emails count + overflow management
            * from / to icon
        * subject
        * aside informations
            * date
                * display only hours:minutes if date is today
                * display month day if date is within this year
                * display full date for older mails
            * attachment
            * decorative arrow
* hi-res screens specific parts
    * header
        * no adjustments
    * Sorting options
        * fixed width columns
        * hide separators
    * Search results
        * from / to container
            * hide from / to icon
            * remove bold on 'from'
        * date
            * text in bold
            * hide decorative arrow
