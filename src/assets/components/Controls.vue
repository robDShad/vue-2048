<script setup>
    const emits = defineEmits(['sendCurrDirection']);
    
    const sendEmits = (direction) => {
        emits('sendCurrDirection', direction);
    }
</script>

<template>
    <div class="controls flex flex-col gap-4 justify-center items-center text-white  hover:">
        <div class="direction up" @click="sendEmits('up')">
            UP
        </div>
         <div class="left-right-logo flex items-center justify-center gap-10">
            <div class="direction left" @click="sendEmits('left')">
                LEFT
            </div>
            <div class="directions-logo w-30"><img src="/public/ArrowCross.svg.png" alt="" ></div>
            <div class="direction right" @click="sendEmits('right')">
                RIGHT
            </div>
        </div>
         <div class="direction down" @click="sendEmits('down')">
            DOWN
        </div>
    </div>
</template>

<style scoped>
    .direction {
        background-color: #6a7282;
        height: 60px;
        width: 120px;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        font-size: large;
        font-weight: 800;
        letter-spacing: 3px;
        border-radius: 10px;
    }
    .direction:hover {
        background-color: #fffbeb;
        color:#6a7282;
        transition: all ease-in 0.05s;
        border: solid 2px #6a7282;
    }
</style>