# pm03
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Введите три различных целых числа:");
        if (scanner.hasNextInt()){
            int num1 = scanner.nextInt();
            int num2 = scanner.nextInt();
            int num3 = scanner.nextInt();

            int max = Math.max(Math.max(num1, num2), num3);
            int min = Math.min(Math.min(num1, num2), num3);

            System.out.println("Максимальное значение: " + max);
            System.out.println("Минимальное значение: " + min);
        } else {

            System.out.println("Ошибка: Введите три целых числа.");
            return;
        }

      // Задача 2
      System.out.println("Введите год рождения, номер месяца рождения, текущий год и номер текущего месяца:");
      if (scanner.hasNextInt()){
          int birthYear = scanner.nextInt();
          int birthMonth = scanner.nextInt();
          int currentYear = scanner.nextInt();
          int currentMonth = scanner.nextInt();

          if (currentYear < birthYear) {
              System.out.println("Ошибка: Введите правильные значения.");
              return;
          }

          int age = currentYear - birthYear;
          if (currentMonth < birthMonth) {
              age--;
          }

          System.out.println("Возраст человека: " + age + " лет.");
      } 

        // Задача 3
        System.out.println("Введите номер дня в году (1 ≤ k ≤ 365):");
        if (scanner.hasNextInt()){
            int k = scanner.nextInt();

            int dayOfWeek = k % 7;
            if (dayOfWeek == 0 || dayOfWeek == 6) {
                System.out.println("Выходной день.");
            } else {
                System.out.println("Рабочий день.");
            }
        } else {
            System.out.println("Ошибка: Введите целое число.");
            return;
        }

        // Задача 4
        System.out.println("Введите количество очков, полученных командой за игру:");
        if (scanner.hasNextInt()){
            int points = scanner.nextInt();

            if (points == 3) {
                System.out.println("Выигрыш.");
            } else if (points == 1) {
                System.out.println("Ничья.");
            } else if (points == 0) {
                System.out.println("Проигрыш.");
            } else {
                System.out.println("Некорректное количество очков.");
            }
        } else {
            System.out.println("Ошибка: Введите целое число.");
            return;
        }

        // Задача 5
        System.out.println("Введите порядковый номер месяца (1-12):");
        if (scanner.hasNextInt()){
            int month = scanner.nextInt();

            switch (month) {
                case 1:
                    System.out.println("Январь, Зима, 31 день.");
                    break;
                case 2:
                    System.out.println("Февраль, Зима, 28 дней.");
                    break;
                case 3:
                    System.out.println("Март, Весна, 31 день.");
                    break;
                case 4:
                    System.out.println("Апрель, Весна, 30 дней.");
                    break;
                case 5:
                    System.out.println("Май, Весна, 31 день.");
                    break;
                case 6:
                    System.out.println("Июнь, Лето, 30 дней.");
                    break;
                case 7:
                    System.out.println("Июль, Лето, 31 день.");
                    break;
                case 8:
                    System.out.println("Август, Лето, 31 день.");
                    break;
                case 9:
                    System.out.println("Сентябрь, Осень, 30 дней.");
                    break;
                case 10:
                    System.out.println("Октябрь, Осень, 31 день.");
                    break;
                case 11:
                    System.out.println("Ноябрь, Осень, 30 дней.");
              break;
                case 12:
                    System.out.println("Декабрь, Зима, 31 день.");
              break;
                 default:
                   System.out.println("Некорректный номер месяца.");
                            }
                        } else {
                   System.out.println("Ошибка: Введите целое число от 1 до 12.");
              return;
                        }
                    }
                }

