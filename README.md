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
    ```cs
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
    ```cs
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

## 입력

* 입력 이벤트는 다음에 의해 발생할 수 있습니다.: 키보드, 마우스, 잉크

* Routed Event의 종류는 다음과 같습니다.
  - Bubbling 이벤트: 중첩된 요소들이 있을 때 하위 요소에서 이벤트가 발생하면 트리의 상위 요소까지 모든 이벤트가 순서대로 발생합니다. (일반적인 경우)
  - Tunneling 이벤트: Bubbling 이벤트와 반대로 트리 루트 이벤트부터 시작하여 가장 하위 요소까지의 이벤트가 순서대로 발생합니다. (Preview 접두사가 붙어 있음, Tunneling 이벤트 직후에 Bubbling 이벤트가 작동하게 되며 상위 요소를 미리 체크하여 무언가를 처리할 경우 이것을 사용하면 됨)
  - Direct 이벤트: 이벤트가 직접적으로 발생한 요소에서만 이벤트가 발생합니다.

* 이벤트 종류
  - 마우스 입력
    | 이벤트 이름 | 라우팅 | 의미 |
    | --- | --- | --- |
    | GotMouseCapture | Bubble | 요소가 마우스에게 잡혔습니다. |
    | LostMouseCapture | Bubble | 요소가 마우스로부터 놓였습니다. |
    | MouseEnter | Direct | 마우스 포인터가 요소 안으로 들어갔습니다. |
    | MouseLeave | Direct | 마우스 포인터가 요소 밖으로 나갔습니다. |
    | (Preview)MouseLeftButtonDown | Tunnel, Bubble | 포인터가 요소 안에 있을 동안 마우스 왼쪽 버튼이 눌렸습니다. |
    | (Preview)MouseLeftButtonUp | Tunnel, Bubble | 포인터가 요소 안에 있을 동안 마우스 왼쪽 버튼을 놓았습니다. |
    | (Preview)MouseRightButtonDown | Tunnel, Bubble | 포인터가 요소 안에 있을 동안 마우스 오른쪽 버튼이 눌렸습니다. |
    | (Preview)MouseRightButtonUp | Tunnel, Bubble | 포인터가 요소 안에 있을 동안 마우스 오른쪽 버튼을 놓았습니다. |
    | (Preview)MouseDown | Tunnel, Bubble | 포인터가 요소 안에 있을 동안 마우스 버튼이 눌렸습니다. |
    | (Preview)MouseUp | Tunnel, Bubble | 포인터가 요소 안에 있을 동안 마우스 버튼을 놓았습니다. |
    | (Preview)MouseMove | Tunnel, Bubble | 포인터가 요소 안에 있을 동안 마우스 포인터가 이동했습니다. |
    | (Preview)MouseWheel | Tunnel, Bubble | 포인터가 요소 안에 있을 동안 마우스 휠이 이동했습니다. |
    | QueryCursor | Bubble | 포인터가 요소 안에 있을 동안 마우스 커서 모양이 결정되었습니다. |
    * WPF에서는 마우스 입력이 바운딩 박스 기준으로 발생하지 않습니다.
      - 예를 들어 도넛 모양의 요소와 그 아래에 사각형 모양의 요소가 있다면 클릭 이벤트가 투명한 영역을 스킵할 수 있다는 것입니다.
        * Fill = Transparent 또는 Opacity = 0인 경우
      - 반대로 눈에는 보여도 클릭되지 않게 할 수도 있습니다.
        * IsHitTestVisible = false로 하면 마우스 입력을 받지 않게 됩니다. (3D 컨텐츠에서 마우스 입력 이벤트가 불필요할 경우 이것을 적용하면 성능이 향상될 수 있음)
    * Mouse 클래스
      - GetPosition 메서드: 마우스 위치를 알려줌
      - Capture 메서드: 특정 요소가 마우스에게 잡히면 요소 위에서 포인터가 벗어나도 모든 마우스 입력 이벤트가 해당 요소에게 전달됨 (가령 Drag 중일 때)
      - Captured 프로퍼티: 어떤 요소가 잡혀 있는지 알 수 있음

  - 키보드 입력: Focus된 요소에 전달될 수 있음
    | 이벤트 이름 | 라우팅 | 의미 |
    | --- | --- | --- |
    | (Preview)GotKeyboardFocus | Tunnel, Bubble | 요소가 키보드 포커스를 받았습니다. |
    | (Preview)LostKeyboardFocus | Tunnel, Bubble | 요소가 키보드 포커스를 잃었습니다. |
    | GotFocus | Bubble | 요소가 논리적 포커스를 받았습니다. |
    | LostFocus | Bubble | 요소가 논리적 포커스를 잃었습니다. |
    | (Preview)KeyDown | Tunnel, Bubble | 키가 눌렸습니다. |
    | (Preview)KeyUp | Tunnel, Bubble | 키가 놓였습니다. |
    | (Preview)TextInput | Tunnel, Bubble | 요소가 텍스트 입력을 받았습니다. |
    * 논리적 포커스: 애플리케이션이 포커스를 잃어도 논리적 포커스는 유지되며, 애플리케이션이 포커스를 받게 되면 마지막으로 키보드 포커스를 가지고 있었던 요소가 포커스를 받게 됩니다.
    * Keyboard 클래스
      - Modifiers 프로퍼티: Alt, Shift, Ctrl 등의 조합 키가 눌린 것을 알 수 있음
      - IsKeyDown, IsKeyUp 메서드: 개별 키의 상태를 알 수 있음
      - FocusedElement 프로퍼티: 어떤 요소가 키보드 포커스를 가지고 있는지 알 수 있음
      - Focus 메서드: 특정 요소에 포커스를 설정할 수 있음

  - 잉크 입력
    | 이벤트 이름 | 라우팅 | 의미 |
    | --- | --- | --- |
    | GotStylusCapture | Bubble | 요소가 스타일러스에게 잡혔습니다. |
    | LostStylusCapture | Bubble | 요소가 스타일러스로부터 놓였습니다. |
    | (Preview)StylusButtonDown | Tunnel, Bubble | 요소 위에 있는 동안 스타일러스 버튼이 눌렸습니다. |
    | (Preview)StylusButtonUp | Tunnel, Bubble | 요소 위에 있는 동안 스타일러스 버튼이 놓였습니다. |
    | (Preview)StylusDown | Tunnel, Bubble | 요소 위에 있는 동안 스타일러스가 스크린을 터치했습니다. |
    | (Preview)StylusUp | Tunnel, Bubble | 요소 위에 있는 동안 스타일러스가 스크린을 떠났습니다. |
    | StylusEnter | Direct | 스타일러스가 요소 안으로 이동했습니다. |
    | StylusLeave | Direct | 스타일러스가 요소를 떠났습니다. |
    | (Preview)StylusInRange | Tunnel, Bubble | 스타일러스가 감지될 정도로 스크린에 근접했습니다. |
    | (Preview)StylusOutOfRange | Tunnel, Bubble | 스타일러스가 감지 범위를 벗어났습니다. |
    | (Preview)StylusMove | Tunnel, Bubble | 요소 위에 있는 동안 스타일러스가 이동했습니다. |
    | (Preview)StylusInAirMove | Tunnel, Bubble | 요소 위에 있는 동안 스타일러스가 이동했지만 스크린에 접촉한 상태는 아닙니다. |
    | (Preview)StylusSystemGesture | Tunnel, Bubble | 스타일러스가 제스처를 수행했습니다. |
    | (Preview)TextInput | Tunnel, Bubble | 요소가 텍스트 입력을 받았습니다. |
    * Stylus 클래스
      - Capture 메서드: Mouse.Capture와 같은 역할을 합니다.
      - Captured 프로퍼티: 어떤 요소가 잡혀 있는지 알 수 있음
    * InkCanvas 클래스
      - 자유로운 형태의 잉크 입력을 받습니다. (가령 필기 중인 상태)

### 커맨드

* 커맨드: 여러 입력(마우스, 키보드, 잉크)들의 조합을 통해 사용자가 애플리케이션에게 명령을 요청하기 위한 동작을 의미합니다.
  - 다음은 XAML에서 정의한 커맨드의 예시입니다. (TextBox의 경우 Cut, Copy, Paste 관련 명령이 내장되어 있으므로 별도의 코드나 이벤트 핸들러가 없어도 됨)
    ```xaml
    <DockPanel>
      <Menu DockPanel.Dock="Top">
        <MenuItem Header="_Edit">
          <MenuItem Header="Cu_t" Command="ApplicationCommands.Cut" />
          <MenuItem Header="_Copy" Command="ApplicationCommands.Copy" />
          <MenuItem Header="_Paste" Command="ApplicationCommands.Paste" />
        </MenuItem>
      </Menu>
      <ToolBarTray DockPanel.Dock="Top">
        <ToolBar>
          <Button Command="Cut" Content="Cut" />
          <Button Command="Copy" Content="Copy" />
          <Button Command="Paste" Content="Paste" />
        </ToolBar>
      </ToolBarTray>
      <TextBox />
    </DockPanel>
    ```
  - 커맨드 시스템의 핵심 개념
    * Command 오브젝트: 특정 커맨드를 식별하는 오브젝트를 의미합니다.
    * 입력 바인딩: 특정 입력과 커맨드 간의 연관성을 의미합니다. (예: Ctrl-C는 Copy)
    * Command 소스: 커맨드가 발생한 오브젝트를 의미합니다.
    * Command 타겟: 커맨드 실행을 요청 받게 되는 UI 요소입니다. (보통 커맨드 발생시 키보드 포커스를 가진 컨트롤)
    * Command 바인딩: 특정 커맨드를 어떻게 처리해야 할지 알고 있는 특정 UI 요소를 선언하는 것입니다.
  - 다음은 커맨드 처리 예시입니다.
    * 이것은 표준 ApplicationCommands.Properties 커맨드 오브젝트를 사용함 (속성 패널 열기)
    * Button 객체를 클릭하거나 키보드의 Alt + Enter를 누르면 커맨드가 실행됨
    * 여기서 커맨드 소스는 버튼, 입력 바인딩입니다.
    ```xaml
    <!-- XAML -->
    <Window ...>
      <Grid>
        <Button Command="ApplicationCommands.Properties" Content="_Properties"/>
      </Grid>
    </Window>
    ```
    ```cs
    // 코드 비하인드
    public partial class Window1 : Window {
        public Window1( ) {
            InitializeComponent( );
            InputBinding ib = new InputBinding(ApplicationCommands.Properties, new KeyGesture(Key.Enter, ModifierKeys.Alt));
            this.InputBindings.Add(ib);
            CommandBinding cb = new CommandBinding(ApplicationCommands.Properties);
            cb.Executed += new ExecutedRoutedEventHandler(cb_Executed);
            this.CommandBindings.Add(cb);
        }
        void cb_Executed(object sender, ExecutedRoutedEventArgs e) {
            MessageBox.Show("Properties");
        }
    }
    ```

