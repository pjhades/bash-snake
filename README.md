About
---------------
A simple snake game written in Bash.


How to Play
---------------
Direction control: vim-style keys h, j, k and l;
Quit: q
Accept both upper- and lower-case.


Key Points
---------------
 * Bash-snake has two processes, the foreground one responds to 
   user's control commands, the background one draws the board.
   `kill` and `trap` are used to enable communication between the two processes.
   
   The foreground process `getchar()` ignores `SIGINT` and `SIGQUIT`, and
   replies to the signal of death `SIG_HEAD` by returning from the function `getchar()`.

   The background process `game_loop()` traps direction control signals from the keyboard,
   and self-defined signal `SIG_QUIT` which indicates the press of Q button.

 * The snake is represented by the coordinate $head\_r and $head\_c indicating
   the row and column on which the snake head is, and a string $body which
   stores the directions from the head to tail. 
   e.g. a snake with this shape (@ is the head):

        @
        oooo
           o

   will have a $body with value '21112', meaning 'down,right,right,right,down'

 * Use $! to grasp the PID of the latest created background process.

 * When setting up several traps, it's necessary to guarantee that 
   the normal execution will not be interrupted by the signal handling 
   if the handlers envolve modification of some variables it depends on.

 * The board is re-drawn in each iteration, which happens every 0.03 seconds.

 * Coloring is implemented with the escaped sequences of the terminal.

