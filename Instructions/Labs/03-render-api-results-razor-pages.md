---
lab:
  title: 练习：在 ASP.NET Core Razor Pages 中呈现 API 响应
  module: 'Module: Render API responses in ASP.NET Core Razor Pages'
---

在本练习中，您将学习如何在 ASP.NET Core Razor Pages 应用程序中添加代码，以呈现 HTTP 操作的结果。 这些代码被添加到 *.cshtml* 文件中。 在 *.cshtml.cs* 文件中执行操作的代码就完成了。

## 目标

完成此练习后，你将能够：

* 在应用程序中实现Razor关键字
* 将 C# 代码与 Razor Pages 语法相结合

## 先决条件

要完成本练习，您需要在系统中安装以下设备：

* [Visual Studio Code](https://code.visualstudio.com)
* [最新的 .NET 7.0 SDK](https://dotnet.microsoft.com/download/dotnet/7.0)
* Visual Studio Code 的 [C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)

**预计练习完成时间**：30 分钟

## 练习场景

本练习有两个组成部分：

* 向 API 发送 HTTP 请求的网络应用程序。 该应用程序在 `http://localhost:5010`
* 响应 HTTP 请求的 API。 API 在 `http://localhost:5050`

![装饰性](media/02-architecture.png)

## 下载代码

在本节中，您将下载 Fruit 网络应用程序和 Fruit API 的代码。 您还将在本地运行 Fruit API，以便网络应用程序可以使用它。

### 任务 1：下载并运行 API 代码

1. 右键单击以下链接并选择**另存链接**选项。 

    * [FruitAPI 项目代码](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitAPI.zip)代码

1. 启动**文件浏览器**并导航到文件保存的位置。

1. 将文件解压缩到它自己的文件夹中。

1. 打开 **Windows 终端**或**命令提示符**，并导航至提取 API 代码的位置。

1. 在 **Windows 终端**窗格中运行以下`dotnet`命令：

    ```
    dotnet run
    ```

1. 以下是生成的输出示例。 请注意`Now listening on: http://localhost:5050`输出中的一行。 它标识了 API 的主机和端口。

    ```
    info: Microsoft.EntityFrameworkCore.Update[30100]
          Saved 3 entities to in-memory store.
    info: Microsoft.Hosting.Lifetime[14]
          Now listening on: http://localhost:5050
    info: Microsoft.Hosting.Lifetime[0]
          Application started. Press Ctrl+C to shut down.
    info: Microsoft.Hosting.Lifetime[0]
          Hosting environment: Development
    info: Microsoft.Hosting.Lifetime[0]
          Content root path: 
          <project location>
    ```

>**注意：** 在本练习的其余部分，让 Fruit API 一直运行。 

### 任务 2：下载并打开网络应用程序项目

1. 右键单击以下链接并选择**另存链接**选项。 

    * [水果网络应用程序渲染项目代码](https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitWebApp-render.zip)

1. 启动**文件浏览器**并导航到文件保存的位置。

1. 将文件解压缩到它自己的文件夹中。

1. 启动 Visual Studio Code，在菜单栏中选择**文件**，然后选择**打开文件夹......**。

1. 导航至解压项目文件的位置，选择 *FruitWebApp-render* 文件夹。

1. **浏览器**窗格中的项目结构应与以下截图类似。 如果**浏览器**窗格不可见，请选择**查看**，然后在菜单栏中选择**浏览器**。

    ![显示 Fruit 网络应用程序项目结构的截图。](media/03-web-app-render-structure.png)

>**注意：** 请花时间查看本练习中编辑的每个文件中的代码。 这些代码都有大量注释，可以帮助你理解代码库。

## 执行代码以在索引页面上呈现数据

Fruit 网络应用程序会在主页上显示 API 示例数据。 您需要添加代码，以遍历由代码隐藏文件中执行的 HTTP `GET`操作返回的示例数据。

### 任务 1：添加代码以在表格中呈现数据

1. 在**浏览器**窗格中选择 *Index.cshtml* 文件，打开该文件进行编辑。

1. 在 `@* Begin render API data code block *@` 和 `@* End render API data code block *@` 注释之间添加以下代码。

    ```csharp
    <tbody>
        
        @*  The Razor keyword @foreach is used to iterate through the
            data returned to the data model from the HTTP operations. *@
        @foreach (var obj in Model.FruitModels)
        {
            <tr>
                @* Display the name of the fruit. *@
                <td width="50%">@obj.name</td>
                @*  The following if statment is a Razor code block that changes the color 
                    and icon of the available indicator in the page rendering. *@
                @{
                    if (@obj.instock)
                    {
                        <td width="20%" class="text-md-center">
                            <i class="bi bi-check-circle" style="font-size: 1rem; color: green;"></i>&nbsp;Yes
                        </td>
                    }
                    else
                    {
                        <td width="20%" class="text-md-center">
                            <i class="bi bi-dash-circle" style="font-size: 1rem; color:red;"></i>&nbsp;No
                        </td>
                    }
                }
                <td width="30%" class="text-center">
                    @*  The following div contains information to handle the edit and delete functions. *@
                    <div class="w-75 btn-group btn-group-sm" role="group" style="text-align:center">
                        @* Routes to the Edit page and passes the id of the record. *@
                        <a asp-page="Edit" asp-route-id="@obj.id" class="btn btn-primary  mx-2">
                            <i class="bi bi-pencil-square"></i> Edit
                        </a>
                        @* Routes to the Delete page and passes the id of the record. *@
                        <a asp-page="Delete" asp-route-id="@obj.id" class="btn btn-danger mx-2">
                            <i class="bi bi-trash"></i> Delete
                        </a>
                    </div>
                </td>
            </tr>
        }
    </tbody>
    ```

1. 保存对 *Index.cshtml* 的更改，并查看代码中的注释。

1. 在 Visual Studio Code 顶部菜单中选择**运行\|开始调试**，或选择 **F5**。 项目构建完成后，浏览器窗口应启动并运行网络应用程序

1. 验证索引页面是否显示来自 API 的示例数据。

    >**注意：** 在本练习稍后添加代码之前，**添加到列表**、**编辑**和**删除**功能将无法运行。

    >**注：** 如果运行应用程序时出现以下提示，您可以放心地忽略它。

    ![安装自签名证书的提示截图。](media/install-cert.png)

1. 要继续练习，请关闭浏览器或浏览器选项卡，并在 Visual Studio Code 中选择**运行 \| 停止调试** 或 **Shift + F5**。

## 执行代码以处理**添加到列表**功能

添加、编辑和删除操作分别在项目中的单独 *.cshtml* 页面上处理。 本节将添加代码，在 *Add.cshtml* 文件中创建一个表单，以便向列表中添加数据。

### 任务 1：添加代码以创建添加数据表单

1. 在**浏览器**窗格中选择 *Add.cshtml* 文件，打开该文件进行编辑。

1. 在 `@* Begin render Add code block *@` 和 `@* End render Add code block *@` 注释之间添加以下代码。

    ```csharp
    <form method="post">
        @*  The FruitModels.id is here so the full data model is represented on the page.
            The database behind the API will assign the id. *@
        <input hidden asp-for="FruitModels.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Add Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.name" class="h5"></label><br/>
                @* Empty text box for the name of the fruit to be added. *@
                <input type="text" asp-for="FruitModels.name" />
                <span asp-validation-for="FruitModels.name" class="text-danger"></span>
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.instock" class="h5"></label><br/>
                @* Render the true/false instock state from the record in an editable checkbox. *@
                <input type="checkbox" asp-for="FruitModels.instock" style="width:20px; height:20px" />
                <label class="h7"><i class="bi bi-arrow-left"></i>  Check the box if it's available.</label>
                <span asp-validation-for="FruitModels.instock" class="text-danger"></span>
            </div>
            @* Submit the addition or return to the Index page if the Add is cancelled.*@
            <button type="submit" class="btn btn-primary" style="width:150px;">Create</button>
            <a asp-page="Index" class="btn btn-secondary" style="width:150px;">Cancel</a>
        </div>
    </form>
    ```

1. 保存对 *Add.cshtml* 的更改，并查看代码中的注释。

1. 在 Visual Studio Code 顶部菜单中选择**运行\|开始调试**，或选择 **F5**。 项目构建完成后，浏览器窗口应启动并运行网络应用程序

1. 在页面上选择**添加到列表**。

1. 输入要添加到列表中的水果名称，并选择复选框表示该水果可用。

1. 选择**创建**将条目添加到列表中，然后您将返回主页。 确认已将条目添加到列表中。

1. 要继续练习，请关闭浏览器或浏览器选项卡，并在 Visual Studio Code 中选择**运行\|停止调试**或 **Shift + F5**。

## 执行代码以处理**编辑**功能

在本节中，您将添加代码，在 *Edit.cshtml* 文件中创建表格，以便编辑列表中的数据。

### 任务 1：为编辑表单添加代码

1. 在**浏览器**窗格中选择 *Edit.cshtml* 文件，打开该文件进行编辑。

1. 在 `@* Begin render Edit code block *@` 和 `@* End render Edit code block *@` 注释之间添加以下代码。

    ```csharp
    <form method="post">
        @*  The id for the data record is hidden because it needs to be available to the 
            code-behind processing, but it's not displayed. *@
        <input hidden asp-for="FruitModels.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Edit Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.name" class="h5"></label><br/>
                @* Render the current name of the fruit in an editable text box. *@
                <input type="text" asp-for="FruitModels.name" />
                <span asp-validation-for="FruitModels.name" class="text-danger"></span>
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.instock" class="h5"></label><br/>
                @* Render the true/false instock state from the record in an editable checkbox. *@
                <input type="checkbox" asp-for="FruitModels.instock" style="width:20px; height:20px" />
                <label class="h7"><i class="bi bi-arrow-left"></i>  Check the box if available.</label>
                <span asp-validation-for="FruitModels.instock" class="text-danger"></span>
            </div>
            @* Submit the changes or return to the Index page if the edit is cancelled.*@
            <button type="submit" class="btn btn-primary" style="width:150px;">Update</button>
            <a asp-page="Index" class="btn btn-secondary" style="width:150px;">Cancel</a>
        </div>
    </form>
    ```

1. 保存对 *Edit.cshtml* 的更改，并查看代码中的注释。

1. 在 Visual Studio Code 顶部菜单中选择**运行\|开始调试**，或选择 **F5**。 项目构建完成后，浏览器窗口应启动并运行网络应用程序

1. 在列表中选择一个要更改的项目，然后在该行中选择**编辑**。

1. 编辑果实名称并选择复选框以更改其可用状态。

1. 选择**更新**保存更改，您将返回主页。 确认列表中显示了您的更改。

1. 要继续练习，请关闭浏览器或浏览器选项卡，并在 Visual Studio Code 中选择**运行\|停止调试**或 **Shift + F5**。

## 执行代码以处理**删除**功能

在本节中，您将添加代码，在 *Delete.cshtml* 文件中创建一个表单，以便从列表中删除数据。

### 任务 1：为删除表单添加代码

1. 在**浏览器**窗格中选择* Delete.cshtml* 文件，打开该文件进行编辑。

1. 在 `@* Begin render Delete code block *@` 和 `@* End render Delete code block *@` 注释之间添加以下代码。

    ```csharp
    <form method="post">
        @*  The id for the data record is hidden because it needs to be avaialable to the 
            code-behind processing, but it's not displayed. *@
        <input hidden asp-for="FruitModels.id" />
        <div class="border p-3 mt-4" style="width:50%">
            <div class="row pb-2">
                <h2 class="text-primary pl-3">Delete Fruit</h2>
                <hr />
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.name" class="h5"></label><br/>
                @* Render the name of the fruit in a non-editable text box. *@
                <input type="text" asp-for="FruitModels.name" disabled/>
                <span asp-validation-for="FruitModels.name" class="text-danger"></span>
            </div>
            <div class="mb-3">
                <label asp-for="FruitModels.instock" class="h5"></label><br/>
                @* Render the true/false instock state from the record in a non-editable checkbox. *@
                <input type="checkbox" asp-for="FruitModels.instock" style="width:20px; height:20px" disabled  />
                <span asp-validation-for="FruitModels.instock" class="text-danger"></span>
            </div>
            @* Submit the changes or return to the Index page if the delete is cancelled.*@
            <button type="submit" class="btn btn-danger " style="width:150px;">Delete</button>
            <a asp-page="Index" class="btn btn-secondary" style="width:150px;">Cancel</a>
        </div>
    </form>
    ```

1. 保存对 *Delete.cshtml* 的更改，并查看代码中的注释。

1. 在 Visual Studio Code 顶部菜单中选择**运行\|开始调试**，或选择 **F5**。 项目构建完成后，浏览器窗口应启动并运行网络应用程序

1. 在列表中选择一个要**删除**的项目，然后在该行中选择删除。

1. 选择**删除**后将返回主页。 确认已删除的项目不再显示在列表中。

准备结束练习时

* 关闭浏览器或浏览器选项卡，在 Visual Studio Code 中选择**运行 \| 停止调试** 或 **Shift + F5**。 

* 在 Fruit API 运行的终端中输入 **Ctrl + C**，停止 Fruit API。

## 审阅

在本练习中，你了解了如何：

* 在应用程序中实现Razor关键字
* 将 C# 代码与 Razor Pages 语法相结合