#### Command 오브젝트

* Command 오브젝트: 특정 커맨드를 식별합니다.
  - 이 자체는 커맨드를 처리하는 방법을 모릅니다. (커맨드 바인딩이 필수)
  - 예: ApplicationCommands.Properties
  - 내장된 컨트롤들은 대부분 표준 커맨드 오브젝트가 있지만, 경우에 따라서는 직접 구현해야 할 수도 있습니다.
    | 표준 커맨드 클래스 | 커맨드 타입 |
    | --- | --- |
    | ApplicationCommands | 대부분의 모든 애플리케이션에 있는 커맨드. Undo, Redo, 도큐먼트 레벨 동작 (open, close, print) 등 |
    | ComponentCommands | 정보 안에서의 이동 동작. 스크롤 Up/Down, 끝으로 이동, 텍스트 선택 |
    | EditingCommands | Bold, Italic, Center, Justify와 같은 텍스트 편집 커맨드. |
    | MediaCommands | 미디어 재생 동작. Play, Pause, 볼륨 제어, 트랙 선택. |
    | NavigationCommands | 브라우저 관련 네비게이션 커맨드. Back, Forward, Refresh. |
  - 커스텀 커맨드 정의하기: Add to Basket 커맨드이며 단축키는 Ctrl + B 또는 Shift + B
    ```cs
    ...
    using System.Windows.Input;
    namespace MyNamespace {
        public class MyAppCommands {
            public static RoutedUICommand AddToBasketCommand;
            static MyAppCommands() {
                InputGestureCollection addToBasketInputs = new InputGestureCollection();
                addToBasketInputs.Add(new KeyGesture(Key.B, ModifierKeys.Control|ModifierKeys.Shift));
                AddToBasketCommand = new RoutedUICommand("Add to Basket", "AddToBasket", typeof(MyAppCommands), addToBasketInputs);
            }
        }
    }
    ```
  - XAML에서 커스텀 커맨드 사용하기: 위에서 정의한 AddToBasket 커맨드를 정의한 후에 사용할 수 있습니다.
    ```xaml
    <Window xmlns:m="clr-namespace:MyNamespace;assembly=MyLib" ...>
        ...
        <Button Command="m:MyAppCommands.AddToBasketCommand">Add to Basket</Button>
        ...
    ```

#### 입력 바인딩

* 자주 호출하는 커맨드의 경우, 입력 바인딩(예: 키보드 단축키)을 제공하는 것이 좋습니다.
* 현재 제공하는 입력 제스처는 2가지가 있습니다.
  - MouseGesture: Shift + 왼쪽 클릭과 같은 특정 마우스 입력입니다.
  - KeyGesture: Alt + Enter와 같은 특정 키보드 입력입니다.

#### Command 소스

* Command 소스는 명령을 호출하는 데 사용된 오브젝트입니다. (UI 요소, 입력 제스처 등)
  - ICommandSource 인터페이스를 구현해야 함
    ```cs
    public interface ICommandSource {
        ICommand Command { get; }
        object CommandParameter { get; }
        IInputElement CommandTarget { get; }
    }
    ```
  - CommandParameter 프로퍼티를 사용하면 다음과 같이 커맨드 호출시 정보 전달이 가능합니다.
    ```xaml
    <!-- 이벤트 발생 -->
    <MenuItem Command="m:MyAppCommands.AddToBasketCommand"
              CommandParameter="productId4823"
              Header="Add to basket" />
    ```

    ```cs
    // 이벤트 수신
    void AddToBasketHandler(object sender, ExecutedRoutedEventArgs e) {
        string productId = (string) e.Parameter;
        ...
    }
    ```

#### Command 타겟

* Command 타겟을 설정하지 않으면 기본적으로 입력 포커스를 가진 요소가 됩니다.
  - 만약 사용자가 애플리케이션의 다른 곳에 포커스가 있는 상태에서 이벤트의 효과가 의도한 대로 작동되게 하려면 Command 타겟을 명시해야 합니다.

#### Command 바인딩

* 커스텀 커맨드 처리 로직을 구현하려면 CommandBinding 클래스를 구현하면 됩니다.
  - 다음은 ApplicationCommands.New를 재정의하는 코드입니다.
    ```cs
    public partial class Window1 : Window {
        public Window1( ) {
            InitializeComponent( );
            CommandBinding cmdBindingNew = new CommandBinding(ApplicationCommands.New);
            cmdBindingNew.Executed += NewCommandHandler;
            CommandBindings.Add(cmdBindingNew);
        }

        void NewCommandHandler(object sender, ExecutedRoutedEventArgs e) {
            if (unsavedChanges) {
                MessageBoxResult result = MessageBox.Show(this, "Save changes to existing document?", "New", MessageBoxButton.YesNoCancel);
                if (result == MessageBoxResult.Cancel) {
                    return;
                }
                if (result == MessageBoxResult.Yes) {
                    SaveChanges( );
                }
            }
            // Reset text box contents
            inputBox.Clear( );
        }
        ...
    }
    ```

* 커맨드 활성화/비활성화
  - 다음은 Redo 커맨드를 추가하는 코드입니다.
    ```cs
    public Window1() {
        InitializeComponent();
        CommandBinding redoCommandBinding = new CommandBinding(ApplicationCommands.Redo);
        redoCommandBinding.CanExecute += RedoCommandCanExecute;
        CommandBindings.Add(redoCommandBinding);
    }
    void RedoCommandCanExecute(object sender, CanExecuteRoutedEventArgs e) {
        e.CanExecute = myCustomUndoManager.CanRedo;
    }
    ```

* 포커스 범위
  - 가령 아래와 같이 TextBox가 포커스를 가진 상태에서, 버튼을 누를 때 TextBox에서 이벤트가 발생하게 하려면 포커스 범위를 명시적으로 정의해야 합니다.
  - FocusManager.IsFocusScope="True" 프로퍼티를 넣으면 작동됩니다. TextBox가 논리적 포커스를 가지고 있다면(최근 포커스를 가지고 있었다면), 그것이 Command 타겟이 됩니다.
    ```xaml
    <StackPanel>
      <StackPanel FocusManager.IsFocusScope="True">
        <Button Command="ApplicationCommands.Copy"  Content="Copy" />
        <Button Command="ApplicationCommands.Paste" Content="Paste" />
      </StackPanel>
      <TextBox />
    </StackPanel>
    ```
  - 혹은 다음과 같이 Command 타겟을 명시적으로 지정할 수도 있습니다.
    ```xaml
    <StackPanel>
      <Button Command="Copy" Content="Copy" CommandTarget="{Binding ElementName=targetControl}" />
      <Button Command="Paste" Content="Paste" CommandTarget="{Binding ElementName=targetControl}" />
      <TextBox x:Name="targetControl" />
    </StackPanel>
    ```

## 컨트롤

* 컨트롤이란 특정한 상호작용 동작을 제공하는 사용자 인터페이스 구성요소입니다. (컨트롤은 동작, 외형을 제공함)
  - 커맨드: 동작할 수 있는 행동에 대한 커맨드를 제공함 (TextBox의 경우 잘라내기, 복사, 붙여넣기가 있음)
  - 프로퍼티: 컨트롤의 속성
  - 이벤트: 입력을 받으면 이벤트를 발생시킴

* 컨트롤의 종류는 다음과 같습니다.
  | 컨트롤 | 기능 | 파생 컨트롤
  | --- | --- | --- |
  | Button | 사용자가 클릭할 수 있음 | RadioButton (하나만 선택할 때 사용), CheckBox (체크 여부 입력 가능) |
  | Slider | 값을 조정하는 데 사용함 | - |
  | ScrollBar | 뷰 영역을 스크롤할 때 사용함 | - |
  | ProgressBar | 오래 걸리는 작업을 진행할 때 진행하고 있음을 시각적으로 보여줌 | - |
  | TextBox | 텍스트를 편집, 표시할 수 있음 | PasswordBox (패스워드 입력에 특화됨), RichTextBox (고급 텍스트 포맷팅 기능 제공) |
  | Label | 컨트롤에 대한 캡션을 제공하는 데 사용함 | TextBlock (Label보다 저수준, Label은 AccessText 요소가 있음) |
  | ToolTip | 마우스 등으로 컨트롤 위에 두면 나타나는 떠다니는 라벨 | - |
  | GroupBox | 여러 컨트롤을 포함할 수 있는 컨테이너 | Expander (접혀지면 내용물이 감춰짐) |
  | ListBox | 아이템을 선형적으로 나열해서 보여줌 | ComboBox (펼쳐지고 접히기도 함), ListView (여러 컬럼을 제공함) |
  | TreeView | 아이템을 트리 구조로 보여줌 | - |
  | TabControl | 여러 개의 탭페이지로 구성되어 있으며 각 탭페이지는 하나의 컨테이너 역할을 함 | - |
  | Menu | 메뉴 바를 표현할 때 사용함 | ContextMenu (Menu와 달리 고정된 것이 아니라 마우스 우클릭할 때 나옴) |
  | ToolBarTray | 여러 개의 툴바 요소를 포함할 수 있으며 각각의 ToolBar는 클릭시 즉각 이벤트를 수행하는 작은 버튼을 포함시킬 수 있음 | - |
  | GridSplitter | 화면 영역 너비를 조정할 수 있는 스플리터 | - |

## 간단한 데이터 바인딩

