# Chess.com Frontend Challenge – Chessboard Click Tracker

A responsive Vue 3 + Tailwind CSS app that renders a chessboard with a sidebar.  
Clicking any square highlights it and logs the click history in order.

## Live Demo

https://frontendtest-one-liard.vercel.app/

## Tech Stack

- Vue 3 (Vite)
- Tailwind CSS
- Deployed on Vercel

## Features

- Responsive layout:
  - Sidebar to the right of the board on desktop
  - Sidebar below the board on mobile
- Chessboard resizes while maintaining a perfect square aspect ratio
- Clicking a square:
  - Highlights the selected square
  - Logs the square and click order in the sidebar
- Click history displays:
  - Order number
  - Square name (e.g. e4)
  - Timestamp
  - Clearing all clicks
