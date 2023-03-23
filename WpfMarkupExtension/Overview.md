**Attention!** _This article, as well as this announcement, are automatically translated from Russian_.

The **Net.Leksi.WpfMarkupExtension** library is designed to extend WPF markup. It contains several classes that you may find useful when developing XAML. All classes are contained in the `Net.Leksi.WpfMarkup` namespace.

* [StyleCombiner](StyleCombiner-en) - allows you to apply multiple styles to an element without inheritance.
* [ParameterizedResource](ParameterizedResource-en) - analogue of `StaticResourceExtension`, which allows using resources with parameters that can be replaced with different values in the markup.
* [BindingProxy](BindingProxy-en) is a universal resource that can serve as a link to any object or act as a binding.
* [DataSwitch](DataSwitch-en) - used instead of a large number of `DataTrigger` that have the same binding but different trigger values. Reduces both the XAML text and the number of calls to the binding source.

You can learn how to use the library using the [demo application](Demo).

Sources are [here](../tree/master)

NuGet package: [Net.Leksi.WpfMarkupExtension](https://www.nuget.org/packages/Net.Leksi.WpfMarkupExtension/)

**Next:** ([StyleCombiner](StyleCombiner-en))