* 대부분의 애플리케이션의 목적은 사용자에게 데이터를 보여주고, 그 데이터를 편집할 수 있게 해주는 것입니다.
  - 애플리케이션 개발자는 이를 위해 데이터를 가공하여 사용자에게 보여주고, 입력한 데이터를 적절하게 처리해야 합니다.
  - WPF에서 제공하는 데이터 바인딩 엔진을 이용하면 더 적은 코드로 더 많은 기능을 구현할 수 있습니다.
  - Windows Forms의 경우, 이벤트가 발생하여 데이터가 변경되었을 때 표시된 데이터를 수동으로 업데이트하는 코드를 추가해야 했습니다.
  - 데이터 바인딩은 인터페이스 __INotifyPropertyChanged__ 를 구현하면 됩니다. PropertyChangedEventHandler 이벤트 핸들러와 Notify 함수가 있기 때문에 데이터와 UI가 자동으로 연동됩니다.
    ```xaml
    <!-- Window1.xaml -->
    <Window ...>
      <Grid>
        ...
        <TextBlock ...>Name:</TextBlock>
        <TextBox Name="nameTextBox" ... />
        <TextBlock ...>Age:</TextBlock>
        <TextBox Name="ageTextBox" ... />
        <Button Name="birthdayButton" ...>Birthday</Button>
      </Grid>
    </Window>
    ```

    ```cs
    using System.ComponentModel; // INotifyPropertyChanged

    public class Person : INotifyPropertyChanged {
      // INotifyPropertyChanged 멤버를 추가할 것
      public event PropertyChangedEventHandler PropertyChanged;

      // Notify 함수 포함시킬 것
      protected void Notify(string propName) {
        if( this.PropertyChanged != null ) {
          PropertyChanged(this, new PropertyChangedEventArgs(propName));
        }
      }
      string name;
      public string Name {
        get { return this.name; }
        set {
          if( this.name == value ) { return; }
          this.name = value;
          Notify("Name");  // Name 값이 바뀌었음을 알림
        }
      }
      int age;
      public int Age {
        get { return this.age; }
        set {
          if(this.age == value ) { return; }
          this.age = value;
          Notify("Age");  // Age 값이 바뀌었음을 알림
        }
      }
      public Person() {}
      public Person(string name, int age) {
        this.name = name;
        this.age = age;
      }
    }

    public partial class Window1 : Window {
      Person person = new Person("Tom", 11);
    
      public Window1() {
        InitializeComponent();
    
        // Fill initial person fields
        this.nameTextBox.Text = person.Name;
        this.ageTextBox.Text = person.Age.ToString();

        // Tom의 프로퍼티 변화를 살펴봄 (데이터 바인딩을 할 경우 이 코드 추가할 것)
        person.PropertyChanged += person_PropertyChanged;
    
        // Handle the birthday button click event
        this.birthdayButton.Click += birthdayButton_Click;
      }

      // 데이터 바인딩을 할 경우 이 코드 추가할 것
      void person_PropertyChanged(object sender, PropertyChangedEventArgs e) {
        switch(e.PropertyName) {
          case "Name":
              this.nameTextBox.Text = person.Name;
              break;
          case "Age":
              this.ageTextBox.Text = person.Age.ToString();
              break;
        }
      }
    
      void birthdayButton_Click(object sender, RoutedEventArgs e) {
        ++person.Age;
        this.ageTextBox.Text = person.Age.ToString();  // 수동으로 UI 업데이트하려면 이 코드가 추가되어야 함 (데이터 바인딩을 적용하지 않으면 이 코드를 넣을 것)
        MessageBox.Show(string.Format("Happy Birthday, {0}, age {1}!", person.Name, person.Age), "Birthday");  // 데이터 바인딩이 없으면 메시지 박스에는 Age: 12로 나옵니다. 하지만 여전히 Window1의 Age 값은 11로 나옵니다.
      }
    }
    ```

### 데이터 바인딩

* 바인딩
  - 단축 바인딩 구문은 다음과 같습니다.
    ```xaml
    <TextBox Text="{Binding Path=Age}" />
    ```
  - 모든 기능이 포함된 바인딩 구문은 다음과 같습니다.
    ```xaml
    <TextBox ... Foreground="{Binding Path=Age, Mode=OneWay, Source={StaticResource Tom}, Converter={StaticResource ageConverter}}" />
    ```
  - 바인딩 클래스의 프로퍼티는 다음과 같습니다.
    | 프로퍼티 | 의미 |
    | --- | --- |
    | BindsDirectlyToSource | 기본값은 False. 만약 True로 설정하면 공급자로부터 반환된 데이터 대신 DataSourceProvider의 파라미터에 바인딩한다는 것을 의미합니다. |
    | Converter | 데이터 소스로부터 값을 양방향으로 변환하는데 사용하는 IValueConverter의 구현체입니다. |
    | ConverterCulture | 변환 중에 사용할 문화권을 나타내기 위해 IValueConverter 메서드로 전달되는 선택적 파라미터입니다. |
    | ConverterParameter | 변환 중에 IValueConverter 메서드로 전달되는 선택적 애플리케이션별 파라미터입니다. |
    | ElementName | 데이터의 소스가 타겟과 마찬가지로 UI일 때 사용합니다. |
    | FallbackValue | 데이터 소스에서 값을 가져오는데 실패했거나, 멀티파트 경로 중 일부가 null이거나, 바인딩이 비동기식이고 아직 값을 가져오지 못한 경우 사용할 값입니다. |
    | IsAsync | 기본값은 False. 만약 True로 설정하면 소스에서 데이터를 비동기식으로 가져와서 get/set합니다. 데이터를 가져오는 동안에는 FallbackValue를 사용합니다. |
    | Mode | BindingMode 값 중 하나입니다: TwoWay, OneWay, OneTime, OneWayToSource, Default |
    | NotifyOnSourceUpdated | 기본값은 False. SourceUpdated 이벤트 발생 여부를 의미합니다. |
    | NotifyOnTargetUpdated | 기본값은 False. TargetUpdated 이벤트 발생 여부를 의미합니다. |
    | NotifyOnValidationError | 기본값은 False. Validation.Error 부착 이벤트 발생 여부를 의미합니다. |
    | Path | 데이터 소스 오브젝트의 데이터에 대한 경로입니다. XML 데이터의 XPath 프로퍼티를 사용합니다. |
    | RelativeSource | 타겟에 대한 상대적인 데이터 소스를 탐색하기 위해 사용합니다. |
    | Source | 기본 데이터 문맥 (data context) 대신 사용할 데이터 소스에 대한 참조입니다. |
    | UpdateSourceExceptionFilter | 데이터 소스를 업데이트하는 동안 발생하는 오류를 처리하기 위한 선택적인 델리게이트입니다. ErrorValidationRule이 동반된 경우에만 유효합니다. |
    | UpdateSourceTrigger | UI 타겟으로부터 데이터 소스가 언제 업데이트될지를 결정합니다. 다음 값 중 하나이어야 합니다: PropertyChanged, LostFocus, Explicit, Default |
    | ValidationRules | 0개 이상의 ValidationRule 클래스의 파생 클래스입니다. |
    | XPath | XML 데이터 소스 오브젝트에 있는 데이터의 XPath입니다. 비-XML 데이터의 경우 Path 프로퍼티를 사용하십시오. |

* 암묵적 데이터 소스
  - 데이터 문맥 (data context): 명시적인 바인딩이 없을 때 데이터 소스를 찾는 장소를 의미합니다. WPF의 경우 모든 FrameworkElement와 FrameworkContentElement가 DataContext 프로퍼티를 가지고 있습니다.
  - DataContext 프로퍼티는 Object 타입이므로 어떤 것이든 넣을 수 있습니다. (string, Person, List<Person> 등)
  - 바인딩 객체는 바인딩 소스로 사용할 객체를 찾을 때, 자신이 정의된 위치로부터 트리를 논리적으로 거슬러 올라가면서 설정된 DataContext 프로퍼티를 찾습니다.
    ```cs
    // Window1.xaml.cs
    using System;
    using System.Windows;
    using System.Windows.Controls;
    namespace WithBinding {
        public partial class Window1 : Window {
            Person person = new Person("Tom", 11);
    
            public Window1( ) {
    
                InitializeComponent( );
    
                // grid에게 데이터 문맥을 알려줌
                grid.DataContext = person;
                this.birthdayButton.Click += birthdayButton_Click;
            }
    
            void birthdayButton_Click(object sender, RoutedEventArgs e) {
                // 데이터 바인딩으로 인해 person과 TextBox 간에 동기화가 이루어짐
                ++person.Age;
                MessageBox.Show(string.Format("Happy Birthday, {0}, age {1}!", person.Name, person.Age), "Birthday");
            }
        }
    }
    ```

* 데이터 아일랜드
  - XAML에서 커스텀 타입 인스턴스 생성하기
    ```xaml
    <Window ... xmlns:local="clr-namespace:WithBinding">
      <Window.Resources>
        <local:Person x:Key="Tom" Name="Tom" Age="11" />
      </Window.Resources>
      <Grid>...</Grid>
    </Window
    ```
  - 위 예제에서는 clr-namespace 구문을 사용하여 Window.Resources 안에 데이터 아일랜드를 만들었습니다. 그래서 다음과 같이 코드 비하인드 방식이 아닌 XAML에서 DataContext를 선언적으로 설정할 수 있습니다.
    ```xaml
    <!-- Window1.xaml -->
    <Window ... xmlns:local="clr-namespace:WithBinding">
      <Window.Resources>
        <local:Person x:Key="Tom" Name="Tom" Age="11" />
      </Window.Resources>
      <Grid DataContext="{StaticResource Tom}">
        ...
        <TextBlock ...>Name:</TextBlock>
        <TextBox ... Text="{Binding Path=Name}" />
        <TextBlock ...>Age:</TextBlock>
        <TextBox ... Text="{Binding Path=Age}" />
        <Button ... Name="birthdayButton">Birthday</Button>
      </Grid>
    </Window>
    ```
  - 다음과 같이 버튼 클릭 이벤트가 발생했을 때 멤버 변수 대신 리소스에 정의된 데이터를 사용하도록 업데이트해야 합니다.
    ```cs
    public partial class Window1 : Window {
        ...
        void birthdayButton_Click(object sender, RoutedEventArgs e) {
            // Window의 리소스에서 Person 가져오기
            Person person = (Person)this.FindResource("Tom");
            ++person.Age;
            MessageBox.Show(...);
        }
    }
    ```

