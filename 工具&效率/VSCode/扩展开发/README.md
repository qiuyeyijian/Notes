# VSCode

## 构建脚本

如果修改了扩展代码，想重新加载的话，可以直接在新窗口上按下`Ctrl+R`来快速重新加载，也可以先停止，然后再按`F5`。





## package.joson

```json
{
	// 插件的名字，应全部小写，不能有空格； string
	"name": "my-vscode-plugin",
	// 版本号； string
	"version": "1.0.0",
	// 发布插件的人的名字 string
	"publisher": "Bob",
	// 插件支持的最低 VScode版本，不能是 *； Object
	"engines": {
	    "vscode": "^1.0.0"
	},
	// 许可证，和 npm license 一样； string
	"license": "SEE LICENSE IN <filename>",
	// 插件应用市场显示的名称；string
	"displayName": "myVScodePlugin",
	// 描述；string
	"description": "this is my vscode plugin",
	// 插件应用市场分类；string[]；可选值：[Programming Languages, Snippets, Linters, Themes, Debuggers, Formatters, Keymaps, SCM Providers, Other, Extension Packs, Language Packs, Data Science, Machine Learning, Visualization, Notebooks, Education]
	"categories": ["other"]
	// 关键字，可以在应用市场上进行搜索，最多 5 个关键字；array
	"keywords": ["test", "vscode", "theme"],
	// 格式化市场标题，以匹配图标；object
    "galleryBanner": {
        "color": "#C80000",
        "theme": "dark"
    },
    // 将插件设置为在应用市场中预览；boolean
    "preview": true,
    // 差距的入口文件；string
    "main": "/src/extension"
	// 贡献点，描述插件贡献的对象；object
	"contributes": {
		// 插件配置项
		"configuration": {
			"type": "object",
			// 配置项标题，会显示在vscode的设置页
			"title": "my-vscode-plugin",
			"properties": {
				"myVscodePluginDemo.yourName": {
					"type": "string",
					"default": "guest",
					"description": "this is your name"
				}
			}
		},
		// 命令
		"commands": [
			{
				"command": "extension.sayHello",
				"title": "Hello World"
			}
		],
		// 快捷键绑定
		"keybindings": [
			{
				"command": "extension.sayHello",
				"key": "ctrl+f10",
				"mac": "cmd+f10",
				"when": "editorTextFocus"
			}
		],
		// 菜单
		"menus": {
			// 编辑器右键菜单
			"editor/context": [
				{
					// 表示只有编辑器具有焦点时才会在菜单中出现
					"when": "editorFocus",
					"command": "extension.sayHello",
					// navigation是一个永远置顶的分组，后面的@6是人工进行组内排序
					"group": "navigation@6"
				},
				{
					"when": "editorFocus",
					"command": "extension.demo.getCurrentFilePath",
					"group": "navigation@5"
				},
				{
					// 只有编辑器具有焦点，并且打开的是JS文件才会出现
					"when": "editorFocus && resourceLangId == javascript",
					"command": "extension.demo.testMenuShow",
					"group": "z_commands"
				},
				{
					"command": "extension.demo.openWebview",
					"group": "navigation"
				}
			],
			// 编辑器右上角图标，不配置图片就显示文字
			"editor/title": [
				{
					"when": "editorFocus && resourceLangId == javascript",
					"command": "extension.demo.testMenuShow",
					"group": "navigation"
				}
			],
			// 编辑器标题右键菜单
			"editor/title/context": [
				{
					"when": "resourceLangId == javascript",
					"command": "extension.demo.testMenuShow",
					"group": "navigation"
				}
			],
			// 资源管理器右键菜单
			"explorer/context": [
				{
					"command": "extension.demo.getCurrentFilePath",
					"group": "navigation"
				},
				{
					"command": "extension.demo.openWebview",
					"group": "navigation"
				}
			]
		},
		// 代码片段
		"snippets": [
			{
				"language": "javascript",
				"path": "./snippets/javascript.json"
			},
			{
				"language": "html",
				"path": "./snippets/html.json"
			}
		],
		// 自定义新的 activitybar 图标，也就是左侧侧边栏大的图标
		"viewsContainers": {
			"activitybar": [
				{
					"id": "phone",
					"title": "手机",
					"icon": "images/phone.svg"
				}
			]
		},
		// 自定义侧边栏内 view 的实现
		"views": {
			// 和 viewsContainers 的id对应
			"beautifulGirl": [
				{
					"id": "apple",
					"name": "苹果"
				},
				{
					"id": "huawei",
					"name": "华为"
				},
				{
					"id": "xm",
					"name": "小米"
				}
			]
		},
		// 图标主题
		"iconThemes": [
			{
				"id": "myIconTheme",
				"label": "图标主题",
				"path": "./theme/icon-theme.json"
			}
		]
	},
	// 激活的拓展事件组；array
	"activationEvents": [
	    "*"
	],
	// 徽章，显示在应用市场拓展页侧边栏中；array，只允许受信用的徽章
	"badges": [],
	// 插件市场中使用的 markdown 渲染引擎；string
	"markdown": ""
	// 市场中的链接；marketplace（默认）、string、false
	"qna": "",
	// 插件运行时的 Node.js 依赖项；object；和 npm 中相同
	"dependencies": {
	    
	},
	// 插件开发时的 Node.js 依赖项；object；和 npm 中相同
	"devDependencies": {
	    
	},
	// 与此插件捆绑的插件的 ID 数组；array
	"extensionPack": [
	    "felixfbecker.php-debug",
        "felixfbecker.php-intellisense",
        "Kasik96.format-php"
	],
	// 此插件所依赖的插件的 ID 数组；array
	"extensionDependencies": [
	    
	],
	// 插件在远程配置中运行的位置的数组；array；[ui, workspace],
	"extensionKind": [],
	// 和 npm 中 scripts 相同；object
	"scripts": {
	    
	},
	// 插件 icon，至少为 128 * 128 像素
	"icon": "icon.png"
}

```



