# WPF (Windows Presentation Foundation)

본 내용은 O'REILLY에서 출판한 Programming WPF 2nd edition을 참고하였습니다.

## WPF 애플리케이션

* WPF 애플리케이션은 System.Windows 네임스페이스의 Application 클래스의 인스턴스를 가지고 있습니다.
  - 최상위 창: Application 객체의 MainWindow 프로퍼티에 설정된 창이며 1개 혹은 여러 개가 될 수도 있습니다.
  - 애플리케이션 셧다운 관련 옵션: 여러 최상위 창이 있을 때에는 OnLastWindowsClose, 단일 최상위 창이 있을 때에는 OnMainWindowClose, 수동 Shutdown 함수를 호출해야 할 경우에는 OnExplicitShutdown 모드를 설정해야 합니다.
    ```xaml
    <Application
      x:Class="AppWindowsSample.App"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      StartupUri="Window1.xaml"
      ShutdownMode="OnExplicitShutdown" />
    ```
  - 애플리케이션 이벤트: 생애 주기에 따라 Startup, Activated, Deactivated, DispatcherUnhandledException, SessionEnding, Exit 이벤트가 있습니다.
    * Startup 이벤트: 애플리케이션의 Run 메서드가 호출될 때 발동하는 이벤트입니다. (주로 초기화를 수행함, Mutex를 이용하여 다중 인스턴스를 방지할 수 있음)
    * Activated 이벤트: 최상위 창이 활성화되면 Activated 이벤트가 발동합니다.
    * Deactivated 이벤트: 다른 애플리케이션의 최상위 창이 활성화되면 Deactivated 이벤트가 발동합니다.
    * DispatcherUnhandledException 이벤트: 디스패처(dispatcher)는 이벤트를 처리하는 객체인데 만약 예외가 발생하면 데이터 손실을 막기 위해 이 이벤트를 활용할 수 있습니다.
    * SessionEnding 이벤트: 윈도우 세션이 끝날 때 호출됩니다. (윈도우의 종료, 로그아웃, 재시작)
    * Exit 이벤트: 애플리케이션이 닫힐 때 호출됩니다.

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

* 레이아웃은 다음과 같은 공통 프로퍼티를 가지고 있습니다.
  - Width, Height: 요소의 너비와 높이를 의미하며 이 값을 지정하면 요소 크기가 고정됩니다. 가변 크기일 경우 지정하지 마십시오.
  - MinWidth, MaxWidth: 요소의 최소 너비와 높이
  - MinHeight, MaxHeight: 요소의 최대 너비와 높이
  - HorizontalAlignment: Stretch (기본값이며 최대한 공간을 요소로 채우려고 함), Left, Center, Right (요소를 좌측/중앙/우측 정렬시킴)
  - VerticalAlignment: Stretch (기본값이며 최대한 공간을 요소로 채우려고 함), Top, Center, Bottom (요소를 상단/중앙/하단 정렬시킴)
  - Margin: 요소 외부의 여백 공간을 의미합니다. (4개의 숫자를 입력하면 왼쪽, 위, 오른쪽, 아래의 폭을 의미하고 2개의 숫자를 입력하면 좌우측과 위아래의 폭을 의미하고 1개의 숫자를 입력하면 모든 폭을 의미함)
  - Padding: 요소 내부의 여백 공간을 의미합니다. (내부에 요소를 품을 수 있는 요소만 가지고 있는 프로퍼티)
  - Visibility: Collapsed (요소가 없으면 차지하는 공간이 제로), Hidden (요소가 보이지 않아도 차지하는 공간은 유지됨), Visible (요소가 보임)
  - FlowDirection: 기본적으로는 시스템 로케일 설정을 따름, RightToLeft라고 입력하면 오른쪽부터 배치됨 (아랍어, 히브리어 등), 보통은 LeftToRight를 사용합니다.
  - Panel.ZIndex:  기본값은 0, 숫자가 커질수록 가장 위에 올라옵니다.
  - RenderTransform, LayoutTransform: 본 요소 및 자식 요소에 이동, 스케일링, 회전을 적용할 때 사용됩니다. (RenderTransform은 적용된 요소에만 효과가 발동하고, LayoutTransform은 적용된 요소와 그것을 감싸는 부모 요소까지 효과가 발동됨)

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
        // INotifyPropertyChanged 멤버
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

* 데이터 템플릿
  - ItemTemplate 프로퍼티를 설정하여 각 ListBox 아이템이 삽입될 데이터 템플릿을 지정합니다.
    ```xaml
    <ListBox
      ItemsSource="{Binding}"
      IsSynchronizedWithCurrentItem="True">
      <ListBox.ItemTemplate>
        <DataTemplate>
          <TextBlock>
            <TextBlock Text="{Binding Path=Name}" />:
            <TextBlock Text="{Binding Path=Nick}" />
          </TextBlock>
        </DataTemplate>
      </ListBox.ItemTemplate>
    </ListBox>
    ```

### 리소스

