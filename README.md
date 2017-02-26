# FauxPas_document_-translation

#### 原文：[http://fauxpasapp.com/docs](http://fauxpasapp.com/docs)
#### 翻译：[DeveloperLx](http://weibo.com/DeveloperLx)

# 文档
---
# 目录
##### 检查一个Xcode项目
###### 选择哪些规则去执行
##### 使用命令行接口（CLI）
##### 配置
###### 项目特定的配置文件
###### 配置建议
##### 过滤诊断
##### 制止诊断（Suppressing Diagnostics）
###### 在代码中制止诊断
###### 在指定的文件或目录制止诊断
###### 从检查中排除指定的文件、目录或Xcode组
##### 启动FauxPas的快捷方式
###### 在FaxuPas图形界面工具（GUI）中快速地打开活跃的Xcode项目
###### 在Xcode build过程中执行检查
###### 在Xcode中手动调用FauxPas
##### 使用定制的脚本处理诊断
###### 发布（Emitting）机器可读的输出
###### 机器可读的输出的结构
###### 处理机器可读的输出
##### 处理故障
###### 当使用Xcode编译我的项目时，FauxPas给出了一些我不理解的编译错误
###### FauxPas检查项目失败
###### 我安装了多个版本的Xcode；怎样确保FauxPas使用了我想用的那一个

---
## 检查一个Xcode项目
> FauxPas执行检查在**Xcode项目**中的**单个target**上，使用单个的**项目配置**.在最简化的情况下，项目必须被指定————如果没有被指定的话，FauxPas就会选择一个默认的target和build配置。

### 选择哪些规则去执行
> 可以通过标签或单独地选择规则。单独的规则也可以被排除。排除的动作将被最后实施，所以如果你排除了一个因选择标签而选择的规则，这条规则将不会被执行.

#### 图形界面工具：
* 在规则选择视图中，通过点击它们的名称来选择标签（规则将通过它们的标签被选择，然后显示一个勾的标记在它们的左边，替换单独地选择勾选框）
* 通过勾选紧挨着它们的框，选择单独的规则
* 通过右击（或command+点击）列表中的一个规则，选择（排除规则），排除单独的规则

#### 命令行：
* 使用 -g/--ruleTags 来选择有指定标签的全部的规则
* 使用 -r/--rules 来选择单独的规则
* 使用 --excludedRules 来排除单独的规则

* **推荐：尽可能地通过标签来选择规则，如果可以的话，避免选择单独的规则。然后排除任何你不想要的规则。这样的话，在FauxPas新版本中的新的规则将被自动地选择，假如你已选择了它对应的标签的话。**

* 在命令行中，你也可以使用--onlyRules参数来仅仅执行指定的规则，覆盖全部的其它的规则选择选项。这对于当你的工程有一个指定的配置文件（带有规则和标签选择及规则排除），而你又想临时地覆盖掉时，是非常便利的。

## 使用命令行接口
* 为了通过命令行使用FauxPas，你必须首先安装fauxpas命令：
* 图形用户界面：从应用菜单中选择 FauxPas > Install CLI Tools... 菜单项。
* 命令行：执行命令：

```
	/Applications/FauxPas.app/Contents/Resources/install-cli-tools
```

* 这将安装fauxpas命令到/usr/local/bin，但是你当然可以移动它到其它你希望的地方。
* 在终端执行 fauxpas help，可以查看命令行接口的文档。

## 配置
* FauxPas可以通过下列方式进行配置：
* 命令行参数（如果你使用命令行接口）
* 图形界面工具
* 配置文件

* 配置文件可以使用JSON来写。查看一个完整的示例配置文件，可以在终端执行执行 fauxpas exampleConfig（注意这要求命令行接口工具必须被安装）
* 当使用命令行接口，配置文件可以选取通过 -c/--configFile 参数

### 项目指定配置文件
* 引用一个项目指定的配置文件，可以通过添加它到一个在项目根目录下（与.xcodeproj目录所在在的相同目录），名为FauxPasConfig的目录下，它的扩展名为.fauxpas.json. 这个文件也可以存在在其它地方，只要它的扩展名如同上述，并且你的Xcode项目包含一个指向它的引用。
* 如果有多个配置文件在FauxPasConfig目录下，则默认使用名为main.fauxpas.json的那个。
* 项目指定的配置文件将被自动选取，无论在命令行接口或在图形界面工具的调用时。

