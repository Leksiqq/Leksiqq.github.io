**Attention!** _This article, as well as this announcement, are automatically translated from Russian_.

# ParameterizedResourceExtension class
Used in XAML markup similar to `<StaticResource>` or `{StaticResource ResourceKeyValue}`. It works in the same way, but if the $ symbol is found in the value of some attributes, it determines it as the beginning of the parameter name and tries to substitute the specified replacement for the parameter.
parameter search is performed:
* In `<Binding>` or `{Binding ... }` elements
     * `Path`
     * `ElementName`
     * `ConverterParameter`
     * `Source`
     * `XPath`
* In `<MultiBinding>` elements
     * `ConverterParameter`
* In `<l:ParameterizedResource>` elements (assuming a prefix is declared: `xmlns:l="clr-namespace:Net.Leksi.WpfMarkup;assembly=Net.Leksi.WpfMarkupExtension"`)
     *`ResourceKey`

**Important!** Note that when using parameters, the resource must be declared with the `x:Shared="False"` attribute, otherwise the substitution will only be done once, when a single instance is created!
### Properties
* `ResourceKey` - Gets or sets the key value passed by this static resource reference. The key is used to return an object that has the corresponding key in the resource dictionary. Syntax options:
     * in attribute: `{l:ParameterizedResource ResourceKeyValue ... }`
     * in element:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue"/>
```
* `Replaces` is an array of strings describing parameter substitutions. Inherited by upstream `ProvideValue` calls on the stack. If substitution is required, the latest definition encountered in the `ProvideValue` call stack is used. Syntax options:
     * in attribute: `{l:ParameterizedResource ResourceKeyValue, Replaces=$param1:replace1|$param2:replace2... }`
     * in element:
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
or
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
* `Defaults` array of strings describing parameter substitutions. Not inherited by upstream `ProvideValue` calls on the stack. If a parameter is set to a default value, then it is substituted when the corresponding substitution is not previously set in the `ProvideValue` call stack. Syntax options:
     * in attribute: `{l:ParameterizedResource ResourceKeyValue, Defaults=$param1:default1|$param2:default2... }`
     * in element:
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
* `At` is an arbitrary string label to track when debugging (see `Verbose` below). Syntax options:
     * in attribute: `{l:ParameterizedResource ResourceKeyValue, At=AtValue ... }`
     * in element:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue" At="AtValue"/>
```
* `Verbose` - verbosity of output to the console during debugging, by default - `0` - no output. The value `1` causes `ProvideValue` calls and actions with parameters to be output to the console: substitution inheritance and substitutions themselves; the value `2`, except for the listed one, displays all checked elements of the logical and visual trees. Note that the application must be compiled as a "Console Application" in order to be output to the console. Syntax options:
     * in attribute - `{l:ParameterizedResource ResourceKeyValue, Verbose=VerboseValue ... }`
     * in element:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue", Verbose="VerboseValue"/>
```
* `Strict` - boolean variable, default - `true` - throws `System.Windows.Markup.XamlParseException` if some parameter is not substituted. A value of `false` causes the appropriate message to be printed to the console, provided that the application is compiled as a "Console Application". Syntax options:
     * in attribute - `{l:ParameterizedResource ResourceKeyValue, Strict=False... }`
     * in element:
```
<l:ParameterizedResource ResourceKey="ResourceKeyValue", Strict="False"/>
```
### Example
As an example, it is proposed to trace in [demo](Demo) on the tab "Demo2" the templates `CellTemplateSelector` and `CellEditingTemplateSelector` in `DataGrid` and resources for them in the file `Dictionary2.xaml`.

**Before:** ([StyleCombiner](StyleCombiner-en)) **Start:**([Overview](Overview)) **Next:**([BindingProxy](BindingProxy-en))