## 前言
由于业务需要，需要使用TextBox输入数字，包括小数。所以需要对TextBox的输入数据进行验证。使用的办法分为以下两种

## 方法一：Validation + Parameter
使用Binding.ValidationRules限制输入值的大小。  

xaml定义
````C#
 <UserControl.Resources>
    <rule:ValidationParams x:Key="validationParams0"/>
</UserControl.Resources>

<TextBox>
    <TextBox.Text>
        <Binding Path="pulsePeriod" Mode="TwoWay" UpdateSourceTrigger="PropertyChanged">
            <Binding.ValidationRules>
                <rule:pulsePeriodRule Params="{StaticResource validationParams0}"/>
            </Binding.ValidationRules>
        </Binding>
    </TextBox.Text>
</TextBox>

````

.cs定义
````C#
Binding binding = new Binding();
binding.Source = slider_pulsewidth2;
binding.Path = new PropertyPath(Slider.ValueProperty);
ValidationRules.ValidationParams validationParams = (ValidationRules.ValidationParams)Resources["validationParams0"];
BindingOperations.SetBinding(validationParams, ValidationRules.ValidationParams.DataProperty, binding);
````

rule定义
````C#
public class pulsePeriodRule:ValidationRule
{
    public ValidationParams Params
    {
        get;set;
    }
    public override ValidationResult Validate(object value, CultureInfo cultureInfo)
    {
        var d = Params.Data ;
        double pulsewidth = (double)d;
        if (double.TryParse(value.ToString(),out double myValue))
        {
            if (myValue >= 100 && myValue <= 2000 && myValue % 50 == 0 && myValue > pulsewidth)
            {
                return new ValidationResult(true, null);
            }
        }
        return new ValidationResult(false, "Input should between 100 and 2000, or Input % 50 != 0");
    }
}

public class ValidationParams : DependencyObject
{
    public static readonly DependencyProperty DataProperty = DependencyProperty.Register(
        "Data", typeof(object), typeof(ValidationParams), new FrameworkPropertyMetadata(null));

    public object Data
    {
        get { return GetValue(DataProperty); }
        set
        {
            SetValue(DataProperty, value);
        }
    }
}
````

### 重点
1. 由于pulsePeriodRule是继承自ValidationRule抽象类，所以不能再继承DependencyObject，所以不能再添加依赖属性。所以在自定义验证类中添加一个继承自DependencyObject类的参数ValidationParams，在参数类中再定义依赖属性。
2. Params不能直接binding，如果直接binding会出现错误如下“Cannot find governing FrameworkElement or FrameworkContentElement for target element”。这是因为binding必须指向一个能查找名字的元素。推荐通过“proxy object”解决问题。即在resource中定义Params，然后在cs中指定它的binding。

## 方法二：NumTextBox自定义

