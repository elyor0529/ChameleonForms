Field Configuration
===================

Field Configuration provides the ability to configure a [Field](field) (and its sub-components) on both an ad-hoc basis within a particular form and a convention basis across all forms. Specifying a Field Configuration is done by chaining calls to the methods on the `IFieldConfiguration` interface.

The `IFieldConfiguration` interface is translated to an `IReadonlyFieldConfiguration` just before it's passed to the template to make sure that modifications can't be made to it after it's processed by the template.

The `IFieldConfiguration` interface looks like this and is in the `ChameleonForms.Component.Config` namespace:

```csharp
    /// <summary>
    /// Holds configuration data for a form field.
    /// </summary>
    public interface IFieldConfiguration : IHtmlString
    {
        /// <summary>
        /// A dynamic bag to allow for custom extensions using the field configuration.
        /// </summary>
        dynamic Bag { get; }

        /// <summary>
        /// Returns data from the Bag stored in the given property or default(TData) if there is none present.
        /// </summary>
        /// <typeparam name="TData">The type of the expected data to return</typeparam>
        /// <param name="propertyName">The name of the property to retrieve the data for</param>
        /// <returns>The data from the Bag or default(TData) if there was no data against that property in the bag</returns>
        TData GetBagData<TData>(string propertyName);

        /// <summary>
        /// Attributes to add to the form element's HTML.
        /// </summary>
        HtmlAttributes Attributes { get; }

        /// <summary>
        /// Override the default id for the field.
        /// </summary>
        /// <param name="id">The text to use for the id</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Id(string id);

        /// <summary>
        /// Adds a CSS class (or a number of CSS classes) to the attributes.
        /// </summary>
        /// <param name="class">The CSS class(es) to add</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration AddClass(string @class);

        /// <summary>
        /// Adds or updates a HTML attribute with a given value.
        /// </summary>
        /// <param name="key">The name of the HTML attribute to add/update</param>
        /// <param name="value">The value of the HTML attribute to add/update</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Attr(string key, object value);

        /// <summary>
        /// Adds or updates a HTML attribute with using a lambda method to express the attribute.
        /// </summary>
        /// <example>
        /// h.Attr(style => "width: 100%;")
        /// </example>
        /// <param name="attribute">A lambda expression representing the attribute to set and its value</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Attr(Func<object, object> attribute);

        /// <summary>
        /// Adds or updates a set of HTML attributes using lambda methods to express the attributes.
        /// </summary>
        /// <param name="attributes">A list of lambas where the lambda variable name is the name of the attribute and the value is the value</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Attrs(params Func<object, object>[] attributes);

        /// <summary>
        /// Adds or updates a set of HTML attributes using a dictionary to express the attributes.
        /// </summary>
        /// <param name="attributes">A dictionary of attributes</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Attrs(IDictionary<string, object> attributes);

        /// <summary>
        /// Adds or updates a set of HTML attributes using anonymous objects to express the attributes.
        /// </summary>
        /// <param name="attributes">An anonymous object of attributes</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Attrs(object attributes);

        /// <summary>
        /// Sets the number of rows for a textarea to use.
        /// </summary>
        /// <param name="numRows">The number of rows for the textarea</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Rows(int numRows);

        /// <summary>
        /// Sets the number of cols for a textarea to use.
        /// </summary>
        /// <param name="numCols">The number of cols for the textarea</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Cols(int numCols);

        /// <summary>
        /// Sets the field to be disabled (value not submitted, can not click).
        /// </summary>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Disabled(bool disabled = true);

        /// <summary>
        /// Sets the field to be readonly (value can not be modified).
        /// </summary>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Readonly(bool @readonly = true);

        /// <summary>
        /// Sets a hint to the user of what can be entered in the field.
        /// </summary>
        /// <param name="placeholderText">The text to use for the placeholder</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Placeholder(string placeholderText);

        /// <summary>
        /// Gets any text that has been set for an inline label.
        /// </summary>
        IHtmlString InlineLabelText { get; }

        /// <summary>
        /// Sets an inline label for a checkbox.
        /// </summary>
        /// <param name="labelText">The text to use for the label</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration InlineLabel(string labelText);

        /// <summary>
        /// Sets an inline label for a checkbox.
        /// </summary>
        /// <param name="labelHtml">The html to use for the label</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration InlineLabel(IHtmlString labelHtml);

        /// <summary>
        /// Gets any text that has been set for the label.
        /// </summary>
        IHtmlString LabelText { get; }

        /// <summary>
        /// Override the default label for the field.
        /// </summary>
        /// <param name="labelText">The text to use for the label</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Label(string labelText);

        /// <summary>
        /// Override the default label for the field.
        /// </summary>
        /// <param name="labelHtml">The text to use for the label</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Label(IHtmlString labelHtml);

        /// <summary>
        /// Returns the display type for the field.
        /// </summary>
        FieldDisplayType DisplayType { get; }

        /// <summary>
        /// Renders the field as a list of radio options for selecting single values or checkbox items for selecting multiple values.
        /// Use for a list or boolean value.
        /// </summary>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        /// <seealso cref="AsCheckboxList"/>
        IFieldConfiguration AsRadioList();

        /// <summary>
        /// Renders the field as a list of radio options for selecting single values or checkbox items for selecting multiple values.
        /// Use for a list or boolean value.
        /// </summary>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        /// <seealso cref="AsRadioList"/>
        IFieldConfiguration AsCheckboxList();

        /// <summary>
        /// Renders the field as a drop-down control.
        /// Use for a list or boolean value.
        /// </summary>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration AsDropDown();

        /// <summary>
        /// The label that represents true.
        /// </summary>
        string TrueString { get; }

        /// <summary>
        /// Change the label that represents true.
        /// </summary>
        /// <param name="trueString">The label to use as true</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration WithTrueAs(string trueString);

        /// <summary>
        /// The label that represents false.
        /// </summary>
        string FalseString { get; }

        /// <summary>
        /// Change the label that represents none.
        /// </summary>
        /// <param name="noneString">The label to use as none</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration WithNoneAs(string noneString);
        
        /// <summary>
        /// The label that represents none.
        /// </summary>
        string NoneString { get; }

        /// <summary>
        /// Change the label that represents false.
        /// </summary>
        /// <param name="falseString">The label to use as false</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration WithFalseAs(string falseString);
        
        /// <summary>
        /// Sets a lambda expression to get the field that the field configuration is wrapping so that
        ///     a call to ToHtmlString() will output the given field.
        /// </summary>
        /// <param name="field">A lambda returning the HTML to output</param>
        void SetField(Func<IHtmlString> field);

        /// <summary>
        /// Sets the field that the field configuration is wrapping so that
        ///     a call to ToHtmlString() will output the given field.
        /// </summary>
        /// <param name="field">The field being configured</param>
        void SetField(IHtmlString field);

        /// <summary>
        /// Supply a string hint to display along with the field.
        /// </summary>
        /// <param name="hint">The hint string</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration WithHint(string hint);

        /// <summary>
        /// Supply a HTML hint to display along with the field.
        /// </summary>
        /// <param name="hint">The hint markup</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration WithHint(IHtmlString hint);

        /// <summary>
        /// Get the hint to display with the field.
        /// </summary>
        IHtmlString Hint { get; }

        /// <summary>
        /// Prepends the given HTML to the form field.
        /// </summary>
        /// <param name="html">The HTML to prepend</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Prepend(IHtmlString html);

        /// <summary>
        /// Prepends the given string to the form field.
        /// </summary>
        /// <param name="str">The string to prepend</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Prepend(string str);

        /// <summary>
        /// A list of HTML to be prepended to the form field in ltr order.
        /// </summary>
        IEnumerable<IHtmlString> PrependedHtml { get; }

        /// <summary>
        /// Appends the given HTML to the form field.
        /// </summary>
        /// <param name="html">The HTML to append</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Append(IHtmlString html);

        /// <summary>
        /// Appends the given string to the form field.
        /// </summary>
        /// <param name="str">The string to prepend</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration Append(string str);

        /// <summary>
        /// A list of HTML to be appended to the form field in ltr order.
        /// </summary>
        IEnumerable<IHtmlString> AppendedHtml { get; }

        /// <summary>
        /// Override the HTML of the form field.
        /// 
        /// This gives you ultimate flexibility with your field HTML when it's
        /// not quite what you want, but you still want the form template
        /// (e.g. label, surrounding html and validation message).
        /// </summary>
        /// <param name="html">The HTML for the field</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration OverrideFieldHtml(IHtmlString html);

        /// <summary>
        /// The HTML to be used as the field html.
        /// </summary>
        IHtmlString FieldHtml { get; }
        
        /// <summary>
        /// Uses the given format string when outputting the field value.
        /// </summary>
        /// <param name="formatString">The format string to use</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration WithFormatString(string formatString);

        /// <summary>
        /// The format string to use for the field.
        /// </summary>
        string FormatString { get; }

        /// <summary>
        /// Hide the empty item that would normally display for the field.
        /// </summary>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration HideEmptyItem();

        /// <summary>
        /// Whether or not the empty item is hidden.
        /// </summary>
        bool EmptyItemHidden { get; }

        /// <summary>
        /// Don't use a &lt;label&gt;, but still include the label text for the field.
        /// </summary>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration WithoutLabel();

        /// <summary>
        /// Whether or not to use a &lt;label&gt;.
        /// </summary>
        bool HasLabel { get; }

        /// <summary>
        /// Specify one or more CSS classes to use for the field label.
        /// </summary>
        /// <param name="class">Any CSS class(es) to use for the field label</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration AddLabelClass(string @class);

        /// <summary>
        /// Any CSS class(es) to use for the field label.
        /// </summary>
        string LabelClasses { get; }

        /// <summary>
        /// Specify one or more CSS classes to use for the field container element.
        /// </summary>
        /// <param name="class">Any CSS class(es) to use for the field container element</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration AddFieldContainerClass(string @class);

        /// <summary>
        /// Any CSS class(es) to use for the field container element.
        /// </summary>
        string FieldContainerClasses { get; }

        /// <summary>
        /// Specify one or more CSS classes to use for the field validation message.
        /// </summary>
        /// <param name="class">Any CSS class(es) to use for the field validation message</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration AddValidationClass(string @class);

        /// <summary>
        /// Any CSS class(es) to use for the field validation message.
        /// </summary>
        string ValidationClasses { get; }

        /// <summary>
        /// Returns readonly field configuration from the current field configuration.
        /// </summary>
        /// <returns>A readonly field configuration</returns>
        IReadonlyFieldConfiguration ToReadonly();

        /// <summary>
        /// Enum value(s) to exclude from the generated field.
        /// </summary>
        Enum[] ExcludedEnums { get; }

        /// <summary>
        /// 
        /// </summary>
        /// <returns></returns>
        IFieldConfiguration WithoutInlineLabel();

        /// <summary>
        /// Whether or not to use an inline &lt;label&gt;.
        /// </summary>
        bool HasInlineLabel { get; }

        /// <summary>
        /// Specify that inline labels should wrap their input element. Important for bootstrap.
        /// </summary>
        /// <param name="wrapElement">True if the input element should be wrapped.</param>
        /// <returns>The <see cref="IFieldConfiguration"/> to allow for method chaining</returns>
        IFieldConfiguration InlineLabelWrapsElement(bool wrapElement = true);
    }
```

