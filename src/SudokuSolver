import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class SudokuSolver extends JFrame {
    private static final int SIZE = 9; // Size of the Sudoku grid
    private JTextField[][] grid = new JTextField[SIZE][SIZE];
    private JButton solveButton, resetButton, stopButton; // Added stop button
    private volatile boolean isSolving = false; // Flag to indicate if solving is in progress
    private volatile boolean shouldStop = false; // Flag to stop the solving process

    public SudokuSolver() {
        setTitle("Sudoku Solver");
        setSize(400, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(SIZE, SIZE));
        for (int row = 0; row < SIZE; row++) {
            for (int col = 0; col < SIZE; col++) {
                grid[row][col] = new JTextField(1);
                grid[row][col].setHorizontalAlignment(JTextField.CENTER);
                grid[row][col].setFont(new Font("Arial", Font.BOLD, 20)); // Increase font size
                panel.add(grid[row][col]);
            }
        }

        solveButton = new JButton("Solve");
        resetButton = new JButton("Reset");
        stopButton = new JButton("Stop"); // Initialize stop button

        solveButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                if (!isSolving) {
                    if (validateInput()) { // Check for valid input
                        shouldStop = false; // Reset stop flag
                        isSolving = true; // Set flag to indicate solving is in progress
                        new Thread(() -> solveSudoku()).start(); // Start solving in a new thread
                    } else {
                        JOptionPane.showMessageDialog(SudokuSolver.this, "Wrong input: duplicate number detected or invalid input!", "Input Error", JOptionPane.ERROR_MESSAGE);
                    }
                }
            }
        });

        resetButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                resetBoard();
            }
        });

        stopButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                shouldStop = true; // Set stop flag to true
                isSolving = false; // Indicate that solving is no longer in progress
            }
        });

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(solveButton);
        buttonPanel.add(resetButton);
        buttonPanel.add(stopButton); // Add stop button to the panel

        add(panel, BorderLayout.CENTER);
        add(buttonPanel, BorderLayout.SOUTH);
    }

    private void resetBoard() {
        for (int row = 0; row < SIZE; row++) {
            for (int col = 0; col < SIZE; col++) {
                grid[row][col].setText("");
                grid[row][col].setBackground(Color.WHITE);
            }
        }
        isSolving = false; // Reset solving flag
        shouldStop = false; // Reset stop flag
    }

    private void solveSudoku() {
        if (backtrack(0, 0)) {
            isSolving = false; // Reset flag if solved
            JOptionPane.showMessageDialog(this, "Puzzle solved!", "Success", JOptionPane.INFORMATION_MESSAGE); // Show success message
        } else {
            JOptionPane.showMessageDialog(this, "No solution found!", "Error", JOptionPane.ERROR_MESSAGE);
            isSolving = false; // Reset flag if no solution
        }
    }

    private boolean backtrack(int row, int col) {
        if (shouldStop) {
            return false; // Exit immediately if stop flag is set
        }

        if (row == SIZE) {
            return true; // Solved
        }

        if (col == SIZE) {
            return backtrack(row + 1, 0); // Move to the next row
        }

        if (!grid[row][col].getText().isEmpty()) {
            // Skip cells that are already filled
            return backtrack(row, col + 1);
        }

        for (int num = 1; num <= SIZE; num++) {
            if (isValid(row, col, num)) {
                // Place the number
                grid[row][col].setText(String.valueOf(num));
                grid[row][col].setBackground(Color.LIGHT_GRAY);

                // Delay using Thread.sleep
                delay(100); // Reduced delay for faster visual feedback

                // Check if the user has requested to stop
                if (shouldStop) {
                    return false; // Exit if the process should stop
                }

                // Recur
                if (backtrack(row, col + 1)) {
                    return true;
                }

                // Reset the cell if the number doesn't lead to a solution
                grid[row][col].setText("");
                grid[row][col].setBackground(Color.WHITE);
            }
        }
        return false; // Trigger backtracking
    }

    private boolean isValid(int row, int col, int num) {
        for (int x = 0; x < SIZE; x++) {
            if (grid[row][x].getText().equals(String.valueOf(num)) || 
                grid[x][col].getText().equals(String.valueOf(num)) || 
                grid[row / 3 * 3 + x / 3][col / 3 * 3 + x % 3].getText().equals(String.valueOf(num))) {
                return false;
            }
        }
        return true;
    }

    private boolean validateInput() {
        for (int row = 0; row < SIZE; row++) {
            for (int col = 0; col < SIZE; col++) {
                String value = grid[row][col].getText();
                if (!value.isEmpty()) {
                    int num;
                    try {
                        num = Integer.parseInt(value);
                    } catch (NumberFormatException e) {
                        return false; // Not a valid number
                    }
                    
                    if (num < 1 || num > 9) {
                        return false; // Invalid number
                    }
                    // Check for duplicates in row, column, and box
                    for (int x = 0; x < SIZE; x++) {
                        if (x != col && grid[row][x].getText().equals(value)) {
                            return false; // Duplicate in the same row
                        }
                        if (x != row && grid[x][col].getText().equals(value)) {
                            return false; // Duplicate in the same column
                        }
                        int boxRowStart = (row / 3) * 3;
                        int boxColStart = (col / 3) * 3;
                        for (int y = 0; y < 3; y++) {
                            for (int z = 0; z < 3; z++) {
                                if ((boxRowStart + y != row || boxColStart + z != col) &&
                                    grid[boxRowStart + y][boxColStart + z].getText().equals(value)) {
                                    return false; // Duplicate in the 3x3 box
                                }
                            }
                        }
                    }
                }
            }
        }
        return true; // Input is valid
    }

    private void delay(int milliseconds) {
        try {
            Thread.sleep(milliseconds); // Delay for visual feedback
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            SudokuSolver solver = new SudokuSolver();
            solver.setVisible(true);
        });
    }
}