* 명시적 데이터 소스
  - 일단 이름이 있는 데이터 소스를 확보하면 트리의 어딘가에 설정된 DataContext 프로퍼티에 암시적으로 바인딩하는 것에 의존하는 대신 XAML에서 바인딩 소스를 명시적으로 지정할 수 있습니다. (Source 프로퍼티)
  - 다음은 2개의 TextBox가 2개의 Person 오브젝트에 각각 바인딩을 한 예제입니다.
    ```xaml
    <!-- Window1.xaml -->
    <Window ...>
      <Window.Resources>
        <local:Person x:Key="Tom" ... />
        <local:Person x:Key="John" ... />
      </Window.Resources>
      <Grid>
        ...
        <TextBox Name="tomTextBox" Text="{Binding Path=Name, Source={StaticResource Tom}}" />
        <TextBox Name="johnTextBox" Text="{Binding Path=Name, Source={StaticResource John}}" />
        ...
      </Grid>
    </Window>
    ```

* 다른 컨트롤에게 바인딩하기
  - 컨트롤끼리도 서로 바인딩하는 것이 가능합니다.
    ```xaml
    <TextBox Name="ageTextBox" Foreground="Red" ... />
    <!-- 버튼과 age TextBox 전경 브러시가 동기화됨 -->

    <Button ...
      Foreground="{Binding Path=Foreground, ElementName=ageTextBox}">Birthday</Button>
    ```

* 값 변환
  - 바인딩한 값의 타입이 호환되지 않는 경우 값 변환이 필요합니다. 아래의 경우 Foreground(Brush)와 Age(Int32)가 바인딩 되어 있습니다.
    ```xaml
    <!-- Window1.xaml -->
     <Window ...>
      <Grid>
        ...
        <TextBox
          Text="{Binding Path=Age}"
          Foreground="{Binding Path=Age, ...}"
          ...
        />
        ...
      </Grid>
    </Window>
    ```
  - 값 변환기는 IValueConverter 인터페이스 구현이며 __Convert__, __ConvertBack__ 메서드를 포함합니다.
    ```cs
    [ValueConversion(/*sourceType*/ typeof(int), /*targetType*/ typeof(Brush))]
    public class AgeToForegroundConverter : IValueConverter {
      // 변환: Source(Age : Int32) -> Target UI(Foreground : Brush)
      public object Convert(object value, Type targetType, ...) {
        if( targetType != typeof(Brush) ) { return null; }
        int age = int.Parse(value.ToString());
        return (age > 25 ? Brushes.Red : Brushes.Black);
      }

      // 역변환: Target UI(Foreground : Brush) -> Source(Age : Int32)
      public object ConvertBack(object value, Type targetType, ...) {
        // 여기서는 호출되어서는 안 됨
        throw new NotImplementedException();
      }
    }
    ```
  - 이제 다음과 같이 값 변환기를 붙일 수 있습니다.
    ```xaml
    <!-- Window1.xaml -->
    <Window ... xmlns:local="clr-namespace:WithBinding">
      <Window.Resources>
        <local:Person x:Key="Tom" ... />
        <local:AgeToForegroundConverter x:Key="ageConverter" />
      </Window.Resources>
      <Grid DataContext="{StaticResource Tom}">
        ...
        <TextBox
          Text="{Binding Path=Age}"
          Foreground="{Binding Path=Age,
          Converter={StaticResource ageConverter}}"
          ... />
        ...
      <Button ...
        Foreground="{Binding Path=Foreground, ElementName=ageTextBox}">Birthday</Button>
      </Grid>
    </Window>
    ```
  - 위에서 이상한 점을 느끼지 못하셨나요? TextBox.Text가 Int32 타입인 Age와 바인딩 되었는데 자동으로 타입이 변환되고 있습니다. 이는 Binding 클래스가 TypeConverter를 지원하기 때문에 가능한 것입니다.

* 편집가능한 값 변환
  - 다음은 Int32와 10진수 표현의 String 간의 변환하는 변환기입니다. (단, 16진수로 해석될 수 없는 문자열을 입력하면 예외가 발생하기 때문에 유효성 검사가 필요합니다)
    ```cs
    public class Base16Converter : IValueConverter {
      public object Convert(object value, Type targetType, ...) {
        // 10진수 : Int32 -> 16진수 : String
        return ((int)value).ToString("x");
      }
      public object ConvertBack(object value, Type targetType, ...) {
        // 16진수 : String -> 10진수 : Int32
        return int.Parse((string)value, System.Globalization.NumberStyles.HexNumber);
      }
    }
    ```
    ```xaml
    <TextBox ...
      Text="{Binding
              Path=Age,
              Converter={StaticResource base16Converter}}" />
    ```

* 유효성 검사 (Validation)
  - 다음은 ExceptionValidationRule이라는 내장 유효성 검사 규칙을 적용하여 Age-to-Foreground 값 변환기가 지원하는 범위를 벗어나는 데이터를 사용자가 입력하는 것에 대해 어느 정도 보호합니다. (만약 Age 값이 유효하지 않으면 하이라이트됨)
    ```xaml
    <Window ... xmlns:local="clr-namespace:WithBinding">
      <Window.Resources>
        ...
        <local:AgeToForegroundConverter x:Key="ageConverter" />
      </Window.Resources>
    ...
    <TextBox ...
      Foreground="
        {Binding
          Path=Age,
          Converter={StaticResource ageConverter}}">

      <TextBox.Text>
        <Binding Path="Age">
          <Binding.ValidationRules>
            <ExceptionValidationRule />
          </Binding.ValidationRules>
        </Binding>
      </TextBox.Text>
    </TextBox>
    ```
  - 다음은 유효성 검사 결과가 유효하지 않을 경우 오류 메시지를 표시합니다. Binding.NotifyOnValidationError = "True"이어야 합니다.
    ```cs
    // Window1.cs
    ...
    public Window1() {
      InitializeComponent();
      this.birthdayButton.Click += birthdayButton_Click;
      // age TextBox에 Validation Error 이벤트가 발생하는지 여부를 리스닝
      Validation.AddErrorHandler(this.ageTextBox, ageTextBox_ValidationError);
    }
    
    void ageTextBox_ValidationError(object sender, ValidationErrorEventArgs e) {
      // ExceptionValidationRule에 의해 발생한 예외 문자열을 보여줌
      MessageBox.Show((string)e.Error.ErrorContent, "Validation Error");
    }
    ...
    ```
    ```xaml
    <!-- Window1.xaml -->
    ...
    <TextBox Name="ageTextBox" ...>
      <TextBox.Text>
        <Binding Path="Age" NotifyOnValidationError="True">
          <Binding.ValidationRules>
            <ExceptionValidationRule />
          </Binding.ValidationRules>
        </Binding>
      </TextBox.Text>
    </TextBox>
    ...
    ```

* 커스텀 유효성 검사 규칙
  - ValidationRule 클래스를 상속해서 Validate 메서드를 재정의(override)하면 커스텀 유효성 검사 규칙을 만들 수 있습니다.
    ```cs
    public class NumberRangeRule : ValidationRule {
      int min;
      public int Min {
        get { return min; }
        set { min = value; }
      }
      int max;
      public int Max {
        get { return max; }
        set { max = value; }
      }

      public override ValidationResult Validate(
        object value, System.Globalization.CultureInfo cultureInfo) {
        int number;
        // int로 파싱 실패시
        if( !int.TryParse((string)value, out number) ) {
          return new ValidationResult(false, "Invalid number format");
        }
        // 값 범위 초과시
        if( number < min || number > max ) {
          return new ValidationResult(false, string.Format("Number out of range ({0}-{1})", min, max));
        }
        //return new ValidationResult(true, null); // valid result
        return ValidationResult.ValidResult; // static valid result to save on garbage
      }
    }
    ```
    ```xaml
    <TextBox ...
      Foreground="
        {Binding
          Path=Age,
          Converter={StaticResource ageConverter}}">
      <TextBox.Text>
        <Binding Path="Age">
          <Binding.ValidationRules>
            <local:NumberRangeRule Min="0" Max="128" />
          </Binding.ValidationRules>
        </Binding>
      </TextBox.Text>
    </TextBox>
    ```
  - 만약 메시지 박스 대신 툴팁을 넣고 싶다면 다음과 같이 처리할 수도 있습니다. (다만 툴팁에서 오류 메시지를 지울 수 있게 하는 ValidationSuccess 이벤트는 없습니다)
    ```cs
    void ageTextBox_ValidationError(object sender, ValidationErrorEventArgs e) {
      // NumberRangeRule.Validate에 생성된 문자열을 보여줌
      ageTextBox.ToolTip = (string)e.Error.ErrorContent;
    }
    ```

* 바인딩 Path 구문
  - 바인딩의 Path에 할당하는 값의 형태는 다음과 같이 다양하게 존재합니다.
    | 구문 | 의미 |
    | --- | --- |
    | Path=Property | 현재 오브젝트의 프로퍼티에 바인딩합니다. 프로퍼티는 CLR 프로퍼티이거나 의존성 프로퍼티, 부착된 프로퍼티일 수 있습니다. (예. Path=Age) |
    | Path=(OwnerType.AttachedProperty) | 부착된 의존성 프로퍼티에 바인딩합니다. (예. Path=(Validation.HasError)) |
    | Path=Property.SubProperty | 현재 오브젝트의 서브-프로퍼티에 바인딩합니다. (예. Path=Name.Length) |
    | Path=Property[n] | 인덱서에 바인딩합니다. (예. Path=Names[0]) |
    | Path=Property/Property | Master/Detail 바인딩입니다. (예. Path=Customers/Orders) |
    | Path=(OwnerType.AttachedProperty)[n].SubProperty | 위 구문들의 조합입니다. (예. Path=(Validation.Errors)[0].ErrorContent) |
  - 다음은 툴팁 프로퍼티에 유효성 검사 오류 메시지를 바인딩하는 예제입니다. 이렇게 하면 오류가 해소될 때 툴팁도 비어 있게 됩니다.
    ```xaml
    <TextBox
      Name="ageTextBox" ...
      ToolTip="{Binding
                 ElementName=ageTextBox,
                 Path=(Validation.Errors)[0].ErrorContent}">
      <TextBox.Text>
        <Binding Path="Age">
          <!-- NotifyOnValidationError="true"를 넣을 필요 없음 -->
          <Binding.ValidationRules>
            <local:NumberRangeRule Min="0" Max="128" />
          </Binding.ValidationRules>
        </Binding>
      </TextBox.Text>
    </TextBox>
    ```