The `IReadonlyFieldConfiguration` interface can be created by calling the `ToReadonly()` method on the `IFieldConfiguration`; it is in the `ChameleonForms.Component.Config` namespace and looks like this:

```csharp
    /// <summary>
    /// Immutable field configuration for use when generating a field's HTML.
    /// </summary>
    public interface IReadonlyFieldConfiguration
    {
        /// <summary>
        /// A dynamic bag to allow for custom extensions using the field configuration.
        /// </summary>
        dynamic Bag { get; }

        /// <summary>
        /// Returns data from the Bag stored in the given property or default(TData) if there is none present.
        /// </summary>
        /// <typeparam name="TData">The type of the expected data to return</typeparam>
        /// <param name="propertyName">The name of the property to retrieve the data for</param>
        /// <returns>The data from the Bag or default(TData) if there was no data against that property in the bag</returns>
        TData GetBagData<TData>(string propertyName);

        /// <summary>
        /// Attributes to add to the form element's HTML.
        /// </summary>
        // todo: consider making this a readonly dictionary
        IDictionary<string, object> HtmlAttributes { get; }

        /// <summary>
        /// Gets any text that has been set for an inline label.
        /// </summary>
        IHtmlString InlineLabelText { get; }

        /// <summary>
        /// Gets any text that has been set for the label.
        /// </summary>
        IHtmlString LabelText { get; }

        /// <summary>
        /// Returns the display type for the field.
        /// </summary>
        FieldDisplayType DisplayType { get; }

        /// <summary>
        /// The label that represents true.
        /// </summary>
        string TrueString { get; }

        /// <summary>
        /// The label that represents false.
        /// </summary>
        string FalseString { get; }

        /// <summary>
        /// The label that represents none.
        /// </summary>
        string NoneString { get; }

        /// <summary>
        /// Get the hint to display with the field.
        /// </summary>
        IHtmlString Hint { get; }

        /// <summary>
        /// A list of HTML to be prepended to the form field in ltr order.
        /// </summary>
        IEnumerable<IHtmlString> PrependedHtml { get; }

        /// <summary>
        /// A list of HTML to be appended to the form field in ltr order.
        /// </summary>
        IEnumerable<IHtmlString> AppendedHtml { get; }

        /// <summary>
        /// The HTML to be used as the field html.
        /// </summary>
        IHtmlString FieldHtml { get; }

        /// <summary>
        /// The format string to use for the field.
        /// </summary>
        string FormatString { get; }

        /// <summary>
        /// Whether or not the empty item is hidden.
        /// </summary>
        bool EmptyItemHidden { get; }

        /// <summary>
        /// Whether or not to use a &lt;label&gt;.
        /// </summary>
        bool HasLabel { get; }

        /// <summary>
        /// Any CSS class(es) to use for the field label.
        /// </summary>
        string LabelClasses { get; }

        /// <summary>
        /// Any CSS class(es) to use for the field container element.
        /// </summary>
        string FieldContainerClasses { get; }

        /// <summary>
        /// Any CSS class(es) to use for the field validation message.
        /// </summary>
        string ValidationClasses { get; }

        /// <summary>
        /// Enum value(s) to exclude from the generated field.
        /// </summary>
        Enum[] ExcludedEnums { get; }

        /// <summary>
        /// Whether or not to use an inline &lt;label&gt;.
        /// </summary>
        bool HasInlineLabel { get; }

        /// <summary>
        /// Whether or not inline &lt;label&gt; should wrap their &lt;input&gt; element.
        /// </summary>
        bool ShouldInlineLabelWrapElement { get; }
    }
```

