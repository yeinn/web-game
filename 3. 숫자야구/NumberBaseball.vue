<template>
    <div>
        <h1>{{result}}</h1>
        <form @submit.prevent="onSubmitForm">
            <div class="form-group">
            <input class="form-control" ref="answer" minlength="4" maxlength="4" v-model="value"/>
            <button class="btn btn-primary" type="submit">입력</button>
            </div>
        </form>
        <div>시도: {{tries.length}}</div>
        <ul>
            <li v-for="t in tries" :key="t.index">
                {{t.try}}
                {{t.result}}    
            </li>
        </ul>
    </div>
</template>

<script>
const getNumbers=()=>{
    const candidates=[1,2,3,4,5,6,7,8,9]
    const array=[]
    for(let i=0; i<4; i++){
        const chosen = candidates.splice(Math.floor(Math.random()*(9-i)),1)[0]
        array.push(chosen)
    }
    return array
}

export default {
    data() {
        return {
            answer:getNumbers(), //ex [1,5,3,5]
            tries:[], //시도 수
            value:'', //입력값
            result:'', //결과
        }
    },
    methods:{
        onSubmitForm(e){
            if (this.value === this.answer.join('')){ //정답이면
            this.tries.push({
                try:this.value,
                result: "⚾ 홈런"
            })
            this.result="⚾ 홈런"
            alert('🎊 게임을 다시 실행합니다!')
            this.value=''
            this.answer=getNumbers()
            this.tries=[]
            this.$refs.answer.focus()
            }else{//틀리면
            if(this.tries.length>=9){
                this.result=`10번 넘게 들려서 실패하셨습니다😢 정답은 ${this.answer.join(',')} 이였습니다!`
                this.value=''
                this.answer=getNumbers()
                this.tries=[]
                this.$refs.answer.focus()
            }
                let strike=0
                let ball=0
                const answerArray = this.value.split('').map(v=>parseInt(v))
                for(let i=0; i<4; i+=1){
                    if(answerArray[i]===this.answer[i]){//숫자 자리수 모두 정답
                        strike++;
                    } else if (this.answer.includes(answerArray[i])){//숫자만 정답
                        ball++
                    }
                }
                this.tries.push({
                    try: this.value,
                    result:`${strike} 스크라이크, ${ball} 볼 입니다.`
                })
                this.value=''
                this.$refs.answer.focus()
            }
        }
    }
}
</script>
<style>
    
</style>