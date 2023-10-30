# -2.-
Урок 2. Данные и функции

1. Полностью разобраться с кодом программы разработанной на семинаре, переписать программу. Это минимальная задача для сдачи домашней работы.

package lesson2;

import java.util.Random;
import java.util.Scanner;

public class Program {

    private static final char DOT_HUMAN = 'X'; // Фишка игрока - человека
    private static final char DOT_AI = '0'; // Фишка игрока - компьютер
    private static final char DOT_EMPTY = '*'; // Признак пустого поля
    private static final Scanner scanner = new Scanner(System.in);
    private static  final Random random = new Random();
    private static char[][] field; // Двумерный массив хранит состояние игрового поля
    private static int fieldSizeX; // Размерность игрового поля
    private static int fieldSizeY; // Размерность игрового поля
    private static final int WIN_COUNT = 4; // Кол-во фишек для победы

    public static void main(String[] args) {
        while (true){
            initialize();
            printField();
            while (true){
                humanTurn();
                printField();
                if (gameCheck(DOT_HUMAN, "Вы победили!"))
                    break;
                aiTurn();
                printField();
                if (gameCheck(DOT_AI, "Победил компьютер!"))
                    break;
            }
            System.out.print("Желаете сыграть еще раз? (Y - да): ");
            if (!scanner.next().equalsIgnoreCase("Y"))
                break;
        }
    }

    /**
     * Инициализация начального состояния игры
     */
    private static void initialize(){
        fieldSizeX = 5;
        fieldSizeY = 5;
        field = new char[fieldSizeY][fieldSizeX];
        for (int y = 0; y < fieldSizeY; y++){
            for (int x = 0; x < fieldSizeX; x++){
                field[y][x] = DOT_EMPTY;
            }
        }
    }

    /**
     * Отрисовать текущее состояние игрового поля
     */
    private static void printField(){
        System.out.print("+");
        for (int i = 0; i < fieldSizeX*2 + 1; i++){
            System.out.print((i % 2 == 0) ? "-" : i / 2 + 1);
        }
        System.out.println();

        for (int i = 0; i < fieldSizeY; i++){
            System.out.print(i + 1 + "|");
            for (int j = 0; j < fieldSizeX; j++){
                System.out.print(field[i][j] + "|");
            }
            System.out.println();
        }

        for (int i = 0; i < fieldSizeX*2 + 2; i++){
            System.out.print("-");
        }
        System.out.println();
    }

    /**
     * Обработка хода игрока (человека)
     */
    private static void humanTurn(){
        int x, y;
        do{
            System.out.print("Укажите координаты хода X и Y (от 1 до 3)\nчерез пробел: ");
            x = scanner.nextInt() - 1;
            y = scanner.nextInt() - 1;
        }
        while (!isCellValid(x, y) || !isCellEmpty(x, y));
        field[x][y] = DOT_HUMAN;
    }

    /**
     * Обработка хода компьютера
     */
    static void aiTurn(){
        int x, y;
        do{
            x = random.nextInt(fieldSizeX);
            y = random.nextInt(fieldSizeY);
        }
        while (!isCellEmpty(x, y));
        field[x][y] = DOT_AI;
    }

    /**
     * Проверка, ячейка является пустой (DOT_EMPTY)
     * @param x
     * @param y
     * @return
     */
    static boolean isCellEmpty(int x, int y){
        return field[x][y] == DOT_EMPTY;
    }

    /**
     * Проверка состояния игры
     * @param dot фишка игрока
     * @param winStr победный слоган
     * @return признак продолжения игры (true - завершение игры)
     */
    static boolean gameCheck(char dot, String winStr){
        if (checkWin(dot)){
            System.out.println(winStr);
            return true;
        }
        if (checkDraw()){
            System.out.println("Ничья!");
            return true;
        }
        return false; // Продолжим игру
    }

