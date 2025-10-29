# WPF (Windows Presentation Foundation)

## WPF 애플리케이션

* WPF 애플리케이션은 System.Windows 네임스페이스의 Application 클래스의 인스턴스를 가지고 있습니다.

* 코드는 "외형"(창 속성)과 "행동"(이벤트 처리)을 정의하는 두 부분을 나뉩니다.
  - 여기서 "외형"을 정의하는 코드를 따로 분리할 수 있는데 C# 코드를 통해 정의할 수도 있고, WPF의 경우 XAML로 정의할 수 있습니다.
  - XAML의 요소 이름은 .NET 클래스 이름이며, XAML 애트리뷰트는 해당 클래스의 프로퍼티 또는 이벤트입니다.
  - 기존 Windows Forms처럼 이벤트 함수를 등록할 수도 있는데 이 경우 *.xaml.cs (code-behind) 파일에 정의됩니다.

## XAML

### XAML 기초

* XAML은 .NET 오브젝트를 생성/초기화하기 위한 XML 기반 언어입니다. 다음은 Window 객체를 정의하는 빈 XAML 파일의 예제입니다.
  - Visual Studio에서는 WPF 프로젝트를 생성하면 WPF 디자이너를 이용하여 XAML을 시각적으로 생성/편집할 수 있습니다.

    ```xaml
    <Window x:Class="Carimatec3DPrinter.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:Carimatec3DPrinter"
            mc:Ignorable="d"
            Title="MainWindow" Height="450" Width="800">
        <Grid>

        </Grid>
    </Window>
    ```

* 중첩된 태그를 이용해서 요소를 다양하게 표현할 수 있습니다.
  - 예제 1: 이미지가 들어 있는 버튼
    ```xaml
    <Button Width="100" Height="100">
      <Image Source="image.png" />
    </Button>
    ```
  - 예제 2: 텍스트박스가 들어 있는 버튼
    ```xaml
    <Button Width="100" Height="100">
      <TextBox Width="75">edit me</TextBox>
    </Button>
    ```

### 레이아웃

* 레이아웃은 요소가 정렬되는 방식을 지정해 주며 다음과 같은 Panel들을 제공합니다. (컨테이너 역할)
  - Canvas: Canvas 내에 요소를 배치할 때 절대 위치를 직접 입력해야 하며, Canvas가 리사이즈되어도 내부 요소의 위치는 변경되지 않음
  - DockPanel: 내부 요소를 모서리에 배치함 (Left, Right, Top, Bottom)
  - Grid: 내부 요소에게 행(row)과 열(column) 번호를 부여하여 해당 위치에 배치함
  - StackPanel: 내부 요소를 특정 방향(Horizontal, Vertical)으로 배치함
  - UniformGrid: Grid와 동일하나 행과 열 너비를 균등하게 배치함
  - WrapPanel: StackPanel과 비슷하나 너비 또는 높이가 부족하면 다음 행 또는 열에 이어서 배치함

* Grid 레이아웃 예제
  - RowDefinition, ColumnDefinition 개수만큼 행/열이 생성됨
  - 내부 요소는 Row, Column 번호를 지정하여 위치를 정할 수 있고, RowSpan, ColumnSpan을 지정하여 차지하는 행/열 개수를 지정할 수 있음
    ```xaml
    <Window ...>
      <Grid>
        <Grid.RowDefinitions>
          <RowDefinition />
          <RowDefinition />
          <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
          <ColumnDefinition />
          <ColumnDefinition />
          <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <Button Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2">A</Button>
        <Button Grid.Row="0" Grid.Column="2">C</Button>
        <Button Grid.Row="1" Grid.Column="0" Grid.RowSpan="2">D</Button>
        <Button Grid.Row="1" Grid.Column="1">E</Button>
        <Button Grid.Row="1" Grid.Column="2">F</Button>
        <Button Grid.Row="2" Grid.Column="1">H</Button>
        <Button Grid.Row="2" Grid.Column="2">I</Button>
      </Grid>
    </Window>
    ```
  - 레이아웃도 다른 요소와 마찬가지로 요소 안에 중첩된 형태로 정의할 수 있음
    ```xaml
    <Button Width="100" Height="100">
      <Button.Content>
        <Grid>
          <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
          </Grid.RowDefinitions>
          <Image Grid.Row="0" Source="tom.png" />
          <TextBlock Grid.Row="1" HorizontalAlignment="Center">Tom</TextBlock>
        </Grid>
      </Button.Content>
    </Button>
    ```