### 配置建议
* 很多FauxPas的配置选项会影响它检查你的项目的时间。对于大多数Xcode项目的，默认是一个尝试利于平衡速度和兼容性的配置；对于你特定的项目，为了提高检查速度，调整一些选项，也是OK的。
* 这里有一些可以帮助你检查项目速度更快的建议：
> 避免选择workspace和scheme。假如你想检查的项目可以被独立地build（也就是说不用作为一个workspace的一部分来build），请设置“Xcode workspace to build project with”和“Xcode scheme to build project with”选项不被选中。
> 避免执行完全build（full builds）。如果 “Build project before checking” 选项关闭，FauxPas可以更快地开始坚持你的项目。有时这个选项是必须被打开的（例如如果项目在build过程中会生成头文件），但是对于大多数项目可以安全地关闭这个选项。
> 仅仅处理指定目标的PCHs。如果“Process only target precompiled headers” 选项是打开的，FauxPas可以更快地开始坚持你的项目。有时这个选项必须被关闭，但是对于大多数项目，它是可以安全地被打开的。
> 选择仅仅一个架构（architecture）。如果你的项目通常是在多个中央处理器架构（CPU architectures）上build的，你可以限制build FauxPas调用仅仅一个架构来提升FauxPas的速度。你可以通过在“Additional xcodebuild arguments to use”选项上添加一个ARCHS build设置的值（例如ARCHS=armv7）来实现。 

## 过滤诊断
* 在图形界面工具中，“Diagnostics”视图有一个文本输入框，它允许你输入一些限制展示诊断的过滤器。
* 你可以添加过个过滤器，当遇到要让它们一个接一个地全部执行的情况
* 支持下列过滤器
* 文件：仅展示匹配文件的诊断。可能会用到通配符\*。例如：file=foo.m 或 file=\*ViewController.m
* 规则：仅展示zhid规则的诊断。例如：rule=Dot syntax usage
* 效果（Impact）：仅展示指定效果（ (Functionality, Maintainability, Style)）的诊断。例如：impact=Functionality
* 靠谱程度（Confidence）：仅展示指定靠谱程度(Absolute, High, Medium, Low)的诊断。例如：confidence=High
* 严重性：仅展示指定严重性(Error, Warning, Concern)的诊断。例如：severity=Error

## 制止诊断
* 如果通过一个特定的规则，你没有得到任何有用的诊断，或者不同意这个规则的基本前提，只需从被执行的规则中排除这条即可。

### 在代码中制止诊断
* 如果你想保持一个规则打开，但在你代码中一个特定区域中制止它的诊断，你可以使用定义在头文件FauxPasAnnotations.h中的宏来解决。
* 要添加上述头文件到你的项目中，按下面的做：
* 运行FauxPas图形界面工具
* 打开你的工程
* 选择 Project > Add Annotations Header… 菜单项
* 关于怎样使用那些宏，更多的说明包含在这个头文件自身中

### 在特定的文件或目录中制止诊断
* 在FauxPas中，每条规则都有一个名叫“Regexes for ignored file paths”的选项。这个选项可以被用来制止在这条规则下，全部的匹配它指定的正则表达式的文件路径

### 从检查中排除指定的文件、目录或Xcode组
* 如果你想从被FauxPas检查的地方，排除匹配特定样式的文件，你可以使用下列选项：
* 排除文件的正则表达式：[--fileExclusionRegexes]
* 排除文件的前缀：[--fileExclusionPrefixes]
* 排除的Xcode组：[--fileExclusionXcodeGroups]
* 注意这些选项不仅仅制止诊断：它们完全从检查开始就排除了那些匹配的文件

## 启动FauxPas的快捷方式
### 在FauxPas图形界面工具中快速打开活动的Xcode项目
* 在FauxPas图形界面工具中，你可以使用open命令打开一个指定的Xcode项目。
* 使用一个简短的AppleScript片段来从Xcode中获取当前活动的项目的路径：

```
	open -a FauxPas "`osascript -e 'tell application \"Xcode\" to return path of active project document'`"

```

* 你可以为此使用一个协助应用（像是Keyboard Maestro, FastScripts或Spark）来指定一个全局的热键

### 在Xcode build期间执行检查
* FauxPas可以在Xcode的build phase中被执行。你可以看到它产生的诊断结果在Xcode图形界面工具中，并且轻松地跳转到相应的文件位置中。这个方法也允许FauxPas打断build，通过返回一个非零的退出状态（by returning a nonzero exit status）（--minErrorStatusSeverity这个选项可以被用来控制FauxPas染回错误退出状态的条件）。