## Demo01

````tsx
// extension.ts
export function activate(context: vscode.ExtensionContext) {
	context.subscriptions.push(vscode.commands.registerCommand("emtest.askQuestion", async () => {
		let answer = await vscode.window.showErrorMessage("How was your day?", "Good", "Bad");

		if (answer === "Bad") {
			vscode.window.showErrorMessage("Sorry to hear that");
		} else {
			console.log(`${answer}`);
		}
	}));
}
````

```json
// package.json
"activationEvents": [
    "onCommand:emtest.askQuestion"
  ],
 "contributes": {
    "commands": [
      {
        "command": "emtest.askQuestion",
        "category": "emtest",
        "title": "askQuestion"
      }
    ]
  },
```



## webview

### Demo002：展示静态文本内容

```typescript
import * as vscode from 'vscode';
import * as path from 'path';

function getWebviewContent(url: string) {
	return `
	<!DOCTYPE html>
	<html lang="en">
		<head>
			<meta charset="UTF-8">
			<meta http-equiv="X-UA-Compatible" content="IE=edge">
			<meta name="viewport" content="width=device-width, initial-scale=1.0">
			<title>Document</title>
		</head>
		<body>
		<img src="${url}" width="600" />
		</body>
	</html>
	`;
}

context.subscriptions.push(vscode.commands.registerCommand("emtest.webview001", () => {
    // 创建并显示一个新的webview
    const panel = vscode.window.createWebviewPanel(
        "Test", // 标识webview的类型
        "webview001",   // 展示给用户的标题
        vscode.ViewColumn.One,  // // 显示在编辑器的哪个部位,One,Two,...
        {}  // webview的其他选项
    );

    // webview的内容，其实就是一个html
    panel.webview.html = getWebviewContent("https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif");

}));
```



### Demo002: 动态传值更新webview内容

