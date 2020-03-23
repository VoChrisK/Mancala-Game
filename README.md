## Mancala Game

A three-persons team project for `CS151 - Object-Oriented Design`. This project is a window-based, GUI application replicating the traditional Mancala game.

<h1 align="center">
  <img src="https://github.com/VoChrisK/Mancala-Game/blob/master/assets/mancala-gif-1.gif" alt="gif-1" />
</h1>

## Languages/Technologies
* Java
* Java Foundation Classes (AWT, Swing)

## Background and Overview

### Game Logic

This project complies with the logic of the Mancala game with some additional rules. 
[Here is a video showing how to play Mancala.](https://www.youtube.com/watch?v=-A-djjimCcM&feature=youtu.be)

Each player takes turn picking a non-empty pit. When they pick a pit, they take all of its stones and put a stone in subsequent pits counter-clockwise. When a player
puts their last stone in an empty pit on their side, they can take all the stones from that point and the opposing player's pit adjacent to
it. When a player drop their last stone in their goal pit, they will receive an extra turn.

Here is a code snippet that handles part of the game's logic in choosing a pit and updating other pits:

```Java
while(stones > 0) { //as long as there are stones to be placed in other pits
  if((index + 1) < players[playerid].getPit().length) { //if it does not exceed the total # of pits
    if(originalplayerid == playerid) { 
      if(players[originalplayerid].getPit()[index + 1].isEmpty() && stones == 1)
        EmptyPit = true;
    }
    players[playerid].getPit()[index + 1].AddStone();
    index++;
    mancalaflag = false;
  }
  else { //if it exceeds the # of pits, put the stone in the mancala and change to the other players pit
    if(originalplayerid == playerid) //prevent from adding to opponent's mancala
      players[playerid].getMancala().AddStone();
    if(stones == 1) //if the last stone is placed in the player's mancala
      mancalaflag = true; //to get an extra turn
    if(playerid == 0)
      playerid = 1; //changes to player 2
    else
      playerid = 0; //changes to player 1

    index = -1; //set index to -1 to preserve the if clause's condition
  }
  stones--;
}
```

### Undo Button

This project also have an undo button at the bottom. Each player can click that button to undo their previous turn. Players can have up to three undos per turn.

<h1 align="center">
  <img src="https://github.com/VoChrisK/Mancala-Game/blob/master/assets/mancala-gif-2.gif" alt="gif-1" />
</h1>

### MVC Architecture

The project follows the `Model-View-Controller` design pattern. 

* The **Model** portion keeps track of the game's state. It keeps track of the number of stones for each of the 14 
pits and make changes to them accordingly. 
* The **Controller** portion waits for player inputs/requests. In this context, the pits are the controllers. We appended mouse events and bounds for each pit in order to notify whether a pit is clicked or not.
Whenever a player chooses a pit, it will communicate to the Model to make changes to the data. Here is a code snippet handling mouse events for each pit:
```Java
for(int j = 0; j < 6; j++) {
  //checks if the player clicks one of their pits
  if(board.getEllipses()[board.getModel().getPlayerID()][j].contains(mousePoint) 
    && board.getModel().getPlayers()[board.getModel().getPlayerID()].getPit()[j].isChosen()) {
    flag = board.getModel().ChoosePit(j);
    if(!flag) { //if the player did not get a free turn, rotate players
      if(board.getModel().getPlayerID() == 0) {
        board.getModel().setPlayerID(1);
      }
      else
        board.getModel().setPlayerID(0);
    }
    break; //no need to check for remaining pits if the pit is found
  }
}  
```
* The **View** portion is the overall presentation of the game. Whenever the **Model** updates the state, 
the View will redraw the entire game based on the new state. We also applied the `Decorator Pattern` to the View. This allows the
game to have multiple styles without altering the game's structure.


![mancala styles](https://i.imgur.com/xoluMGZ.png)

## Team Members
* [Chris Vo](https://www.linkedin.com/in/chris-vo-/)
* [Kevin Prakasa](https://www.linkedin.com/in/kevinprakasa/)
* [Dung (Richard) Pham](https://www.linkedin.com/in/dungtn911/)
