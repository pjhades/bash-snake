About
---------------
A simple snake game written in Bash.


How to Play
---------------
Direction control: vim-style keys h, j, k and l;
Quit: q
Accept both upper- and lower-case.


Some Implementation Details
---------------
 * Bash-snake has two processes, the foreground one responds to 
   user's control commands, the background one draws the board.
   `kill` and `trap` are used to enable communication between the two processes.

   The foreground process `getchar()` ignores `SIGINT` and `SIGQUIT`, and
   replies to the signal of death `SIG_HEAD` by returning from the function `getchar()`.

   The background process `game_loop()` traps direction control signals from the keyboard,
   and self-defined signal `SIG_QUIT` which indicates the press of Q button.

 * Use `$!` to grasp the PID of the latest created background process.

 * When setting up several traps, it's necessary to guarantee that 
   the normal execution will not be interrupted by the signal handling 
   if the handlers envolve modification of some variables it depends on.

 * The snake is represented by the coordinate `$head_r` and `$head_c` indicating
   the row and column on which the snake head is, and a string `$body` which
   stores the directions from the head to tail. 
   e.g. a snake with this shape (`@` is the head):

```
@
oooo
   o
```

   will have a `$body` with value 21112, meaning 'down, right, right, right, down'.

   With the above scheme and the coordinates of the snake head, we can figure out
   the position the the entire snake body, without storing every part of its body.

 * The board is represented by a "2D array", which is actually implemented by
   a bunch of bash arrays and the `eval` command. For example, assigning 123 to the
   array entry `arr[5][6]` is done with `eval "arr$i[$j]=123"`.

 * The board is re-drawn in each iteration, which happens every 0.03 seconds.

 * Coloring is implemented with the escaped sequences of the terminal.