* 리소스는 애플리케이션에 포함되어 있지만 코드와 분리된 데이터를 의미합니다. (png 이미지, 텍스트 등)
  - Window.Resources로 정의한 리소스 예제는 다음과 같습니다. XAML 안에 정의한 정적 리소스는 StaticResource로 참조할 수 있습니다.
    ```xaml
    <!-- Window1.xaml -->
    <Window ... xmlns:local="clr-namespace:DataBindingDemo" />
      <Window.Resources>
        <local:Nicknames x:Key="names">
          <local:Nickname Name="Don" Nick="Naked" />
          <local:Nickname Name="Martin" Nick="Gudge" />
          <local:Nickname Name="Tim" Nick="Stinky" />
        </local:Nicknames>
      </Window.Resources>
      <DockPanel DataContext="{StaticResource names}">
        <TextBlock DockPanel.Dock="Top" Orientation="Horizontal">
          <TextBlock VerticalAlignment="Center">Name: </TextBlock>
          <TextBox Text="{Binding Path=Name}" />
          <TextBlock VerticalAlignment="Center">Nick: </TextBlock>
          <TextBox Text="{Binding Path=Nick}" />
        </TextBlock>
        ...
      </DockPanel>
    </Window>
    ```
  - CS 코드에서도 다음과 같이 정적 리소스를 불러올 수 있습니다.
    ```xaml
    public partial class Window1 : Window {
        Nicknames names;
    
        public Window1() {
            InitializeComponent();
            this.addButton.Click += addButton_Click;

            // 리소스에서 names 컬렉션 가져오기
            this.names = (Nicknames)this.FindResource("names");

            // 위 코드가 있으므로 여기서 바인딩할 필요가 없음
            //dockPanel.DataContext = this.names;
        }

        void addButton_Click(object sender, RoutedEventArgs e) {
            this.names.Add(new Nickname());
        }
    }
    ```

### 스타일

* 스타일은 여러 요소에 적용할 수 있는 프로퍼티/값의 집합입니다.
  - 요소의 프로퍼티를 직접 설정하는 방법은 다음과 같습니다. (마치 HTML 태그에 프로퍼티를 직접 지정하는 것과 비슷함)
    ```xaml
    <Window ...>
      <DockPanel ...>
        <TextBlock ...>
          <TextBlock VerticalAlignment="Center">Name: </TextBlock>
          <TextBox Text="{Binding Path=Name}" />
          <TextBlock VerticalAlignment="Center">Nick: </TextBlock>
          <TextBox Text="{Binding Path=Nick}" />
        </TextBlock>
        ...
      </DockPanel>
    </Window>
    ```
  - 스타일 리소스를 만들고 그것을 적용하는 방법은 다음과 같습니다. (마치 CSS에 스타일을 지정하고 태그에 스타일 이름을 지정하는 것과 비슷함)
    ```xaml
    <Window ...>
      <Window.Resources>
        ...
        <Style x:Key="myStyle" TargetType="{x:Type TextBlock}">
          <Setter Property="VerticalAlignment" Value="Center" />
          <Setter Property="Margin" Value="2" />
          <Setter Property="FontWeight" Value="Bold" />
          <Setter Property="FontStyle" Value="Italic" />
        </Style>
      </Window.Resources>
      <DockPanel ...>
        <TextBlock ...>
          <TextBlock Style="{StaticResource myStyle}">Name: </TextBlock>
          <TextBox Text="{Binding Path=Name}" />
          <TextBlock Style="{StaticResource myStyle}">Nick: </TextBlock>
          <TextBox Text="{Binding Path=Nick}" />
        </TextBlock>
        ...
      </DockPanel>
    </Window>
    ```

* 컨트롤 템플릿
  - ControlTemplate 프로퍼티를 이용해서 컨트롤의 외형을 바꿀 수도 있습니다.
  - 다음 예시는 버튼의 외형을 Ellipse 모양으로 바꾼 것입니다. (너비 128, 높이 32, 채우기 Yellow, 선 Black)
    ```xaml
    <Button DockPanel.Dock="Bottom" x:Name="addButton" Content="Add">
      <Button.Template>
        <ControlTemplate TargetType="{x:Type Button}">
          <Grid>
            <Ellipse Width="128" Height="32" Fill="Yellow" Stroke="Black" />
            <ContentPresenter
              VerticalAlignment="Center" HorizontalAlignment="Center" />
          </Grid>
        </ControlTemplate>
      </Button.Template>
    </Button>
    ```

* 그래픽
  - 텍스트로 직접 아이콘 등을 그릴 수도 있습니다.
  - 다음 예제는 Ellipse와 Path 요소를 이용해서 스마일 아이콘을 그린 것입니다. 버튼 안에 스마일 아이콘과 Click! 텍스트가 들어가 있습니다. ScaleTransform은 확대 비율을 조정할 수 있습니다.
    ```xaml
    <Button>
      <Button.LayoutTransform>
        <ScaleTransform ScaleX="3" ScaleY="3" />
      </Button.LayoutTransform>
      <StackPanel Orientation="Horizontal">
        <Canvas Width="20" Height="18" VerticalAlignment="Center">
          <Ellipse Canvas.Left="1" Canvas.Top="1" Width="16" Height="16" Fill="Yellow" Stroke="Black" />
          <Ellipse Canvas.Left="4.5" Canvas.Top="5" Width="2.5" Height="3" Fill="Black" />
          <Ellipse Canvas.Left="11" Canvas.Top="5" Width="2.5" Height="3" Fill="Black" />
          <Path Data="M 5,10 A 3,3 0 0 0 13,10" Stroke="Black" />
        </Canvas>
        <TextBlock VerticalAlignment="Center">Click!</TextBlock>
      </StackPanel>
    </Button>
    ```
  - 이외에도 3D 이미지, 고급 문서 (타이포그래피, 스펠체크 등), 인쇄 기능도 제공합니다.








<!-- Programming WPF 2nd edition 참조... 페이지 132/867 -->
