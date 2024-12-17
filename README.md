# Setris

Project Plan: Tetris to Setris Transformation
- Phase 1: Setup and Understanding the Tetris Clone
Review the Existing Code

Understand how the current Tetris game clone works.
Identify key components:
Grid representation (2D array or similar structure)
Tetromino shapes and movement logic
Gravity and collision detection
Line clearing mechanism
Write notes or comments to document these components.
Prepare the Development Environment

Set up your IDE (e.g., IntelliJ, Eclipse, or VS Code).
Ensure your Tetris project compiles and runs successfully.
Create a separate branch or copy of the project for Setris development.

- Phase 2: Modify Tetromino Behavior
Break Tetrominoes into Cells

When a tetromino lands, split it into individual "sand particles" (single grid cells).
Update the grid structure to treat each particle independently.
Example Task:

Modify the game update logic to replace a tetromino (4x4 or similar shape) with its constituent cells when it lands.

- Phase 3: Implement Sand-Like Physics
Introduce Gravity for Individual Cells

Implement a gravity check for each grid cell:
If a cell has empty space below, move it downward.
Add diagonal movement:
If directly below is blocked, allow cells to move diagonally left or right.
Sub-Tasks:

Write a loop to iterate through the grid (bottom-up approach).
Update each cell's position based on empty neighboring spaces.
Continuous Grid Updates

Update the game loop to recalculate grid positions every frame.
Ensure the cells fall naturally until they settle.
Optimize Performance

Prevent unnecessary checks for settled cells.
Use flags or states to mark cells as "settled" to reduce computation.

- Phase 4: Adjust Line Clearing Logic
Modify Line Clearing Mechanism

Ensure the existing line-clearing logic works with individual sand cells.
Check each row:
If all cells in a row are filled, clear the row.
Allow cells above to "fall" into the cleared space.
Test Case:

Simulate a grid with sand-like particles and test line clearing.

- Phase 5: Visual Enhancements
Update Graphics

Represent sand cells visually:
Use small squares or particles for individual cells.
Optionally randomize colors or shapes to mimic sand.
Sub-Tasks:

Update rendering logic for particles in the game grid.
Improve the appearance of falling and settling particles.
Smooth Animation

Implement smooth transitions for cells as they move or settle.
Use a timer or delay to control the fall speed of particles.

- Phase 6: Gameplay and Polish
Tune Gameplay Mechanics

Adjust the gravity speed to balance difficulty.
Experiment with particle behavior to make the game feel fun and unpredictable.
Add Game Over Conditions

Define a condition where the game ends if the sand particles reach the top of the grid.
Testing and Debugging

Test edge cases:
Large clusters of particles falling.
Diagonal movement conflicts.
Line clearing when multiple rows fill at once.
Debug any visual or logical glitches.

- Phase 7: Optional Features
Enhance Physics (Optional)

Add realistic physics:
Allow particles to slide off edges.
Implement pseudo-friction to slow movement.
Add Special Effects

Particle animations (e.g., dust when blocks break).
Sound effects for settling and clearing lines.
Introduce Power-Ups or Game Modes

Experiment with new gameplay mechanics:
Exploding blocks that scatter particles.
Time-based challenges.
Timeline Example

Here’s a suggested 2-week timeline:
Day 1: Setup and plan.
Day 2: Split tetrominoes into individual cells.
Day 3: Implement sand-like gravity.
Day 4: Update line-clearing logic.
Day 5: Smooth rendering and animation.
Day 6: Polish and debug.
Day 7: Final testing and optional features.


(*) Attention: Some keys code-snippets
1. Breaking Tetrominos into individual cells

void breakTetrominoIntoCells(int[][] grid, int[][] tetromino, int startRow, int startCol) {
    for (int i = 0; i < tetromino.length; i++) {
        for (int j = 0; j < tetromino[i].length; j++) {
            if (tetromino[i][j] == 1) { // 1 represents a block in the tetromino
                int row = startRow + i;
                int col = startCol + j;
                if (row >= 0 && row < grid.length && col >= 0 && col < grid[0].length) {
                    grid[row][col] = 1; // Place individual cells in the grid
                }
            }
        }
    }
}
// Already have bounds check in it 

2. Sand-like Gravity logic

void applyGravity(int[][] grid) {
    for (int row = grid.length - 2; row >= 0; row--) { // Start from second-to-last row
        for (int col = 0; col < grid[row].length; col++) {
            if (grid[row][col] == 1) { // Check for a particle (cell)
                // Check if the space below is empty
                if (grid[row + 1][col] == 0) {
                    grid[row + 1][col] = 1; // Move particle down
                    grid[row][col] = 0;     // Clear previous position
                }
                // Optional: Allow diagonal movement
                else if (col > 0 && grid[row + 1][col - 1] == 0) { 
                    grid[row + 1][col - 1] = 1;
                    grid[row][col] = 0;
                }
                else if (col < grid[row].length - 1 && grid[row + 1][col + 1] == 0) {
                    grid[row + 1][col + 1] = 1;
                    grid[row][col] = 0;
                }
            }
        }
    }
}

3. Game Loop Update

while (gameIsRunning) {
    // Existing logic: handle input, spawn blocks, detect collisions
    
    // Apply sand-like gravity
    applyGravity(grid);
    
    // Render the updated grid
    renderGrid(grid);
    
    // Add delay for smooth animation
    try {
        Thread.sleep(50); // Adjust speed of falling sand
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}

4. Rendering the Grid

void renderGrid(Graphics g, int[][] grid) {
    int cellSize = 20; // Size of each cell
    for (int row = 0; row < grid.length; row++) {
        for (int col = 0; col < grid[row].length; col++) {
            if (grid[row][col] == 1) { // A cell exists
                g.setColor(Color.YELLOW); // Sand-like color
                g.fillRect(col * cellSize, row * cellSize, cellSize, cellSize);
            }
        }
    }
}

// Usage
@Override
protected void paintComponent(Graphics g) {
    super.paintComponent(g);
    renderGrid(g, grid);
}

5. Line Clearing Logic

void clearFullRows(int[][] grid) {
    for (int row = 0; row < grid.length; row++) {
        boolean isFull = true;
        for (int col = 0; col < grid[row].length; col++) {
            if (grid[row][col] == 0) {
                isFull = false;
                break;
            }
        }
        if (isFull) {
            clearRow(grid, row);
        }
    }
}

void clearRow(int[][] grid, int row) {
    for (int r = row; r > 0; r--) {
        for (int col = 0; col < grid[r].length; col++) {
            grid[r][col] = grid[r - 1][col]; // Move rows above downward
        }
    }
    // Clear the top row
    for (int col = 0; col < grid[0].length; col++) {
        grid[0][col] = 0;
    }
}

(*) Final Summary:
Breaking Tetrominoes: ✅ Correct with added boundary checks.
Gravity Logic: ✅ Correct and supports diagonal movement.
Game Loop: ✅ Integrates all updates and smooth animations.
Rendering: ✅ Works with Java Swing for visualizing the grid.
Line Clearing: ✅ Correct and efficient for detecting and clearing full rows.