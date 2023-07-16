<script>
import { writable } from "svelte/store";
import DCodeLogoUrl from './assets/dcode_logo.png'
import ErrorismLogoUrl from './assets/errorism_logo.png'
import { createDropdownMenu } from '@melt-ui/svelte'

import {flip} from 'svelte/animate';
import {crossfade} from 'svelte/transition';
const [send, receive] = crossfade({});


const { menu, item, trigger, arrow } = createDropdownMenu()
export const TodoList = writable(JSON.parse(localStorage.getItem("todolist") ?? "[]"));
TodoList.subscribe(value => {
    localStorage.setItem("todolist", (typeof value === "object" ) ? JSON.stringify(value) : value);
})

export const DoneList = writable(JSON.parse(localStorage.getItem("donelist") ?? "[]"));
DoneList.subscribe(value => {
    localStorage.setItem("donelist", (typeof value === "object" ) ? JSON.stringify(value) : value);
})

function inputKeyDown(e){
  if (e.target.value === null || e.target.value.length <= 0 || e.target.value === "") return;
  if(e.key === 'Enter'){
    addTask(e.target.value)
    e.target.value = ""
  }
}

function inputOnClick(){
  // @ts-ignore
  let input = document.getElementById("taskInput").value
  if (input === null || input.length <= 0 || input === "") return;
  addTask(input)
  document.getElementById("taskInput").value = ""

}
function addTask(title){
  TodoList.update((v)=>{
    v.push({title:title})
    return v
  })
}

function move(item, from, to) {
  from.update( (v) => {
    v = v.filter(i => i !== item)
    return v
  });
  to.update( (v) => {
    v.push(item)
    return v
  });
}

