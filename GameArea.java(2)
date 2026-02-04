import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.util.Random;

public class GameArea extends JPanel implements ActionListener {

    static final int WIDTH = 600;
    static final int HEIGHT = 600;
    static final int SIZE = 25;
    static final int CELLS = (WIDTH * HEIGHT) / (SIZE * SIZE);
    int speed = 150;

    int[] snakeX = new int[CELLS];
    int[] snakeY = new int[CELLS];
    int length = 6;
    int score = 0;
    int foodX;
    int foodY;
    char move = 'R';
    boolean play = false;
    Timer timer;
    Random rand;
    int bestScore = 0;

    GameArea() {
        rand = new Random();
        this.setPreferredSize(new Dimension(WIDTH, HEIGHT));
        this.setBackground(Color.black);
        this.setFocusable(true);
        this.addKeyListener(new KeyHandler());
        loadBestScore();
        start();
    }

    public void start() {
        makeFood();
        play = true;
        timer = new Timer(speed, this);
        timer.start();
    }

    public void paintComponent(Graphics g) {
        super.paintComponent(g);
        draw(g);
    }

    public void draw(Graphics g) {
        if (play) {
            g.setColor(Color.red);
            g.fillOval(foodX, foodY, SIZE, SIZE);

            for (int i = 0; i < length; i++) {
                if (i == 0) g.setColor(new Color(0, 200, 0));
                else g.setColor(new Color(0, 120, 0));
                g.fillRect(snakeX[i], snakeY[i], SIZE, SIZE);
            }

            g.setColor(Color.white);
            g.setFont(new Font("Arial", Font.BOLD, 20));
            g.drawString("Score: " + score, 250, 25);

        } else {
            gameOver(g);
        }
    }

    public void makeFood() {
        foodX = rand.nextInt(WIDTH / SIZE) * SIZE;
        foodY = rand.nextInt(HEIGHT / SIZE) * SIZE;
    }

    public void moveSnake() {
        for (int i = length; i > 0; i--) {
            snakeX[i] = snakeX[i - 1];
            snakeY[i] = snakeY[i - 1];
        }

        switch (move) {
            case 'U':
                snakeY[0] -= SIZE;
                break;
            case 'D':
                snakeY[0] += SIZE;
                break;
            case 'L':
                snakeX[0] -= SIZE;
                break;
            case 'R':
                snakeX[0] += SIZE;
                break;
        }
    }

    public void checkFood() {
        if (snakeX[0] == foodX && snakeY[0] == foodY) {
            length++;
            score++;
            makeFood();

            if (speed > 50) {
                speed -= 5;
                timer.setDelay(speed);
            }
        }
    }

    public void checkHit() {
        // hit self
        for (int i = length; i > 0; i--) {
            if (snakeX[0] == snakeX[i] && snakeY[0] == snakeY[i]) {
                play = false;
            }
        }
        // hit wall
        if (snakeX[0] < 0 || snakeX[0] >= WIDTH || snakeY[0] < 0 || snakeY[0] >= HEIGHT) {
            play = false;
        }

        if (!play) {
            timer.stop();
            if (score > bestScore) {
                bestScore = score;
                saveBestScore();
            }
        }
    }

    public void gameOver(Graphics g) {
        g.setColor(Color.white);
        g.setFont(new Font("Arial", Font.BOLD, 25));
        g.drawString("Score: " + score, 240, 250);
        g.drawString("Best: " + bestScore, 240, 290);

        g.setColor(Color.red);
        g.setFont(new Font("Arial", Font.BOLD, 60));
        g.drawString("GAME OVER", 100, 350);
    }

    public void actionPerformed(ActionEvent e) {
        if (play) {
            moveSnake();
            checkFood();
            checkHit();
        }
        repaint();
    }

    class KeyHandler extends KeyAdapter {
        public void keyPressed(KeyEvent e) {
            switch (e.getKeyCode()) {
                case KeyEvent.VK_LEFT:
                    if (move != 'R') move = 'L';
                    break;
                case KeyEvent.VK_RIGHT:
                    if (move != 'L') move = 'R';
                    break;
                case KeyEvent.VK_UP:
                    if (move != 'D') move = 'U';
                    break;
                case KeyEvent.VK_DOWN:
                    if (move != 'U') move = 'D';
                    break;
            }
        }
    }

    private void loadBestScore() {
        try (BufferedReader br = new BufferedReader(new FileReader("best.txt"))) {
            bestScore = Integer.parseInt(br.readLine());
        } catch (Exception e) {
            bestScore = 0;
        }
    }

    private void saveBestScore() {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("best.txt"))) {
            bw.write(String.valueOf(bestScore));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
