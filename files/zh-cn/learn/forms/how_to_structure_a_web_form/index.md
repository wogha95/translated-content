---
title: 如何构造 HTML 表单
slug: Learn/Forms/How_to_structure_a_web_form
---

{{LearnSidebar}}{{PreviousMenuNext("Learn/HTML/Forms/Your_first_HTML_form", "Learn/HTML/Forms/The_native_form_widgets", "Learn/HTML/Forms")}}

有了基础知识，我们现在更详细地了解了用于为表单的不同部分提供结构和意义的元素。

<table class="learn-box standard-table">
  <tbody>
    <tr>
      <th scope="row">前提：</th>
      <td>
        基本的计算机能力，和基本的
        <a href="/zh-CN/docs/Learn/HTML/Introduction_to_HTML">对 HTML 的理解</a
        >。
      </td>
    </tr>
    <tr>
      <th scope="row">目标：</th>
      <td>
        要理解如何构造 HTML 表单并赋予它们语义，以便它们是可用的和可访问的。
      </td>
    </tr>
  </tbody>
</table>

HTML 表单的灵活性使它们成为 HTML 中最复杂的结构之一;你可以使用专用的表单元素和属性构建任何类型的基本表单。在构建 HTML 表单时使用正确的结构将有助于确保表单可用性和无障碍。

## \<form> 元素

{{HTMLElement("form")}} 元素按照一定的格式定义了表单和确定表单行为的属性。当你想要创建一个 HTML 表单时，都必须从这个元素开始，然后把所有内容都放在里面。许多辅助技术或浏览器插件可以发现{{HTMLElement("form")}}元素并实现特殊的钩子，使它们更易于使用。

我们早在之前的文章中就遇见过它了。

> **备注：** 严格禁止在一个表单内嵌套另一个表单。嵌套会使表单的行为不可预知，而这取决于正在使用的浏览器。

请注意，在{{HTMLElement("form")}}元素之外使用表单小部件是可以的，但是如果你这样做了，那么表单小部件与任何表单都没有任何关系。这样的小部件可以在表单之外使用，但是你应该对于这些小部件有特别的计划，因为它们自己什么也不做。你将不得不使用 JavaScript 定制他们的行为。

> **备注：** HTML5 在 HTML 表单元素中引入`form`属性。它让你显式地将元素与表单绑定在一起，即使元素不在{{ HTMLElement("form") }}中。不幸的是，就目前而言，跨浏览器对这个特性的实现还不足以使用。

## \<fieldset> 和 \<legend> 元素

{{HTMLElement("fieldset")}}元素是一种方便的用于创建具有相同目的的小部件组的方式，出于样式和语义目的。你可以在`<fieldset>`开口标签后加上一个 {{HTMLElement("legend")}}元素来给{{HTMLElement("fieldset")}} 标上标签。 {{HTMLElement("legend")}}的文本内容正式地描述了{{HTMLElement("fieldset")}}里所含有部件的用途。