The xmldoc comments above should give a pretty good indication of how each of those methods are meant to be used. There is further documentation about the effect of the different methods in the following documentation:

* [Field](field)
* [Field Label](field-label)
* [Field Types](./#field-types)

How does the IFieldConfiguration output the Field HTML?
-------------------------------------------------------

The astute viewer will notice that the various `FieldFor`, `FieldElementFor`, `LabelFor` and `ValidationMessageFor` extension methods all return an `IFieldConfiguration` as opposed to a `string` or `IHtmlString`, yet when prefixed  with `@` (with or without chaining any Field Configuration methods) will always output the correct HTML.

This works because:

* The `IFieldConfiguration` interface extends `IHtmlString`, which forces it to implement the `.ToHtmlString()` method (which will be called by razor via the `@` operator)
* All the methods on `IFieldConfiguration` return the same instance of the `IFieldConfiguration` object so the `@` operator will apply to that Field Configuration regardless of what methods the user calls
* The `SetField(IHtmlString)` method or the `SetField(Func<IHtmlString>)` method will be called before returning the `IFieldConfiguration` to indicate what HTML should be output by the Field Configuration when the `.ToHtmlString()` method is called
* The `SetField` method approach allows for lazy evaluation of the HTML to output, meaning the HTML generation can occur after all of the `IFieldConfiguration` methods have been called (allowing the Field Configuration to be mutated before eventually being used)

Passing HTML to field configuration methods
-------------------------------------------

For all the field configuration methods that take an `IHtmlString` you have a few options available to you:

* Pass the HTML as a string e.g. `.Label(new HtmlString("<strong>My label</strong>"))`
* Pass the HTML by calling any method that returns an `IHtmlString` including a razor helper e.g.: `.Label(GetLabelHtml())`
```
    @helper GetLabelHtml() {
		<strong>My label</strong>
	}
```
* Passing the HTML inline using the `Func<object, IHtml>` extension method overloads e.g. `.Label(@<strong>My label</strong>)`