```typescript
const cats = {
	"codingCat": "https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif",
	"compilingCat": "https://media.giphy.com/media/mlvseq9yvZhba/giphy.gif"
};
export function activate(context: vscode.ExtensionContext) {
	context.subscriptions.push(vscode.commands.registerCommand("emtest.webview001", () => {
		// 创建并显示一个新的webview
		const panel = vscode.window.createWebviewPanel(
			"Test", // 标识webview的类型
			"webview001",   // 展示给用户的标题
			vscode.ViewColumn.One,  // 显示webview面板并以编辑器新列的方式
			{}  // webview的其他选项
		);

		let flag = true;
		setInterval(() => {
			const cat = flag ? "codingCat" : "compilingCat";
			panel.title = cat;
			panel.webview.html = getWebviewContent(cats[cat]);
			flag = !flag;
		}, 1000);

	}));
}
```



### Demo003: 加载本地资源

上面的例子，gif图片是在线地址；如果要加载本地资源，该如何将地址传给webview呢；我们直接在return 返回的html里面设置本地资源地址是无效的；这里我们需要借助两个api

- `vscode.Uri.file`，通过该api获取资源的所在磁盘地址

- `panel.webview.asWebviewUri`，将其转换为vscode-resource地址，然后将该地址传给html即可。



```typescript
export function activate(context: vscode.ExtensionContext) {
	context.subscriptions.push(vscode.commands.registerCommand("emtest.webview001", () => {
		// 创建并显示一个新的webview
		const panel = vscode.window.createWebviewPanel(
			"Test", // 标识webview的类型
			"webview001",   // 展示给用户的标题
			vscode.ViewColumn.One,  // 显示webview面板并以编辑器新列的方式
			{}  // webview的其他选项
		);

		// 获取到cat.gif的文件路径
		const filepath = vscode.Uri.file(path.join(context.extensionPath, "media", "cat.gif"));
		// 将文件地址转换成vscode-resource地址
		const url = panel.webview.asWebviewUri(filepath);
		// 将地址传给html
		panel.webview.html = getWebviewContent(url.toString());

		console.log("context.extensionPath", context.extensionPath);
		console.log("filepath", filepath);
		console.log("url", url);

	}));
}
```





## Demo04 读取某一路径下的文本文件

```typescript
import * as vscode from 'vscode';
import * as Path from 'path';
import * as Fs from "fs";

// 定义小说存放路径
const localNovelsPath = `D:\\Workspace\\Temp\\Test\\Novel`;

interface NovelItem {
	name: string;
	path: string;
}

class NovelTreeItem extends vscode.TreeItem {
	constructor(info: NovelItem) {
		super(`${info.name}`);

		// tooltip是悬浮的条提示
		this.tooltip = `名称: ${info.name}`;
		// 我们设置一下点击该行的命令，并且传参进去
		this.command = {
			command: "emtest.openSelectedNovel",
			title: "打开该小说",
			arguments: [{ name: info.name, path: info.path }],
		};
	}
}

class Provider implements vscode.TreeDataProvider<NovelItem> {
	// 提供单行的UI展示
	getTreeItem(info: NovelItem): NovelTreeItem {
		return new NovelTreeItem(info);
	}

	// 提供每一行的数据
	getChildren(): Promise<NovelItem[]> {
		const files = Fs.readdirSync(localNovelsPath);
		const localnovellist: NovelItem[] = [];

		files.forEach((file: string) => {
			if (Path.extname(file) === ".txt") {
				localnovellist.push({ name: file, path: Path.join(localNovelsPath, file) });
			}
		});

		return Promise.resolve(localnovellist);
	}
}

export function activate(context: vscode.ExtensionContext) {
	context.subscriptions.push(vscode.window.registerTreeDataProvider("emtest.readNovel", new Provider()));
	context.subscriptions.push(vscode.commands.registerCommand("emtest.openSelectedNovel", (args) => {
		vscode.commands.executeCommand("vscode.open", vscode.Uri.file(args.path));
	}));
}
```

```json
"activationEvents": [
    "onView:emtest.readNovel"
  ],
  "main": "./dist/extension.js",
  "contributes": {
    "commands": [],
    "viewsContainers": {
      "activitybar": [
        {
          "id": "readNovel",
          "title": "readNovel",
          "icon": "./assets/img/logo.svg"
        }
      ]
    },
    "views": {
      "readNovel": [
        {
          "name": "小说列表",
          "id": "emtest.readNovel"
        }
      ]
    }
  },
```





