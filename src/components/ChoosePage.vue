<template>
    <div class="footer">
        <button @click="stepLeft"><span class="fas fa-angle-left"></span></button>
        <button             
            v-for="(number, idx) in quantity"
            :key="number"
            @click="setActivePage(idx)"
            :class="activePage === idx ? 'active' : 'passive'"
        >
        {{number}}</button>   
        <button @click="stepRight"><span class="fas fa-angle-right"></span></button>     
    </div>
</template>

<script>
export default {
    props: ['quantity'],
    emits: ['activePage'],
    data() {
        return {
            activePage: 0,
        }
    },
    methods:{
        send(){
            this.$emit('activePage', this.activePage)
        },
        setActivePage(idx){
            this.activePage = idx
            this.send()
        },        
        stepLeft() {
            if( this.activePage !== 0 ) {
                this.setActivePage(this.activePage - 1)
            } 
        },
        stepRight(){
            if ( this.activePage !== (this.quantity - 1) ) {
                this.setActivePage(this.activePage + 1)
            }
        }
    }
}
</script>
