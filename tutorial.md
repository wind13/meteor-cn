# Meteor Tutorial
# Meteor 教程

## Installing Meteor
## 安装 Meteor

Install the newest version of Meteor with one command on OS X or Linux. Open your terminal and type:

在 OS X 或 Linux 系统中只需要一个命令就可以安装最新版的 Meteor 了。打开你的控制终端键入：

```
curl https://install.meteor.com/ | sh
```

The official installer supports Mac OS X 10.6 (Snow Leopard) and above, and Linux on x86 and x86_64 architectures. Using Windows?

官方的安装支持 Mac OS X 10.6 以上版本，和 Linux x86 和 x86_64 架构。[使用 Windows？](https://www.meteor.com/install#)

Now that you've installed Meteor, check out the tutorial that teaches you how to build a collaborative todo list app while showing you Meteor's most exciting and useful features. You can also read about the design of the Meteor platform or check out the complete documentation.

现在你已经安装了 Meteor，学习这个[教程](https://www.meteor.com/try)，它会告诉你怎么建立一个协作的 ToDo 列表 App，并展示 Meteor 最令人激动和有用的功能。你也可以获取[全部的文档](https://docs.meteor.com/)来学习 [Meteor 平台的设计](https://www.meteor.com/projects)。

## Creating your first app
## 创建你第一个 App

To create a Meteor app, open your terminal and type:

要创建一个 Meteor App，打开控制终端键入：

```
meteor create simple-todos
```

This will create a new folder called simple-todos with all of the files that a Meteor app needs:

这将会创建一个叫 simple-todos 的目录，里面是这个 Meteor App 所需要的所有文件：

```
simple-todos.js       # a JavaScript file loaded on both client and server
simple-todos.html     # an HTML file that defines view templates
simple-todos.css      # a CSS file to define your app's styles
.meteor               # internal Meteor files
```

To run the newly created app:

要运行这个新创建的 App：

```
cd simple-todos
meteor
```

Open your web browser and go to http://localhost:3000 to see the app running.

打开你的浏览器到http://localhost:3000就可以看到运行的 App 了。

You can play around with this default app for a bit before we continue. For example, try editing the text in ``<h1>`` inside simple-todos.html using your favorite text editor. When you save the file, the page in your browser will automatically update with the new content. We call this "hot code push".

你可以在继续之前到处看看这个默认的 App。比如，试着使用你喜欢的文本编辑器修改一下``<h1>``里面的文字。当你保存这文件时，浏览器里的这个页面就会自动更新为新的内容。我们称之为“热代码推送”。

Now that you have some experience editing the files in your Meteor app, let's start working on a simple todo list application.

现在你已经有些在 Meteor App 里修改文件的经验了，让我们开始做一个简单的 ToDo 列表应用吧。

#### See the code for step 1 on GitHub!
  * [simple-todos.html](https://github.com/meteor/simple-todos/blob/377a8610c2fa77056d015e6998d5eb894436c99e/simple-todos.html)
  * [simple-todos.js](https://github.com/meteor/simple-todos/blob/377a8610c2fa77056d015e6998d5eb894436c99e/simple-todos.js)
  * [simple-todos.css](https://github.com/meteor/simple-todos/blob/377a8610c2fa77056d015e6998d5eb894436c99e/simple-todos.css)
  * [diff of all files](https://github.com/meteor/simple-todos/commit/377a8610c2fa77056d015e6998d5eb894436c99e)

## Defining views with templates
## 使用模板定义界面

To start working on our todo list app, let's replace the code of the default starter app with the code below. Then we'll talk about what it does.

要开始做我们的 ToDo 列表应用，让我们用下面的代码替换默认的启动 App 的代码。然后我们会讲一下它都做了什么。

```
<!-- simple-todos.html -->
<head>
  <title>Todo List</title>
</head>

<body>
  <div class="container">
    <header>
      <h1>Todo List</h1>
    </header>

    <ul>
      {{#each tasks}}
        {{> task}}
      {{/each}}
    </ul>
  </div>
</body>

<template name="task">
  <li>{{text}}</li>
</template>
```

```
// simple-todos.js
if (Meteor.isClient) {
  // This code only runs on the client
  Template.body.helpers({
    tasks: [
      { text: "This is task 1" },
      { text: "This is task 2" },
      { text: "This is task 3" }
    ]
  });
}
```
In our browser, the app will now look much like this:

在我们的浏览器中，这个 App 应该看起来象这样：

##### Todo List

  * This is task 1
  * This is task 2
  * This is task 3

Now let's find out what all these bits of code are doing!

现在让我们看看这些代码做了什么！


### HTML files in Meteor define templates
### HTML 文件在 Meteor 定义模板

Meteor parses all of the HTML files in your app folder and identifies three top-level tags: ``<head>``, ``<body>``, and `<template>`.

Meteor 转换所有你的 App 目录中的 HTML 文件 并且识别三个顶级标签`<head>`，`<body>`和`<template>`内容。

Everything inside any `<head>` tags is added to the head section of the HTML sent to the client, and everything inside `<body>` tags is added to the body section, just like in a regular HTML file.

所有`<head>`标签中的内容都加入到 HTML 的 head 段发到客户端，所有在`<body>`标签中的内容都加入到 body 段就象普通的 HTML 文件一样。

Everything inside `<template>` tags is compiled into Meteor templates, which can be included inside HTML with {{> templateName}} or referenced in your JavaScript with Template.templateName.

所有放在`<template>`标签的内容都编译到 Meteor 的模板中，这些模板可以在 HTML 中用{{>templateName}}来调用，或者在 JavaScript 中用 Template.templateName 来调用。

### Adding logic and data to templates
### 添加逻辑和数据到模板中

All of the code in your HTML files is compiled with Meteor's Spacebars compiler. Spacebars uses statements surrounded by double curly braces such as {{# each}} and {{#if}} to let you add logic and data to your views.

你的 HTML 文件中的所有代码会通过 Meteor 的 Spacebars 进行编译。Spacebars 使用两个大括号包围的标记如{{# each}} 和 {{#if}} 来让你添加逻辑和数据到界面上。

You can pass data into templates from your JavaScript code by defining helpers. In the code above, we defined a helper called tasks on Template.body that returns an array. Inside the body tag of the HTML, we can use{{#each tasks}} to iterate over the array and insert a task template for each value. Inside the #eachblock, we can display the text property of each array item using {{text}}.

你可以从你的 JavaScript 代码中定义 helpers 来传数据到模板中。上面的代码中，我们在 Template.body 里定义了一个叫 tasks 的模板返回一个数组。在 HTML 的 body 标签中，我们可以使用 {{#each tasks}} 循环那个数组并且每一个值对应插入一个 task 模板，在#each 循环中，我们可以使用 {{text}} 显示每个数组项中的 text 属性。

In the next step, we will see how we can use helpers to make our templates display dynamic data from a database collection.

在下一步，我们将看到如何从数据库集合中使用 helpers 使我们的模板显示动态数据。

### Adding CSS
### 添加 CSS

Before we go any further, let's make our app look nice by adding some CSS.

在我们进入后面之前，先让我们添加一些 CSS 让界面好看些。

Since this tutorial is focused on working with HTML and JavaScript, just copy all the CSS code below intosimple-todos.css. This is all the CSS code you will need until the end of the tutorial. The app will still work without the CSS, but it will look much nicer if you add it.

因为这个教程是针对使用 HTML 和 JavaScript的，所以只需拷贝下面这些 CSS 代码到 simple-todos.css 中即可。这些就是直到教程最后所需要的所有 CSS 代码。没有这些 CSS，这个 App 也可以正常工作，但是你加上它会显得更好看些。

#### See the code for step 2 on GitHub!
  * [simple-todos.html](https://github.com/meteor/simple-todos/blob/9f5c65302f1527f25cacaaf275ea5c747d55e405/simple-todos.html)
  * [simple-todos.js](https://github.com/meteor/simple-todos/blob/9f5c65302f1527f25cacaaf275ea5c747d55e405/simple-todos.js)
  * [simple-todos.css](https://github.com/meteor/simple-todos/blob/9f5c65302f1527f25cacaaf275ea5c747d55e405/simple-todos.css)
  * [diff of all files](https://github.com/meteor/simple-todos/commit/9f5c65302f1527f25cacaaf275ea5c747d55e405)


### Storing tasks in a collection
### 将任务保存到一个集合

Collections are Meteor's way of storing persistent data. The special thing about collections in Meteor is that they can be accessed from both the server and the client, making it easy to write view logic without having to write a lot of server code. They also update themselves automatically, so a template backed by a collection will automatically display the most up-to-date data.

Collections 集合是 Meteor 持久化保存数据的方式。Meteor 集合最特别的一点是从服务端和客户端都能被访问到，这样写界面逻辑就变得简单因为不用写很多的服务端代码。同时它们会自动更新自己，所以一个装有集合的模板会即时更新显示。

Creating a new collection is as easy as calling ``MyCollection = new Mongo.Collection("my-collection");`` in your JavaScript. On the server, this sets up a MongoDB collection called my-collection; on the client, this creates a cache connected to the server collection. We'll learn more about the client/server divide in step 12, but for now we can write our code with the assumption that the entire database is present on the client.

在你的 JavaScript 中创建一个集合就象 ``MyCollection = new Mongo.Collection("my-collection");`` 这样简单。在服务端，这会创建一个叫“my-collection”的 MongoDB 集合；在客户端，这会创建一个缓存连接到服务端的集合。我们会在第12节学习更多的客户端/服务端划分的知识，但现在我们假定这些实体数据库存在客户端来写我们的代码。

Let's update our JavaScript code to get our tasks from a collection instead of a static array:

让我们更新我们的 JavaScript 代码让我们的任务来自一个集合而不是一个静态的数组：

```
// simple-todos.js
Tasks = new Mongo.Collection("tasks");

if (Meteor.isClient) {
  // This code only runs on the client
  Template.body.helpers({
    tasks: function () {
      return Tasks.find({});
    }
  });
}
```

When you make these changes to the code, you'll notice that the tasks that used to be in the todo list have disappeared. That's because our database is currently empty — we need to insert some tasks!

当你修改这些代码，你将看到列表中的 ToDo 项目消失了。这是因为我们的数据库现在是空的——我们需要添加一些任务！

