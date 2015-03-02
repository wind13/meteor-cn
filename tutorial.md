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

### Inserting tasks from the console
### 从控制台添加任务

Items inside collections are called documents. Let's use the server database console to insert some documents into our collection. In a new terminal tab, go to your app directory and type:

在集合里面的项目叫文档。让我们用服务端的数据库控制台给集合里添加一些文档。在一个新的终端里，进入你的 App 目录并且键入：

```
meteor mongo
```

This opens a console into your app's local development database. Into the prompt, type:

这会打开一个控制台进入到你的 App 的本地开发数据库。在命令行里键入：

```
db.tasks.insert({ text: "Hello world!", createdAt: new Date() });
```

In your web browser, you will see the UI of your app immediately update to show the new task. You can see that we didn't have to write any code to connect the server-side database to our front-end code — it just happened automatically.

在你的网页浏览器，你会看到你的 App 马上就显示了这些新任务。你能看到我们不需要写什么代码就连接到服务端的数据库到前端的代码了，它就是这样自动发生了。

Insert a few more tasks from the database console with different text. In the next step, we'll see how to add functionality to our app's UI so that we can add tasks without using the database console.

再插入一些不同的任务到数据库。在下一节，我们将学习如何不使用数据库控制台而是通过我们的 App 界面添加任务的功能。

