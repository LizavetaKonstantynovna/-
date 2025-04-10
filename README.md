# -
using System.Text;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;
using static System.Net.Mime.MediaTypeNames;

namespace кнопка_wpf
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        private double x = 0;
        private double y = 0;
        private string Operation = "";
        private bool Decimal = false;

        public MainWindow()
        {
            InitializeComponent();
        }


        void OnClick2(object sender, RoutedEventArgs e )
        {
            var btn = (Button)sender;
            var number = (string)btn.Content;

            if (y == 0 && Screen.Text != "0")
            {
                Screen.Text += number;
            }
            else if (y == 0)
            {
                Screen.Text = number;
            }
            else 
            {
                Screen.Text += number;
            }
            //Screen.Text += number;

            if (Decimal)
            {
                y = y * 10 + Convert.ToInt32(number);
                y = y / 10;
            }
            else
            {
                //y = Convert.ToDouble(Screen.Text);
                y = y*10+Convert.ToInt32(number);

            }


            //y = y*10+Convert.ToInt32(number);
        }

        void OnClickSign(object sender, RoutedEventArgs e )
        {
            var btn = (Button)sender;
            var sign = (string)btn.Content;
            //if (Operation == "")
            //{
            //    x = y;
            //    y = 0;
            //    if (sign != "=")
            //    {
            //        Operation = sign;
            //        Screen.Text = x + Operation; // Выводим выражение, если это не "="
            //    }
            //    else
            //    {
            //        Screen.Text = x.ToString(); // Просто выводим x, если это "=" и первая операция
            //    }
            //}
            //else
            //{
            //    switch (Operation)
            //    {
            //        case "+": x = x + y; break;
            //        case "-": x = x - y; break;
            //        case "*": x = x * y; break;
            //        case "/":
            //            if (y != 0)
            //            {
            //                x = x / y;
            //            }
            //            else
            //            {
            //                MessageBox.Show("Деление на ноль!", "Ошибка");
            //                ToClean(null, null);
            //                return;
            //            }
            //            break;
            //    }

            //    if (sign == "=")
            //    {
            //        Screen.Text = x.ToString(); // Выводим только число, если это "="
            //        Operation = "";  // Сбрасываем операцию для дальнейших вычислений
            //        y = 0; // Сбрасываем y
            //    }
            //    else
            //    {
            //        Operation = sign;
            //        Screen.Text = x + Operation; // Выводим новое выражение
            //    }
            //}
            //Decimal = false;
            int Flag = 0;

            if (Operation == "")
            {
                x = y;
                y = 0;
            }
            else
            {
                switch (Operation)
                {
                    case "+": x = x + y; break;
                    case "-": x = x - y; break;
                    case "*": x = x * y; break;
                    case "/":
                        if (y != 0)
                        {
                            x = x / y;
                        }
                        else
                        {
                            MessageBox.Show("Деление на ноль!", "Ошибка");
                            ToClean(null, null);
                            return;
                        }
                        break;
                    case "=":
                        //Screen.Text = Convert.ToString(x);
                        //Operation = "";
                        Flag = 1;
                        break;


                }
            }
            y = 0;
            Operation = sign;
            if( Flag == 0)
            {
                Screen.Text = x + Operation;
            }
            else
            {
                Screen.Text = x + "";
            }
            //Screen.Text = x + Operation;
            Decimal = false;



        }

        void ChangeOfSign(object sender, RoutedEventArgs e)
        {
            Screen.Text=Convert.ToString(-y);
            y = -y;
        }

        void ClicOnPoint(object sender, RoutedEventArgs e)
        {
            if ((Screen.Text.Contains(",") && Operation=="") || Screen.Text.Count(c => c == ',') >= 2 || (Screen.Text[Screen.Text.Length - 1] == '+' || Screen.Text[Screen.Text.Length - 1] == '-' || Screen.Text[Screen.Text.Length - 1] == '*' || Screen.Text[Screen.Text.Length - 1] == '/'))
            {
                Screen.Text += "";
            }
            else 
            {
                Screen.Text += ",";
                Decimal = true;
            }
        }
        void ToCleanOne(object sender, RoutedEventArgs e)
        {
            string text = Screen.Text;
            if (text.Length > 0)
            {
                if (text[text.Length - 1]=='+' || text[text.Length - 1] == '-' || text[text.Length - 1] == '*' || text[text.Length - 1] == '/' )
                {
                    Operation = "";
                }
                text = text.Substring(0, text.Length - 1);
                if (text.Length == 0)
                {
                    text = "0";
                }
                Screen.Text = text;

                if (Decimal)
                {
                    //y = Convert.ToDouble(Screen.Text);
                    if (!text.Contains(","))
                    {
                        Decimal = false;
                        y = Convert.ToInt32(Screen.Text);
                    }
                    else
                    {
                        y = Convert.ToDouble(Screen.Text);
                    }
                }
                else
                {
                    y = Convert.ToInt32(Screen.Text);
                }



            }
        }



            // Очистка
        private void ToClean(object sender, RoutedEventArgs e)
        {
            Screen.Text = "0";
            x = 0;
            Operation = "";
            y = 0;
            Decimal = false;

        }



    }
}
