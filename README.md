# tic-tac-toe-group-work

package ui;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import javax.swing.JDialog;

public class XandO {
    ArrayList<Integer> playerOne = new ArrayList<>();
    ArrayList<Integer> playerTwo = new ArrayList<>();

    JFrame xAndO = new JFrame("Tic-Tac-Toe Game");
    JPanel myPanel = new JPanel(new GridLayout(3, 3));

    int flag = 0;
    String playerOneName = "Player 1";
    String playerTwoName = "Player 2";

    JButton[] buttons = {
            new JButton(), new JButton(), new JButton(),
            new JButton(), new JButton(), new JButton(),
            new JButton(), new JButton(), new JButton()
    };

    void showWelcomeScreen() {
        JDialog dialog = new JDialog();
        dialog.setUndecorated(true);
        dialog.setSize(Toolkit.getDefaultToolkit().getScreenSize());
        dialog.setLocationRelativeTo(null);

        JLabel title = new JLabel("🎮 WELCOME TO TIC-TAC-FUN! 🎉");
        title.setFont(new Font("Comic Sans MS", Font.BOLD, 60));
        title.setForeground(Color.MAGENTA);
        title.setHorizontalAlignment(SwingConstants.CENTER);

        JLabel subtitle = new JLabel("Get ready for an epic X and O battle of champions! 🤺💥");
        subtitle.setFont(new Font("Comic Sans MS", Font.ITALIC, 36));
        subtitle.setForeground(new Color(0, 102, 204));
        subtitle.setHorizontalAlignment(SwingConstants.CENTER);

        JButton continueButton = new JButton("🚀 Let's Play!");
        continueButton.setFont(new Font("Comic Sans MS", Font.BOLD, 32));
        continueButton.setBackground(new Color(255, 153, 0));
        continueButton.setForeground(Color.WHITE);
        continueButton.setFocusPainted(false);
        continueButton.addActionListener(e -> dialog.dispose());

        JPanel content = new JPanel(new GridLayout(3, 1, 30, 30));
        content.setBackground(new Color(255, 255, 204)); // Light yellow
        content.setBorder(BorderFactory.createEmptyBorder(100, 100, 100, 100));
        content.add(title);
        content.add(subtitle);
        content.add(continueButton);

        dialog.add(content);
        dialog.setModal(true);
        dialog.setVisible(true);
    }


    void askPlayerNames() {
        JDialog dialog = new JDialog();
        dialog.setUndecorated(true);
        dialog.setSize(Toolkit.getDefaultToolkit().getScreenSize());
        dialog.setLocationRelativeTo(null);

        JTextField player1Field = new JTextField();
        JTextField player2Field = new JTextField();

        JLabel p1Label = new JLabel("👤 Enter Player 1 Name (X):");
        JLabel p2Label = new JLabel("👤 Enter Player 2 Name (O):");

        p1Label.setFont(new Font("Comic Sans MS", Font.BOLD, 32));
        p2Label.setFont(new Font("Comic Sans MS", Font.BOLD, 32));
        player1Field.setFont(new Font("Arial", Font.PLAIN, 28));
        player2Field.setFont(new Font("Arial", Font.PLAIN, 28));

        JButton startButton = new JButton("🎲 Start Game!");
        startButton.setFont(new Font("Comic Sans MS", Font.BOLD, 30));
        startButton.setBackground(new Color(50, 205, 50)); // Lime Green
        startButton.setForeground(Color.WHITE);
        startButton.setFocusPainted(false);

        startButton.addActionListener(e -> {
            if (!player1Field.getText().trim().isEmpty()) {
                playerOneName = player1Field.getText().trim();
            }
            if (!player2Field.getText().trim().isEmpty()) {
                playerTwoName = player2Field.getText().trim();
            }
            dialog.dispose();
        });

        JPanel form = new JPanel(new GridLayout(5, 1, 20, 20));
        form.setBackground(new Color(204, 255, 255)); // Light cyan
        form.setBorder(BorderFactory.createEmptyBorder(100, 300, 100, 300));
        form.add(p1Label);
        form.add(player1Field);
        form.add(p2Label);
        form.add(player2Field);
        form.add(startButton);

        dialog.add(form);
        dialog.setModal(true);
        dialog.setVisible(true);
    }


