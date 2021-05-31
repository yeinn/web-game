<template>
<div>
    <div id="computer" :style="computedStyleObject"></div>
    <div>
        <button @click="onClickBtn('바위')">✊</button>
        <button @click="onClickBtn('가위')">✌</button>
        <button @click="onClickBtn('보')">✋</button>
    </div>
    <div>{{result}}</div>
    <div>현재 총점:{{score}}</div>
</div>

</template>
<script>
const rspCoords={
    바위:'0',
    가위:'-142px',
    보:'-284px'
};

const scores={
    가위:1,
    바위:0,
    보:-1
}

const computerChoice=(imgCoord)=>{
    return Object.entries(rspCoords).find(function(v){
        return v[1]===imgCoord
    })[0]
}

let interval=null;
export default {
    data(){
        return{
            imgCoord:rspCoords.바위,
            result:'',
            score:0
        }
    },
    computed:{
        //캐싱을 통한 성능 향상
        computedStyleObject(){
            return{
                background:`url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`
            }
        }

    },
    methods: {
        changeHand(){
            interval=setInterval(()=>{
            if(this.imgCoord===rspCoords.바위){
                this.imgCoord=rspCoords.가위
            } else if (this.imgCoord===rspCoords.가위){
                this.imgCoord=rspCoords.보
            } else if(this.imgCoord===rspCoords.보){
                this.imgCoord=rspCoords.바위
            }
        },100);
        },
        onClickBtn(choice){
            clearInterval(interval)

            const myScore=scores[choice]
            const cpuScore=scores[computerChoice(this.imgCoord)]
            const diff=myScore-cpuScore

            if(diff===0){
                this.result='비겼습니다 :)'
            } else if([-1,2].includes(diff)){
                this.result='이겼습니다 :D'
                this.score+=1
            } else {
                this.result='졌습니다 :('
                this.score-=1
            }
            setTimeout(()=>{
                this.changeHand()
            },1000)
            console.log(this.myScore)
            console.log(this.cpuScore)
            console.log(diff)
        }
    },
    mounted() {
        this.changeHand()     
    },
    beforeDestroyed() {
        clearInterval(interval)
    },
}
</script>
<style scoped>
    #computer{
        width: 142px;
        height: 200px;
        background-position: 0 0;
    }
</style>