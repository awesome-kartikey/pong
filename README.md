# Awesome Kartikey Pong

A simple, classic Pong game implementation playable in the browser. Built with plain HTML, CSS, and JavaScript.

## Description

This project is a lightweight, browser-based implementation of the classic arcade game Pong. The player controls the bottom paddle using their mouse, competing against a basic computer-controlled opponent controlling the top paddle. The first player to reach the winning score wins the game.

## Features

- Classic Pong gameplay (Player vs. Computer)
- Score tracking
- Ball speed increases during gameplay
- Simple Computer AI opponent
- Responsive design for different screen sizes (adjusts speeds and layout)
- Game over screen with a "Play Again" option
- Smooth animation using `requestAnimationFrame`

## Tech Stack

- **HTML5:** For the basic structure and canvas element.
- **CSS3:** For styling the game elements, layout, and responsiveness.
- **JavaScript (ES6+):** For all game logic, including rendering, physics, controls, AI, and game state management.

## Setup Instructions

No complex setup is required. You can run this project directly in your web browser.

1.  **Clone or Download:**

    - Clone the repository:
      ```bash
      git clone https://github.com/awesome-kartikey/pong.git
      ```
    - Or download the source code ZIP file and extract it.

2.  **Open the Game:**
    - Navigate to the project directory (`pong/`).
    - Double-click the `index.html` file, or open it using your preferred web browser (e.g., Chrome, Firefox, Safari, Edge).

## Usage

1.  Once the `index.html` file is opened in your browser, the game will start automatically.
2.  Move your mouse horizontally across the game area to control the bottom paddle.
3.  Your objective is to hit the ball back towards the computer's side.
4.  Score a point each time the computer fails to return the ball.
5.  The computer scores a point if you fail to return the ball.
6.  The first player to reach 7 points wins the game.
7.  After the game ends, click the "Play Again" button to start a new match.
