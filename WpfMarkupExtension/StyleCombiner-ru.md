# Класс StyleCombiner

Применяется в разметке XAML аналогично `<Style>`. Имеет атрибут `TargetType` с тем же смыслом. Содержимое состоит из элементов:
* `<Style>`
* `<l:StyleCombiner>` (предполагается, что объявлен префикс: `xmlns:l="clr-namespace:Net.Leksi.WpfMarkup;assembly=Net.Leksi.WpfMarkupExtension"`)
* `<StaticResource>` с `ResourceKey`, ссылающимся на `<Style>`или `<l:StyleCombiner>`
* `<l:ParameterizedResource>` ([ParameterizedResource](ParameterizedResource-ru)) с `ResourceKey`, ссылающимся на `<Style>`или `<l:StyleCombiner>` 

В результате в разметку возвращается объект типа `Style` с триггерами и сеттерами в порядке их появления в содержимом `<l:StyleCombiner>`.

Это расширение даёт возможность применять стили, связанные с независимыми аспектами. 

### Пример
В [демо](Демо) на вкладке «Demo1» кнопки в рядах имеют фон одного цвета, а в колонках - одинаковые рамки. Соответствующие ряду и колонке стили применяются независимо. Также для последней кнопки в последнем ряду задана особая надпись. Кнопка, находящаяся на горизонтали, вертикали или диагонали, соединяющей «чекнутые» «чекбоксы», принимает форму эллипса, соответствующий стиль `ButtonShapeStyle` содержит параметр `$path` который может принимать значения имени свойства главного окна, показывающего, «чекнуты» ли оба соответствующих «чекбокса». Соответственно, стиль `ButtonShapeStyle` добавлен с соответствующей подстановкой параметров для каждой прямой. При наведении курсора на кнопку остальные кнопки из колонки меняют цвет фона на цвет фона кнопки под курсором, а остальные кнопки из ряда меняют рамку на такую же, как у кнопки под курсором. Для этого стили `ButtonRepaintColStyle` и `ButtonRepaintRowStyle` также добавлены с соответствующей заменой параметра `$coordinates` координатой каждой кнопки, которая оказывает влияние на данную. И наконец, при нажатии на любую кнопку центрально-симметричная ей мигает. Это достигается добавлением стиля `ButtonAnimationStyle` с заменой параметра `$coordinates` координатой кнопки, которая при нажатии вызовет её мигание.

                <Button Grid.Row="4" Grid.Column="6">
                    <Button.Style>
                        <l:StyleCombiner TargetType="Button">
                            <StaticResource ResourceKey="ButtonMouseEventStyle"/>
                            <StaticResource ResourceKey="ButtonAtRow4Style"/>
                            <StaticResource ResourceKey="ButtonAtCol6Style"/>
                            <Style TargetType="Button">
                                <Setter Property="Content">
                                    <Setter.Value>
                                        <TextBlock>Это<LineBreak/>нижняя<LineBreak/>правая</TextBlock>
                                    </Setter.Value>
                                </Setter>
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

**Начало:** ([Обзор](Обзор)) **Дальше:** ([ParameterizedResource](ParameterizedResource-ru))