#### See the code for step 3 on GitHub!
  * [simple-todos.html](https://github.com/meteor/simple-todos/blob/ab14d317db174a3b2d408f4e24a50c7af13342cd/simple-todos.html)
  * [simple-todos.js](https://github.com/meteor/simple-todos/blob/ab14d317db174a3b2d408f4e24a50c7af13342cd/simple-todos.js)
  * [simple-todos.css](https://github.com/meteor/simple-todos/blob/ab14d317db174a3b2d408f4e24a50c7af13342cd/simple-todos.css)
  * [diff of all files](https://github.com/meteor/simple-todos/commit/ab14d317db174a3b2d408f4e24a50c7af13342cd)

### Adding tasks with a form

In this step, we'll add an input field for users to add tasks to the list.

在这一节，我们要添加一个输入框架用来添加任务到列表中。

First, let's add a form to our HTML:

首先，让我们添加一个 Form 表单到我们的 HTML 中：

```
<header>
  <h1>Todo List</h1>

  <!-- add a form below the h1 -->
  <form class="new-task">
    <input type="text" name="text" placeholder="Type to add new tasks" />
  </form>
</header>
```
Here's the JavaScript code we need to add to listen to the submit event on the form:

这些是 JavaScript 代码，我们需要在这个 Form 上添加监听提交事件：

```
// Inside the if (Meteor.isClient) block, right after Template.body.helpers:
Template.body.events({
  "submit .new-task": function (event) {
    // This function is called when the new task form is submitted

    var text = event.target.text.value;

    Tasks.insert({
      text: text,
      createdAt: new Date() // current time
    });

    // Clear form
    event.target.text.value = "";

    // Prevent default form submit
    return false;
  }
});
```

Now your app has a new input field. To add a task, just type into the input field and hit enter. If you open a new browser window and open the app again, you'll see that the list is automatically synchronized between all clients.

现在你的 App 有一个新的输入框了。要添加一个任务，只需要在输入框中打字然后按回车就行了。如果你打开一个新的浏览器窗口并且打开这个 App，你就会看到那个列表自动在所有客户端之间同步了。

### Attaching events to templates
### 附加事件到模板中

Event listeners are added to templates in much the same way as helpers are: by calling `Template.templateName.events(...)` with a dictionary. The keys describe the event to listen for, and the values are event handlers that are called when the event happens.

事件监听添加到模板的办法几乎是一样的：通过调用 `Template.templateName.events(...)` 里面放一个 dictionary（键值对）。其中 key 键指的是所监听的事件名称，而值里面放的是事件发生后所要执行的方法。

In our case above, we are listening to the submit event on any element that matches the CSS selector .new-task. When this event is triggered by the user pressing enter inside the input field, our event handler function is called.

在我们上面的例子中，我们监听了匹配这个 CSS 选择器 .new-task 的提交事件。当这些事件被用户在输入框中敲回车键时被触发，我们的事件响应方法就会被执行。

The event handler gets an argument called event that has some information about the event that was triggered. In this case event.target is our form element, and we can get the value of our input with event.target.text.value. You can see all of the other properties of the event object by adding a `console.log(event)` and inspecting the object in your browser console.

接收事件的方法收到一个叫 event 的参数，里面包含触发的这个事件的一些信息。在这个例子里 event.target 就是我们的 form 对象，并且我们可以通过 event.target.text.value 来取得我们输入的值。你可以通过在你的浏览器控制台里添加一个 `console.log(event)` 来仔细观察这个事件对象的其他属性。

The last two lines of our event handler perform some cleanup — first we make sure to make the input blank, and then we return false to tell the web browser to not do the default form submit action since we have already handled it.

那接收事件的方法最后两行执行了一些清除工作——首先我们清空了输入框，其次我们返回了 false 告诉网页浏览器不要执行默认的表单提交动作，因为我们已经处理它了。

### Inserting into a collection
### 添加到集合

Inside the event handler, we are adding a task to the tasks collection by calling `Tasks.insert()`. We can assign any properties to the task object, such as the time created, since we don't ever have to define a schema for the collection.

在那个事件处理方法中，我们通过`Tasks.insert()`添加了一个任务到任务集合中。我们可以给这个任务对象附加任意属性，比如创建的时间，因为我们不需要给MongoDB的集体定义什么表结构。

Being able to insert anything into the database from the client isn't very secure, but it's okay for now. In step 10 we'll learn how we can make our app secure and restrict how data is inserted into the database.

虽然能从客户端插入东西到数据库不太安全，但目前来说是Okay的。在第10节我们将学习如何使我们的App更安全以及如何限定插入数据库的数据等。

### Sorting our tasks
### 对我们的任务列表进行排序

Currently, our code displays all new tasks at the bottom of the list. That's not very good for a task list, because we want to see the newest tasks first.

现在，我们的代码已经可以将最新的任务显示在列表的下方。但这对于任务列表来说并不好，因为我们总是希望先看到最新的任务。

We can solve this by sorting the results using the createdAt field that is automatically added by our new code. Just add a sort option to the find call inside the tasks helper:

我们可以通过让列表按（代码自动添加的）创建时间倒序来解决这个问题，只需要添加一个sort的选项到查询方法中：

```
Template.body.helpers({
  tasks: function () {
    // Show newest tasks first
    return Tasks.find({}, {sort: {createdAt: -1}});
  }
});
```

In the next step, we'll add some very important todo list functions: checking off and deleting tasks.

在下一节中，我们将添加非常有用的功能：完成任务和删除任务。

#### See the code for step 4 on GitHub!
  * [simple-todos.html](https://github.com/meteor/simple-todos/blob/9c24b998e540848f0dbc241702a4fcfa48fb9087/simple-todos.html)
  * [simple-todos.js](https://github.com/meteor/simple-todos/blob/9c24b998e540848f0dbc241702a4fcfa48fb9087/simple-todos.js)
  * [simple-todos.css](https://github.com/meteor/simple-todos/blob/9c24b998e540848f0dbc241702a4fcfa48fb9087/simple-todos.css)
  * [diff of all files](https://github.com/meteor/simple-todos/commit/9c24b998e540848f0dbc241702a4fcfa48fb9087)

### Checking off and deleting tasks

### Deploying your app

### Running your app on Android or iOS

### Storing temporary UI state in Session

### Adding user accounts
### 添加用户账号

Meteor comes with an accounts system and a drop-in login user interface that lets you add multi-user functionality to your app in minutes.

Meteor 有一个账户系统和一个内置的用户登录界面，从而可以让你分分钟添加多个用户的功能到App中。

To enable the accounts system and UI, we need to add the relevant packages. In your app directory, run the following command:

为了使用账户系统和界面，我们需要添加相应的包。在你的App目录下，运行如下命令：

```
meteor add accounts-ui accounts-password
```

In the HTML, right under the checkbox, include the following code to add a login dropdown:

在那个HTML的右边checkbox下面，添加如下代码来加上一个登录下拉选项：

```
{{> loginButtons}}
```

Then, in your JavaScript, add the following code to configure the accounts UI to use usernames instead of email addresses:

然后，在你的JavaScript，添加下面的代码来配置登录界面使用usernames而不是email地址：

```
// At the bottom of the client code
Accounts.ui.config({
  passwordSignupFields: "USERNAME_ONLY"
});
```

Now users can create accounts and log into your app! This is very nice, but logging in and out isn't very useful yet. Let's add two functions:

现在用户可以创建账户并且登录到你的App了！很爽吧，但登录和登出目前还没什么用。让我们加两个方法：

  1. Only display the new task input field to logged in users
  2. Show which user created each task

  1. 只给登录的用户显示新任务的文本框。
  2. 显示每个任务是哪个用户创建的。

To do this, we will add two new fields to the tasks collection:

为了实现它，我们将添加两个新字段到tasks的集合中：

  1. owner - the _id of the user that created the task.
  2. username - the username of the user that created the task. We will save the username directly in the task object so that we don't have to look up the user every time we display the task.

  1. owner - 创建任务的用户的_id。
  2. username - 创建任务的用户的username。我们将直接保存username到task对象中，这样我们就不用每次显示任务时再去查找user了。

First, let's add some code to save these fields into the submit .new-task event handler:

首先，让我们在提交.new-task的事件响应中添加一些代码保存这些字段：

```
Tasks.insert({
  text: text,
  createdAt: new Date(),            // current time
  owner: Meteor.userId(),           // _id of logged in user
  username: Meteor.user().username  // username of logged in user
});
```

Then, in our HTML, add an #if block helper to only show the form when there is a logged in user:

然后，在我们的HTML中，添加一个 ``#if`` 块，只有用户登录后才显示这个表单：

```
{{#if currentUser}}
  <form class="new-task">
    <input type="text" name="text" placeholder="Type to add new tasks" />
  </form>
{{/if}}
```

Finally, add a Spacebars statement to display the username field on each task right before the text:

最后，添加一个Spacebars声明在任务右边显示用户名字段：

```
<span class="text"><strong>{{username}}</strong> - {{text}}</span>
```

Now, users can log in and we can track which user each task belongs to. Let's look at some of the concepts we just discovered in more detail.

现在，用户可以登录并且我们可以知道每个任务属于哪个用户。让我们再仔细看一下刚才这些概念。



### Security with methods
### 用methods解决权限问题

Before this step, any user of the app could edit any part of the database. This might be okay for very small internal apps or demos, but any real application needs to control permissions for its data. In Meteor, the best way to do this is by declaring methods. Instead of the client code directly calling insert, update, and remove, it will instead call methods that will check if the user is authorized to complete the action and then make any changes to the database on the client's behalf.

在这一步之前，App的用户可以编辑任何数据库的任何部分。这在非常小的内部App或演示项目中还可以，但在实际应用中就需要控制数据权限。在Meteor，最好的办法就是通过声明methods。而不是客户端代码直接调用插入（insert），更新（update），和删除（delete），它将通过调用methods来检查用户是否被授权完成相应的动作，然后更改客户所属的数据库。

#### Removing ``insecure``
#### 移除``insecure``

Every newly created Meteor project has the insecure package added by default. This is the package that allows us to edit the database from the client. It's useful when prototyping, but now we are taking off the training wheels. To remove this package, go to your app directory and run:

每个新创建的Meteor项目都默认加入了insecure包。有了这个包才使得我们通过客户端去修改数据库。这在做原型时很有用，但现在我们要进入更深入的层次了。要移除这个包，进入到App目录并运行：

```
meteor remove insecure
```

If you try to use the app after removing this package, you will notice that none of the inputs or buttons work anymore. This is because all client-side database permissions have been revoked. Now we need to rewrite some parts of our app to use methods.

如果你移除这个包后尝试运行App，你会发现所有的输入框或按钮都不能用了。这是因为所有客户端数据库权限已经被吊销了。现在我们需要使用methods重写我们App的相应代码了。

#### Defining methods
#### 定义methods

First, we need to define some methods. We need one method for each database operation we want to perform on the client. Methods should be defined in code that is executed on the client and the server - we will discuss this a bit later in the section titled `Latency compensation`.

首先，我们需要定义一些methods。我们需要给每一个想从客户端执行的数据库操作定义一个method。Methods应该定义在客户端和服务端需要执行的代码中——我们会在稍后的“延迟补偿”章节中讨论它。

```
// At the bottom of simple-todos.js, outside of the client-only block
Meteor.methods({
  addTask: function (text) {
    // Make sure the user is logged in before inserting a task
    if (! Meteor.userId()) {
      throw new Meteor.Error("not-authorized");
    }

    Tasks.insert({
      text: text,
      createdAt: new Date(),
      owner: Meteor.userId(),
      username: Meteor.user().username
    });
  },
  deleteTask: function (taskId) {
    Tasks.remove(taskId);
  },
  setChecked: function (taskId, setChecked) {
    Tasks.update(taskId, { $set: { checked: setChecked} });
  }
});
```

Now that we have defined our methods, we need to update the places we were operating on the collection to use the methods instead:

现在我们已经定义了我们的methods，我们需要相应地更新操作集合的代码：

```
// replace Tasks.insert( ... ) with:
Meteor.call("addTask", text);

// replace Tasks.update( ... ) with:
Meteor.call("setChecked", this._id, ! this.checked);

// replace Tasks.remove( ... ) with:
Meteor.call("deleteTask", this._id);
```

Now all of our inputs and buttons will start working again. What did we gain from all of this work?

现在我们所有的输入框和按钮又可以用了。那么我们之前这些工作到底有什么用呢？

  1. When we insert tasks into the database, we can now securely verify that the user is logged in, that the ``createdAt`` field is correct, and that the owner and username fields are correct and the user isn't impersonating anyone.
  2. We can add extra validation logic to setChecked and deleteTask in later steps when users can make tasks private.
  3. Our client code is now more separated from our database logic. Instead of a lot of stuff happening inside our event handlers, we now have methods that can be called from anywhere.

  1. 当我们插入任务插入到数据库，我们现在可以安全地验证用户登录，``createdat``字段是正确的，而且老板和用户名域是正确的，用户不模仿任何人。 


### Adding tasks with a form