* 상대적 소스
  - Source를 지정할 때 자신을 기준으로 요소를 지정할 때 RelativeSource 프로퍼티를 이용할 수 있습니다. (Self, FindAncestor, Previous, TemplatedParent 등이 있음)
    ```xaml
    <TextBox ...
      ToolTip="{Binding RelativeSource={RelativeSource Self},
                        Path=(Validation.Errors)[0].ErrorContent}">
    ```

* 업데이트 소스 트리거
  - 위의 경우 TextBox가 포커스를 잃을 때 데이터 업데이트가 발생합니다.
  - 만약 컨트롤 상태가 변경되는 즉시 유효성 검사가 일어나게 하려면 Binding 오브젝트의 UpdateSourceTrigger 프로퍼티를 변경하면 됩니다.
    ```cs
    namespace System.Windows.Data {
      public enum UpdateSourceTrigger {
        Default = 0, // 타켓 컨트롤을 기반으로 "자연스럽게" 업데이트함 (예: TextBox의 경우 LostFocus)
        PropertyChanged = 1, // 즉시 소스를 업데이트함
        LostFocus = 2, // 포커스가 변경될 때 소스를 업데이트함
        Explicit = 3, // 반드시 BindingExpression.UpdateSource()를 호출해야 함
      }
    }
    ```
  - 다음과 같이 업데이트 소스 트리거를 변경할 수 있습니다. (LostFocus를 PropertyChanged로 변경함)
    ```xaml
    <TextBox ...>
      <TextBox.Text>
        <Binding Path="Age" UpdateSourceTrigger="PropertyChanged">
          ...
        </Binding>
      </TextBox.Text>
    </TextBox>
    ```

## List 데이터에 바인딩하기

### List 데이터에 바인딩하기

* 바인딩에 있던 예제를 활용해서 설명을 이어가겠습니다.
  ```cs
  using System.Collections.Generic; // List<T>
  ...
  namespace PersonBinding {
    public class Person : INotifyPropertyChanged {
      // INotifyPropertyChanged 멤버
      public event PropertyChangedEventHandler PropertyChanged;
      protected void Notify(string propName) {
        if( this.PropertyChanged != null ) {
          PropertyChanged(this, new PropertyChangedEventArgs(propName));
        }
      }
      string name;
      public string Name {
        get { return this.name; }
        set {
          if( this.name == value ) { return; }
          this.name = value;
          Notify("Name");
        }
      }
      int age;
      public int Age {
        get { return this.age; }
        set {
          if(this.age == value ) { return; }
          this.age = value;
          Notify("Age");
        }
      }
      public Person( ) {}
      public Person(string name, int age) {
        this.name = name;
        this.age = age;
      }
    }
    // 새로 추가된 부분입니다: Person 오브젝트 리스트를 만들 수 있도록 제네릭 타입에 대한 별명을 생성합니다.
    class People : List<Person> {}
    ...
  }
  ```
  ```xaml
  <!-- Window1.xaml -->
  <Window ... xmlns:local="clr-namespace:ListBinding">
    <Window.Resources>
      <local:People x:Key="Family">
        <local:Person Name="Tom" Age="11" />
        <local:Person Name="John" Age="12" />
        <local:Person Name="Melissa" Age="38" />
      </local:People>
      <local:AgeToForegroundConverter x:Key="ageConverter" />
    </Window.Resources>
    <Grid DataContext="{StaticResource Family}">
      ...
      <TextBlock ...>Name:</TextBlock>
      <TextBox Text="{Binding Path=Name}" ... />
      <TextBox
        Text="{Binding Path=Age}" Foreground="{Binding Path=Age, Converter=...}" ... />
      <Button ...>Birthday</Button>
    </Grid>
  </Window>
  ```

* 현재 아이템 가져오기
  - XAML에서 선언된 커스텀 오브젝트를 가져오는 방법은 다음과 같습니다.
    ```cs
    public partial class Window1 : Window {
      ...
      void birthdayButton_Click(object sender, RoutedEventArgs e) {
        Person person = (Person)this.FindResource("Tom"));
        ++person.Age;
        MessageBox.Show(...);
      }
    }
    ```
  - 컬렉션 뷰에 대한 예제입니다. 컬렉션 뷰는 컬렉션에 대한 정렬, 필터링, 그룹화 등의 서비스를 제공하며 여기서는 현재 아이템을 가져오는 방법을 보여줍니다.
    ```cs
    public partial class Window1 : Window {
      ...
      void birthdayButton_Click(object sender, RoutedEventArgs e) {
        // 컬렉션 뷰에서 현재 person 가져옴
        People people = (People)this.FindResource("Family");
        ICollectionView view = CollectionViewSource.GetDefaultView(people);
        Person person = (Person)view.CurrentItem;
        ++person.Age;
        MessageBox.Show(...);
      }
    }
    ```
  - 다음은 현재 아이템을 기준으로 리스트 내 다른 아이템으로 탐색할 수 있는 이벤트 함수를 보여줍니다.
    ```cs
    public partial class Window1 : Window {
      ...
      ICollectionView GetFamilyView() {
        People people = (People)this.FindResource("Family");
        return CollectionViewSource.GetDefaultView(people);
      }
    
      void birthdayButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        Person person = (Person)view.CurrentItem;
        ++person.Age;
        MessageBox.Show(...);
      }
    
      void backButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        view.MoveCurrentToPrevious();
        if( view.IsCurrentBeforeFirst ) {
          view.MoveCurrentToFirst();  // 처음으로 이동
        }
      }

      void forwardButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        view.MoveCurrentToNext();
        if( view.IsCurrentAfterLast ) {
          view.MoveCurrentToLast();  // 마지막으로 이동
        }
      }
    }
    ```

* List 데이터 타겟
  - 다음은 ListBox 컨트롤을 이용하여 List 데이터를 한꺼번에 보여주는 예제입니다. (IsSynchronizedWithCurrentItem="True": 현재 아이템을 선택하면 현재 아이템에 대한 선택이 활성화됨, 아직은 ListBox 컨트롤에 Name과 Age 속성이 제대로 나타나지 않습니다)
    ```xaml
    <!-- Window1.xaml -->
    <Window ... xmlns:local="clr-namespace:ListBinding2">
      <Window.Resources>
        <local:People x:Key="Family">...</local:People>
        ...
      </Window.Resources>
      <Grid DataContext="{StaticResource Family}">
        ...
        <ListBox
          ItemsSource="{Binding}"
          IsSynchronizedWithCurrentItem="True" ... />
        <TextBlock ...>Name:</TextBlock>
        <TextBox Text="{Binding Path=Name}" ... />
        ...
    </Window>    
    ```

* 디스플레이 멤버, 값 멤버, 바인딩 탐색
  - 만약 ItemsControl 파생 컨트롤(Menu, ListBox, ListView, ComboBox, TreeView 클래스 등)에서 프로퍼티 중 하나만 보여주고 싶으면 DisplayMemberPath 프로퍼티를 사용하십시오.
    ```xaml
    <ListBox ... ItemsSource="{Binding}" DisplayMemberPath="Name" />
    ```
  - 이외에도 ItemsControl 클래스는 데이터 항목의 선택한 값을 표시하는 경로를 제공합니다. SelectedValue 프로퍼티는 표시되는 것과 (실제) 데이터를 분리하기 위해 사용됩니다.
    ```xaml
     <ListBox ... Name="lb" ItemsSource="{Binding}" DisplayMemberPath="Name" SelectedValuePath="Age" />
    ```
    ```cs
    void lb_MouseDoubleClick(object sender, MouseButtonEventArgs e) {
      int index = lb.SelectedIndex;
      if( index < 0 ) { return; }

      Person item = (Person)lb.SelectedItem;
      int value = (int)lb.SelectedValue; // Age

      // Do something profitable with this data
      ...
    }
    ```

* 데이터 템플릿
  - ItemsControl 컨트롤에서 데이터를 다양한 형태로 보여주고 싶으면 데이터 템플릿을 사용하면 됩니다.
  - 아래 예제는 ListBox의 ItemTemplate 요소 내부에 DateTemplate을 생성하는 방식으로 ListBox 아이템에 대한 데이터 템플릿을 명시적으로 설정했습니다.
    ```xaml
    <ListBox ... ItemsSource="{Binding}">
      <ListBox.ItemTemplate>
        <DataTemplate>
          <TextBlock>
            <TextBlock Text="{Binding Path=Name}" />
            (age: <TextBlock
              Text="{Binding Path=Age}"
              Foreground="{Binding
                            Path=Age,
                            Converter={StaticResource ageConverter}}" />)
          </TextBlock>
        </DataTemplate>
      </ListBox.ItemTemplate>
    </ListBox>
    ```
  - 만약 객체가 어디에 표시되든 상관없이 특정 템플릿을 갖게 하고 싶으면, 다음과 같이 타입 지정 데이터 템플릿을 사용하면 됩니다.
    ```xaml
    <Window.Resources>
      <local:People x:Key="Family">...</local:People>
      ...
      <DataTemplate DataType="{x:Type local:Person}">
        <TextBlock>
          <TextBlock Text="{Binding Path=Name}" />
          (age: <TextBlock Text="{Binding Path=Age}" ... />)
        </TextBlock>
      </DataTemplate>
      ...
     </Window.Resources>
     ...
     <!-- ItemTemplate 설정 불필요함 -->
    <ListBox ItemsSource="{Binding}" ... />
    ```

