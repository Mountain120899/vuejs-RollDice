<template>
	<div id="app">
		<div class="wrapper clearfix">
			<player v-bind:player1="player1" v-bind:player2="player2" v-bind:finalscore="finalscore"
            v-bind:winner1="winner1" v-bind:winner2="winner2"/>            
            <controller v-bind:rule="rule" v-bind:finalscore="finalscore" v-on:ResetAllScore="Reset"
             v-bind:start="start" v-on:RandomNumber="random"
             v-on:Hold="Hold"/>           
			<dice v-bind:randomNumber="randomNumber"/>
        </div>
	</div>
</template>

<script>
import player from "./components/PlayerComp.vue"
import controller from "./components/ControllerComp.vue"
import dice from "./components/DiceComp.vue"
export default {
	name: 'app',
	data () {
		return {
			player1:{name:"player1",score:43, currentScore:11, active:false},
            player2:{name:"player2",score:72, currentScore:0, active:false},
            start:false,
            finalscore: 0,
            rule: "Chơi theo lượt, bạn sẽ bị mất lượt và điểm quay của lần đó nếu quay vào ô mất lượt",
            randomNumber: [1,1],
            winner1: false,
            winner2:false
		}
	},
    methods:{
        Reset(data){
            this.player1 = {name:"player1",score:0, currentScore:0, active:true};
            this.player2 = {name:"player2",score:0, currentScore:0, active:false};
            this.start = true;
            this.finalscore = data;
            this.winer1 = false;
            this.winer2 = false;
        },
        LoseTurn(){
            if(this.player1.active){
                this.player1.score = 0;
                this.player2.active = true;
                this.player1.active = false;
            }
            else{
                this.player2.score = 0;
                this.player1.active = true;
                this.player2.active = false;
            }
        },
        random(){
            var numb1 = Math.floor(Math.random() * (6 - 1)) + 1;
            var numb2 = Math.floor(Math.random() * (6 - 1)) + 1;
            this.randomNumber = [numb1,numb2];
            var tempScore = numb1 + numb2;
            // Cộng dồn điểm
            if(this.player1.active){
                this.player1.score += tempScore;
            }
            else{
                 this.player2.score += tempScore;
            }
            
            if(numb1 == 1 || numb2 == 1)
            {
                this.LoseTurn();
                alert("Bạn đã quay vào ô mất lượt! Đen thôi!")
            }
        },
        Winner(){
            if(this.player1.currentScore >= this.finalscore){
                return 1;
            }
            if(this.player2.currentScore >= this.finalscore){
                return 2;
            }
            else{
                return 0;
            }
        },
        Hold(){
            if(this.player1.active){
                this.player1.currentScore += this.player1.score;
            }
            else{
                this.player2.currentScore += this.player2.score;
            }
            // Xử lý win
            console.log(this.Winner());
            if(this.Winner() != 0)
            {
                if(this.Winner() == 1){
                    this.winner1 = true;
                    this.player1.name = 'Winner';
                }
                else{
                    this.winner2 = true;
                    this.player2.name = 'Winner';
                }
                alert("Game over");
                this.start = false;
                return;
            }
            this.LoseTurn();
        }
    },
	components: {
		player,
		controller,
		dice
	}
}
</script>

<style>
	* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url(../public/assets/back.jpg);
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}
</style>
