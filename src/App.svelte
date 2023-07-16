<script>
import { writable } from "svelte/store";
import DCodeLogoUrl from './assets/dcode_logo.png'
import ErrorismLogoUrl from './assets/errorism_logo.png'
import { createDropdownMenu } from '@melt-ui/svelte'
const { menu, item, trigger, arrow } = createDropdownMenu()
export const TodoList = writable(JSON.parse(localStorage.getItem("todolist")));
TodoList.subscribe(value => {
    localStorage.setItem("todolist", (typeof value === "object" ) ? JSON.stringify(value) : value);
})

/**
 * Add new item to list
 * @param {string} value
 * @returns {void}
 */
function add(value) {
  TodoList.update(n => {
    n.push(value);
  })
}
/**
 * remove selected item from list
 * @param {number} index
 * @returns {void}
 */
 function remove(index) {
  TodoList.update(n => {
    n.splice(index, 1);
  })
}

</script>

<main>
  <div class="relative flex flex-col items-center justify-start min-w-full min-h-screen overflow-hidden bg-base-1 ">
    <div class="absolute opacity-[0.03]">
      <img class="select-none md:scale-[4.0] scale-[3.0] rotate-12 md:-rotate-12 transition-all duration-300" src="{ErrorismLogoUrl}" alt="errorism logo">
    </div>
    <div class="container z-10 mx-auto">
    <!-- app bar -->
    <div class="h-[134px] flex items-center justify-start px-2 relative">
      <div class="flex items-center justify-center gap-x-3">
        <img src="{DCodeLogoUrl}" alt="Dcode logo">
        <div class="flex flex-col justify-center leading-tight">
          <div class="text-[32px] font-bold text-base-12">Todo List</div>
          <div class="text-[16px] font-base text-base-11">D*CODE research laboratory </div>
        </div>
      </div>
      <div class="absolute right-2 sm:hidden">
        <div melt={$menu} class="z-30 w-40 rounded-sm bg-base-3 focus:!outline-none">
          <div {...$item} use:item class="m-1 ">
            <button on:click={()=>{window.location.href = 'https://portfolio.errorism.cc/'}} class="flex items-center w-full gap-1 p-1 text-xs transition-all duration-300 rounded-sm group hover:bg-brand-5 text-brand-12">
              <svg class="w-3 h-3 transition-transform duration-300 text-base-12 group-hover:-rotate-12" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M7.22303 0.665992C7.32551 0.419604 7.67454 0.419604 7.77702 0.665992L9.41343 4.60039C9.45663 4.70426 9.55432 4.77523 9.66645 4.78422L13.914 5.12475C14.18 5.14607 14.2878 5.47802 14.0852 5.65162L10.849 8.42374C10.7636 8.49692 10.7263 8.61176 10.7524 8.72118L11.7411 12.866C11.803 13.1256 11.5206 13.3308 11.2929 13.1917L7.6564 10.9705C7.5604 10.9119 7.43965 10.9119 7.34365 10.9705L3.70718 13.1917C3.47945 13.3308 3.19708 13.1256 3.25899 12.866L4.24769 8.72118C4.2738 8.61176 4.23648 8.49692 4.15105 8.42374L0.914889 5.65162C0.712228 5.47802 0.820086 5.14607 1.08608 5.12475L5.3336 4.78422C5.44573 4.77523 5.54342 4.70426 5.58662 4.60039L7.22303 0.665992Z" fill="currentColor"></path></svg>
              Portfolio
            </button>
          </div>
          <div {...$item} use:item class="m-1">
            <button on:click={()=>{window.location.href = 'https://github.com/Errorismx/tdl'}} class="flex items-center w-full gap-1 p-1 text-xs transition-all duration-300 rounded-sm group hover:bg-brand-5 text-brand-12">
              <svg class="w-3 h-3 transition-transform duration-300 text-base-12 group-hover:-rotate-12 " width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M7.49933 0.25C3.49635 0.25 0.25 3.49593 0.25 7.50024C0.25 10.703 2.32715 13.4206 5.2081 14.3797C5.57084 14.446 5.70302 14.2222 5.70302 14.0299C5.70302 13.8576 5.69679 13.4019 5.69323 12.797C3.67661 13.235 3.25112 11.825 3.25112 11.825C2.92132 10.9874 2.44599 10.7644 2.44599 10.7644C1.78773 10.3149 2.49584 10.3238 2.49584 10.3238C3.22353 10.375 3.60629 11.0711 3.60629 11.0711C4.25298 12.1788 5.30335 11.8588 5.71638 11.6732C5.78225 11.205 5.96962 10.8854 6.17658 10.7043C4.56675 10.5209 2.87415 9.89918 2.87415 7.12104C2.87415 6.32925 3.15677 5.68257 3.62053 5.17563C3.54576 4.99226 3.29697 4.25521 3.69174 3.25691C3.69174 3.25691 4.30015 3.06196 5.68522 3.99973C6.26337 3.83906 6.8838 3.75895 7.50022 3.75583C8.1162 3.75895 8.73619 3.83906 9.31523 3.99973C10.6994 3.06196 11.3069 3.25691 11.3069 3.25691C11.7026 4.25521 11.4538 4.99226 11.3795 5.17563C11.8441 5.68257 12.1245 6.32925 12.1245 7.12104C12.1245 9.9063 10.4292 10.5192 8.81452 10.6985C9.07444 10.9224 9.30633 11.3648 9.30633 12.0413C9.30633 13.0102 9.29742 13.7922 9.29742 14.0299C9.29742 14.2239 9.42828 14.4496 9.79591 14.3788C12.6746 13.4179 14.75 10.7025 14.75 7.50024C14.75 3.49593 11.5036 0.25 7.49933 0.25Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
              Repository
            </button>
          </div>

          <div melt={$arrow} />
        </div>
        <button melt={$trigger} class:-rotate-12={!$menu.hidden} class:!text-base-12={!$menu.hidden}  class="transition-all duration-300 focus:outline-none text-base-9 hover:text-base-12 hover:-rotate-12">
          <svg class="w-7 h-7" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M1.5 3C1.22386 3 1 3.22386 1 3.5C1 3.77614 1.22386 4 1.5 4H13.5C13.7761 4 14 3.77614 14 3.5C14 3.22386 13.7761 3 13.5 3H1.5ZM1 7.5C1 7.22386 1.22386 7 1.5 7H13.5C13.7761 7 14 7.22386 14 7.5C14 7.77614 13.7761 8 13.5 8H1.5C1.22386 8 1 7.77614 1 7.5ZM1 11.5C1 11.2239 1.22386 11 1.5 11H13.5C13.7761 11 14 11.2239 14 11.5C14 11.7761 13.7761 12 13.5 12H1.5C1.22386 12 1 11.7761 1 11.5Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
        </button>
      </div>

      <div class="absolute hidden right-2 sm:block">
        <div class="flex flex-col items-end justify-center">
          <button on:click={()=>{window.location.href = 'https://portfolio.errorism.cc/'}} class="flex items-center gap-1 transition-all duration-300 group text-base-9 hover:text-base-12 hover:scale-110">
            <svg class="transition-transform duration-300 group-hover:-rotate-12" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M7.22303 0.665992C7.32551 0.419604 7.67454 0.419604 7.77702 0.665992L9.41343 4.60039C9.45663 4.70426 9.55432 4.77523 9.66645 4.78422L13.914 5.12475C14.18 5.14607 14.2878 5.47802 14.0852 5.65162L10.849 8.42374C10.7636 8.49692 10.7263 8.61176 10.7524 8.72118L11.7411 12.866C11.803 13.1256 11.5206 13.3308 11.2929 13.1917L7.6564 10.9705C7.5604 10.9119 7.43965 10.9119 7.34365 10.9705L3.70718 13.1917C3.47945 13.3308 3.19708 13.1256 3.25899 12.866L4.24769 8.72118C4.2738 8.61176 4.23648 8.49692 4.15105 8.42374L0.914889 5.65162C0.712228 5.47802 0.820086 5.14607 1.08608 5.12475L5.3336 4.78422C5.44573 4.77523 5.54342 4.70426 5.58662 4.60039L7.22303 0.665992Z" fill="currentColor"></path></svg>
            Portfolio
          </button>
          <button on:click={()=>{window.location.href = 'https://github.com/Errorismx/tdl'}} class="flex items-center gap-1 transition-all duration-300 group text-base-9 hover:scale-110 hover:text-base-12">
            <svg class="transition-transform duration-300 group-hover:-rotate-12" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M7.49933 0.25C3.49635 0.25 0.25 3.49593 0.25 7.50024C0.25 10.703 2.32715 13.4206 5.2081 14.3797C5.57084 14.446 5.70302 14.2222 5.70302 14.0299C5.70302 13.8576 5.69679 13.4019 5.69323 12.797C3.67661 13.235 3.25112 11.825 3.25112 11.825C2.92132 10.9874 2.44599 10.7644 2.44599 10.7644C1.78773 10.3149 2.49584 10.3238 2.49584 10.3238C3.22353 10.375 3.60629 11.0711 3.60629 11.0711C4.25298 12.1788 5.30335 11.8588 5.71638 11.6732C5.78225 11.205 5.96962 10.8854 6.17658 10.7043C4.56675 10.5209 2.87415 9.89918 2.87415 7.12104C2.87415 6.32925 3.15677 5.68257 3.62053 5.17563C3.54576 4.99226 3.29697 4.25521 3.69174 3.25691C3.69174 3.25691 4.30015 3.06196 5.68522 3.99973C6.26337 3.83906 6.8838 3.75895 7.50022 3.75583C8.1162 3.75895 8.73619 3.83906 9.31523 3.99973C10.6994 3.06196 11.3069 3.25691 11.3069 3.25691C11.7026 4.25521 11.4538 4.99226 11.3795 5.17563C11.8441 5.68257 12.1245 6.32925 12.1245 7.12104C12.1245 9.9063 10.4292 10.5192 8.81452 10.6985C9.07444 10.9224 9.30633 11.3648 9.30633 12.0413C9.30633 13.0102 9.29742 13.7922 9.29742 14.0299C9.29742 14.2239 9.42828 14.4496 9.79591 14.3788C12.6746 13.4179 14.75 10.7025 14.75 7.50024C14.75 3.49593 11.5036 0.25 7.49933 0.25Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
            Repository
          </button>
        </div>
      </div>
    </div>
    <!-- app body -->
      <div class="flex flex-col items-center justify-center gap-4 transition-all duration-300 md:flex-row">
        <div class="w-full h-12 px-2">
          <div class="relative flex w-full gap-x-2">
            <div class="text-2xl font-semibold text-base-12">Task</div>
            <div class="w-[28px] h-[28px] rounded-full bg-brand-3 flex justify-center items-center text-brand-12">0</div>
            <button class=" focus:outline-none hover:text-base-12 transition-all duration-300 hover:scale-110 absolute h-[28px] right-0 flex items-center justify-center text-base-11">clear</button>
          </div>
          <div class="w-full h-[1px] bg-base-6 mt-1"/>
          <!-- task list -->
        </div>
        <div class="w-full h-12 px-2">
          <div class="relative flex w-full gap-x-2">
            <div class="text-2xl font-semibold text-base-12">Done</div>
            <div class="w-[28px] h-[28px] rounded-full bg-green-3 flex justify-center items-center text-green-12">0</div>
            <button class=" focus:outline-none hover:text-base-12 transition-all duration-300 hover:scale-110 absolute h-[28px] right-0 flex items-center justify-center text-base-11">clear</button>
          </div>
          <div class="w-full h-[1px] bg-base-6 mt-1"/>
          <!-- task list -->
        </div>
      </div>
    </div>




  </div>
</main>

<style>

</style>