    /**
     * Проверка корректности ввода
     * @param x
     * @param y
     * @return
     */
    static boolean isCellValid(int x, int y){
        return x >= 0 && x < fieldSizeX && y >= 0 && y < fieldSizeY;
    }

    /**
     * Проверка победы
     * @param c фишка игрока (X или 0)
     * @return
     */
    static boolean checkWin(char c){
        // Проверка по трем горизонталям
        if (field[0][0] == c && field[0][1] == c && field[0][2] == c) return true;
        if (field[1][0] == c && field[1][1] == c && field[1][2] == c) return true;
        if (field[2][0] == c && field[2][1] == c && field[2][2] == c) return true;

        // Проверка по трем вертикалям
        if (field[0][0] == c && field[1][0] == c && field[2][0] == c) return true;
        if (field[0][1] == c && field[1][1] == c && field[2][1] == c) return true;
        if (field[0][2] == c && field[1][2] == c && field[2][2] == c) return true;

        // Проверка по двум диагоналям
        if (field[0][0] == c && field[1][1] == c && field[2][2] == c) return true;
        if (field[0][2] == c && field[1][1] == c && field[2][0] == c) return true;

        return false;

    }

    static boolean check1(int x, int y, char dot, int win){
        return true;
    }

    /**
     * Проверка на ничью
     * @return
     */
    static boolean checkDraw(){
        for (int i = 0; i < fieldSizeY; i++){
            for (int j = 0; j < fieldSizeX; j++){
                if (isCellEmpty(i, j)) return false;
            }
        }
        return true;
    }

}


Усложняем задачу:

2.* Переработать метод проверки победы, логика проверки победы должна работать для поля 5х5 и
количества фишек 4. Очень желательно не делать это просто набором условий для каждой из
возможных ситуаций! Используйте вспомогательные методы, используйте циклы!



package lesson2;

import java.util.Random;
import java.util.Scanner;

public class Program2 {

    private static final char DOT_HUMAN = 'X'; // Фишка игрока - человека
    private static final char DOT_AI = '0'; // Фишка игрока - компьютер
    private static final char DOT_EMPTY = '*'; // Признак пустого поля
    private static final Scanner scanner = new Scanner(System.in);
    private static  final Random random = new Random();
    private static char[][] field; // Двумерный массив хранит состояние игрового поля
    private static int fieldSizeX; // Размерность игрового поля
    private static int fieldSizeY; // Размерность игрового поля
    private static final int WIN_COUNT = 4; // Кол-во фишек для победы

    public static void main(String[] args) {
        while (true){
            initialize();
            printField();
            while (true){
                humanTurn();
                printField();
                if (gameCheck(DOT_HUMAN, "Вы победили!"))
                    break;
                aiTurn();
                printField();
                if (gameCheck(DOT_AI, "Победил компьютер!"))
                    break;
            }
            System.out.print("Желаете сыграть еще раз? (Y - да): ");
            if (!scanner.next().equalsIgnoreCase("Y"))
                break;
        }
    }

    /**
     * Инициализация начального состояния игры
     */
    private static void initialize(){
        fieldSizeX = 5;
        fieldSizeY = 5;
        field = new char[fieldSizeY][fieldSizeX];
        for (int y = 0; y < fieldSizeY; y++){
            for (int x = 0; x < fieldSizeX; x++){
                field[y][x] = DOT_EMPTY;
            }
        }
    }

    /**
     * Отрисовать текущее состояние игрового поля
     */
    private static void printField(){
        System.out.print("+");
        for (int i = 0; i < fieldSizeX*2 + 1; i++){
            System.out.print((i % 2 == 0) ? "-" : i / 2 + 1);
        }
        System.out.println();

        for (int i = 0; i < fieldSizeY; i++){
            System.out.print(i + 1 + "|");
            for (int j = 0; j < fieldSizeX; j++){
                System.out.print(field[i][j] + "|");
            }
            System.out.println();
        }

        for (int i = 0; i < fieldSizeX*2 + 2; i++){
            System.out.print("-");
        }
        System.out.println();
    }

