<template>
    <form class="addnewcost" @submit.prevent="">
        <button @click="isVisible = !isVisible">{{ isVisible ? 'Закрыть' : 'Добавить новый счет'}}</button>
        <div class="inputs" v-if="isVisible">
            <input id="1" type="date" v-model="idate">

            <select v-model="category">
                <option disabled value="">Выберите категорию</option>
                <option 
                    v-for="cat in categoryList"
                    :key="cat"
                >{{cat}}</option>
            </select>
        
            <input type="number" v-model.number="valuecost" placeholder="Размер платежа">
            <button type="submit" @click="addScore">Добавить</button>            
        </div> 
        <div class="warning">{{warninig}}</div>        
    </form>
</template>

<script>
export default {
    emits: ['addNewScore'],
    data() {
        return {
            isVisible: false,
            idate:     '',
            category:  '',
            valuecost: '' ,
            warninig: '',
            categoryList: [
                'Продукты питания',
                'Транспорт',
                'Здоровье',
                'Одежда и обувь',
                'Коммунальные услуги',
            ]                       
        }
    },
    methods: {
        formIsValid(){                       
            if(!this.idate || !this.category || !this.valuecost){
                console.log('date error');
                this.warninig = 'Введите корректные данные'
                this.idate = ''
                return false
            } else if( +this.idate.slice(0, 4) < 2020 || +this.idate.slice(0, 4) > (new Date()).getFullYear()) {
                this.warninig = 'Некорректное значение года в дате'
                this.idate = ''
                return false                
            } else {
                this.warninig = ''
                return true
            }            
        },
        clearForm(){
            setTimeout(() => {
                this.idate     = ''
                this.category  = ''
                this.valuecost = ''
            }, 500)            
        },
        addScore(){
            const entry = {
                id: Date.now(),
                date: `${this.idate.slice(8)}.${this.idate.slice(5, 7)}.${this.idate.slice(0, 4)}`,
                category: this.category,
                value: this.valuecost
            }
            if(this.formIsValid()){
                this.$emit('addNewScore', entry)
                this.clearForm()
            }            
        }        
    }
}
</script>