* 确保你的FauxPas命令行接口已安装
* 在Xcode你的项目中，为你期望检查的target创建一个新的“Run Script” build phase
* 添加下列内容作为脚本

```
 [[ ${FAUXPAS_SKIP} == 1 ]] && exit 0

 FAUXPAS_PATH="/usr/local/bin/fauxpas"
 if [[ -f "${FAUXPAS_PATH}" ]]; then
   "${FAUXPAS_PATH}" check-xcode
 else
   echo "warning: Faux Pas was not found at '${FAUXPAS_PATH}'"
 fi
```

* 带有这个脚本后，FauxPas检查将在build过程中被执行，除非FAUXPAS_SKIP这个build设置被赋值为1。

### 在Xcode中手动调用FauxPas
* 要想在Xcode中手动运行FauxPas:
* 确保你的FauxPas命令行接口已安装
* 创建一个新的名为“Faux Pas”的Aggregate target到你的项目中
* 给“Faux Pas”这个target创建一个名为“Run Faux Pas”的新的“Run Script” build phase
* 添加下列内容作为其脚本，用你项目实际的名称替代PROJECT_NAME：

```
	/usr/local/bin/fauxpas -o xcode check "PROJECT_NAME.xcodeproj"
```

* 现在你可以通过“Faux Pas”scheme 来调用FauxPas。

## 使用定制的脚本处理诊断
* 如果你想写自己的脚本来处理FauxPas给出的诊断，你必须首先让它产生机器可读的输出。

### 给出机器可读的输出
* 在命令行接口中，--outputFormat (或 -o) 参数允许你指定诊断的标准输出的格式。可选的值如下：
* human —— 人类可读的输出（缺省）
* json —— json输出
* plist —— 属性列表输出（当前这个输出作为一个XML列表）
* xcode —— 能够被Xcode理解的，基于行的输出。当你使用 check-xcode 命令时，它是默认的选项，它可以使Xcode展示诊断作为错误和警告。
* 在图形界面工具中，你可以在诊断视图中，按Export Diagnostics…按钮，以上述任意格式来保存诊断到一个文件中。

### 机器可读的输出的结构
* 对于全部机器可读的格式，其实际诊断的结构都是相同的————仅仅是序列化输出的变量（serialization format varies）不同（仅存在的例外是由于属性列表域中没有“null”的值，因此这些域只是简化给出）。

* 根对象有下列域：
* fauxPasVersion —— FauxPas用来生成输出的版本
* timeStamp —— 当给出输出时的一个unix时间戳
* projectPath —— .xcodeproj目录的文件系统路径（filesystem path）
* projectName —— 被检查的Xcode项目的名称
* targetName —— 在Xcode项目中被检查的target
* targetBundleVersion —— 被检查target的Info.plist中，CFBundleVersion
* buildConfigurationName —— Xcode使用的build配置的名称
* projectIconBase64PNG —— 项目的64位编码的PNG的图标（展示在FauxPas图形界面工具上）（120✖️120像素）
* versionControlSystemName —— 项目用到的版本控制系统的名称
* versionControlRevision —— 当前版本控制系统的版本标识（当前仅支持Git）
* diagnostics —— 一个诊断对象的数组

* 诊断对象有下列域：
* ruleShortName —— 给出诊断的规则的简短的名称
* ruleName —— 给出诊断的规则的全称
* ruleDescription —— 给出诊断的规则的全部描述
* ruleWarning —— 对于一些规则，关联到产生的诊断的人类可读的警告
* info —— 对于诊断的人类可读的信息
* html —— 一个包含上述所有键的字典，且带有HTML格式的值
* identifier —— 被每条规则选择的一个任意的值，通常用来指出诊断指向的那部分数据（例如： Missing Translation 这个规则，将指出似乎缺失的字符串资源的键）
* file —— 诊断指向的文件的全路径
* context —— 诊断指向的那段代码的parent代码标记（a “parent” code symbol） 的名称或标记（例如：一个函数，方法或类）
* extent —— 诊断指向的文件的位置的标记
* 	start和end，都有下列的结构
* 		line —— 行号
* 		byteColumn —— 每行以字节为单位的列
* 		byteOffset —— 以字节为单位的相对于文件开头的偏移量
* 		utf16Column —— 每行以UTF-16为单位的列
* 		utf16Offset —— 以UTF-16为单位的相对于文件开头的偏移量
* fileSnippet —— 诊断指向文件的段的内容
* impact —— 这个诊断描述的问题的基本影响（Impact），应当是（functionality, maintainability or style）
* severity —— 诊断的严重程度（一个[0-10]的整数值，较高的值表明较高的严重程度。3是“concern”，5是“warning”，9是“error”。）
* severityDescription —— 对于severity的文字描述
* confidence —— 我们认为的这个诊断的靠谱程度（一个[0-10]的整数值，较高的值表明较高的靠谱程度）
* confidenceDescription —— 对于confidence的文字描述

