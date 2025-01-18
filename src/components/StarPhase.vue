<template>  
    <div class="star-phase">
        <div>
            <label for="star-numbers">별 수:</label>
            <input type="text" id="star-numbers" v-model="inputStarNumbers">
            <button @click="setStarNumbers">별 개수 변경하기</button>
        </div>
        <div class="space" v-if="shadowPosArray.length">
            <div class="space__star-containers" v-for="(starIndex, i) in starNumbers" :key="starIndex">
                <div class="star shadow" :style="`left:${shadowPosArray[i]}%`"></div>
                <div class="star"></div>
            </div>
        </div>
    </div>
</template>

<script setup>
    import {ref, onMounted} from 'vue'
    const maxShadowPos = 100
    const minShadowPos = -100
    const starNumbers = ref(100)
    const inputStarNumbers = ref(100)
    const shadowPosArray = ref([])
    const shadowDirectionArray = ref([])
    const shadowMoveInterval = ref(null)
    const returnRandomShadow = () =>{
        const minCeiled = Math.ceil(minShadowPos);
        const maxFloored = Math.floor(maxShadowPos);
        const randomValue = Math.floor(Math.random() * (maxFloored - minCeiled + 1) + minCeiled)
        return randomValue
    }
    const setStarShadowMove = () =>{
        clearInterval(shadowMoveInterval.value)
        shadowMoveInterval.value = setInterval(()=>{
            for(let i = 0; i<shadowPosArray.value.length; i++){
                if(shadowPosArray.value[i] > maxShadowPos ){
                    console.log(`${i}커짐`)
                    shadowDirectionArray.value[i] = 'left'
                }
                else if(shadowPosArray.value[i] < minShadowPos ){
                    shadowDirectionArray.value[i] = 'right'
                }
                if(shadowDirectionArray.value[i] === 'right'){
                    shadowPosArray.value[i]++
                }
                else {
                    shadowPosArray.value[i]--
                }
            }
        }, 10)
    }

    const setShadowPos = () =>{
        shadowPosArray.value = []
        for(let i = 0; i<starNumbers.value; i++){
            const randomLeftPos = returnRandomShadow()
            shadowPosArray.value[i] = randomLeftPos
            shadowDirectionArray.value[i]= 'right'
        }
        console.log(shadowPosArray.value)
    }
    const setStarNumbers = () =>{
        starNumbers.value = Number(inputStarNumbers.value)
        setShadowPos()
        setStarShadowMove()
    }
    onMounted(()=>{
        setShadowPos()
        setStarShadowMove()
    })
</script>

<style scoped>
    .space{
        display:flex;
        justify-content: flex-start;
        flex-wrap:wrap;
        background:#000;
    }
    .space__star-containers{
        width:calc(100% / 30);
        max-width:100px;
        border:1px solid #ddd;
        height:100px;
        min-width:50px;
        position: relative;
        overflow: hidden;
    }
    .star{
        position: absolute;
        left:50%;
        top:50%;
        transform:translate(-50%, -50%);
        border-radius:50%; 
        width:50%; 
        aspect-ratio: 1/1;
        background:#fff;
    }
    .star.shadow{
        background:#000;
        z-index:1;
    }
</style>