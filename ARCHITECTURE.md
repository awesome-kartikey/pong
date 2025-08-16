# System Architecture

This document outlines the architecture, components, and design decisions for the Awesome Kartikey Pong game.

## 1. System Overview

Awesome Kartikey Pong is a **client-side, browser-based application**. It runs entirely within the user's web browser using standard web technologies (HTML, CSS, JavaScript). There is no server-side component or backend involved.

The core of the application relies on the **HTML5 Canvas API** for rendering all visual elements (paddles, ball, score, center line) and **plain JavaScript** for managing the game state, handling user input, implementing game logic (physics, AI), and controlling the game loop.

### Conceptual Diagram

```
+-------------------+      +----------------------+      +---------------------+
|   User            |----->|      Web Browser     |----->|     Pong Game       |
| (Mouse Input)     |      | (HTML/CSS/JS Engine) |      | (Canvas Rendering)  |
+-------------------+      +----------------------+      +---------------------+
                                      |
                                      | Loads Files:
                                      | + index.html
                                      | + style.css
                                      | + script.js
                                      v
                               +-----------------+
                               |   Local Files   |
                               +-----------------+
```

## 2. Folder Structure

The project has a very simple flat structure:

```
pong/
├── index.html       # Main HTML file, contains the canvas container
├── script.js        # JavaScript file with all game logic
└── style.css        # CSS file for styling and layout
```

- `index.html`: Provides the basic HTML document structure and includes the CSS and JavaScript files. It contains the `<canvas>` element where the game is rendered (though the canvas is actually created dynamically by `script.js`).
- `style.css`: Defines the visual appearance of the game elements, the canvas positioning, styling for the game-over screen, and media queries for basic responsiveness.
- `script.js`: Contains the entirety of the game's logic.

## 3. Major Components (within `script.js`)

The `script.js` file centralizes all the game's functionality. Key logical components include:

1.  **Initialization & Setup:**

    - Gets DOM elements (`body`).
    - Creates the `<canvas>` element dynamically and gets its 2D rendering context (`context`).
    - Defines game constants (dimensions, speeds, winning score).
    - Detects mobile devices (`isMobile`) to adjust initial settings.
    - Sets initial game state variables (paddle positions, ball position, scores, flags).

2.  **Game State Variables:**

    - `paddleBottomX`, `paddleTopX`: Store horizontal positions of player and computer paddles.
    - `ballX`, `ballY`: Store the current position of the ball.
    - `speedX`, `speedY`: Store the ball's velocity components.
    - `playerScore`, `computerScore`: Track the scores.
    - `isGameOver`, `isNewGame`, `playerMoved`, `paddleContact`: Flags managing the game's state transitions and logic flow.

3.  **Rendering Engine (`renderCanvas` function):**

    - Responsible for drawing all visual elements onto the canvas in each frame.
    - Clears the canvas.
    - Draws the background, paddles, center line, ball, and scores using the Canvas 2D API methods (`fillRect`, `beginPath`, `arc`, `fill`, `fillText`, etc.).

4.  **Game Loop (`animate` function):**

    - The heart of the game execution.
    - Uses `window.requestAnimationFrame(animate)` to create a smooth, efficient loop synchronized with the browser's rendering cycle.
    - Calls functions for rendering (`renderCanvas`), updating ball position (`ballMove`), checking collisions/boundaries (`ballBoundaries`), updating the computer's paddle (`computerAI`), and checking for game end conditions (`gameOver`) within each frame.
    - Stops looping when `isGameOver` becomes true.

5.  **Physics & Collision Detection (`ballMove`, `ballBoundaries` functions):**

    - `ballMove`: Updates the `ballX` and `ballY` based on `speedX` and `speedY`.
    - `ballBoundaries`: Checks for collisions between the ball and the walls or paddles. It handles:
      - Bouncing off walls (reversing `speedX`).
      - Bouncing off paddles (reversing `speedY`, adjusting `speedX` for angle/trajectory, increasing speed).
      - Detecting scoring conditions (when the ball goes past a paddle) and triggering `ballReset`.