### 데이터 바인딩

* 데이터 바인딩이란: 컨트롤과 데이터(변수, 함수) 간에 양방향 혹은 단방향으로 연결시켜 놓는 것을 의미함
  - 다음은 상단 바에 Name과 Nickname을 표시하고, 하단 바에 Add 버튼을 표시하고, 나머지 가운데 영역에 ListBox를 보여주는 예제입니다.
  - ListBox에서 아이템을 선택한 후 Add 버튼을 누르면 해당 아이템의 Name, Nickname을 상단에 표시합니다.
  - 바인딩하게 되는 요소는 __INotifyPropertyChanged__ 인터페이스를 상속해야 합니다.
  - 바인딩하는 요소들을 담는 컨테이너 객체로는 __ObservableCollection__ 을 사용해야 합니다.
    ```csharp
    // 클래스 Nickname
    public class Nickname : INotifyPropertyChanged {
        // INotifyPropertyChanged Member
        public event PropertyChangedEventHandler PropertyChanged;  // 인터페이스 정의 때문에 PropertyChanged 이벤트 핸들러가 포함되어 있어야 함
        void Notify(string propName) {
            if (PropertyChanged != null) {
              PropertyChanged(this, new PropertyChangedEventArgs(propName));
            }
        }
        string name;
        public string Name {
            get { return name; }
            set {
                name = value;
                Notify("Name"); // notify consumers
            }
        }
        string nick;
        public string Nick {
            get { return nick; }
            set {
                nick = value;
                Notify("Nick"); // notify consumers
            }
        }
        public Nickname() : this("name", "nick") { }
        public Nickname(string name, string nick) {
            this.name = name;
            this.nick = nick;
        }
    }

    // Notify consumers
    public class Nicknames : ObservableCollection<Nickname> { }  // ObservableCollection 객체

    // 윈도우 객체
    public partial class Window1 : Window {
        Nicknames names;  // Nickname 컬렉션
    
        public Window1() {
            InitializeComponent();
            this.addButton.Click += addButton_Click;

            // Nickname 컬렉션 생성
            this.names = new Nicknames();
    
            // dockPanel에 바인딩 (DataContext 프로퍼티 활용)
            dockPanel.DataContext = this.names;
        }
    
        void addButton_Click(object sender, RoutedEventArgs e) {
            this.names.Add(new Nickname());
        }
    }
    ```
  - {Binding ...} 문법을 통해 변수를 연결할 수 있습니다. dockPanel에 바인딩한 것은 자식 요소인 ListBox의 ItemsSource로 연결되게 됩니다. (Binding만 써도 알아서 감지함) IsSynchronizedWithCurrentItem가 True이므로 TextBox는 현재 선택한 아이템(Nickname 객체)의 Name과 Nick이 표시됩니다.
    ```xaml
    <!-- Window1.xaml -->
    <Window ...>
      <DockPanel x:Name="dockPanel">
        <TextBlock DockPanel.Dock="Top">
          <TextBlock VerticalAlignment="Center">Name: </TextBlock>
          <TextBox Text="{Binding Path=Name}" />
          <TextBlock VerticalAlignment="Center">Nick: </TextBlock>
          <TextBox Text="{Binding Path=Nick}" />
        </TextBlock>
        <Button DockPanel.Dock="Bottom" x:Name="addButton">Add</Button>
        <ListBox
          ItemsSource="{Binding}"
          IsSynchronizedWithCurrentItem="True" />
      </DockPanel>
    </Window>
    ```



<!-- Programming WPF 2nd edition 참조... 페이지 49/867 -->