许多辅助技术将使用{{HTMLElement("legend")}} 元素，就好像它是相应的 {{HTMLElement("fieldset")}} 元素里每个部件的标签的一部分。例如，在说出每个小部件的标签之前，像[Jaws](http://www.freedomscientific.com/products/fs/jaws-product-page.asp)或[NVDA](http://www.nvda-project.org/)这样的屏幕阅读器会朗读出 legend 的内容。

这里有一个小例子：

```html
<form>
  <fieldset>
    <legend>Fruit juice size</legend>
    <p>
      <input type="radio" name="size" id="size_1" value="small" />
      <label for="size_1">Small</label>
    </p>
    <p>
      <input type="radio" name="size" id="size_2" value="medium" />
      <label for="size_2">Medium</label>
    </p>
    <p>
      <input type="radio" name="size" id="size_3" value="large" />
      <label for="size_3">Large</label>
    </p>
  </fieldset>
</form>
```

> **备注：** 你可以在 [fieldset-legend.html](https://github.com/mdn/learning-area/blob/main/html/forms/html-form-structure/fieldset-legend.html) (你也可以看[预览版](https://mdn.github.io/learning-area/html/forms/html-form-structure/fieldset-legend.html)) 看到该例。

当阅读上述表格时，屏幕阅读器将会读第一个小部件“Fruit juice size small”，“Fruit juice size medium”为第二个，“Fruit juice size large”为第三个。

本例中的用例是最重要的。每当你有一组单选按钮时，你应该将它们嵌套在{{HTMLElement("fieldset")}}元素中。还有其他用例，一般来说，{{HTMLElement("fieldset")}}元素也可以用来对表单进行分段。理想情况下，长表单应该在拆分为多个页面，但是如果表单很长，却必须在单个页面上，那么将以不同的关联关系划分的分段，分别放在不同的 fieldset 里，可以提高可用性。

因为它对辅助技术的影响， {{HTMLElement("fieldset")}} 元素是构建可访问表单的关键元素之一。无论如何，你有责任不去滥用它。如果可能，每次构建表单时，尝试侦听[屏幕阅读器](https://www.nvaccess.org/download/)如何解释它。如果听起来很奇怪，试着改进表单结构。

## \<label> 元素

正如我们在前一篇文章中看到的， {{HTMLElement("label")}} 元素是为 HTML 表单小部件定义标签的正式方法。如果你想构建可访问的表单，这是最重要的元素——当实现的恰当时，屏幕阅读器会连同有关的说明和表单元素的标签一起朗读。以我们在上一篇文章中看到的例子为例：

```html
<label for="name">Name:</label> <input type="text" id="name" name="user_name" />
```

`<label>` 标签与 `<input>` 通过他们各自的`for` 属性和 `id` 属性正确相关联（label 的 for 属性和它对应的小部件的 id 属性），这样，屏幕阅读器会读出诸如“Name, edit text”之类的东西。

如果标签没有正确设置，屏幕阅读器只会读出“Edit text blank”之类的东西，这样会没什么帮助。

注意，一个小部件可以嵌套在它的{{HTMLElement("label")}}元素中，就像这样：

```html
<label for="name">
  Name: <input type="text" id="name" name="user_name" />
</label>
```

尽管可以这样做，但人们认为设置`for`属性才是最好的做法，因为一些辅助技术不理解标签和小部件之间的隐式关系。

### 标签也可点击！

正确设置标签的另一个好处是可以在所有浏览器中单击标签来激活相应的小部件。这对于像文本输入这样的例子很有用，这样你可以通过点击标签，和点击输入区效果一样，来聚焦于它，这对于单选按钮和复选框尤其有用——这种控件的可点击区域可能非常小，设置标签来使它们可点击区域变大是非常有用的。

举个例子：

```html
<form>
  <p>
    <input type="checkbox" id="taste_1" name="taste_cherry" value="1" />
    <label for="taste_1">I like cherry</label>
  </p>
  <p>
    <input type="checkbox" id="taste_2" name="taste_banana" value="2" />
    <label for="taste_2">I like banana</label>
  </p>
</form>
```

> **备注：** 你可以在 [checkbox-label.html](https://github.com/mdn/learning-area/blob/main/html/forms/html-form-structure/checkbox-label.html) (你也可以看[预览版](https://mdn.github.io/learning-area/html/forms/html-form-structure/checkbox-label.html)) 看到该例。

### 多个标签

严格地说，你可以在一个小部件上放置多个标签，但是这不是一个好主意，因为一些辅助技术可能难以处理它们。在多个标签的情况下，你应该将一个小部件和它的标签嵌套在一个{{HTMLElement("label")}}元素中。

让我们考虑下面这个例子：

```html
<p>Required fields are followed by <abbr title="required">*</abbr>.</p>

<!--这样写：-->
<div>
  <label for="username">Name:</label>
  <input type="text" name="username" />
  <label for="username"><abbr title="required">*</abbr></label>
</div>

<!--但是这样写会更好：-->
<div>
  <label for="username">
    <span>Name:</span>
    <input id="username" type="text" name="username" />
    <abbr title="required">*</abbr>
  </label>
</div>

<!--但最好的可能是这样：-->
<div>
  <label for="username">Name: <abbr title="required">*</abbr></label>
  <input id="username" type="text" name="username" />
</div>
```

顶部的段落定义了所需元素的规则。它必须在开始时确保像屏幕阅读器这样的辅助技术在用户找到必需的元素之前显示或念出它们。这样，他们就知道星号表达的是什么意思了。根据屏幕阅读器的设置，屏幕阅读器会把星号读为“star”或“required”，取决于屏幕阅读器的设置——不管怎样，要念出来的都会在第一段清楚的呈现出来。

- 在第一个例子中，标签根本没有和`input`一起被念出来——读出来的只是“edit the blank”，和单独被念出的标签。多个`<label>`元素会使屏幕阅读器迷惑。
- 在第二个例子中，事情变得清晰一点了——标签和输入一起，读出的是“name star name edit text”，但标签仍然是单独读出的。这还是有点令人困惑，但这次还是稍微好一点了，因为`input`和`label`联系起来了。
- 第三个例子是最好的——标签是一起读出的，标签和输入读出的是“name star edit text”。

> **备注：** 你可能会得到一些不同的结果，这取决于你的屏幕阅读器。这是在 VoiceOver 上测试的（NVDA 的行为也类似）。我们也乐于听听你的试验结果。

> **备注：** 你可以在 GitHub 上看到 [required-labels.html](https://github.com/mdn/learning-area/blob/main/html/forms/html-form-structure/required-labels.html)（你也可以看[预览版](https://mdn.github.io/learning-area/html/forms/html-form-structure/required-labels.html)）。不要运行 2 个或 3 个未注释版本的示例——如果你有多个标签和多个输入相同的 ID，那么屏幕阅读器肯定会感到困惑！

## 用于表单的通用 HTML 结构

除了特定于 HTML 表单的结构之外，还应该记住表单同样是 HTML。这意味着你可以使用 HTML 的所有强大功能来构造一个 HTML 表单。

正如你在示例中可以看到的，用{{HTMLElement("div")}}元素包装标签和它的小部件是很常见的做法。{{HTMLElement("p")}}元素也经常被使用，HTML 列表也是如此（后者在构造多个复选框或单选按钮时最为常见）。

除了{{HTMLElement("fieldset")}}元素之外，使用 HTML 标题（例如，{{htmlelement("h1")}}、{{htmlelement("h2")}}）和分段（如{{htmlelement("section")}}）来构造一个复杂的表单也是一种常见的做法。

最重要的是，你要找到一种你觉得很舒服的风格去码代码，而且它也能带来可访问的、可用的形式。

它包含了从功能上划分开并分别包含在{{htmlelement("section")}}元素中的部分，以及一个{{htmlelement("fieldset")}}来包含单选按钮。

### 自主学习：构建一个表单结构

让我们把这些想法付诸实践，建立一个稍微复杂一点的表单结构——一个支付表单。这个表单将包含许多你可能还不了解的小部件类型—现在不要担心这个；在下一篇文章（[原生表单小部件](/zh-CN/docs/Learn/HTML/Forms/The_native_form_widgets)）中，你将了解它们是如何工作的。现在，当你遵循下面的指令时，请仔细阅读这些描述，并开始理解我们使用的包装器元素是如何构造表单的，以及为什么这么做。

1. 在开始之前，在计算机上的一个新目录中，创建一个[空白模板文件](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html)和[我们的支付表单的 CSS 样式](https://github.com/mdn/learning-area/blob/main/html/forms/html-form-structure/payment-form.css)的本地副本。
2. 首先，通过添加下面这行代码到你的 HTML{{htmlelement("head")}}使你的 HTML 应用 CSS。

   ```html
   <link href="payment-form.css" rel="stylesheet" />
   ```

3. 接下来，通过添加外部{{htmlelement("form")}}元素来开始一张表单：

   ```html
   <form></form>
   ```

4. 在 `<form>` 标签内，以添加一个标题和段落开始，告诉用户必需的字段是如何标记的：

   ```html
   <h1>Payment form</h1>
   <p>
     Required fields are followed by
     <strong><abbr title="required">*</abbr></strong
     >.
   </p>
   ```

5. 接下来，我们将在表单中添加一个更大的代码段，在我们之前的代码下面。在这里，你将看到，我们正在将联系人信息字段包装在一个单独的{{htmlelement("section")}}元素中。此外，我们有一组两个单选按钮，每个单选按钮都放在自己的列表中 ({{htmlelement("li")}})) 元素。最后，我们有两个标准文本{{htmlelement("input")}}和它们相关的{{htmlelement("label")}}元素，每个元素包含在{{htmlelement("p")}}中，加上输入密码的密码输入。现在将这些代码添加到你的表单中：

   ```html
   <section>
     <h2>Contact information</h2>
     <fieldset>
       <legend>Title</legend>
       <ul>
         <li>
           <label for="title_1">
             <input type="radio" id="title_1" name="title" value="K" />
             King
           </label>
         </li>
         <li>
           <label for="title_2">
             <input type="radio" id="title_2" name="title" value="Q" />
             Queen
           </label>
         </li>
         <li>
           <label for="title_3">
             <input type="radio" id="title_3" name="title" value="J" />
             Joker
           </label>
         </li>
       </ul>
     </fieldset>
     <p>
       <label for="name">
         <span>Name: </span>
         <strong><abbr title="required">*</abbr></strong>
       </label>
       <input type="text" id="name" name="username" />
     </p>
     <p>
       <label for="mail">
         <span>E-mail: </span>
         <strong><abbr title="required">*</abbr></strong>
       </label>
       <input type="email" id="mail" name="usermail" />
     </p>
     <p>
       <label for="pwd">
         <span>Password: </span>
         <strong><abbr title="required">*</abbr></strong>
       </label>
       <input type="password" id="pwd" name="password" />
     </p>
   </section>
   ```

6. 现在，我们将转到表单的第二个`<section>`——支付信息。在这里，我们有三个不同的小部件以及它们的标签，每个都包含在一个`<p>`中。第一个是选择信用卡类型的下拉菜单 ({{htmlelement("select")}})。第二个是输入一个信用卡号的类型编号的 `<input>` 元素。最后一个是输入`date`类型的`<input>` 元素，用来输入卡片的过期日期（这将在支持的浏览器中出现一个日期选择器小部件，并在非支持的浏览器中回退到普通的文本输入）。同样，在之前的代码后面输入以下内容：

   ```html
   <section>
     <h2>Payment information</h2>
     <p>
       <label for="card">
         <span>Card type:</span>
       </label>
       <select id="card" name="usercard">
         <option value="visa">Visa</option>
         <option value="mc">Mastercard</option>
         <option value="amex">American Express</option>
       </select>
     </p>
     <p>
       <label for="number">
         <span>Card number:</span>
         <strong><abbr title="required">*</abbr></strong>
       </label>
       <input type="number" id="number" name="cardnumber" />
     </p>
     <p>
       <label for="date">
         <span>Expiration date:</span>
         <strong><abbr title="required">*</abbr></strong>
         <em>formatted as mm/yy</em>
       </label>
       <input type="date" id="date" name="expiration" />
     </p>
   </section>
   ```

7. 我们要添加的最后一个部分要简单得多，它只包含了一个`submit`类型的 {{htmlelement("button")}} ，用于提交表单数据。现在把这个添加到你的表单的底部：

   ```html
   <p><button type="submit">Validate the payment</button></p>
   ```

你可以在下面看到已完成的表单 (你可以在 Github 上看到[源码](https://github.com/mdn/learning-area/blob/main/html/forms/html-form-structure/payment-form.html)和[预览版](https://mdn.github.io/learning-area/html/forms/html-form-structure/payment-form.html)）：

{{EmbedLiveSample("自主学习：构建一个表单结构","100%",620)}}

## 总结

现在，你已经具备了正确地构造 HTML 表单所需的所有知识;下一篇文章将深入介绍各种不同类型的表单小部件，你将希望从用户那里收集信息。

## 参见

- [A List Apart: _Sensible Forms: A Form Usability Checklist_](http://www.alistapart.com/articles/sensibleforms/)

{{PreviousMenuNext("Learn/HTML/Forms/Your_first_HTML_form", "Learn/HTML/Forms/The_native_form_widgets", "Learn/HTML/Forms")}}
