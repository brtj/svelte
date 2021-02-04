<script>
     import { fade } from 'svelte/transition';
     import { income_val } from './stores.js';


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

     $: income_ac = $income_val - costs_val;
     $: income_acz = income_ac - zus_type_selected;
     $: income_aczt = (income_acz * tax_type_selected) / 100
     $: zus_tax = zus_type_selected + income_aczt
     $: income_netto = income_ac - income_aczt - zus_type_selected

     const validateIncome = () => {
          if ($income_val > 99999) {
               $income_val = Math.floor($income_val / 10);
          }
          if ($income_val > zus_type_selected) {
               income_netto = Math.round(income_netto)
          }
     };
</script>
   
<div in:fade="{{duration: 1500}}">
     <div class="input-div tooltip">

               
          {#if $income_val != null}
               <span class="span-placeholder">Przychód</span>
          {:else}
               <span class="span-placeholder" style="visibility: hidden">-</span>
          {/if}
          <br>
          <div class="input-container">
          <input 
               id="income_val" 
               class="input-style1"
               type=number bind:value={$income_val}
               min=0
               max=99999
               placeholder="Przychód"
               on:input={validateIncome($income_val)}
          >
          {#if (($income_val === 0) || ($income_val === null))}
          <span class="tooltiptext">Wprowadź przychód (bez VAT)</span>
          {/if}
          {#if $income_val != null}
               <span class="pln-suffix">PLN</span>
          {/if}
          </div>
     </div>
     
     
     {#if $income_val >= (zus_type_selected + 10)}
          <div>
               <p class="netto">Netto: {income_netto} PLN</p>
               <p><small>Wyliczenia dla podatku dochodowego 19% oraz dużego ZUSu.</small></p>
          </div>
     {/if}
</div>


<style>

</style>