    /**
     * Обработка хода игрока (человека)
     */
    private static void humanTurn(){
        int x, y;
        do{
            System.out.print("Укажите координаты хода X и Y (от 1 до 3)\nчерез пробел: ");
            x = scanner.nextInt() - 1;
            y = scanner.nextInt() - 1;
        }
        while (!isCellValid(x, y) || !isCellEmpty(x, y));
        field[x][y] = DOT_HUMAN;
    }

    /**
     * Обработка хода компьютера
     */
    static void aiTurn(){
        int x, y;
        do{
            x = random.nextInt(fieldSizeX);
            y = random.nextInt(fieldSizeY);
        }
        while (!isCellEmpty(x, y));
        field[x][y] = DOT_AI;
    }

    /**
     * Проверка, ячейка является пустой (DOT_EMPTY)
     * @param x
     * @param y
     * @return
     */
    static boolean isCellEmpty(int x, int y){
        return field[x][y] == DOT_EMPTY;
    }

    /**
     * Проверка состояния игры
     * @param dot фишка игрока
     * @param winStr победный слоган
     * @return признак продолжения игры (true - завершение игры)
     */
    static boolean gameCheck(char dot, String winStr){
        if (checkWin(dot)){
            System.out.println(winStr);
            return true;
        }
        if (checkDraw()){
            System.out.println("Ничья!");
            return true;
        }
        return false; // Продолжим игру
    }

    /**
     * Проверка корректности ввода
     * @param x
     * @param y
     * @return
     */
    static boolean isCellValid(int x, int y){
        return x >= 0 && x < fieldSizeX && y >= 0 && y < fieldSizeY;
    }

    /**
     * Проверка победы
     * @param c фишка игрока (X или 0)
     * @return
     */
    static boolean checkWin(char c) {
        // Проверка по горизонталям
        for (int i = 0; i < fieldSizeY; i++) {
            int count = 0;
            for (int j = 0; j < fieldSizeX; j++) {
                if (field[i][j] == c) {
                    count++;
                    if (count == WIN_COUNT) return true;
                } else {
                    count = 0;
                }
            }
        }

        // Проверка по вертикалям
        for (int j = 0; j < fieldSizeX; j++) {
            int count = 0;
            for (int i = 0; i < fieldSizeY; i++) {
                if (field[i][j] == c) {
                    count++;
                    if (count == WIN_COUNT) return true;
                } else {
                    count = 0;
                }
            }
        }

        // Проверка по диагонали
        for (int i = 0; i <= fieldSizeY - WIN_COUNT; i++) {
            for (int j = 0; j <= fieldSizeX - WIN_COUNT; j++) {
                int count = 0;
                for (int k = 0; k < WIN_COUNT; k++) {
                    if (field[i + k][j + k] == c) {
                        count++;
                        if (count == WIN_COUNT) return true;
                    } else {
                        count = 0;
                    }
                }
            }
        }

        // Проверка по диагонали
        for (int i = 0; i <= fieldSizeY - WIN_COUNT; i++) {
            for (int j = fieldSizeX - 1; j >= WIN_COUNT - 1; j--) {
                int count = 0;
                for (int k = 0; k < WIN_COUNT; k++) {
                    if (field[i + k][j - k] == c) {
                        count++;
                        if (count == WIN_COUNT) return true;
                    } else {
                        count = 0;
                    }
                }
            }
        }

        return false;
    }


    static boolean check1(int x, int y, char dot, int win){
        return true;
    }

    /**
     * Проверка на ничью
     * @return
     */
    static boolean checkDraw(){
        for (int i = 0; i < fieldSizeY; i++){
            for (int j = 0; j < fieldSizeX; j++){
                if (isCellEmpty(i, j)) return false;
            }
        }
        return true;
    }

}










