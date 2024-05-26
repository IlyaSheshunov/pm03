# pm03
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.table.*;
import java.text.DecimalFormat;

public class Main extends JFrame implements ActionListener {

    private JTextField purchasePriceField;
    private JTextField interestRateField;
    private JTextField loanTermField;
    private JLabel monthlyPaymentLabel;
    private JTable paymentScheduleTable;
    private JButton calculateButton;
    private JRadioButton annuityRadioButton;
    private JRadioButton differentiatedRadioButton;
    private ButtonGroup paymentFrequencyGroup;

    private double monthlyPayment;

    public Main() {
        super("Кредитный калькулятор");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(850, 800);
        setLayout(new FlowLayout());

        // Установка цветов
        Color backgroundColor = new Color(240, 240, 240);
        Color textColor = new Color(50, 50, 50);
        Color buttonColor = new Color(50, 100, 200);
        Color tableHeaderColor = new Color(150, 150, 150);
        Color tableRowColor = new Color(230, 230, 230);

        // Поля ввода и другие компоненты
        JLabel purchasePriceLabel = new JLabel("Сумма кредита:");
        purchasePriceField = new JTextField(10);
        JLabel interestRateLabel = new JLabel("Процентная ставка (%):");
        interestRateField = new JTextField(10);
        JLabel loanTermLabel = new JLabel("Срок кредита(лет):");
        loanTermField = new JTextField(10);

        monthlyPaymentLabel = new JLabel("");
        monthlyPaymentLabel.setForeground(textColor);

        // Создание таблицы схемы платежей
        String[] columnNames = {"Период", "Сумма платежа", "Основной долг", "Начисленные проценты", "Остаток задолженности"};
        paymentScheduleTable = new JTable(new DefaultTableModel(columnNames, 0));
        paymentScheduleTable.setPreferredScrollableViewportSize(new Dimension(800, 300));
        paymentScheduleTable.getTableHeader().setBackground(tableHeaderColor);
        paymentScheduleTable.getTableHeader().setForeground(textColor);
        paymentScheduleTable.setBackground(tableRowColor);
        paymentScheduleTable.setForeground(textColor);
        JScrollPane scrollPane = new JScrollPane(paymentScheduleTable);

        // Кнопка расчета
        calculateButton = new JButton("Рассчитать");
        calculateButton.addActionListener(this);
        calculateButton.setBackground(buttonColor);
        calculateButton.setForeground(Color.WHITE);

        // Переключение
        annuityRadioButton = new JRadioButton("Аннуитетный платеж", true);
        differentiatedRadioButton = new JRadioButton("Дифференцированный платеж");
        paymentFrequencyGroup = new ButtonGroup();
        paymentFrequencyGroup.add(annuityRadioButton);
        paymentFrequencyGroup.add(differentiatedRadioButton);
        annuityRadioButton.setForeground(textColor);
        differentiatedRadioButton.setForeground(textColor);

        // Добавление элементов на форму
        getContentPane().setBackground(backgroundColor);
        add(purchasePriceLabel);
        add(purchasePriceField);
        add(interestRateLabel);
        add(interestRateField);
        add(loanTermLabel);
        add(loanTermField);
        add(monthlyPaymentLabel);
        add(scrollPane);
        add(calculateButton);
        add(annuityRadioButton);
        add(differentiatedRadioButton);

        // Обработчики событий для радиокнопок
        annuityRadioButton.addActionListener(this);
        differentiatedRadioButton.addActionListener(this);

        setVisible(true);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == calculateButton) {
            try {
                // Получение значений из полей ввода
                double purchasePrice = Double.parseDouble(purchasePriceField.getText());
                double interestRate = Double.parseDouble(interestRateField.getText()) / 100;
                int loanTerm = Integer.parseInt(loanTermField.getText());

                // Расчет фактического остатка
                double actualBalance = purchasePrice;

                // Выбор типа платежа
                if (annuityRadioButton.isSelected()) {
                    // Расчет аннуитетного платежа
                    monthlyPayment = actualBalance * (interestRate / 12) / (1 - Math.pow(1 + interestRate / 12, -loanTerm * 12));
                } else if (differentiatedRadioButton.isSelected()) {
                    // Расчет дифференцированного платежа
                    double principalPayment = actualBalance / (loanTerm * 12);
                    monthlyPayment = principalPayment + (actualBalance * (interestRate / 12));
                }

                // Форматирование вывода
                DecimalFormat df = new DecimalFormat("#.##");

                // Создание таблицы схемы платежей
                DefaultTableModel model = (DefaultTableModel) paymentScheduleTable.getModel();
                model.setRowCount(0); // Очистка таблицы

                double remainingBalance = actualBalance;
                double totalPayment = 0;
                double totalPrincipal = 0;
                double totalInterest = 0;

                if (annuityRadioButton.isSelected()) {
                    // Аннуитетный платеж
                    for (int i = 1; i <= loanTerm * 12; i++) {
                        double interestPayment = remainingBalance * (interestRate / 12);
                        double principalPayment = monthlyPayment - interestPayment;
                        remainingBalance -= principalPayment;

                        Object[] row = {i, df.format(monthlyPayment), df.format(principalPayment), df.format(interestPayment), df.format(remainingBalance)};
                        model.addRow(row);

                        totalInterest += interestPayment;
                        totalPrincipal += principalPayment;
                        totalPayment += monthlyPayment;
                    }
                } else if (differentiatedRadioButton.isSelected()) {
                    // Дифференцированный платеж
                    for (int i = 1; i <= loanTerm * 12; i++) {
                        double principalPayment = actualBalance / (loanTerm * 12);
                        double interestPayment = remainingBalance * (interestRate / 12);
                        double monthlyPayment = principalPayment + interestPayment;
                        remainingBalance -= principalPayment;

                        Object[] row = {i, df.format(monthlyPayment), df.format(principalPayment), df.format(interestPayment), df.format(remainingBalance)};
                        model.addRow(row);

                        totalInterest += interestPayment;
                        totalPrincipal += principalPayment;
                        totalPayment += monthlyPayment;
                    }
                }
                // Добавление строки с итогами
              Object[] totalRow = {"Итого", df.format(totalPayment), df.format(totalPrincipal), df.format(totalInterest), "-"};
              model.addRow(totalRow);

              // Обновление размера таблицы
              paymentScheduleTable.setRowHeight(20);
              paymentScheduleTable.getColumnModel().getColumn(0).setPreferredWidth(50);
              paymentScheduleTable.getColumnModel().getColumn(1).setPreferredWidth(100);
              paymentScheduleTable.getColumnModel().getColumn(2).setPreferredWidth(100);
              paymentScheduleTable.getColumnModel().getColumn(3).setPreferredWidth(100);
              paymentScheduleTable.getColumnModel().getColumn(4).setPreferredWidth(100);
              paymentScheduleTable.validate();

              // Обновление интерфейса

              } catch (NumberFormatException ex) {
                  JOptionPane.showMessageDialog(this, "Неверный формат данных!");
              }
              } else if (e.getSource() == annuityRadioButton) {
                  monthlyPaymentLabel.setText("");
              } else if (e.getSource() == differentiatedRadioButton) {
                  monthlyPaymentLabel.setText("");
              }
              }

              public static void main(String[] args) {
                  new Main();
              }
              }