### 处理机器可读的输出
* 你当然可以用任何你认为合适的方法来处理诊断，但是这里有一条建议：使用 jq 来给出JSON格式的输出。
* 这里有一个例子，打印出项目中可能未被使用的资源文件的列表：
```
	$ fauxpas --onlyRules UnusedResource -o json check MyProject.xcodeproj
  | jq --raw-output '.diagnostics[] | .file'
	/Users/username/myproject/unused1.jpg
	/Users/username/myproject/unused2.xml
```

## 处理故障
### 当使用Xcode编译我的项目时，FauxPas给出了一些我不理解的编译错误
* FauxPas使用了一个相对于Xcode些微不同的Clang编译器的版本（这是不得已而为之的：尽管LLVM/Clang是开源的，但苹果有它自己闭源的fork）。

* 比较Clang的版本
* 执行下列命令来查看你安装的Xcode使用的Clang的版本：

```
	xcrun clang -dM -E -x c /dev/null | grep __VERSION__
```

* 执行下列命令来查看你安装的FauxPas使用的Clang的版本：

```
	fauxpas -v version | grep Clang
```

* 注意：然而苹果为它的Clang fork使用它自己版本号的方案，这造成了比较的不同。这个资源可以提供帮助[https://gist.github.com/yamaya/2924292.](https://gist.github.com/yamaya/2924292.)

* 制止编译器警告
* 如果你在项目中使用-Weverything警告标记，请尝试移除它（这个标记打开在Clang中的全部警告，甚至新的、错误的、试验性的都有）。如果你不想在你的项目配置中关掉这个标记，但就是想为FauxPas关掉它，你可以添加-Wno-everything这个值到“Additional compiler arguments to use”(--extraCompilerArgs)这个配置选项中。
* 如果你只是想对于FauxPas禁掉全部的编译警告（对于你的项目却不），添加-w标记到“Additional compiler arguments to use”(--extraCompilerArgs)这个配置选项中。

### FauxPas检查我的项目失败
* 请确认FauxPas对你的项目使用了正确的xcodebuild参数。如果你打开了--verbose选项，FauxPas将打印被用到log输出中的xcodebuild参数。
* 为了确定正确地build你的项目需要什么xcodebuild参数，你可以在终端cd到你.xcodeproj目录所在的目录，然后尝试在这里执行xcodebuild（开始使用和FauxPas相同的参数？？）

* 生成源码的项目
* 如果你的项目在build过程中生成或修改源文件（例如：头文件），你必须打开“Build project before checking” (--fullBuild)选项。如果正确地解释（interpreting）你项目的源码需要依赖在build过程中生成的其它东西（例如：一个依赖的项目），你也需要进行同样的操作。

* 项目使用一个workspace来编译
* 如果你的项目通常使用Xcode Workspace来build，并且无法作为一个独立的项目来build，你就必须指定--workspace和--scheme选项。
* 当你在图形界面工具中，打开一个没有关联的配置文件的项目，就会展示一个配置帮助表（configuration help sheet），让你来轻松选择workspace和scheme来用。

### 我安装了多个版本的Xcode；我怎样确保FauxPas使用了我想用的那一个
* FauxPas使用xcrun来寻找它需要的全部Xcode开发者工具（例如xcodebuild）。如果你打开了--verbose选项，FauxPas将在你检查项目时打印这些路径到log中。
* 你可以使用xcode-select程序来改变你的系统使用哪个Xcode

```
	xcode-select -switch /Applications/Xcode.app
```

* 如果你不想让这个改变全局化，你可以在执行FauxPas前设置 DEVELOPER_DIR 环境变量

```
	DEVELOPER_DIR="/Applications/Xcode.app" fauxpas <arguments...>
```