NumTextBox定义
````C#
public class NumTextBox : TextBox
{
    static NumTextBox()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(NumTextBox), new FrameworkPropertyMetadata(typeof(NumTextBox)));
    }
    /// <summary>
    /// Textbox可以保留的小数位数
    /// </summary>
    public int Decimals
    {
        get { return (int)GetValue(DecimalsProperty); }
        set { SetValue(DecimalsProperty, value); }
    }
    public static readonly DependencyProperty DecimalsProperty =
        DependencyProperty.Register("Decimals", typeof(int), typeof(NumTextBox), new PropertyMetadata(0));
    /// <summary>
    /// TextBox可以输入的最大值，默认最大值为100
    /// </summary>
    public double MaxNum
    {
        get { return (double)GetValue(MaxNumProperty); }
        set { SetValue(MaxNumProperty, value); }
    }
    public static readonly DependencyProperty MaxNumProperty =
        DependencyProperty.Register("MaxNum", typeof(double), typeof(NumTextBox), new PropertyMetadata(10000D));
    /// <summary>
    /// TextBox可以输入的最小值，默认最小值为0
    /// </summary>
    public double MinNum
    {
        get { return (double)GetValue(MinNumProperty); }
        set { SetValue(MinNumProperty, value); }
    }
    public static readonly DependencyProperty MinNumProperty =
        DependencyProperty.Register("MinNum", typeof(double), typeof(NumTextBox), new PropertyMetadata(0D));
    /// <summary>
    /// 当数值错误或越界时
    /// </summary>
    public bool Error
    {
        get { return (bool)GetValue(ErrorProperty); }
        set { SetValue(ErrorProperty, value); }
    }
    public static readonly DependencyProperty ErrorProperty =
        DependencyProperty.Register("Error", typeof(bool), typeof(NumTextBox), new PropertyMetadata(false));
    /// <summary>
    /// 数据之间的间隔，只能使用整数或一位小数
    /// </summary>
    public double Tick
    {
        get { return (double)GetValue(TickProperty); }
        set { SetValue(TickProperty, value); }
    }
    private static readonly DependencyProperty TickProperty =
        DependencyProperty.Register("Tick", typeof(double), typeof(NumTextBox), new PropertyMetadata(1D));

    //限制非法字符输入，只允许输入数字和小数点，且可以限制保留几位小数
    protected override void OnPreviewTextInput(TextCompositionEventArgs e)
    {
        Regex eText = new Regex("[^0-9.]+");
        if (eText.IsMatch(e.Text))
        {
            e.Handled = true;
        }
        else
        {
            Regex re = new Regex(@"^([1-9]{1}\d*)$");    //默认为整数
            if (Decimals == 2)
            {
                re = new Regex(@"^(([1-9]{1}\d*)|(0{1}))(\.\d{1,2})?$");//保留两位小数
            }
            else if (Decimals == 1)
            {
                re = new Regex(@"^(([1-9]{1}\d*)|(0{1}))(\.\d{1})?$");//保留一位小数
            }

            if (e.Text == "." && !Text.Contains(".") && Decimals != 0)
            {
                e.Handled = false;  //false保留输入，true屏蔽输入
            }
            else
                e.Handled = !re.IsMatch(Text.Insert(SelectionStart, e.Text));
            //Console.WriteLine($"限制非法字符输入状态{e.Handled}");
        }

        base.OnPreviewTextInput(e);
    }


    /// <summary>
    /// 键盘输入值事件(可以小键盘上的数字键和英文输入状态下的句点键盘)
    /// </summary>
    /// <param name="e"></param>
    //protected override void OnPreviewKeyDown(KeyEventArgs e)
    //{
    //    if (e.Key == Key.Back || //针对Backspace(退格)键
    //        e.Key == Key.Enter || //针对Enter(回车)键
    //        e.Key == Key.Tab ||//针对Tab键
    //        e.Key == Key.Left || //针对左方向键(即允许在文本框中移动光标)
    //        e.Key == Key.Right) //针对右方向键(即允许在文本框中移动光标)
    //    {
    //        e.Handled = false; //允许输入
    //    }
    //    else if ((e.Key >= Key.NumPad0 && e.Key <= Key.NumPad9) || //针对小键盘上的数字键(0-9)
    //     (e.Key == Key.Decimal)) //针对小键盘上的点号键
    //    {
    //        //如果文本框中已经有"."字符，此时再输入"."字符的话，会被屏蔽掉
    //        if (this.Text.Contains(".") && e.Key == Key.Decimal) //Key.Decimal是：小键盘上的点号键
    //        {
    //            e.Handled = true; //屏蔽输入
    //            return;
    //        }
    //        e.Handled = false; //允许输入
    //    }
    //    else if (((e.Key >= Key.D0 && e.Key <= Key.D9) || //针对数字键(0-9)
    //              (e.Key == Key.OemPeriod )) && //针对点号键
    //               e.KeyboardDevice.Modifiers != ModifierKeys.Shift) //针对按住Shfit键+数字键的情况(如Shift+1会输入"!")
    //    {
    //        //如果文本框中已经有"."字符，此时再输入"."字符的话，会被屏蔽掉
    //        if (this.Text.Contains(".") && e.Key == Key.OemPeriod) //Key.OemPeriod是：非小键盘上的点号键
    //        {
    //            e.Handled = true; //屏蔽输入
    //            return;
    //        }
    //        e.Handled = false; //允许输入
    //    }
    //    else
    //    {
    //        e.Handled = true; //屏蔽输入
    //    }

    //    base.OnPreviewKeyDown(e);
    //}


    protected override void OnTextChanged(TextChangedEventArgs e)
    {
        if (string.IsNullOrEmpty(this.Text))
        {
            return;
        }
        bool d = double.TryParse(this.Text, out double c);

        if (!d)
        {
            Error = true;
        }
        else if (c > MaxNum || c < MinNum || Text.EndsWith(".") || (int)(c * 10) % (int)(Tick * 10) != 0)
        {
            Error = true;
        }
        else
        {
            Error = false;
        }

        base.OnTextChanged(e);
    }
}
````
style设置
````C#
<SolidColorBrush x:Key="TextBox.Static.Border" Color="#FFABAdB3"/>
    <SolidColorBrush x:Key="TextBox.MouseOver.Border" Color="#FF7EB4EA"/>
    <SolidColorBrush x:Key="TextBox.Focus.Border" Color="#FF569DE5"/>
    <Style TargetType="{x:Type style:NumTextBox}">
        <Setter Property="Background" Value="{DynamicResource {x:Static SystemColors.WindowBrushKey}}"/>
        <Setter Property="BorderBrush" Value="{StaticResource TextBox.Static.Border}"/>
        <Setter Property="Foreground" Value="{DynamicResource {x:Static SystemColors.ControlTextBrushKey}}"/>
        <Setter Property="BorderThickness" Value="1"/>
        <Setter Property="KeyboardNavigation.TabNavigation" Value="None"/>
        <Setter Property="HorizontalContentAlignment" Value="Left"/>
        <Setter Property="FocusVisualStyle" Value="{x:Null}"/>
        <Setter Property="AllowDrop" Value="true"/>
        <Setter Property="ScrollViewer.PanningMode" Value="VerticalFirst"/>
        <Setter Property="Stylus.IsFlicksEnabled" Value="False"/>

        <Setter Property="InputMethod.IsInputMethodEnabled" Value="False"/>
        <Setter Property="InputScope" Value="Digits"/>

        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate  TargetType="{x:Type style:NumTextBox}">
                    <Grid>
                        <Border x:Name="border" BorderBrush="{TemplateBinding BorderBrush}" BorderThickness="{TemplateBinding BorderThickness}" Background="{TemplateBinding Background}" SnapsToDevicePixels="True">
                            <ScrollViewer x:Name="PART_ContentHost" Focusable="false" HorizontalScrollBarVisibility="Hidden" VerticalScrollBarVisibility="Hidden"/>
                        </Border>
                        <!-- BorderBrush="red"-->
                        <Border x:Name="error" BorderBrush="#FF6060" BorderThickness="2" Visibility="Collapsed"/>
                    </Grid>

                    <ControlTemplate.Triggers>
                        <Trigger Property="IsEnabled" Value="false">
                            <Setter Property="Opacity" TargetName="border" Value="0.56"/>
                        </Trigger>
                        <Trigger Property="IsMouseOver" Value="true">
                            <Setter Property="BorderBrush" TargetName="border" Value="{StaticResource TextBox.MouseOver.Border}"/>
                        </Trigger>
                        <Trigger Property="IsKeyboardFocused" Value="true">
                            <Setter Property="BorderBrush" TargetName="border" Value="{StaticResource TextBox.Focus.Border}"/>
                        </Trigger>
                        <Trigger Property="Error" Value="True">
                            <Setter Property="Visibility" TargetName="error" Value="Visible"/>
                        </Trigger>
                    </ControlTemplate.Triggers>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
        <Style.Triggers>
            <MultiTrigger>
                <MultiTrigger.Conditions>
                    <Condition Property="IsInactiveSelectionHighlightEnabled" Value="true"/>
                    <Condition Property="IsSelectionActive" Value="false"/>
                </MultiTrigger.Conditions>
                <Setter Property="SelectionBrush" Value="{DynamicResource {x:Static SystemColors.InactiveSelectionHighlightBrushKey}}"/>
            </MultiTrigger>
        </Style.Triggers>
    </Style>
````


重点：
1. 重写OnPreviewTextInput或OnPreviewKeyDown可以做到屏蔽非数字相关的字符输入
2. ErrorDependency可以判断输入的值是否正确，如果不正确会标红。但是存在一个问题，即使显示数据不对，但也可以设置Text成功。和Validation比较，Validation验证失败时不会将该值设置。（如果大家对于NumTextBox验证有更好的办法，欢迎留言讨论）
3. 在style中需要设置InputMethod.IsInputMethodEnabled为false，因为在中文输入法下还是能够强制输入字符无法做到屏蔽，需要保持为英文输入法。

## 参考
1. https://stackoverflow.com/questions/28645688/validationrule-with-parameters
2. https://blog.csdn.net/duan20102480102/article/details/80571722
