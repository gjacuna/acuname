+++
title = "About"
date = "2014-04-09"
menu = "main"
+++

**Hola/Hello/Hallo**

If you're here, you're either stalking me or are genuinely interested in me. Regardless, check out my [social media profiles](/) or <span id = 'coming'>wait 20 seconds to see something cool (for me at least) I implemented here.</span>

Also, **help us figure out the answer to humanity's ultimate question: [The correct way to orient Toilet Paper](http://toiletpaperorientation.org)**, via the Toilet Paper Orientation Foundation.

Salud
<div id="timer"></div>
<script type="text/javascript">
var x = 1000;
var y = 1000;
var cycles = 1000;
var side = 5;
let board = Array.from(Array(x/side), () => new Array(y/side));
for(i=0;i<x/side;i++){
    for(j=0;j<y/side;j++){
        if(Math.floor(Math.random()*100+1)<=20){
            board[i][j]=1;
        }
        else{
            board[i][j]=0;
        }
    }
}
function sumOfNeighbours(temp_board,i,j){
    var left = i-1;
    var right = i+1;
    var up = j-1;
    var down = j+1;
    if(i==0){
        left = x/side-1;
    }
    else if(i == x/side-1){
        right = 0;
    }
    if(j==0){
        up = y/side-1;
    }
    else if(j == y/side-1){
        down = 0;
    }
    return (temp_board[left][up]+temp_board[i][up]+temp_board[right][up]+temp_board[left][j]+temp_board[right][j]+temp_board[left][down]+temp_board[i][down]+temp_board[right][down]);
}
//var c = document.getElementById("game");
var c = document.createElement('canvas');
c.width = 1000;
c.height = 1000;
var ctx = c.getContext("2d");
var cycle_time = 1000;
var wait_time = 20000;
var total_time = cycle_time * cycles + wait_time;
// var my_wait_interval = setInterval(function(){
//     wait_time -= cycle_time;
//     $("#timer").html("Seconds of wait left:"+(wait_time/cycle_time-1)+".");
//     if(wait_time<=0){
//         clearInterval(my_wait_interval);
//     }
// }, cycle_time);
var first_cycle = true;
var my_interval = setInterval(function(){
    if(wait_time>0){
        $("#coming").html("wait "+(wait_time/cycle_time-1)+" seconds to see something cool (for me at least) I implemented here.");
        wait_time -= cycle_time;
    }else{
        if(first_cycle)
            $("#coming").html("the <a href='https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life' target='_blank'>Conway’s Game of Life</a> I implemented on JS around this div (I’m still not convinced it’s properly done though, arrays in JS are a bitch).");
        let previous_board = JSON.parse(JSON.stringify(board));
        $("#timer").html("Cycles left:"+(total_time/cycle_time-1)+".");
        for(i=0;i<x/side;i++){
    		for(j=0;j<y/side;j++){
                //check for neighbours
    			if(previous_board[i][j] == 1){
                    if(sumOfNeighbours(previous_board,i,j) < 2 || sumOfNeighbours(previous_board,i,j) > 3){
                        board[i][j] = 0;//dies
                    }
                }
                else{
                    if(sumOfNeighbours(previous_board,i,j) == 3){
                        board[i][j] = 1;//lives
                    }
                }
    		}
    	}
        for(i=0;i<x/side;i++){
    		for(j=0;j<y/side;j++){
    			if(board[i][j]==1){
                    ctx.fillStyle = "#f0f0f0";
    			    ctx.fillRect(i*side, j*side, side, side);
                }
                else{
                    ctx.fillStyle = "white";
    			    ctx.fillRect(i*side, j*side, side, side);
                }
    		}
    	}
        document.getElementsByClassName('post-content')[0].style.background = 'url(' + c.toDataURL() + ')';
    }    
    total_time -= cycle_time;
    if(total_time<=0){
        clearInterval(my_interval);
    }
}, cycle_time);
</script>