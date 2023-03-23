# Класс ParameterizedResourceExtension
Применяется в разметке XAML аналогично `<StaticResource>` или `{StaticResource ResourceKeyValue}`. Действует так же, но при обнаружении в значении некоторых атрибутов символа $ определяет его как начало имени параметра и пытается подставить вместо параметра заданную замену. 
поиск параметров производится:
* В элементах `<Binding>` или `{Binding ... }`
    * `Path`
    * `ElementName`  
    * `ConverterParameter`
    * `Source`  
    * `XPath`
* В элементах `<MultiBinding>`
    * `ConverterParameter`
* В элементах `<l:ParameterizedResource>` (предполагается, что объявлен префикс: `xmlns:l="clr-namespace:Net.Leksi.WpfMarkup;assembly=Net.Leksi.WpfMarkupExtension"`)
    * `ResourceKey`

**Важно!** Следует иметь в виду, что при использовании параметров ресурс должен быть объявлен с атрибутом `x:Shared="False"`, так как в противном случае подстановка будет выполнена только один раз при создании единственного экземпляра!
### Свойства
* `ResourceKey` - Получает или задает значение ключа, передаваемое данной статической ссылкой ресурса. Ключ используется для возврата объекта, имеющего соответствующий ключ в словаре ресурсов. Варианты синтаксиса: 
    * в атрибуте: `{l:ParameterizedResource ResourceKeyValue ... }`
    * в элементе:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue"/>
```
* `Replaces` - массив строк, описывающий подстановки параметров. Наследуется вышележащими вызовами `ProvideValue` в стеке. При необходимости подстановки используется самое позднее встреченное в стеке вызовов `ProvideValue` определение. Варианты синтаксиса: 
    * в атрибуте: `{l:ParameterizedResource ResourceKeyValue, Replaces=$param1:replace1|$param2:replace2... }`
    * в элементе:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue">
    <x:Array Type="sys:String">
        <sys:String>$param1:replace1</sys:String>
        <sys:String>$param2:replace2</sys:String>
        ...
    </x:Array>
    ...
</l:ParameterizedResource>
```
или
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue">
    <l:ParameterizedResource.Replaces>
        <x:Array Type="sys:String">
            <sys:String>$param1:replace1</sys:String>
            <sys:String>$param2:replace2</sys:String>
            ...
        </x:Array>
    </l:ParameterizedResource.Replaces>    
    ...
</l:ParameterizedResource>
```
* `Defaults`массив строк, описывающий подстановки параметров. Не наследуется вышележащими вызовами `ProvideValue` в стеке. Если для параметра задано значение по умолчанию, то оно подставляется, когда соответствующая подстановка не задана ранее в стеке вызовов `ProvideValue`. Варианты синтаксиса: 
    * в атрибуте: `{l:ParameterizedResource ResourceKeyValue, Defaults=$param1:default1|$param2:default2... }`
    * в элементе:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue">
    <l:ParameterizedResource.Defaults>
        <x:Array Type="sys:String">
            <sys:String>$param1:default1</sys:String>
            <sys:String>$param2:default2</sys:String>
            ...
        </x:Array>
    </l:ParameterizedResource.Defaults>    
    ...
</l:ParameterizedResource>
```
* `At` - произвольная строковая метка для отслеживания при отладке (смотрите `Verbose` ниже). Варианты синтаксиса: 
    * в атрибуте: `{l:ParameterizedResource ResourceKeyValue, At=AtValue ... }`
    * в элементе:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue" At="AtValue"/>
```
* `Verbose` - степень детализации вывода на консоль при отладке, по умолчанию - `0` - отсутствие вывода. Значение `1` приводит к выводу на консоль вызовов `ProvideValue` и действия с параметрами: наследование подстановок и сами подстановки; значение `2` кроме перечисленного выводит все проверяемые элементы логического и визуального деревьев. Следует иметь в виду, что для вывода на консоль приложение должно компилироваться как "Консольное приложение". Варианты синтаксиса: 
    * в атрибуте - `{l:ParameterizedResource ResourceKeyValue, Verbose=VerboseValue ... }`
    * в элементе:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue", Verbose="VerboseValue"/>
```
* `Strict` - булева переменная, по умолчанию - `true` - вызывает `System.Windows.Markup.XamlParseException`, если какой-то параметр остался не подставленным. Значение `false` приводит к выводу на консоль соответствующего сообщения при условии, что приложение скомпилировано как "Консольное приложение". Варианты синтаксиса: 
    * в атрибуте - `{l:ParameterizedResource ResourceKeyValue, Strict=False... }`
    * в элементе:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue", Strict="False"/>
```
### Пример
В качестве примера предлагается проследить в [демо](Демо) на вкладке «Demo2» шаблоны `CellTemplateSelector` и `CellEditingTemplateSelector` в `DataGrid` и ресурсы для них в файле `Dictionary2.xaml`. 

**Раньше:** ([StyleCombiner](StyleCombiner-ru)) **Начало:**([Обзор](Обзор)) **Дальше:**([BindingProxy](BindingProxy-ru))