* 리스트 변경
  - 다음은 Add 버튼에 의한 리스트 변경을 보여줍니다.
    ```cs
    public partial class Window1 : Window {
      ...
      void addButton_Click(object sender, RoutedEventArgs e) {
        People people = (People)this.FindResource("Family");
        people.Add(new Person("Chris", 37));
      }
    }
    ```
  - 이때, ListBox에 새로운 것이 추가되었음을 알아차리기 위해 데이터 바인딩된 객체가 INotifyPropertyChanged 인터페이스를 구현한 것처럼 데이터 바인딩된 리스트는 INotifyCollectionChanged 인터페이스를 구현해야 합니다.
    ```cs
    namespace System.Collections.Specialized {
      public interface INotifyCollectionChanged {
        event NotifyCollectionChangedEventHandler CollectionChanged;
      }
    }
    ```
  - 다음은 INotifyCollectionChanged 인터페이스가 구현된 컬렉션이며 일반적인 List<T>보다 ObservableCollection<T>를 사용하는 것을 권장합니다.
    ```cs
    namespace System.Collections.ObjectModel {
      public class ObservableCollection<T> :
        Collection<T>, INotifyCollectionChanged, INotifyPropertyChanged {
        ...
      }
    }
    ```
    ```cs
    using System.ComponentModel; // INotifyPropertyChanged
    using System.Collections.ObjectModel; // ObservableCollection<T>
    ...
    class Person : INotifyPropertyChanged {...}
    class People : ObservableCollection<Person> {}
    ...
    ```

* 리스트 정렬
  - 다음과 같이 뷰는 데이터가 표시되는 순서를 변경할 수 있습니다.
    ```cs
    public partial class Window1 : Window {
      ...
      ICollectionView GetFamilyView() {
        People people = (People)this.FindResource("Family");
        return CollectionViewSource.GetDefaultView(people);
      }

      // 정렬이 안 되어 있으면, 1차로 Name 기준으로 오름차순 정렬, 2차로 Age 기준으로 내림차순 정렬
      // 정렬이 되어 있을 때, 다시 정렬 버튼을 누르면 원래 상태대로 돌아옴
      void sortButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        if( view.SortDescriptions.Count == 0 ) {
          view.SortDescriptions.Add(new SortDescription("Name", ListSortDirection.Ascending));
          view.SortDescriptions.Add(new SortDescription("Age", ListSortDirection.Descending));
        }
        else {
          view.SortDescriptions.Clear();
        }
      }
    }
    ```
  - 커스텀 정렬
    ```cs
    class PersonSorter : IComparer {
      public int Compare(object x, object y) {
        Person lhs = (Person)x;
        Person rhs = (Person)y;
        // Name 오름차순, Age 내림차순 정렬
        int nameCompare = lhs.Name.CompareTo(rhs.Name);
        if( nameCompare != 0 ) return nameCompare;
        return rhs.Age - lhs.Age;
      }
    }

    public partial class Window1 : Window {
      ...
      ICollectionView GetFamilyView() {
        People people = (People)this.FindResource("Family");
        return CollectionViewSource.GetDefaultView(people);
      }
      void sortButton_Click(object sender, RoutedEventArgs e) {
        ListCollectionView view = (ListCollectionView)GetFamilyView();
        if( view.CustomSort == null ) {
          view.CustomSort = new PersonSorter();
        }
        else {
          view.CustomSort = null;
        }
      }
    }
    ```

*  기본 컬렉션 뷰
  - 각 컬렉션 데이터 타입에 대한 기본 뷰
    | 컬렉션 데이터 | 기본 뷰 |
    | --- | --- |
    | IEnumerable | CollectionView |
    | IList | ListCollectionView |
    | IBindingList | BindingListCollectionView |

* 리스트 필터링
  - 다음과 같이 Fileter 프로퍼티를 이용해서 조건에 따라 원하는 것만 리스트에 표시하게 할 수 있습니다.
    ```cs
    public partial class Window1 : Window {
      ...
      ICollectionView GetFamilyView() {
        People people = (People)this.FindResource("Family");
        return CollectionViewSource.GetDefaultView(people);
      }

      void filterButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        if( view.Filter == null ) {
           view.Filter = delegate(object item) {
            // 25 이상만 보여줌
            return ((Person)item).Age >= 25;
          };
        }
        else {
          view.Filter = null;
        }
      }
    }
    ```

* 리스트 그룹화
  - 그룹화를 설정하려면 (1) 뷰의 GroupDescriptions 컬렉션을 조작하여 그룹을 설정해야 하고, (2) PropertyGroupDescription 오브젝트는 그룹화에 사용하고 싶은 프로퍼티의 이름을 받아야 합니다.
    ```cs
    public partial class Window1 : Window {
      ...
      ICollectionView GetFamilyView() {
        People people = (People)this.FindResource("Family");
        return CollectionViewSource.GetDefaultView(people);
      }

      void groupButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        if( view.GroupDescriptions.Count == 0 ) {
          // 나이 기준으로 그룹화
          view.GroupDescriptions.Add(new PropertyGroupDescription("Age"));
        }
        else {
          view.GroupDescriptions.Clear();
        }
      }
    }
    ```
  - 그룹 스타일: 컨테이너의 실제 스타일, 헤더의 데이터 템플릿, 빈 그룹을 숨길지 여부 등 그룹 시각화 관련 정보 등을 담고 있음
    ```xaml
    <ListBox ... ItemsSource="{Binding}" >
      <ListBox.GroupStyle>
        <x:Static Member="GroupStyle.Default" />
      </ListBox.GroupStyle>
    </ListBox>
    ```
  - 커스텀 그룹 스타일 (이렇게 하면 그룹의 배경은 Black, 글자는 White에 Bold 스타일로 나오고, 그룹 내 아이템 개수가 표시됨)
    ```xaml
    <ListBox ... ItemsSource="{Binding}">
      <ListBox.GroupStyle>
        <GroupStyle>
          <GroupStyle.HeaderTemplate>
            <DataTemplate>
              <TextBlock
                Background="Black" Foreground="White" FontWeight="Bold">
                <TextBlock Text="{Binding Name}" />
                (<TextBlock Text="{Binding ItemCount}" />)
              </TextBlock>
            </DataTemplate>
          </GroupStyle.HeaderTemplate>
        </GroupStyle>
      </ListBox.GroupStyle>
    </ListBox>
    ```
  - 커스텀 값 변환기를 이용해서 그룹화 기준을 변경할 수 있습니다. (다음의 경우 25세 미만인지 아닌지 여부로 그룹을 나눔)
    ```cs
    public class AgeToRangeConverter : IValueConverter {
      public object Convert(object value, Type targetType, ...) {
        return (int)value < 25 ? "Under the Hill" : "Over the Hill";
      }
      public object ConvertBack(object value, Type targetType, ...) {
        // 여기서는 호출되어서는 안 됨
        throw new NotImplementedException();
      }
    }

    void groupButton_Click(object sender, RoutedEventArgs e) {
      ICollectionView view = GetFamilyView();
      if( view.GroupDescriptions.Count == 0 ) {
        // 범위 기준으로 그룹화
        view.GroupDescriptions.Add(new PropertyGroupDescription("Age", new AgeToRangeConverter()));
      }
      else {
        view.GroupDescriptions.Clear();
      }
    }
    ```
  - 여러 PropertyGroupDescription을 넣어서 여러 수준으로 그룹화할 수 있습니다.
    ```cs
    void groupButton_Click(object sender, RoutedEventArgs e) {
      ICollectionView view = GetFamilyView();
      if( view.GroupDescriptions.Count == 0 ) {
        // 범위, 그 다음에 나이 기준으로 그룹화
        view.GroupDescriptions.Add(new PropertyGroupDescription("Age", new AgeToRangeConverter()));
        view.GroupDescriptions.Add(new PropertyGroupDescription("Age"));
      }
      else {
        view.GroupDescriptions.Clear();
      }
    }
    ```
  - 코드 비하인드 방식이 아닌 XAML에서의 선언적인 방식으로 정렬/그룹화할 수도 있습니다. (CollectionViewSource 태그, 다만 이 방식은 커스텀 방식 정렬을 지원하지 않으며 필터링도 불가능함)
    ```xaml
    <Window ...
      xmlns:local="clr-namespace:CollectionViewSourceBinding"
      xmlns:compModel="clr-namespace:System.ComponentModel;assembly=WindowsBase"
      xmlns:data="clr-namespace:System.Windows.Data;assembly=PresentationFramework">
      <Window.Resources>
        <local:People x:Key="Family">
          <local:Person Name="Tom" Age="11" />
          <local:Person Name="John" Age="12" />
          <local:Person Name="Melissa" Age="38" />
          <local:Person Name="Penny" Age="38" />
        </local:People>
        <local:AgeToRangeConverter x:Key="ageConverter" />
        <CollectionViewSource x:Key="SortedGroupedFamily"
                              Source="{StaticResource Family}">
          <CollectionViewSource.SortDescriptions>
            <compModel:SortDescription PropertyName="Name" Direction="Ascending" />
            <compModel:SortDescription PropertyName="Age" Direction="Descending" />
          </CollectionViewSource.SortDescriptions>
          <CollectionViewSource.GroupDescriptions>
            <data:PropertyGroupDescription PropertyName="Age" Converter="{StaticResource ageConverter}" />
            <data:PropertyGroupDescription PropertyName="Age" />
          </CollectionViewSource.GroupDescriptions>
        </CollectionViewSource>
      </Window.Resources>
      <Grid>
        <ListBox
          ItemsSource="{Binding Source={StaticResource SortedGroupedFamily}}"
          DisplayMemberPath="Name">
          <ListBox.GroupStyle>
            <x:Static Member="GroupStyle.Default" />
          </ListBox.GroupStyle>
        </ListBox>
      </Grid>
    </Window>
    ```

### 데이터 소스 공급자

* 만약 오브젝트가 네트워크를 통해 오거나 대용량 XML 또는 관계형 데이터로부터 변환되는 것처럼 시간이 오래 걸릴 수 있습니다.
  - 이 경우 다른 소스에서 오브젝트를 가져오기 위한 간접 참조 계층, 심지어 가져오는 작업을 워커 스레드에게 넘겨 버릴 수도 있습니다.
  - 간접 참조를 위해 데이터 소스 공급자를 사용합니다.
  - WPF에서는 DataSourceProvider 베이스 클래스로부터 파생된 2가지 데이터 소스 공급자를 제공합니다: ObjectDataProvider, XmlDataProvider.