6.  **Input Handling (Event Listener):**

    - An event listener (`canvas.addEventListener('mousemove', ...`)`) tracks mouse movement over the canvas.
    - Updates the player's paddle position (`paddleBottomX`) based on the mouse's `clientX`.
    - Includes logic to compensate for canvas centering (`canvasPosition`) and clamp the paddle within the canvas bounds.
    - Sets the `playerMoved` flag to true once the player interacts.

7.  **Computer AI (`computerAI` function):**

    - Implements the opponent's logic.
    - Moves the top paddle (`paddleTopX`) horizontally towards the ball's `ballX` position at a fixed `computerSpeed`.

8.  **Game Flow Management (`startGame`, `gameOver`, `ballReset`, `showGameOverEl` functions):**
    - `startGame`: Initializes or resets the game state (scores, ball position, flags) and starts the `animate` loop. Sets up the input listener.
    - `gameOver`: Checks scores against `winningScore` to determine if the game should end.
    - `showGameOverEl`: Hides the canvas and displays the game over screen with the winner and a "Play Again" button.
    - `ballReset`: Resets the ball's position and speed after a point is scored.

## 4. Data Flow

The game operates on a continuous loop driven by `requestAnimationFrame`:

1.  **Initialization:** `startGame()` is called on page load. It sets up the initial state and starts the loop.
2.  **Input:** The `mousemove` event listener constantly updates `paddleBottomX` based on user mouse movement.
3.  **Game Loop (`animate`):**
    a. **Render:** `renderCanvas()` draws the current state (paddles, ball, score) onto the canvas.
    b. **Move Ball:** `ballMove()` updates `ballX`, `ballY` based on current `speedX`, `speedY`.
    c. **Check Collisions/Boundaries:** `ballBoundaries()` checks if the ball hit a wall or paddle:
    _ If hit wall: Reverse relevant speed component (`speedX`).
    _ If hit paddle: Reverse `speedY`, calculate new `speedX` based on hit location, potentially increase `speedY` magnitude. Set `paddleContact` flag. \* If missed paddle (score): Update score (`playerScore` or `computerScore`), call `ballReset()`.
    d. **Update AI:** `computerAI()` updates `paddleTopX` based on `ballX` and `computerSpeed`.
    e. **Check Game Over:** `gameOver()` checks if `winningScore` is reached. If yes, set `isGameOver = true` and call `showGameOverEl()`.
    f. **Loop:** If `!isGameOver`, schedule the next call to `animate` via `requestAnimationFrame`.
4.  **Game End:** When `isGameOver` is true, the loop stops. The game over screen is shown.
5.  **Restart:** Clicking "Play Again" calls `startGame()` again, resetting the state and restarting the loop.

## 5. Design Decisions

- **Canvas 2D API:** Chosen for its suitability for simple 2D graphics rendering directly in the browser without external libraries. It provides the necessary drawing primitives (`rect`, `arc`, `line`).
- **Pure HTML/CSS/JavaScript:** Selected for simplicity, zero dependencies, and ease of understanding and deployment. Avoids the overhead of frameworks for such a small project.
- **`requestAnimationFrame`:** Used for the game loop instead of `setInterval` or `setTimeout` because it synchronizes with the browser's rendering cycle, leading to smoother animations and better performance/efficiency (pauses when the tab is inactive).
- **Dynamic Canvas Creation:** The `<canvas>` element is created in JS (`document.createElement('canvas')`) rather than hardcoded in HTML. This keeps the HTML minimal and demonstrates dynamic element creation.
- **Single Script File (`script.js`):** For a project of this small scale, keeping all JavaScript logic in one file is manageable. For larger projects, this would be split into modules (e.g., `Paddle.js`, `Ball.js`, `Game.js`).
- **Simple AI:** The computer AI is intentionally kept basic (reactive movement towards the ball) to focus on the core Pong mechanics and make the game winnable.
- **Responsiveness Handling:** Basic responsiveness is handled using both CSS media queries (for layout/sizing) and JavaScript checks (`isMobile.matches`) for adjusting dynamic game parameters like speed, providing a slightly different feel on mobile vs. desktop.
- **State Flags:** Boolean flags (`isGameOver`, `playerMoved`, etc.) are used extensively to manage different states and transitions within the game flow.