    void drawGrid() {
        showWelcomeScreen();
        askPlayerNames();

        myPanel.removeAll();

        for (int i = 0; i < 9; i++) {
            JButton button = buttons[i];
            myPanel.add(button);
            int position = i + 1;
            addAction(button, position);
        }

        xAndO.setLayout(new BorderLayout());
        xAndO.add(myPanel, BorderLayout.CENTER);
        xAndO.setSize(400, 400);
        xAndO.setVisible(true);
        xAndO.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

    void addAction(JButton button, int position) {
        button.setFont(new Font("Arial", Font.BOLD, 40));
        button.setFocusPainted(false);
        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                if (flag == 0) {
                    playerOne.add(position);
                    button.setText("X");
                    flag = 1;
                } else {
                    playerTwo.add(position);
                    button.setText("O");
                    flag = 0;
                }
                button.setEnabled(false);
                checkWinOrDraw();
            }
        });
    }

    void checkWinOrDraw() {
        if (hasPlayerWon(playerOne)) {
            showWinnerDialog(playerOneName);
        } else if (hasPlayerWon(playerTwo)) {
            showWinnerDialog(playerTwoName);
        } else if (playerOne.size() + playerTwo.size() == 9) {
            showDrawDialog();
        }
    }

    boolean hasPlayerWon(ArrayList<Integer> player) {
        int[][] winPositions = {
                {1, 2, 3}, {4, 5, 6}, {7, 8, 9},
                {1, 4, 7}, {2, 5, 8}, {3, 6, 9},
                {1, 5, 9}, {3, 5, 7}
        };

        for (int[] combo : winPositions) {
            if (player.contains(combo[0]) && player.contains(combo[1]) && player.contains(combo[2])) {
                return true;
            }
        }
        return false;
    }

    void showWinnerDialog(String winner) {
        showResultDialog("🏆 " + winner.toUpperCase() + " WINS! 🥳");
    }

    void showDrawDialog() {
        showResultDialog("🤝 It's a Draw!");
    }

    void showResultDialog(String message) {
        JDialog dialog = new JDialog();
        dialog.setUndecorated(true);
        dialog.setSize(Toolkit.getDefaultToolkit().getScreenSize());
        dialog.setLocationRelativeTo(null);

        JLabel resultLabel = new JLabel(message);
        resultLabel.setFont(new Font("Impact", Font.BOLD, 60));
        resultLabel.setForeground(new Color(255, 69, 0)); // Orange-Red
        resultLabel.setHorizontalAlignment(SwingConstants.CENTER);

        JLabel promptLabel = new JLabel("🎯 Would you like to play again?");
        promptLabel.setFont(new Font("Comic Sans MS", Font.PLAIN, 36));
        promptLabel.setHorizontalAlignment(SwingConstants.CENTER);

        JButton playAgain = new JButton("🔁 Yes, Play Again!");
        JButton exit = new JButton("❌ Exit Game");

        playAgain.setFont(new Font("Comic Sans MS", Font.BOLD, 30));
        exit.setFont(new Font("Comic Sans MS", Font.BOLD, 30));

        playAgain.setBackground(new Color(0, 204, 102));
        playAgain.setForeground(Color.WHITE);
        exit.setBackground(new Color(255, 51, 51));
        exit.setForeground(Color.WHITE);

        playAgain.setFocusPainted(false);
        exit.setFocusPainted(false);

        playAgain.addActionListener(e -> {
            dialog.dispose();
            resetGame();
        });

        exit.addActionListener(e -> {
            JOptionPane.showMessageDialog(null, "🎉 Thanks for playing! 👋", "Bye Bye!", JOptionPane.INFORMATION_MESSAGE);
            System.exit(0);
        });

        JPanel buttonPanel = new JPanel(new FlowLayout());
        buttonPanel.setBackground(new Color(255, 239, 213)); // Papaya whip
        buttonPanel.add(playAgain);
        buttonPanel.add(exit);

        JPanel content = new JPanel(new GridLayout(3, 1, 30, 30));
        content.setBorder(BorderFactory.createEmptyBorder(100, 100, 100, 100));
        content.setBackground(new Color(255, 239, 213));
        content.add(resultLabel);
        content.add(promptLabel);
        content.add(buttonPanel);

        dialog.add(content);
        dialog.setModal(true);
        dialog.setVisible(true);
    }


    void resetGame() {
        playerOne.clear();
        playerTwo.clear();
        flag = 0;

        for (JButton button : buttons) {
            button.setText("");
            button.setEnabled(true);
        }
    }

    public static void main(String[] args) {
        XandO xandO = new XandO();
        xandO.drawGrid();
    }
}