* 오브젝트 데이터 공급자
  - 다음은 People 컬렉션을 로드한 후에 렌더링하도록 하는 코드입니다. (ObjectDataProvider 태그와 RemotePeopleLoader 클래스)
    ```cs
    ...
    public class Person : INotifyPropertyChanged {...}
    public class People : ObservableCollection<Person> {}
    public class RemotePeopleLoader {
      // ObjectDataProvider will expose results for binding
      public People LoadPeople() {
        // Load people from afar
        People people = new People();
        ...
        return people;
      }
    }
    ...
    ```
    ```xaml
    <Window.Resources>
      ...
      <ObjectDataProvider
        x:Key="Family"
        ObjectType="{x:Type local:RemotePeopleLoader}"
        MethodName="LoadPeople" />
    </Window.Resources>
    <Grid DataContext="{StaticResource Family}">
      ...
      <ListBox ItemsSource="{Binding}" .../>
    </Grid>
    ```
  - 이제 오브젝트 데이터 공급자가 데이터와 바인딩 사이의 중개자 역할을 하므로 거기서 데이터를 가져와야 합니다. (DataSourceProvider 클래스)
    ```cs
    public partial class Window1 : Window {
      ...
      ICollectionView GetFamilyView() {
        DataSourceProvider provider =
          (DataSourceProvider)this.FindResource("Family");
        People people = (People)provider.Data;
        return CollectionViewSource.GetDefaultView(people);
      }
    
      void birthdayButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        Person person = (Person)view.CurrentItem;
        ++person.Age;
        MessageBox.Show(...);
      }

      void addButton_Click(object sender, RoutedEventArgs e) {
        DataSourceProvider provider = (DataSourceProvider)this.FindResource("Family");
        People people = (People)provider.Data;
        people.Add(new Person("Chris", 37));
      }
      ...
    }
    ```

* 비동기식 데이터 수신
  - 동기식으로 데이터를 수신하면 그것을 다 받을 때까지는 UI 스레드가 차단됩니다.
  - UI 스레드가 계속 작동하면서도 데이터를 가져오는데 시간이 오래 걸릴 경우 다음과 같이 비동기식으로 데이터를 가져오게 됩니다. (IsAsynchronous="True")
    ```xaml
    <ObjectDataProvider
      x:Key="Family"
      ObjectType="{x:Type local:RemotePeopleLoader}"
      IsAsynchronous="True"
      MethodName="LoadPeople" />
    ```

* 파라미터 전달
  - 오브젝트 데이터 공급자는 MethodParameters 프로퍼티를 제공하는데 이것은 데이터를 검색하는 메서드에게 전달될 오브젝트 컬렉션입니다.
  - 다음은 ObjectDataProvider를 통해 데이터 검색을 시도할 URL 문자열 컬렉션을 전달하는 예제입니다. (ObjectDataProvider.MethodParameters)
    ```xaml
    <Window ...
      xmlns:sys="clr-namespace:System"
      xmlns:local="clr-namespace:ObjectBinding">
      <Window.Resources>
        <ObjectDataProvider
          x:Key="Family"
          ObjectType="{x:Type local:RemotePeopleLoader}"
          IsAsynchronous="True"
          MethodName="LoadPeople">
          <ObjectDataProvider.MethodParameters>
            <sys:String>http://sellsbrothers.com/boys.dat</sys:String>
            <sys:String>http://sellssisters.com/girls.dat</sys:String>
          </ObjectDataProvider.MethodParameters>
        </ObjectDataProvider>
        ...
      </Window.Resources>
      ...
    </Window>
    ```
  - LoadPeople 메서드를 수정하여 URL을 입력 받을 수 있게 합니다.
    ```cs
    namespace PersonBinding {
      public class RemotePeopleLoader : People {
        public People LoadPeople(string url1, string url2) {
          // Load People from afar using two URLs
          ...
        }
    }
    ```

