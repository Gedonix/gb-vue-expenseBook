<template>
  <div class="showgb">
    <button @click="$emit('showlist', picked)">Показать книгу</button>
    <p>Сортировка:</p>
    <div class="radio">
        <input type="radio" id="one" value="1" v-model="picked" />
        <label for="one" style="padding-left: 5px">По порядку ввода</label>     
    </div>
    <div class="radio">
        <input type="radio" id="two" value="2" v-model="picked" />
        <label for="two" style="padding-left: 5px">Сначала более свежие</label>
    </div>    
  </div>
</template>

<script>
export default {
    emits: ['showlist'],
    data(){
        return {
            picked: '1'
        }
    }
}
</script>

<style>

</style>