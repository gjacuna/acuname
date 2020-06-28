+++
title = "About"
date = "2014-04-09"
menu = "main"
+++

**Hola/Hello/Hallo**

If you're here, you're either stalking me or are genuinely interested in me. Regardless, check out my [social media profiles](/) or the Conway's Game of Life I implemented on JS below (I'm still not convinced it's properly done though).

Also, **help us figure out the answer to humanity's ultimate question: [The correct way to orient Toilet Paper Orientation](http://toiletpaperorientation.org)**, via the Toilet Paper Orientation Foundation.

Salud
</br>
<canvas id="game" width="500" height="500" style="border:1px solid #d3d3d3;"></div>
<div id="timer"></div>
<script type="text/javascript">
var x = 500;
var y = 500;
var cycles = 1000;
var side = 10;
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
var c = document.getElementById("game");
var ctx = c.getContext("2d");
var cycle_time = 1000;
var total_time = cycle_time * cycles;
var my_interval = setInterval(function(){
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
                ctx.fillStyle = "black";
			    ctx.fillRect(i*10, j*10, 10, 10);
            }
            else{
                ctx.fillStyle = "white";
			    ctx.fillRect(i*10, j*10, 10, 10);
            }
		}
	}
    total_time -= cycle_time;
    if(total_time<=0){
        clearInterval(my_interval);
    }
}, cycle_time);
</script>