* 관계형 데이터와 바인딩
  - 다음과 같은 Access 데이터베이스 예제가 주어졌다고 간주하고 진행하겠습니다. (family.mdb)
    | ID | Name | Age |
    | -- | ---- | --- |
    |  1 | Tom  |  11 |
    |  2 | John |  12 |
    |  3 | Melissa | 38 |
  - 서버 탐색기(Server Explorer) > 데이터 연결(Data Connection) > 연결 추가(Add Connection)를 통해 데이터베이스 파일을 연결합니다.
  - People 테이블을 디자이너 화면에 끌어다 놓으면 다음과 같이 3가지 클래스가 생성됩니다.
    ```cs
    namespace AdoBinding {
      ...
      public partial class Family : System.Data.DataSet {
        ...
        public partial class PeopleRow : System.Data.DataRow {
          ...
          public int ID { get {...} set {...} } }
          public string Name { get {...} set {...} } }
          public int Age { get {...} set {...} } }
          ...
        }
        public partial class PeopleDataTable :
          System.Data.DataTable, System.Collections.IEnumerable {
          ...
          public PeopleRow AddPeopleRow(string Name, int Age) {...}
          public PeopleRow FindByID(int ID) {...}
          public void RemovePeopleRow(PeopleRow row) {...}
          ...
        }
      }
      namespace FamilyTableAdapters {
        ...
        public partial class PeopleTableAdapter :
          System.ComponentModel.Component {
          ...
          public virtual Family.PeopleDataTable GetData() {...}
          ...
        }
      }
    }
    ```
    * PeopleRow 클래스: ADO.NET에 내장된 DataRow 클래스를 타입화하여 감싼 래퍼(wrapper)입니다. 이 클래스는 기본 데이터베이스 유형과 CLR 유형 간의 매핑을 담당합니다. WPF에서 관계형 데이터에 바인딩할 때, 여러분은 이러한 DataRow 파생 객체들로 채워진 DataTable에 바인딩하게 될 것입니다.
    * PeopleDataTable 클래스: PeopleRow 객체를 생성하고 찾는 가장 빠른 방법을 알고 있습니다.
    * PeopleTableAdapter 클래스: Access로부터 데이터를 읽고 쓰는 방법, 데이터를 가져오는 방법, 그리고 변경 사항을 추적하여 데이터베이스에 다시 반영(pushing back)하는 방법을 알고 있습니다.
  - 연결 문자열(connection string)이 app.config 파일에 추가되어 코드와 분리된 채로 유지 보수가 가능합니다.
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
     <configuration>
      <connectionStrings>
        <add name="AdoBinding.Properties.Settings.familyConnectionString"
          connectionString="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=family.mdb"
          providerName="System.Data.OleDb" />
      </connectionStrings>
    </configuration>
    ```
  - 메인 윈도우의 생성자에서 PeopleTableAdapter의 인스턴스를 생성하고 GetData를 호출하여 바인딩할 수 있습니다.
    ```cs
    public Window1() {
      InitializeComponent();

      // 동기적 바인딩
      DataContext = (new FamilyTableAdapters.PeopleTableAdapter()).GetData();
      ...
    }
    ```
    ```xaml
    <!-- Window1.xaml -->
    <!-- 비동기적 바인딩 -->
     <Window ...
      xmlns:local="clr-namespace:AdoBinding"
      xmlns:tableAdapters="clr-namespace:AdoBinding.FamilyTableAdapters">
      <Window.Resources>
        <ObjectDataProvider
          x:Key="Family"
          ObjectType="{x:Type tableAdapters:PeopleTableAdapter}"
          IsAsynchronous="True"
          MethodName="GetData" />
        <local:AgeToForegroundConverter x:Key="ageConverter" />
      </Window.Resources>
      <Grid DataContext="{StaticResource Family}">
        ...
        <ListBox ... ItemsSource="{Binding}">
          <ListBox.ItemTemplate>
            <DataTemplate>
              <TextBlock>
                <TextBlock Text="{Binding Path=Name}" />
                (age: <TextBlock Text="{Binding Path=Age}"
                  Foreground="
                    {Binding
                      Path=Age,
                      Converter=
                        {StaticResource ageConverter}}" />)
              </TextBlock>
            </DataTemplate>
          </ListBox.ItemTemplate>
        </ListBox>
        ...
      </Grid>
    </Window>
    ```
  - 다음은 PeopleRow 객체에 접근/추가/필터링하는 방법에 대한 예제입니다.
    ```cs
    // Window1.xaml.cs
    ...
    using System.Data;
    using System.Data.OleDb;
    public partial class Window1 : Window {
      public Window1() {
        InitializeComponent();

        this.birthdayButton.Click += birthdayButton_Click;
        this.backButton.Click += backButton_Click;
        this.forwardButton.Click += forwardButton_Click;
        this.addButton.Click += addButton_Click;
        this.sortButton.Click += sortButton_Click;
        this.filterButton.Click += filterButton_Click;
        this.groupButton.Click += groupButton_Click;
      }
      ICollectionView GetFamilyView() {
        DataSourceProvider provider = (DataSourceProvider)this.FindResource("Family");
        return CollectionViewSource.GetDefaultView(provider.Data);
      }
      void birthdayButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        // 각각의 아이템은 DataRowView입니다. 이걸로 타입화된 PersonRow에 접근할 수 있습니다.
        AdoBinding.Family.PeopleRow person = (AdoBinding.Family.PeopleRow)((DataRowView)view.CurrentItem).Row;
        ++person.Age;
        MessageBox.Show(string.Format("Happy Birthday, {0}, age {1}!", person.Name, person.Age), "Birthday");
      }
      void backButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        view.MoveCurrentToPrevious();
        if( view.IsCurrentBeforeFirst ) {
          view.MoveCurrentToFirst( );
        }
      }
      void forwardButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        view.MoveCurrentToNext();
        if( view.IsCurrentAfterLast ) {
          view.MoveCurrentToLast( );
        }
      }
      void addButton_Click(object sender, RoutedEventArgs e) {
        // 새로운 PeopleRow 만들기
        DataSourceProvider provider = (DataSourceProvider)this.FindResource("Family");
        AdoBinding.Family.PeopleDataTable table = (AdoBinding.Family.PeopleDataTable)provider.Data;
        table.AddPeopleRow("Chris", 37);
      }
      void sortButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        if( view.SortDescriptions.Count == 0 ) {
          view.SortDescriptions.Add(new SortDescription("Name", ListSortDirection.Ascending));
          view.SortDescriptions.Add(new SortDescription("Age", ListSortDirection.Descending));
        }
        else {
          view.SortDescriptions.Clear( );
        }
      }
      void filterButton_Click(object sender, RoutedEventArgs e) {
        // Filter 프로퍼티를 설정할 수 없습니다. 대신 BindingListCollectionView의 CustomFilter를 설정할 수 있습니다.
        BindingListCollectionView view = (BindingListCollectionView)GetFamilyView();
        if( string.IsNullOrEmpty(view.CustomFilter) ) {
          view.CustomFilter = "Age > 25";
        }
        else {
          view.CustomFilter = null;
        }
      }
      void groupButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        if( view.GroupDescriptions.Count == 0 ) {
          // 나이별로 그룹화
          view.GroupDescriptions.Add(new PropertyGroupDescription("Age"));
        }
        else {
          view.GroupDescriptions.Clear( );
        }
      }
    }
    ```

* XML 데이터 소스 공급자
  - 다음은 XML로 표현된 family 데이터입니다.
    ```xml
    <Family xmlns="http://sellsbrothers.com">
      <Person Name="Tom" Age="11" />
      <Person Name="John" Age="12" />
      <Person Name="Melissa" Age="38" />
    </Family>
    ```
  - 다음은 XmlDataProvider를 사용하여 바인딩하는 예제입니다. (네임스페이스 접두사를 사용하면 Person 요소 집합을 찾는 XPath 구문을 구성할 수 있음)
    ```xaml
    <!-- Window1.xaml -->
    <Window ...>
      <Window.Resources>
        <XmlDataProvider
          x:Key="Family"
          Source="family.xml"
          XPath="/sb:Family/sb:Person">
          <XmlDataProvider.XmlNamespaceManager>
            <XmlNamespaceMappingCollection>
              <XmlNamespaceMapping Uri="http://sellsbrothers.com" Prefix="sb" />
            </XmlNamespaceMappingCollection>
          </XmlDataProvider.XmlNamespaceManager>
        </XmlDataProvider>
        <local:AgeToForegroundConverter
          x:Key="ageConverter" />
      </Window.Resources>
      <Grid DataContext="{StaticResource Family}">
        ...
        <ListBox ... ItemsSource="{Binding}">
          <ListBox.ItemTemplate>
            <DataTemplate>
              <StackPanel Orientation="Horizontal">
                <TextBlock Text="{Binding XPath=@Name}" />
                <TextBlock Text=" (age: " />
                <TextBlock Text="{Binding XPath=@Age}"
                  Foreground="{Binding XPath=@Age,
                                Converter={StaticResource ageConverter}}" />
                <TextBlock Text=")" />
              </StackPanel>
            </DataTemplate>
          </ListBox.ItemTemplate>
        </ListBox>
        ...
      </Grid>
    </Window>
    ```
  - XML 데이터 아일랜드: 내장된 XML 데이터 소스를 의미하며 컴파일 타임에 데이터를 미리 알 수 있음
    ```xaml
    <XmlDataProvider x:Key="Family" XPath="/sb:Family/sb:Person">
      <XmlDataProvider.XmlNamespaceManager>
        <XmlNamespaceMappingCollection>
          <XmlNamespaceMapping Uri="http://sellsbrothers.com" Prefix="sb" />
        </XmlNamespaceMappingCollection>
      </XmlDataProvider.XmlNamespaceManager>
      <x:XData>
        <Family xmlns="http://sellsbrothers.com">
          <Person Name="Tom" Age="11" />
          <Person Name="John" Age="12" />
          <Person Name="Melissa" Age="38" />
        </Family>
      </x:XData>
    </XmlDataProvider>
    ```
  - 다음 코드는 오브젝트 데이터 소스 대신 XML 데이터 소스를 사용할 수 있도록 수정된 것입니다.
    ```cs
    // Window1.xaml.cs
    ...
    using System.Xml;
    public partial class Window1 : Window {
      public Window1() {
        InitializeComponent();

        this.birthdayButton.Click += birthdayButton_Click;
        this.backButton.Click += backButton_Click;
        this.forwardButton.Click += forwardButton_Click;
        this.addButton.Click += addButton_Click;
        this.sortButton.Click += sortButton_Click;
        this.filterButton.Click += filterButton_Click;
        this.groupButton.Click += groupButton_Click;
      }
      ICollectionView GetFamilyView() {
        DataSourceProvider provider = (DataSourceProvider)this.FindResource("Family");
        return CollectionViewSource.GetDefaultView(provider.Data);
      }
      void birthdayButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        // 각각의 "person"은 XmlElement이며 애트리뷰트 값은 문자열 기반 인덱서로부터 가져옵니다.
        XmlElement person = (XmlElement)view.CurrentItem;
        person.SetAttribute("Age", (int.Parse(person.Attributes["Age"].Value) + 1).ToString());
        MessageBox.Show(string.Format("Happy Birthday, {0}, age {1}!", person.Attributes["Name"].Value, person.Attributes["Age"].Value), "Birthday");
      }
      void backButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        view.MoveCurrentToPrevious();
        if( view.IsCurrentBeforeFirst ) {
          view.MoveCurrentToFirst( );
        }
      }
      void forwardButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        view.MoveCurrentToNext();
        if( view.IsCurrentAfterLast ) {
          view.MoveCurrentToLast( );
        }
      }
      void addButton_Click(object sender, RoutedEventArgs e) {
        // 새로운 XmlElement 만들기
        XmlDataProvider provider = (XmlDataProvider)this.FindResource("Family");
        XmlElement person = provider.Document.CreateElement("Person", "http://sellsbrothers.com");
        person.SetAttribute("Name", "Chris");
        person.SetAttribute("Age", "37");
        provider.Document.ChildNodes[0].AppendChild(person);
      }
      void sortButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        if( view.SortDescriptions.Count == 0 ) {
          view.SortDescriptions.Add(new SortDescription("@Name", ListSortDirection.Ascending));
          view.SortDescriptions.Add(new SortDescription("@Age", ListSortDirection.Descending));
        }
        else {
          view.SortDescriptions.Clear( );
        }
      }
      void filterButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        if( view.Filter == null ) {
          view.Filter = delegate(object item) {
            return int.Parse(((XmlElement)item).Attributes["Age"].Value) > 25;
          };
        }
        else {
          view.Filter = null;
        }
      }
      void groupButton_Click(object sender, RoutedEventArgs e) {
        ICollectionView view = GetFamilyView();
        if( view.GroupDescriptions.Count == 0 ) {
          // 나이별로 그룹화
          view.GroupDescriptions.Add(new PropertyGroupDescription("@Age"));
        }
        else {
          view.GroupDescriptions.Clear( );
        }
      }
    }
    ```
  - 다음은 데이터 소스 공급자가 없는 XML 바인딩 예제 코드입니다. (수동으로 XML 로드)
    ```xaml
    <!-- Window1.xaml -->
    <Window ...>
      <Window.Resources>
        <!-- XmlDataProvider 없음 -->
        <local:AgeToForegroundConverter x:Key="ageConverter" />
      </Window.Resources>
      <!-- 코드 비하인드에서 DataContext 설정 -->
      <Grid Name="grid">...</Grid>
    </Window>
    ```
    ```cs
    // Window1.xaml.cs
    ...
    public partial class Window1 : Window {
      // family XML 문서
      XmlDocument doc;
      public Window1() {
        ...
        LoadFamilyXml();
      }
      void LoadFamilyXml() {
        // XmlDocument를 이용하여 XML 로드
        doc = new XmlDocument();
        doc.Load("family.xml");
        // 바인딩에서 사용할 수 있도록 네임스페이스 접두사 매핑을 만듭니다.
        XmlNamespaceManager manager = new XmlNamespaceManager(doc.NameTable);
        manager.AddNamespace("sb", "http://sellsbrothers.com");
        Binding.SetXmlNamespaceManager(grid, manager);
        // 데이터 바인딩을 위해 XML을 사용 가능하게 만듭니다. 여기서 바인딩을 사용하는 이유는 소스 문서가 변경될 때 이를 감지하여 XPath 쿼리가 반환하는 노드 집합을 새로 고칠 수 있기 때문입니다.
        Binding b = new Binding();
        b.XPath = "/sb:Family/sb:Person";
        b.Source = doc;
        grid.SetBinding(Grid.DataContextProperty, b);
      }
      ICollectionView GetFamilyView() {
        // 데이터로부터 직접 가져온 기본 뷰
        return CollectionViewSource.GetDefaultView(grid.DataContext);
      }
      ...
      void addButton_Click(object sender, RoutedEventArgs e) {
        // 새로운 XmlElement 만들기
        XmlElement person = doc.CreateElement("Person", "http://sellsbrothers.com");
        person.SetAttribute("Name", "Chris");
        person.SetAttribute("Age", "37");
        doc.DocumentElement.AppendChild(person);
      }
      ...
    }
    ```

### Master-Detail 바인딩

...

### 계층적 바인딩

...

## 스타일

... Without Styles

... Inline Styles

... Named Styles

... Element-Typed Styles

... Data Templates and Styles

... Triggers

## 컨트롤 템플릿

... Beyond Styles

... Logical and Visual Trees

... Data-Driven UI

## 창 및 다이얼로그

... Window

... Dialogs

## 네비게이션

... NavigationWindow

... Pages

... Frames

... XBAPs

... Navigation to HTML

## 리소스

... Creating and Using Resources

... Resources and Styles

... Binary Resources

... Global Applications

## 그래픽

... Graphics Fundamentals

... Shapes

... Bitmaps

... Brushes and Pens

... Transformations

... Visual Layer Programming

## 텍스트 및 Flow 도큐먼트

... Fonts and Text Styles

... Text and the User Interface

... Texst Object Model

... Typography

## 인쇄 및 XPS

... XPS

... XPS Document Classes

... Generating XPS Output

... XPS File Generation Features

... System.Printing

... Displaying Fixed Documents

## 애니메이션 및 미디어

... Animation Fundamentals

... Timelines

... Keyframe Animations

... Path Animations

... Clocks and Control

... Transition Animations

... Audio and Video

## 3D 그래픽

... 3D Content in a 2D World

... Cameras

... Models

... Lights

... Textures

... Transforms

... 3D Data Visualization

... Hit Testing

## 커스텀 컨트롤

... Custom Control Basics

... Choosing a Base Class

... Custom Functionality

... Supporting Templates in Custom Controls

... Default Styles

... UserControl

... Adorners





<!-- Programming WPF 2nd edition 참조... 페이지 268/867 -->
