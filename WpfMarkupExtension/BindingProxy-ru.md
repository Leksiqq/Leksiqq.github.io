# Класс BindingProxy
Универсальный ресурс, который можно использовать в различных ситуациях. 
### Свойства
* `Value` - получает и возвращает любой объект, может быть как целевым свойством, так и источником привязки.
### Примеры
#### Пример 1. 
Разместить ресурс в ресурсах окна, связав его с каким-либо свойством `DataContext` окна. В ресурсах `DataGrid` или `ComboBox` при разметке шаблонов при необходимости получить привязку к этому свойству, не занимаясь поиском `DataContext` окна (так как в шаблонах `DataContext` - это текущий элемент коллекции).

В `MainWindow.xaml.cs`:
```
...
public RemoveCommand RemoveCommand { get; init; }
public ObservableCollection<DataHolder> Datas { get; init; } = new(); 
public bool IsDatasEditable { get; set; }
...
public MainWindow()
{
    DatasViewSource.Source = Datas;
    RemoveCommand = new RemoveCommand(Datas);
    ...
}
```
В словаре ресурсов `MainWindow.xaml`:
```
<Window.Resources>
    <ResourceDictionary>
        ...
        <l:BindingProxy x:Key="RemoveCommand" Value="{Binding RemoveCommand}"/>
        <l:BindingProxy x:Key="IsEditable" Value="{Binding IsDatasEditable}"/>
        ...
    </ResourceDictionary>
</Window.Resources>
```
В шаблоне `DataGrid` в `MainWindow.xaml`:
```
<DataGrid ItemsSource="{Binding DatasViewSource.View}" AutoGenerateColumns="False" Margin="10,10,10,10" 
                          CanUserAddRows="False" x:Name="DataGrid">
    <DataGrid.Resources>
        <DataTemplate x:Key="Actions">
            <StackPanel Orientation="Horizontal">
                <Button Content="➖" Command="{Binding Value, Source={StaticResource RemoveCommand}}" 
                        ToolTip="Remove row">
                    <Button.CommandParameter>
                         <MultiBinding Converter="{l:ParameterizedResource DataConverter}" 
                                       ConverterParameter="RemoveCommandCanExecute">
                             <Binding Path="."/>
                             <Binding Path="Value" Source="{StaticResource IsEditable}"/>
                         </MultiBinding>
                    </Button.CommandParameter>
                </Button>
            </StackPanel>
        </DataTemplate>
    </DataGrid.Resources>
    <DataGrid.Columns>
        ...
        <DataGridTemplateColumn Header="Actions" CellTemplate="{StaticResource Actions}"/>
    </DataGrid.Columns>
</DataGrid>

```
Здесь мы в каждом ряду размещаем кнопку «Удалить», которая связана с командой `RemoveCommand`, которая является свойством окна, а в качестве параметра передаём мультипривязку на текущий элемент коллекции и свойство окна, показывающее, является ли коллекция в данный момент редактируемой.
#### Пример 2. 
Использование в качестве ячейки для передачи какого-то текущего состояния или объекта в разметку и из неё. Например, в [демо](Демо) на вкладке «Demo2» в таблице каждое поле представляет собой не только значение, но и текущий тип данных. Чтобы конвертер мог узнать, как конвертировать строку и значение поля при редактировании ячейки, вводится ресурс окна:
```
<Window.Resources>
    <ResourceDictionary>
        ...
        <l:BindingProxy x:Key="CurrentEditedItem"/>
        ...
    </ResourceDictionary>
</Window.Resources>
```
В `MainWindow.xaml.cs` этот ресурс передаётся конвертеру, у которого для этого предусмотрено свойство `CurrentEditedItem`:
```
public MainWindow()
{
    ...
    InitializeComponent();
    (FindResource("DataConverter") as DataConverter)!.CurrentEditedItem = FindResource("CurrentEditedItem") as BindingProxy;
}
```
При включении редактирования ячейки вызывается `DataTemplateSelector`, который знает, _что_ в данный момент редактируется и помещает ссылку в ресурс `CurrentEditedItem`. В `TableDataTemplateSelector.cs`:
```
public string FieldName { get; set; } = null!;
...
public override DataTemplate SelectTemplate(object item, DependencyObject container)
{
    if (item is DataHolder row && typeof(DataHolder).GetProperty(FieldName.ToString()!)?.GetValue(row) is FieldHolder field)
    {
        if(container is ContentPresenter cp && cp.FindResource("CurrentEditedItem") is BindingProxy bp)
        {
            bp.Value = field;
        }
        return EditValue;
    }
    return base.SelectTemplate(item, container);
}
```
#### Пример 3
Этот вариант использования в демо отсутствует, но может оказаться полезным. Если применяется внедрение зависимости, то через ресурс `BindingProxy` можно передавать в разметку `IServiceProvider`.

В `MainWindow.xaml`:
```
<Window.Resources>
    <ResourceDictionary>
        ...
        <l:BindingProxy x:Key="ServiceProvider"/>
        ...
    </ResourceDictionary>
</Window.Resources>
```
В `MainWindow.xaml.cs`:
```
public MainWindow(IServiceProvider services)
{
    ...
    InitializeComponent();
    (FindResource("ServiceProvider") as BindingProxy)!.Value = services;
}
```
Где-то в расширении разметки:
```
public override object ProvideValue(IServiceProvider serviceProvider)
{
    IServiceProvider hostServiceProvider = 
            ((FindResource("ServiceProvider") as BindingProxy)!.Value as IServiceProvider)!;
    ....
}
```
Очевидно, лишнего `IServiceProvider` здесь нет, так как аргумент метода `ProvideValue` не предоставляет те сервисы, которые конфигурируются перед созданием и запуском `Host`.

Вероятно, есть ещё варианты.

**Раньше:** ([ParameterizedResource](ParameterizedResource-ru)) **Начало:**([Обзор](Обзор)) **Дальше:**([DataSwitch](DataSwitch-ru))