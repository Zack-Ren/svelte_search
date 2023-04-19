<script lang="ts">
    import debounce from "lodash/debounce";
    import Item from "./Item.svelte";
    import { data } from "./mockData";

    let filteredResults:string[] = [];
    let inputValue:string = "";
    let searchInput:any;
    let idx:number = -1;

    async function fetchResults() {
        await new Promise((resolve) => setTimeout(resolve, 20));
        let tempArr:string[] = [];
        if (inputValue) {
            data.forEach(d => {
                if (d.toLowerCase().startsWith(inputValue.toLowerCase())) {
                    tempArr = [...tempArr, boldMatch(d)];
                }
            });
        }
        // tempArr.length > 0 ? filteredResults = tempArr.slice(0, 8) : filteredResults = ["No results found"];
        filteredResults = tempArr.slice(0,10);
    }

    const filterResults = debounce(() => {fetchResults()}, 1000)
    
    $: if (!inputValue) {
        filteredResults = [];
        idx = -1;
    }

    const clearInput = () => {
        inputValue = "";
        searchInput.focus();
    }

    const setInputValue = (resultName:string) => {
        inputValue = removeBold(resultName);
        filteredResults = [];
        idx = -1;
        document.querySelector<HTMLInputElement>('#search')?.focus();
        submitValue();
    }

    const submitValue = () => {
        if (inputValue) {
            console.log(inputValue);
            clearInput();
        }
        else {}
    }

    const boldMatch = (str:string) => {
        let matched = str.substring(0, inputValue.length);
        let makeBold = `<strong>${matched}</strong>`;
        let boldMatch = str.replace(matched, makeBold);
        return boldMatch;
    }

    const removeBold = (str:string) => {
        if (str.length > 0) {
            return str.replaceAll(/<[^>]*>/g, "");
        }
        else return str;
    }

    const navList = (e:any) => {
        if (filteredResults.length > 0) {
            if (e.key === "ArrowDown" && idx <= filteredResults.length - 1) {
                idx === -1 || idx === filteredResults.length-1 ? idx = 0 : idx += 1;
                inputValue = removeBold(filteredResults[idx]);
            } else if (e.key === "ArrowUp") {
                idx <= 0 ? idx = filteredResults.length - 1 : idx -= 1;
                inputValue = removeBold(filteredResults[idx]);
            } 
        } else {
            return;
        }

    }
</script>

<svelte:window on:keydown={navList} />

<form class="w-11/12 md:w-3/4 lg:w-1/2 m-auto" on:submit={submitValue}>
    <h1 class="text-5xl font-medium dark:text-slate-400 w-full text-center mb-6">Relic</h1>
    <div class="relative w-full">
        <div class="flex absolute inset-y-0 items-center text-gray-500 dark:text-gray-400 left-0 pl-2.5 pointer-events-none">
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd"></path></svg>
        </div>
        <input placeholder="Search web3 anything" class="block w-full disabled:cursor-not-allowed disabled:opacity-50 pl-11 focus:border-blue-500 focus:ring-blue-500 dark:focus:border-blue-500 dark:focus:ring-blue-500 bg-gray-50 text-gray-900 dark:bg-gray-700 dark:text-white dark:placeholder-gray-400 border-gray-300 dark:border-gray-600  p-4 sm:text-base rounded-lg" id="search" spellcheck="false" bind:value={inputValue} bind:this={searchInput} on:input={filterResults}>
        {#if filteredResults.length > 0}
            <ul  id="autocomplete-list">
                {#each filteredResults as result, i} 
                    <Item itemLabel={result} highlighted={i===idx} on:click={() => setInputValue(result)}/>
                {/each}
            </ul>
        {/if}
    </div>
    <p class="text-sm dark:text-slate-600 leading-normal font-normal text-right whitespace-normal mt-2 z-0">Press ⏎ to submit</p>
    
</form>

<style>
    #autocomplete-list {
        position: absolute;
        margin-top: 50px;
        padding: 0;
        top: 0;
        width: 100%;
        border-bottom-right-radius: 0.6rem;
        border-bottom-left-radius: 0.6rem;
        background-color: #ddd;
    }

    input:focus {
        outline: none;
    }

    input:hover {
        background-color: rgb(75 85 99)
    }
</style>