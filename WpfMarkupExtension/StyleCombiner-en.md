**Attention!** _This article, as well as this announcement, are automatically translated from Russian_.

# StyleCombiner class

Used in XAML markup similar to `<Style>`. Has a `TargetType` attribute with the same meaning. Content consists of elements:
* `<Style>`
* `<l:StyleCombiner>` (assuming a prefix is declared: `xmlns:l="clr-namespace:Net.Leksi.WpfMarkup;assembly=Net.Leksi.WpfMarkupExtension"`)
* `<StaticResource>` with `ResourceKey` referring to `<Style>` or `<l:StyleCombiner>`
* `<l:ParameterizedResource>` ([ParameterizedResource](ParameterizedResource-en)) with `ResourceKey` referring to `<Style>` or `<l:StyleCombiner>`

As a result, an object of type `Style` is returned to the markup with triggers and setters in the order they appear in the content of `<l:StyleCombiner>`.

This extension allows you to apply styles associated with independent aspects.

### Example
In [demo](Demo) on the "Demo1" tab, the buttons in the rows have the same background color, and the buttons in the columns have the same borders. The corresponding row and column styles are applied independently. Also, the last button in the last row has a special label. A button located on a horizontal, vertical, or diagonal line connecting "checked" "checkboxes" takes the form of an ellipse, the corresponding `ButtonShapeStyle` style contains a `$path` parameter that can take the value of the name of the main window property, indicating whether both corresponding "checkbox". Accordingly, the `ButtonShapeStyle` style is added with the corresponding parameter substitution for each line. When you hover over a button, the rest of the buttons in the column change the background color to the background color of the button under the cursor, and the rest of the buttons in the row change the frame to the same as the button under the cursor. To do this, the `ButtonRepaintColStyle` and `ButtonRepaintRowStyle` styles are also added, with the appropriate replacement of the `$coordinates` parameter with the coordinate of each button that affects this one. And finally, when you press any button, the centrally symmetrical one blinks. This is achieved by adding a `ButtonAnimationStyle` style, replacing the `$coordinates` parameter with the coordinate of the button, which, when pressed, will make it blink.

                 <Button Grid.Row="4" Grid.Column="6">
                     <Button.Style>
                         <l:StyleCombiner TargetType="Button">
                             <StaticResourceResourceKey="ButtonMouseEventStyle"/>
                             <StaticResource ResourceKey="ButtonAtRow4Style"/>
                             <StaticResource ResourceKey="ButtonAtCol6Style"/>
                             <Style TargetType="Button">
                                 <Setter Property="Content">
                                     <Setter.Value>
                                         <TextBlock>This<LineBreak/>bottom<LineBreak/>right</TextBlock>
                                     </Setter.Value>
                                 </setter>
                                 <Setter Property="FontSize" Value="10.0"/>
                             </Style>
                             <l:ParameterizedResource ResourceKey="ButtonShapeStyle">
                                 <x:Array Type="sys:String">
                                     <sys:String>$path:Col_6_On</sys:String>
                                 </x:Array>
                             </l:ParameterizedResource>
                             <l:ParameterizedResource ResourceKey="ButtonShapeStyle">
                                 <x:Array Type="sys:String">
                                     <sys:String>$path:Row_4_On</sys:String>
                                 </x:Array>
                             </l:ParameterizedResource>
                             <l:ParameterizedResource ResourceKey="ButtonShapeStyle">
                                 <x:Array Type="sys:String">
                                     <sys:String>$path:LH_9_On</sys:String>
                                 </x:Array>
                             </l:ParameterizedResource>
                             <l:ParameterizedResource ResourceKey="ButtonShapeStyle">
                                 <x:Array Type="sys:String">
                                     <sys:String>$path:HL_6_On</sys:String>
                                 </x:Array>
                             </l:ParameterizedResource>

                             <l:ParameterizedResource ResourceKey="ButtonRepaintColStyle" Replaces="$coordinates:1.6"/>
                             <l:ParameterizedResource ResourceKey="ButtonRepaintColStyle" Replaces="$coordinates:2.6"/>
                             <l:ParameterizedResource ResourceKey="ButtonRepaintColStyle" Replaces="$coordinates:3.6"/>

                             <l:ParameterizedResource ResourceKey="ButtonRepaintRowStyle" Replaces="$coordinates:4.1"/>
                             <l:ParameterizedResource ResourceKey="ButtonRepaintRowStyle" Replaces="$coordinates:4.2"/>
                             <l:ParameterizedResource ResourceKey="ButtonRepaintRowStyle" Replaces="$coordinates:4.3"/>
                             <l:ParameterizedResource ResourceKey="ButtonRepaintRowStyle" Replaces="$coordinates:4.4"/>
                            <l:ParameterizedResource ResourceKey="ButtonRepaintRowStyle" Replaces="$coordinates:4.5"/>

                            <l:ParameterizedResource ResourceKey="ButtonAnimationStyle" Replaces="$coordinates:1.1"/>
                        </l:StyleCombiner>
                    </Button.Style>
                </Button>

**Start:** ([Overview](Overview)) **Next:** ([ParameterizedResource](ParameterizedResource-en))
