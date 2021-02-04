<script>
     import { fade } from 'svelte/transition';
</script>
   
<div in:fade="{{duration: 1500}}">
     <h2>Ustawienia</h2>
     <p>Rodzaj opłacanej składki ZUS</p>
     <select>
          <option>Mały ZUS</option>
          <option>Duży ZUS</option>
     </select>
     <input> 
     <p>Rodzaj podatku dochodowego</p>
     <select>
          <option>Ryczałt - 18/32%</option>
          <option>Liniowy - 19%</option>
     </select>
</div>


<style>

</style>