function Done(item) {
  move(item, TodoList, DoneList);
}
function UnDone(item) {
  move(item, DoneList, TodoList);
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
      <div class="flex flex-col items-start justify-center transition-all duration-300 gap-x-4 gap-y-20 xl:flex-row xl:gap-y-0">
        <div class="w-full xl:max-w-[50%] px-2">
          <div class="relative flex w-full gap-x-2">
            <div class="text-2xl font-semibold text-base-12">Task</div>
            <div class="w-[28px] h-[28px] rounded-full bg-brand-3 flex justify-center items-center text-brand-12">{$TodoList.length}</div>
            <button on:click={()=>{TodoList.set([])}}
             class=" focus:outline-none hover:text-base-12 transition-all duration-300 hover:scale-110 absolute h-[28px] right-0 flex items-center justify-center text-base-11">clear</button>
          </div>
          <div class="w-full h-[1px] bg-base-6 mt-1 mb-2"/>
          <label class="relative group">
            <button on:click={inputOnClick} class="absolute inset-y-0 left-0 flex items-center pl-4 text-brand-11">
              <div class="relative flex items-center justify-center h-full pl-1">
                <svg class="absolute transition-all duration-300 opacity-100 group-focus-within:opacity-0" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M8 2.75C8 2.47386 7.77614 2.25 7.5 2.25C7.22386 2.25 7 2.47386 7 2.75V7H2.75C2.47386 7 2.25 7.22386 2.25 7.5C2.25 7.77614 2.47386 8 2.75 8H7V12.25C7 12.5261 7.22386 12.75 7.5 12.75C7.77614 12.75 8 12.5261 8 12.25V8H12.25C12.5261 8 12.75 7.77614 12.75 7.5C12.75 7.22386 12.5261 7 12.25 7H8V2.75Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
                <svg class="absolute transition-all duration-300 opacity-0 group-focus-within:opacity-100" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M7.49991 0.876892C3.84222 0.876892 0.877075 3.84204 0.877075 7.49972C0.877075 11.1574 3.84222 14.1226 7.49991 14.1226C11.1576 14.1226 14.1227 11.1574 14.1227 7.49972C14.1227 3.84204 11.1576 0.876892 7.49991 0.876892ZM1.82707 7.49972C1.82707 4.36671 4.36689 1.82689 7.49991 1.82689C10.6329 1.82689 13.1727 4.36671 13.1727 7.49972C13.1727 10.6327 10.6329 13.1726 7.49991 13.1726C4.36689 13.1726 1.82707 10.6327 1.82707 7.49972ZM7.50003 4C7.77617 4 8.00003 4.22386 8.00003 4.5V7H10.5C10.7762 7 11 7.22386 11 7.5C11 7.77614 10.7762 8 10.5 8H8.00003V10.5C8.00003 10.7761 7.77617 11 7.50003 11C7.22389 11 7.00003 10.7761 7.00003 10.5V8H4.50003C4.22389 8 4.00003 7.77614 4.00003 7.5C4.00003 7.22386 4.22389 7 4.50003 7H7.00003V4.5C7.00003 4.22386 7.22389 4 7.50003 4Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
              </div>
            </button>
            <input id="taskInput" required on:keypress={inputKeyDown} type="text" placeholder="Add Task" class="placeholder:text-brand-11 text-brand-12 pl-10 w-full h-[38px] rounded-md bg-brand-3 focus:outline-none focus:ring-2 focus:ring-brand-7">
          </label>
          <!-- task list -->
          {#each $TodoList as item (item)}
            <div
              animate:flip
              in:receive={{key: item}}
              out:send={{key: item}}
              class=" min-h-[38px] rounded-[6px] bg-base-3 mt-[6px] flex items-center text-base-12 relative"
            >
            <button on:click={() => Done(item)} class="absolute group">
              <div class="relative flex items-center justify-center h-full">
                <svg class="absolute transition-all duration-300 left-3 group-hover:opacity-0" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M0.877075 7.49991C0.877075 3.84222 3.84222 0.877075 7.49991 0.877075C11.1576 0.877075 14.1227 3.84222 14.1227 7.49991C14.1227 11.1576 11.1576 14.1227 7.49991 14.1227C3.84222 14.1227 0.877075 11.1576 0.877075 7.49991ZM7.49991 1.82708C4.36689 1.82708 1.82708 4.36689 1.82708 7.49991C1.82708 10.6329 4.36689 13.1727 7.49991 13.1727C10.6329 13.1727 13.1727 10.6329 13.1727 7.49991C13.1727 4.36689 10.6329 1.82708 7.49991 1.82708Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
                <svg class="absolute transition-all duration-300 opacity-0 left-3 group-hover:opacity-100 text-green-11" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M7.49991 0.877045C3.84222 0.877045 0.877075 3.84219 0.877075 7.49988C0.877075 11.1575 3.84222 14.1227 7.49991 14.1227C11.1576 14.1227 14.1227 11.1575 14.1227 7.49988C14.1227 3.84219 11.1576 0.877045 7.49991 0.877045ZM1.82708 7.49988C1.82708 4.36686 4.36689 1.82704 7.49991 1.82704C10.6329 1.82704 13.1727 4.36686 13.1727 7.49988C13.1727 10.6329 10.6329 13.1727 7.49991 13.1727C4.36689 13.1727 1.82708 10.6329 1.82708 7.49988ZM10.1589 5.53774C10.3178 5.31191 10.2636 5.00001 10.0378 4.84109C9.81194 4.68217 9.50004 4.73642 9.34112 4.96225L6.51977 8.97154L5.35681 7.78706C5.16334 7.59002 4.84677 7.58711 4.64973 7.78058C4.45268 7.97404 4.44978 8.29061 4.64325 8.48765L6.22658 10.1003C6.33054 10.2062 6.47617 10.2604 6.62407 10.2483C6.77197 10.2363 6.90686 10.1591 6.99226 10.0377L10.1589 5.53774Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
              </div>
            </button>
            <button on:click={() => TodoList.update( (v) => {
              v = v.filter(i => i !== item)
              return v
            })
            } class="absolute right-3 group ">
              <svg class="transition-all duration-300 group-hover:text-red-11 group-hover:-rotate-12"width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M5.5 1C5.22386 1 5 1.22386 5 1.5C5 1.77614 5.22386 2 5.5 2H9.5C9.77614 2 10 1.77614 10 1.5C10 1.22386 9.77614 1 9.5 1H5.5ZM3 3.5C3 3.22386 3.22386 3 3.5 3H5H10H11.5C11.7761 3 12 3.22386 12 3.5C12 3.77614 11.7761 4 11.5 4H11V12C11 12.5523 10.5523 13 10 13H5C4.44772 13 4 12.5523 4 12V4L3.5 4C3.22386 4 3 3.77614 3 3.5ZM5 4H10V12H5V4Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
            </button>
            <span class="block h-auto max-w-[99%] px-10 text-base break-words text-start py-2">{item.title}</span>
            </div>
          {/each}
        </div>
        <div class="w-full px-2 xl:max-w-[50%]">
          <div class="relative flex w-full gap-x-2">
            <div class="text-2xl font-semibold text-base-12">Done</div>
            <div class="w-[28px] h-[28px] rounded-full bg-green-3 flex justify-center items-center text-green-12">{$DoneList.length}</div>
            <button on:click={()=>{DoneList.set([])}}
            class=" focus:outline-none hover:text-base-12 transition-all duration-300 hover:scale-110 absolute h-[28px] right-0 flex items-center justify-center text-base-11">clear</button>
          </div>
          <div class="w-full h-[1px] bg-base-6 mt-1 mb-2"/>
          <!-- done list -->
          {#each $DoneList as item (item)}
              <div
              animate:flip
              in:receive={{key: item}}
              out:send={{key: item}}
              class=" min-h-[38px] rounded-[6px] bg-green-3 mt-[6px] flex items-center text-base-12 relative"
            >
            <button on:click={() => UnDone(item)} class="absolute group">
              <div class="relative flex items-center justify-center h-full">
                <svg class="absolute transition-all duration-300 left-3 group-hover:opacity-0 text-green-11" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M7.49991 0.877045C3.84222 0.877045 0.877075 3.84219 0.877075 7.49988C0.877075 11.1575 3.84222 14.1227 7.49991 14.1227C11.1576 14.1227 14.1227 11.1575 14.1227 7.49988C14.1227 3.84219 11.1576 0.877045 7.49991 0.877045ZM1.82708 7.49988C1.82708 4.36686 4.36689 1.82704 7.49991 1.82704C10.6329 1.82704 13.1727 4.36686 13.1727 7.49988C13.1727 10.6329 10.6329 13.1727 7.49991 13.1727C4.36689 13.1727 1.82708 10.6329 1.82708 7.49988ZM10.1589 5.53774C10.3178 5.31191 10.2636 5.00001 10.0378 4.84109C9.81194 4.68217 9.50004 4.73642 9.34112 4.96225L6.51977 8.97154L5.35681 7.78706C5.16334 7.59002 4.84677 7.58711 4.64973 7.78058C4.45268 7.97404 4.44978 8.29061 4.64325 8.48765L6.22658 10.1003C6.33054 10.2062 6.47617 10.2604 6.62407 10.2483C6.77197 10.2363 6.90686 10.1591 6.99226 10.0377L10.1589 5.53774Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
                <svg class="absolute transition-all duration-300 opacity-0 left-3 group-hover:opacity-100 text-red-11" width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M0.877075 7.49988C0.877075 3.84219 3.84222 0.877045 7.49991 0.877045C11.1576 0.877045 14.1227 3.84219 14.1227 7.49988C14.1227 11.1575 11.1576 14.1227 7.49991 14.1227C3.84222 14.1227 0.877075 11.1575 0.877075 7.49988ZM7.49991 1.82704C4.36689 1.82704 1.82708 4.36686 1.82708 7.49988C1.82708 10.6329 4.36689 13.1727 7.49991 13.1727C10.6329 13.1727 13.1727 10.6329 13.1727 7.49988C13.1727 4.36686 10.6329 1.82704 7.49991 1.82704ZM9.85358 5.14644C10.0488 5.3417 10.0488 5.65829 9.85358 5.85355L8.20713 7.49999L9.85358 9.14644C10.0488 9.3417 10.0488 9.65829 9.85358 9.85355C9.65832 10.0488 9.34173 10.0488 9.14647 9.85355L7.50002 8.2071L5.85358 9.85355C5.65832 10.0488 5.34173 10.0488 5.14647 9.85355C4.95121 9.65829 4.95121 9.3417 5.14647 9.14644L6.79292 7.49999L5.14647 5.85355C4.95121 5.65829 4.95121 5.3417 5.14647 5.14644C5.34173 4.95118 5.65832 4.95118 5.85358 5.14644L7.50002 6.79289L9.14647 5.14644C9.34173 4.95118 9.65832 4.95118 9.85358 5.14644Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
              </div>
            </button>
            <button on:click={() => DoneList.update( (v) => {
              v = v.filter(i => i !== item)
              return v
            })
            } class="absolute right-3 group ">
              <svg class="transition-all duration-300 group-hover:text-red-11 group-hover:-rotate-12"width="15" height="15" viewBox="0 0 15 15" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M5.5 1C5.22386 1 5 1.22386 5 1.5C5 1.77614 5.22386 2 5.5 2H9.5C9.77614 2 10 1.77614 10 1.5C10 1.22386 9.77614 1 9.5 1H5.5ZM3 3.5C3 3.22386 3.22386 3 3.5 3H5H10H11.5C11.7761 3 12 3.22386 12 3.5C12 3.77614 11.7761 4 11.5 4H11V12C11 12.5523 10.5523 13 10 13H5C4.44772 13 4 12.5523 4 12V4L3.5 4C3.22386 4 3 3.77614 3 3.5ZM5 4H10V12H5V4Z" fill="currentColor" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
            </button>
            <span class="block h-auto max-w-[99%] px-10 text-base break-words text-start py-2 line-through	text-green-11">{item.title}</span>
            </div>
          {/each}
        </div>
      </div>
      <div class="w-1 h-24"></div>
    </div>




  </div>
</main>

<style>

</style>
