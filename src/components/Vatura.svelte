<script>
     import { fade, fly } from 'svelte/transition';
     import Vatura_settings from './Vatura_settings.svelte'
     import Vatura_calculation from './Vatura_calculation.svelte'
     import Tabs from './Tabs.svelte';

     let income_val = null;
     let costs_val = 0;
     let costs_visibility = false;
     let income_tax_val = 1445;

     let zus_type = [
		{ id: 1, text: `Mały ZUS`, value:650 },
		{ id: 2, text: `Duży ZUS`, value:1450 },
     ];
     let tax_type = [
          { id: 1, text: 'Liniowy 19%', value:19 },
          { id: 2, text: 'Ryczałt 18%', value:18 }
     ];
     let tax_type_selected = 19;
     let zus_type_selected = 1445;

     $: income_ac = income_val - costs_val;
     $: income_acz = income_ac - zus_type_selected;
     $: income_aczt = (income_acz * tax_type_selected) / 100
     $: zus_tax = zus_type_selected + income_aczt
     $: income_netto = income_ac - income_aczt - zus_type_selected

     const validateIncome = () => {
          if (income_val > 99999) {
               income_val = Math.floor(income_val / 10);
          }
          if (income_val > zus_type_selected) {
               income_netto = Math.round(income_netto)
          }
     };

     let show_settings = false;

     const showSettings = () => {
          show_settings = !show_settings;
          console.log(show_settings)
     }

     //tabs
     let items = ['Home', 'Settings'];
     let activeItem = 'Home';
     const tabChange = () => {
          if (activeItem === 'Home') {
               activeItem = 'Settings'
          }
          else {
               activeItem = 'Home'
          }
               
     }
</script>

<div class="card">
     <span class="appname">VATura</span>
     <div class='block2' on:click={tabChange}>Ustawienia</div>
     {#if activeItem === 'Home'}
          <Vatura_calculation />
     {:else if activeItem === 'Settings'}
          <Vatura_settings />
     {/if}
</div>
     
     <!-- {#if costs_visibility}
     <div class="input-div">
          <div class="input-style1">
          {#if costs_val != null}
               <span in:fly out:fade class="span-placeholder">Koszty</span>
          {:else}
               <span class="span-placeholder" style="visibility: hidden">-</span>
          {/if}
          <input 
               id="costs_val" 
               type=number bind:value={costs_val}
               min=0
               max=77777
               placeholder="Koszty"
          >
          </div>
     </div>
     {/if}
     <div style="">
          <div>
               <span class="span-select">Rodzaj opodatkowania:</span>
               <select class="custom-select" bind:value={tax_type_selected}>
                    {#each tax_type as tax}
                         <option value={tax.value}>
                              {tax.text}
                         </option>
                    {/each}
               </select>
          </div>
          <div>
               <span class="span-select">ZUS:</span>
               <select class="custom-select" bind:value={zus_type_selected}>
                    {#each zus_type as zus}
                         <option value={zus.value}>
                              {zus.text}
                         </option>
                    {/each}
               </select>
          </div>

     </div>
      -->
     
     <!-- <p>Przychód - Koszty: {income_val} - {costs_val} = {income_ac}</p>
     <p>Dochód - ZUS: {income_ac} - {zus_type_selected} = {income_acz}</p>
     <p>Podatek dochodowy: {income_aczt}</p>
     <p>Koszty bieżące: {costs_val}</p>
     <p>ZUS + Dochodowy: {income_tax_val} + {income_aczt} = {zus_tax}</p> -->
     

<div class="card">
     Wymiar czasu pracy
</div>

<style>
     .card {
     overflow: hidden;
     height: 300px;
     border-radius: 5px;
     /* background: #3c3c3c; */
     background: white;
     margin: 2px;
     padding: 5px;
     text-align: center;
     }
     .appname{
          display: block;
          color:#d6d6d6;
          font-size: 39px;
          line-height:1.2;
          text-align: center;
          padding-bottom: 10px;
          pointer-events: none;
     }
     .input-div{
          position: relative;
          width: 100px;
          margin: auto;
          padding: 1px;
          text-align: center;
     }
     .input-style1{
          display: block;
          width: 100%;
          font-size: 20px;
          color: #555;
          line-height: 1.2;
          border-width: 0px;
          outline: none;
          border: none;
          -moz-appearance: textfield;
     }
     .input-container{
          display: inline-block;
          position: relative;
     }
     .span-placeholder{
          float: left;
          font-size: 20px;
          margin: 2px;
     }
     .pln-suffix {
          position: absolute;
          left: 14;
          top: 12px;
          color: #555;
          padding-left: 20px;
          font: inherit;
          pointer-events: none;
     }
     .span-select{
          width: 100%;
          font-size: 20px;
          color: #555;
          line-height: 1.2;
          border-width: 0px;
          padding-right: 10px;
     }
     .custom-select{
          font: 1200 30px;
          -webkit-appearance: none;
          appearance: none;
          color: var(--baseFg);
          border: 0px solid var(--baseFg);
          line-height: 1;
          outline: 0;
          padding: 0.65em 2.5em 0.55em 0.75em;
          border-radius: var(--radius);
          background-color: var(--baseBg);
          background-repeat: no-repeat, no-repeat, no-repeat, no-repeat;
          background-size: 5px 100%, 20px 22px, 20px 22px, 20px 100%;
          background-position: right 20px center, right bottom, right bottom, right bottom;
     }
     /* Chrome, Safari, Edge, Opera */
     input::-webkit-outer-spin-button,
     input::-webkit-inner-spin-button {
     -webkit-appearance: none;
     margin: 0;
     }

     /* Firefox */
     input[type=number] {
     -moz-appearance: textfield;
     }

     .netto{
          font-size: 39px;
     }
     .tooltip .tooltiptext {
          visibility: hidden;
          width: 120px;
          background-color: #555;
          color: #fff;
          text-align: center;
          border-radius: 5px;
          padding: 5px 0;
          position: absolute;
          z-index: 1;
          bottom: 10%;
          left: 50%;
          margin-left: 60px;
          opacity: 0;
          transition: opacity 0.3s;
     }
     .tooltip .tooltiptext::after {
     content: "";
     position: absolute;
     top: 0%;
     left: 100%;
     margin-left: 50px;
     border-width: 0px;
     border-style: solid;
     border-color: #555 transparent transparent transparent;
     }

     .tooltip:hover .tooltiptext {
     visibility: visible;
     opacity: 1;
     }
     
     .block2 {
    color: rgb(0, 0, 0);
    width: 70px;
    position: absolute;
    top: 45px;
    right: 55px;
}
     .backdrop{
          width: 50%;
          height: 400px;
          position: absolute;
          border-radius: 5px;
          background: white;
          margin: 0 auto;
          padding: 5px;
          border: 1px solid red;
          display: flex;
    justify-content: center
          }
</style>