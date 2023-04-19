<script lang="ts">
    export let itemLabel:string;
    export let highlighted:boolean;
</script>
  
<li class="autocomplete-items" class:autocomplete-active={highlighted} on:click on:keydown>{@html itemLabel}</li>

<style>
    li.autocomplete-items {
        list-style: none;
        z-index: 99;
        top: 100%;
        left: 0;
        right: 0;
        padding: 10px;
        cursor: pointer;
        color: white;
        background-color: rgb(55 65 81);
    }

    li:last-child {
        border-bottom-left-radius: 0.5rem;
        border-bottom-right-radius: 0.5rem;
    }

    li.autocomplete-items:hover {
        /* mouse hover */
        background-color: rgb(75 85 99);
        color: white;
    }

    li.autocomplete-items:active {
        /* arrow keys */
        background-color: rrgb(75 85 99) !important;
        color: #ffffff;
    }	
    
    .autocomplete-active {
        /* arrow keys */
        background-color: rgb(107 114 128) !important;
        color: #ffffff;
    }
</style>
