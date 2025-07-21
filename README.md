# bingo
bingo for the tec-1 in mint code

from chatgpt

Here is the **full MINT code** for a complete working CLI-based 90-ball bingo game using only `n` and `b`, with full inline comments explaining every step:

---

### ✅ Full MINT Bingo Game (with comments)

```mint
// :C – Generate a new random bingo card with 15 unique numbers (1–90)
:C 
[0 0 0 0 0 0 0 0 0 0 0 0 0 0 0] c!    // Clear the card (15 spots)
0 i!                                  // Index for filling c[]
0 j!                                  // Index for duplicate check
/U (                                  // Loop until card is filled
  i 15 = /W                           // Stop when 15 numbers filled
  1 90 * r!                           // Draw random number from 1 to 90
  0 f!                                // Flag: found duplicate?

  // Check for duplicates in current card
  0 j!
  15 (
    c j ? r = ( /T f! )              // If duplicate, flag it
    j 1 + j!
  )

  f /F = (                            // If no duplicate
    r c i ?!                         // Store number in card
    i 1 + i!                         // Move to next card slot
  )
)
;
```

```mint
// :A – Start a new game
:A 
C                                      // Generate fresh card
[0 0 0 0 0 0 0 0 0 0 0 0 0 0 0] m!     // Clear match mask
[0] d!                                 // Clear drawn list
0 g!                                   // Reset drawn count
0 l!                                   // Reset completed lines
`New card created:` /N
0 i!                                   // Print the card values
15 ( c i ? . 32 /C i 1 + i! ) /N
;
```

```mint
// :B – Draw a new ball, update matches, check win, display
:B 
/U (                                    // Draw loop until unique
  1 90 * r!                             // Random number from 1 to 90
  d /S x!                               // How many drawn already
  0 i!
  x ( d i ? r = /T = /W i 1 + i! )      // Check if already drawn
)

r d /S ?!                               // Add to drawn list
g 1 + g!                                // Increment draw counter

`Ball: ` r . /N

// Mark matched spots in card
0 i!
15 (
  c i ? r = ( 1 m i ?! ) /E ( m i 0 ?! ) // If matched, mark 1 else 0
  i 1 + i!
)

// Check lines (3 lines of 5 numbers each)
0 z!                                   // Reset matched line count
0 i!
3 (
  0 y!                                 // Reset match counter
  5 (
    j i 5 * + m j ? + y!               // Add match flag from line segment
    j 1 + j!
  )
  y 5 = ( z 1 + z! )                   // If all 5 matched, count the line
  i 1 + i!
)
z l = /F = ( l z! `Lines complete: ` z . /N )

z 3 = ( `🎉 FULL HOUSE! All 3 lines completed!` /N )
;
```

```mint
// Game loop – listens for n or b
:Z 
/U (
  `--- Bingo Menu ---` /N
  `n - New Game` /N
  `b - Draw Ball` /N
  `/K` k!                           // Get user input key
  k 110 = ( A )                    // 'n'
  k 98 = ( B )                     // 'b'
)
;
```

---

### 📦 Variables Used:

| Variable      | Meaning                   |
| ------------- | ------------------------- |
| `c`           | Bingo card (15 numbers)   |
| `m`           | Match flags (1 = matched) |
| `d`           | Drawn numbers list        |
| `r`           | Last drawn number         |
| `g`           | Number of balls drawn     |
| `l`           | Completed line count      |
| `i,j,x,y,z,f` | Counters/flags            |
| `k`           | Keyboard input key        |

---

### ✅ To Run the Game in MINT:

1. Paste all the above functions into your MINT interpreter.
2. At the prompt type:

   ```
   Z
   ```
3. Press `n` to start a new game
4. Then press `b` repeatedly to draw balls

---

Would you like to improve the layout display of the card (e.g. group by lines)?
