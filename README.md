# .NET-lab-4
using System;
using System.Text;

namespace Lab4_DelegatesAndEvents
{
    public delegate double MathFunction(double x);

    public class KeyProcessor
    {
        public delegate void NameKeyHandler();
        public event NameKeyHandler OnNameInitialKeyPressed;

        public void StartListening()
        {
            Console.WriteLine("\n--- Завдання 2: Події ---");
            Console.WriteLine("Натискайте клавіші. Натисніть 'М' (або 'M'), щоб викликати подію. Натисніть 'Esc' для завершення.");

            while (true)
            {
                var keyInfo = Console.ReadKey(true);
                if (keyInfo.Key == ConsoleKey.Escape) break;

                if (keyInfo.KeyChar == 'м' || keyInfo.KeyChar == 'М' ||
                    keyInfo.KeyChar == 'm' || keyInfo.KeyChar == 'M')
                {
                    OnNameInitialKeyPressed?.Invoke();
                }
            }
        }
    }

    class Program
    {
        static double CalculateIntegral(MathFunction f, double a, double b, int n)
        {
            double dx = (b - a) / n;
            double sum = 0;

            for (int i = 1; i <= n; i++)
            {
                double x = a + i * dx;
                sum += f(x);
            }

            return sum * dx;
        }

        static void Main()
        {
            Console.OutputEncoding = Encoding.UTF8;
            Console.InputEncoding = Encoding.UTF8;

            Console.WriteLine("--- Завдання 1: Інтеграли ---");

            double a = 1.0;
            double b = 4.0;
            int n = 1000;

            // Функція 1: f(x) = 1 / cbrt(x)
            MathFunction func1 = x => 1.0 / Math.Pow(x, 1.0 / 3.0);
            double result1 = CalculateIntegral(func1, a, b, n);
            Console.WriteLine($"Інтеграл 1 / cbrt(x) на [{a}, {b}]: {result1:F4}");

            // Функція 2: f(x) = e^x / sqrt(x)
            MathFunction func2 = x => Math.Exp(x) / Math.Sqrt(x);
            double result2 = CalculateIntegral(func2, a, b, n);
            Console.WriteLine($"Інтеграл e^x / sqrt(x) на [{a}, {b}]: {result2:F4}");

            // Функція 3: f(x) = log2(x)
            MathFunction func3 = x => Math.Log2(x);
            double result3 = CalculateIntegral(func3, a, b, n);
            Console.WriteLine($"Інтеграл log2(x) на [{a}, {b}]: {result3:F4}");

            // Аналітика та похибка для f(x) = x^2
            Console.WriteLine("\n--- Завдання 1.2: Перевірка похибки для f(x) = x^2 ---");
            MathFunction testFunc = x => x * x;

            double analyticalResult = (Math.Pow(b, 3) - Math.Pow(a, 3)) / 3.0;
            double numericalResult = CalculateIntegral(testFunc, a, b, n);

            Console.WriteLine($"Аналітичне значення: {analyticalResult:F4}");
            Console.WriteLine($"Чисельне значення: {numericalResult:F4}");
            Console.WriteLine($"Похибка: {Math.Abs(analyticalResult - numericalResult):F6}");

            // Запуск обробки подій
            KeyProcessor processor = new KeyProcessor();

            processor.OnNameInitialKeyPressed += () =>
            {
                Console.WriteLine("\n[Подія] Максим Печкур");
            };

            processor.StartListening();
        }
    }
}
