**Attention!** _This article, as well as this announcement, are automatically translated from Russian_.

# BindingProxy class
A versatile resource that can be used in a variety of situations.
### Properties
* `Value` - receives and returns any object, can be either a target property or a binding source.
### Examples
#### Example 1.
Allocate a resource to the window's resources by associating it with some `DataContext` property of the window. In the `DataGrid` or `ComboBox` resources, when marking up templates, if necessary, get a binding to this property without searching for the `DataContext` of the window (since in templates `DataContext` is the current element of the collection).

In `MainWindow.xaml.cs`:
```
...
public RemoveCommand RemoveCommand { get; init; }
public ObservableCollection<DataHolder> Datas { get; init; } = new();
public bool IsDataEditable { get; set; }
...
public MainWindow()
{
     DatasViewSource.Source = Datas;
     RemoveCommand = new RemoveCommand(Data);
     ...
}
```
In the `MainWindow.xaml` resource dictionary:
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
In the `DataGrid` template in `MainWindow.xaml`:
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
Here we place the “Delete” button in each row, which is associated with the `RemoveCommand` command, which is a property of the window, and as a parameter we pass a multi-binding to the current element of the collection and a window property indicating whether the collection is currently being edited.
#### Example 2.
Use as a cell to pass some current state or object to and from the markup. For example, in [demo](Demo) on the "Demo2" tab in the table, each field represents not only a value, but also the current data type. In order for the converter to know how to convert the string and field value when editing a cell, a window resource is introduced:
```
<Window.Resources>
     <ResourceDictionary>
         ...
         <l:BindingProxy x:Key="CurrentEditedItem"/>
         ...
     </ResourceDictionary>
</Window.Resources>
```
In `MainWindow.xaml.cs`, this resource is passed to the converter, which has a `CurrentEditedItem` property for this:
```
public MainWindow()
{
     ...
     InitializeComponent();
     (FindResource("DataConverter") as DataConverter)!.CurrentEditedItem = FindResource("CurrentEditedItem") as BindingProxy;
}
```
When cell editing is enabled, the `DataTemplateSelector` is called, which knows what _what_ is currently being edited, and puts a reference to the `CurrentEditedItem` resource. In `TableDataTemplateSelector.cs`:
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
         returnEditValue;
     }
     return base.SelectTemplate(item, container);
}
```
#### Example 3
This use case is not included in the demo, but it might be useful. If dependency injection is used, then the `BindingProxy` resource can be passed to the `IServiceProvider` markup.

In `MainWindow.xaml`:
```
<Window.Resources>
     <ResourceDictionary>
         ...
         <l:BindingProxy x:Key="ServiceProvider"/>
         ...
     </ResourceDictionary>
</Window.Resources>
```
In `MainWindow.xaml.cs`:
```
public MainWindow(IServiceProvider services)
{
     ...
     InitializeComponent();
     (FindResource("ServiceProvider") as BindingProxy)!.Value = services;
}
```
Somewhere in the markup extension:
```
public override object ProvideValue(IServiceProvider serviceProvider)
{
     IServiceProvider hostServiceProvider =((FindResource("ServiceProvider") as BindingProxy)!.Value as IServiceProvider)!;
     ....
}
```
Obviously, there is no extra `IServiceProvider` here, since the argument of the `ProvideValue` method does not provide those services that are configured before creating and starting the `Host`.

There are probably more options.

**Before:** ([ParameterizedResource](ParameterizedResource-en)) **Start:**([Overview](Overview)) **Next:**([DataSwitch](DataSwitch-en))