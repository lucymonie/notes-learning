## Understanding minimax
Ihave been working on an unbeatable tic tac toe game for node terminal, and I would ideally like to implement a minimax algorithm, which can be used to create an unbeatable AI player in games of this kind.

At the moment I'm using a breadth-first function that examines all possible next states of the game board. I use a heuristic function to prioritise a win or prevent a loss. If there is no win available or immediate loss to prevent, I use a 'naive' function with a series of if/else statements to choose the next move.

The game board is stored as a single-dimensional array, with 'X' or 'O' for taken places, and 'e' where the space is empty, for example:
```
let board = ['e','e','X',
             'O','X','e',
             'e','O','e']
```              

The code for the naive function is pretty cumbersome. It looks like this:
```
let getNaiveMove = function (board) {
  let move;
  if (board[4] === 'e') {
    move = 4;
  } else if (board[0] === 'X' && board[8] === 'X' || board[2] === 'X' && board[6] === 'X') {
    if (board[1] === 'e')
      move = 1;
    else if (board[3] === 'e')
      move = 3;
    else if (board[5] === 'e')
      move = 5;
    else if (board[7] === 'e')
      move = 7;
  } else {
    if (board[0] === 'e')
      move = 0;
    else if (board[2] === 'e')
      move = 2;
    else if (board[6] === 'e')
      move = 6;
    else if (board[8] === 'e')
      move = 8;
    else
      move = board.indexOf('e');
  }
  return move;
}
```
This code serves a purpose but it doesn't scale and it's cumbersome and hard to maintain.

The point of a minimax algorithm is to examine every possible move that each player could make with the remaining spaces on the board, and select the option that minimises the AI player's chance of losing. In a game of this type with two players, each is seeking first to win the game and second to prevent a loss. So of all the available outcomes, you need to choose the one that is least likely to lead to a loss.

[Wikipedia](https://en.wikipedia.org/wiki/Minimax) describes it as follows:
>A minimax algorithm is a recursive algorithm for choosing the next move in an n-player game, usually a two-player game. A value is associated with each position or state of the game. This value is computed by means of a position evaluation function and it indicates how good it would be for a player to reach that position. The player then makes the move that maximizes the minimum value of the position resulting from the opponent's possible following moves. If it is A's turn to move, A gives a value to each of his legal moves.

From what I understand, minimax algorithms go for depth-first rather than breadth-first, and use recursion to dig all the way down to the leaf nodes (where there are no more spaces available on the board and there may be a winner or a draw) or, alternatively, a terminal state (in which one player is the winner).

At the terminal state or leaf node, a heuristic function gives the outcome a score (perhaps 1 for a win, 0 for a draw and -1 for a loss). All available scores that stem from each move are assessed, and the least optimal is passed back up to the root, on the assumption that the least is what your opponent will make available to you. Your best next move is the greatest of these available moves. The further away the optimal outcome is, the lower its score should be, so that the AI player prioritises a win that is one move away rather than, say, three moves away.

From [Wikipedia](https://en.wikipedia.org/wiki/Minimax) again:
>An alternative is using a rule that if the result of a move is an immediate win for A it is assigned positive infinity and, if it is an immediate win for B, negative infinity. The value to A of any other move is the minimum of the values resulting from each of B's possible replies. For this reason, A is called the maximizing player and B is called the minimizing player, hence the name minimax algorithm.

An example of a minimax algorithm in Javascript by [Vivek Panyam](https://blog.vivekpanyam.com/how-to-build-an-ai-that-wins-the-basics-of-minimax-search/)
```
function recurseMinimax(board, player) {
    numNodes++;
    var winner = getWinner(board);
    if (winner != null) {
        switch(winner) {
            case 1:
                // AI wins
                return [1, board]
            case 0:
                // opponent wins
                return [-1, board]
            case -1:
                // Tie
                return [0, board];
        }
    } else {
        // Next states
        var nextVal = null;
        var nextBoard = null;

        for (var i = 0; i < 3; i++) {
            for (var j = 0; j < 3; j++) {
                if (board[i][j] == null) {
                    board[i][j] = player;
                    var value = recurseMinimax(board, !player)[0];
                    if ((player && (nextVal == null || value > nextVal)) || (!player && (nextVal == null || value < nextVal))) {
                        nextBoard = board.map(function(arr) {
                            return arr.slice();
                        });
                        nextVal = value;
                    }
                    board[i][j] = null;
                }
            }
        }
        return [nextVal, nextBoard];
    }
}
```
The key part of the code is in the nested for loops in this function, in which the first available space is allocated to 'player' (which is assigned a boolean value).

In the next step, 'value' is the zeroth element of the outcome of a (recursive) call to 'recurseMinimax', which gets passed the board with the new move, plus the other player (!player, or false). These continue getting a new move from the opposite player and the calls to the function keep getting pushed onto the stack until a terminal state is reached.

At this point, the terminal value (an array with a score and the board as its two list items) is assigned to 'value' and then depending on whether it's player or !player and the score of 'value' in comparison with nextVal (initialised as null but assigned the score of 'value' if player is truthy and 'value' is more than 'nextVal', or if player is falsy and 'value' is less than nextVal).

Then a new board is created and the current position is made available again. After every available move has been looped over and evaluated, the most optimal are returned in an array `[nextVal, nextBoard]`.

I found this [MIT lecture](https://www.youtube.com/watch?v=STjW3eH0Cik) on the subject very interesting.
