
<template>
    <main>
        <div class="flex justify-center align-center flex-column p-4" >
        <InputText type="text" :value="formattedDate" readonly class="date-input" placeholder="click button to place a date" />
        <Button label="Open calendar" @click="visible = true" />
    </div>
    <Dialog v-model:visible="visible" header="Select dates" >
        <SelectButton
        v-model="rangeSelected"
        :options="rangeOptions"
        optionLabel="value"
        dataKey="value"
        aria-labelledby="custom"
        class="buttons-menu"
        allowEmpty
        @change="updateRange"
        :disabled="!dates[0]"
        >
       
            <template #option="slotProps">
                <i style="">{{slotProps.option.text}}</i>
            </template>
        </SelectButton>
        <Calendar
        v-model="dates"
        selectionMode="range"
        inline
        @date-select="updateSelection" />
       
    </Dialog>
    </main>
    <footer class="footer flex justify-center align-center flex-column">
         <span> Prueba técnica {{ new Date().getFullYear() }}</span>
        <span>
            Angel Hernández Tapia 
            <a class="link" href="https://www.linkedin.com/in/angel-hernandez-tapia-631a6513a/" target="_blank">
                <i class="pi pi-linkedin"> </i>
            </a>
            <a class="link" href="https://wa.me/524471082981" target="_blank">
                <i class="pi pi-whatsapp"> </i>
            </a>
        </span>
       
    </footer>
    

</template>

<script setup>
import { ref, computed } from "vue";

const formattedDate = computed(()=> {
    if(dates.value.length === 0)return null;
    if(dates.value[1])return formatDate(dates.value[0]) +'-'+formatDate(dates.value[1])
    return formatDate(dates.value[0])
})
const dates = ref([])
const visible = ref(false)
const rangeSelected = ref(null);
const rangeOptions = ref([
    { text: '±0', value: '0' },
    { text: '±1', value: '1' },
    { text: '±3', value: '3' },
    { text: '±7', value: '7' },
    { text: '±15', value: '15' }
]);

const formatDate = (date) => {
    return `${date.getDate()}/${date.getMonth() + 1}/${date.getFullYear()}`
}

const updateRange = ({value}) => {
  if(value === null) return
  
    const days = Number(value.value)
    const firsDate = new Date(dates.value[0])
    dates.value[1] = new Date(firsDate.setDate(firsDate.getDate() + days));
}

const updateSelection = (date) => {
    if(dates.value[0] === date && rangeSelected.value){
        updateRange(rangeSelected);
    }
}


</script>

<style scoped> 
.footer{
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 60px;
    border-top: 1px solid rgba(128, 128, 128, 0.568);
}
.p-4{
    padding: 4rem;
}
.flex{
    display: flex;
}
.date-input{
    margin-bottom: 16px;
    min-width: 200px;
}
.buttons-menu{
    display: block; 
    padding: 10px
}

.justify-center{
    justify-content: center;
}

.align-center{
    align-items: center;
}

.flex-column{
    flex-direction: column;
}

.link{
    text-decoration: none;
    color: #fff;
    padding-right: 10px;
}
</style>

