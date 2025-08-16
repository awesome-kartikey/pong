# Frequently Asked Questions (FAQ)

### Q1: How do I run the game?

**A:** Simply download or clone the project files, then open the `index.html` file in your web browser (like Chrome, Firefox, etc.). No installation or build steps are needed.

### Q2: What are the controls?

**A:** You control the bottom paddle by moving your mouse left and right within the game area. The paddle will follow your mouse's horizontal position.

### Q3: How do I win the game?

**A:** The goal is to score points by making the computer miss the ball. The first player (you or the computer) to reach 7 points wins the game. The winning score (`winningScore = 7`) is defined in `script.js`.

### Q4: Can I play against another human player?

**A:** No, this version is currently only single-player (Player vs. Computer). Adding a two-player mode would require significant changes to the input handling and paddle control logic.

### Q5: How does the computer opponent work? Is it beatable?

**A:** The computer AI is very basic. It tries to move its paddle (the top one) towards the ball's current horizontal position (`ballX`). Its reaction speed (`computerSpeed`) is fixed (though it increases slightly as the ball speed increases). You can beat it by hitting the ball at sharp angles or exploiting its fixed movement speed.

```javascript
// From script.js
function computerAI() {
  if (playerMoved) {
    // AI only moves after the player starts
    if (paddleTopX + paddleDiff < ballX) {
      paddleTopX += computerSpeed; // Move right towards ball
    } else {
      paddleTopX -= computerSpeed; // Move left towards ball
    }
  }
}
```

### Q6: Why does the ball get faster?

**A:** The ball's vertical speed (`speedY`) increases slightly each time it hits the player's paddle, up to a maximum limit. This is designed to make the game progressively more challenging. The computer's speed also increases slightly in response.

```javascript
// From script.js - inside ballBoundaries() when hitting player paddle
if (playerMoved) {
  speedY -= 1; // Increase speed (negative direction)
  // Max Speed
  if (speedY < -5) {
    speedY = -5;
    computerSpeed = 6; // Also boost computer speed slightly
  }
}
speedY = -speedY; // Reverse direction
```

### Q7: Is the game mobile-friendly?

**A:** Yes, it includes basic responsiveness.

- **CSS:** Media queries in `style.css` adjust the canvas and game-over screen layout for smaller screens.
- **JavaScript:** The `script.js` file detects mobile screen widths (`isMobile.matches`) and uses slightly faster initial ball and computer speeds for a better experience on smaller devices.

However, mouse control might not be ideal on touch-only devices. A touch-based control scheme would be an improvement for mobile.

### Q8: Can I change the game settings (like winning score, colors, speeds)?

**A:** Yes, you can modify the constants defined at the beginning of the `script.js` file to change game parameters:

- `winningScore`: Change the score needed to win.
- `paddleWidth`, `paddleHeight`: Adjust paddle sizes.
- `ballRadius`: Change ball size.
- Initial `speedY`, `speedX`, `computerSpeed`: Modify starting speeds (check both mobile and desktop settings).

You can also change colors and styles directly in the `style.css` file.

### Q9: What technologies were used?

**A:** The game is built purely with client-side web technologies: HTML, CSS, and JavaScript. It uses the HTML5 Canvas API for rendering graphics. No external libraries or frameworks are used.
