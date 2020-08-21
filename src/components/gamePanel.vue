<template>
    <div class="gamePanel">
        <div>
            <div>
                <div class="panel top-left"
                v-bind:class="{ active: activeBtn==1 && redyToPlay===false }"
                @click="addPlayerSeq(1)"
                ></div>
                <div class="panel top-right"
                v-bind:class="{ active: activeBtn==2 && redyToPlay===false}"
                @click="addPlayerSeq(2)"
                ></div>
            </div>
            <div>
                <div class="panel bot-left"
                v-bind:class="{ active: activeBtn==4 && redyToPlay===false}"
                @click="addPlayerSeq(4)"
                ></div>
                <div class="panel bot-right"
                v-bind:class="{ active: activeBtn==3 && redyToPlay=== false}"
                @click="addPlayerSeq(3)"
                ></div>   
            </div>
        </div>
        <h2>Round: {{round}}</h2>
        <button class="start"
        @click="start"
        v-bind:disabled="plain"
        >START</button>
        <h2>Select level</h2>
        <div class="levelSelection">
            <input type="radio" name="mode" id="Junior" 
            @change="changeLevel(1500)" 
            v-bind:disabled="hardLevel!=1500 && lvlSelected"
            checked
            > Junior <br>
            <input type="radio" name="mode" id="Middle" 
            @change="changeLevel(1000)"
            v-bind:disabled="hardLevel!=1000 && lvlSelected" 
            > Middle <br>
            <input type="radio" name="mode" id="Senior"
            @change="changeLevel(400)"
            v-bind:disabled="hardLevel!=400 && lvlSelected"
            > Senior <br>
         </div>
    </div>
</template>

<script>
export default {
    name: 'gamePanel',
    data(){
        return {
            sequence: [],
            plain: false,
            redyToPlay: false,
            playerSequence: [],
            activeBtn: '',
            right: false,
            round: 0,
            hardLevel: 1500,
            lvlSelected: false,
            sounds: {
                1: 'https://s3.amazonaws.com/freecodecamp/simonSound1.mp3',
                2: 'https://s3.amazonaws.com/freecodecamp/simonSound2.mp3',
                3: 'https://s3.amazonaws.com/freecodecamp/simonSound3.mp3',
                4: 'https://s3.amazonaws.com/freecodecamp/simonSound4.mp3'
            }
        }
    },
    methods: {
        changeLevel(lvl){
            this.hardLevel = lvl
            this.lvlSelected = true
        },
        playSound (id) {
            var audio = new Audio(this.sounds[id]);
            audio.play()
        },
        generateSequence(i){
            this.sequence.push(Math.floor(Math.random() * 4) + 1)
            
        },
        activeButton(i){
            this.activeBtn = this.sequence[i]
            this.playSound(this.sequence[i])
            setTimeout(()=>{this.activeBtn = ''}, 300)
        },
        showMoves(){
            this.redyToPlay = false
            var i = 0
            var moves = setInterval(()=>{
                this.activeButton(i)
                i++
                if (i >= this.sequence.length) {
                clearInterval(moves)
                }
            }, this.hardLevel)
        },
        start(){
            this.plain = true
            this.generateSequence()
            this.showMoves()
            this.round += 1
        },
        addPlayerSeq(i){
            var f = i
            if(this.sequence.length){
                this.playerSequence.push(i)
                this.activeBtn = i
                this.playSound(i)
                setTimeout(()=>{this.activeBtn = ''}, 300)
                this.gamePlay(i)
            }
        },
        gamePlay(i){
            if(this.playerSequence[this.playerSequence.length-1] !== this.sequence[this.playerSequence.length-1]){
                if(this.sequence.length===0){
                    alert("Start game!")
                } else {
                    alert('Try again!')
                    location.reload()
                }
            } else if(this.playerSequence.length == this.sequence.length) {
                this.playerSequence = []
                this.round += 1
                this.generateSequence()
                this.showMoves()
            }
        }
    }
}
</script>

<style scoped lang="scss">
    .panel{
        width: 100px;
        height: 100px;
        display: inline-block;
        cursor: pointer;
    }
    .panel:hover{
        box-sizing: border-box;
        box-shadow: inset 0px 0px 0px 3px whitesmoke;
    }
    .top-left{
        border-top-left-radius: 100%;
        background-color: #0472E0;
        opacity: 0.6;
    }
    .top-right{
        border-top-right-radius: 100%;
        background-color: #EE1602;
        margin-left: 5px;
        opacity: 0.6;
    }
    .bot-right{
        border-bottom-right-radius: 100%;
        background-color: #A2BC20;
        margin-left: 5px;
        opacity: 0.6;
    }
    .bot-left{
        border-bottom-left-radius: 100%;
        background-color: #EED802;
        opacity: 0.6;
    }
    .active{
        opacity: 1;
    }
    button{
        width: 150px;
        height: 40px;
        border-radius: .4rem;
        border: none;
        background-color: #5200FF;
        color: whitesmoke;
    }
    button:hover{
        background-color: #7533FF;
    }
</style>