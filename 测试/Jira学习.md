# 一、Jira基本概念

## 1、Projiect 项目

==在JIRA系统中的项目概念是一组问题单 (lssue) 的集合==，项目可以根据组织需求来定义，例如: 软件研发项目市场营销活动，一个请假管理系统等。

每一个问题单属于一个项目。每个项目需要有一个名称(例如: Websitessues) 和关键字(Key，例如WEB)。项目的关键字会成为项目问题单前缀，例如WEB-101,WEB-102等

![image-20231025155105705](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231025155105705.png)

## 2、Issue 问题/事件

JIRA跟踪问题，这些问题可以是bug，功能请求或者任何其他想要跟踪的的任务。Issue是jira当中最小的实例单元,可以包含以下几种类型：

- `Bug - 故障`，功能失效:Bug可由任何发现之人录入

- `lmprovement - 提升`，既有功能增强

- `New Feature - 新功能`

- `Task - 任务`,用来管理开发/测试的基准与进度

- `Sub-task - 子任务`，进行任务分解

![image-20231025155807841](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231025155807841.png)

而对于测试用例角度来说，我们主要关注的Issue类型包括：Test Case(测试用例)、Test Plan（测试计划）、Test Cycle（测试周期）,即使用jql语法查询为：`issuetype = "测试用例"/"测试计划"/"测试周期"`



## 3、Version 版本

对于一些类型的项目，尤其是软件研发项目，把一个问题单关联到一个特定的项目版本(例如: 1.0 beta,1.0,1.2,2.0)会非常有用。

问题单 (lssues) 有两个跟版本有关的字段:

​	`影响版本(Affects Version(s))`: 这个是要说明受问题单影响的版本,举例而言，一个软件Bug可能影响1.1和1.2版本.

​	`修复版本 (Fix Version(s))` : 这个是为了标明这个问题单在哪一个版本中被修复。

比如Bug的影响版本号是1.1和1.2，但是可能会在版本2.0中才被修复。没有修复版本号的问题单会被归类为`未规划(Unscheduled)。`

版本可以是下面三种状态之一: `发布 (Released) ，未发布 (Unreleased) 和归档 (Archived)。`

版本会有一个发布日期，并且如果在发布日期之后还没有按时发布，这个状态会自动变为`过期状态 (overdue)。`

![image-20231025160326091](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231025160326091.png)

## 4、Workflows 工作流

工作流(Workflow): JIRA中的工作流由一系列的状态(statuses) 和变迁transitions) 构成，一个问题单在其生命
周期中会经过这些状态和变迁。

不同项目模板会有不同的内置工作流，另外，也可以自己定义工作流。但是，一般用默认就可以了。

![image-20231025160800630](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231025160800630.png)

## 5、Status状态和操作

状态 (Status)每一个问题单都会有一个当前的状态，状态说明了当前lssue的处理状态。这个可以自己定义!!

一个问题单开始阶段可能是Open或者To Do状态，然后可以转移到Resolved或者Closed，具体的迁移依赖于系统流程配置的方式。状态分为: TO DO (待定) ，Progressing (进行中)，Resolved (已解决) ，Done (已完成) ，Reopen (重新打开) ，Pending(搁置，Feedback(反馈)。用户可以通过浏览issue页面中的状态操作按钮，改变当前的状态，操作为[open] [progress) [reopen)pend] [resolve] [ Feedback] [Close]。状态如下分类:

TO DO/Open (待定) :每一个新建的Issue初始状态都为TO DO/Open。

In Progress/Progressing(进行中): lssue (事件) 指定给解决人之后，修改状态为Progressing，表示该Issue正在解决的过程中。

Under Review(在审核): 与Processing类似。

Resolved(已解决):当解决人把指定的Issue决完成后，修改状态为Resolved，表示该Issue已经解决完成，可以进行测试或验证了

Done(已完成):测过人员或验证人员(通常是PM)，确认该Issue正确后，修改状态为Done表示该Issue已经被验证完成，是一个合格的Issue。

Reopened(重新打开): 验证不通过的lssue，修改状态为reopened，表示该问题仍未解决，可以指回给解决人继续解决

Pending(搁置): 无法处理，暂时搁置的问题，修改状态为pending。

Feedback (反馈) : 解决人对问题有疑问的问题，修改状态为Feedback，并指回给reporter.

Cancelled (取消) : 问题单被取消了，但是也可以被再次打开

Approved (审核通过) : 任务审核通过了

Reiected (拒绝) : 任务或者问题被拒绝了

## 6、Resolution 决议

系统默认的问题解决结果（决议）会有以下几种:一个问题可以有多种解决结果，其中只有一种方法是修复。一个解决结果通常会在状态变更时候被设置起来

Fixed - 修复

Unresolved - 未修复的状态

Won't Fix - 不用修复。例如这个问题所描述的现象已不再有影响了。

Duplicate - 重复。同其它已经存在的问题重复了，推荐把相关的单子链接起来lncomplete - 未完成。没有足够的信息继续完成这个问题。

Cannot Reproduce - 不能重现。如果以后有更多信息可以继续可以重新打开这张单子Won't Do - 不做。类似于不用修复的方案，试用于软件项目的默认状态。

# 二、JQL语法

JQL (Jira Query Language) 是一种用于在 Atlassian Jira 中进行高级搜索和过滤的查询语言。它允许用户以灵活的方式构建自定义的查询，以查找、筛选和报告与问题（Issue）相关的数据。

JQL语句一般由`field字段`+`operate操作符`+`value值`组成；

或者是`field字段`+`operate操作符`+`value值` + `keyword关键字`+`方法function`组成

![image-20231025095409297](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231025095409297.png)

- **field (字段) ：** 就是要搜索的JIRA Issue 的各个字段
- **operator(运算符或者也叫操作符)**：如 **=, < , > , in** 等
- **value(值)**：具体要查询的字段匹配的值
- keyword(关键字)：
  - 连接两个表达式，即通常我们所说的逻辑运算符：**AND**， **OR**， **NOT**
  - 排序运算符：**ORDERBY**
  - 还有一部分就是表示空的关键字： **NULL** 和 **EMPTY**，这两个貌似才是通常意义上的关键字
- **function**(**方法**)：即JIRA提供的一些方法，如 **now()**表示当前时间，**currentUser()**表示当前用户等

## 1、field 字段

field是指的是用于搜索和筛选问题的字段。这些字段通常包括内置字段（例如项目、报告人、状态等）和自定义字段（您可以根据项目需求创建的自定义字段）。

| 字段        | 含义     |
| ----------- | -------- |
| project     | 项目     |
| component   | 模块     |
| assignee    | 经办人   |
| priority    | 优先级   |
| labels      | 标签     |
| status      | 状态     |
| resolution  | 解决方案 |
| resolved    | 解决时间 |
| creator     | 创建人   |
| description | 描述     |
| created     | 创建时间 |
| due         | 到期日期 |
| reporter    | 报告人   |
| comment     | 评论     |
| sprint      | 冲刺     |

1. **项目字段 (Project):** 用于指定问题所属的项目，例如，`project = "ProjectName"`。
2. **问题类型字段 (Issue Type):** 用于指定问题的类型，例如，`issuetype = Bug`。
3. **优先级字段 (Priority):** 用于指定问题的优先级，例如，`priority = High`。
4. **状态字段 (Status):** 用于指定问题的当前状态，例如，`status = Open`。
5. **报告人字段 (Reporter):** 用于指定问题的报告人，例如，`reporter = username`。
6. **分配给字段 (Assignee):** 用于指定问题的经办人，例如，`assignee = username`。
7. **标签字段 (Labels):** 用于指定问题的标签，例如，`labels = "labelName"`。
8. **创建日期字段 (Created):** 用于指定问题的创建日期，例如，`created > "2023-01-01"`。
9. **更新日期字段 (Updated):** 用于指定问题的更新日期，例如，`updated >= -1w`（在过去一周内更新的问题）。
10. **自定义字段 (Custom Fields):**Jira还允许您创建自定义字段，并使用它们作为查询条件，例如，`cf[10001] = "customValue"`，其中`10001`是自定义字段的ID。每一个字段都有其唯一的id

## 2、operator 运算符

| 操作       | 含义                             | 适用字段             |
| ---------- | -------------------------------- | -------------------- |
| =          | 精确匹配                         | 数字、日期、优先级等 |
| !=         | 不等于                           | 数字、日期、优先级等 |
| >          | 大于                             | 数字、日期、优先级等 |
| >=         | 大于等于                         | 数字、日期、优先级等 |
| <          | 小于                             | 数字、日期、优先级等 |
| <=         | 小于等于                         | 数字、日期、优先级等 |
| ~          | 包含指定值                       | 文本字段             |
| !~         | 不包含指定值                     | 文本字段             |
| IS         | 值为空（NULL/EMPTY）             | 任何字段             |
| IS NOT     | 值不为空（非 NULL/EMPTY）        | 任何字段             |
| WAS        | 曾经具有指定值                   | 任何字段             |
| WAS NOT    | 从未具有指定值                   | 任何字段             |
| WAS IN     | 曾经具有指定多个值中的任意一个   | 任何字段             |
| WAS NOT IN | 从未具有指定多个值中的任何一个   | 任何字段             |
| IN         | 当前值为指定多个值中的任意一个   | 任何字段             |
| NOT IN     | 当前值不为指定多个值中的任何一个 | 任何字段             |
| CHANGED    | 值曾发生过变化                   | 任何字段             |

## 3、keyword关键字

| 操作     | 含义         | 用法                                                        |
| -------- | ------------ | ----------------------------------------------------------- |
| AND      | 逻辑与       | 使用 AND 连接多个条件以返回同时满足所有条件的问题。         |
| OR       | 逻辑或       | 使用 OR 连接多个条件以返回满足任一条件的问题。              |
| NOT      | 逻辑非       | 使用 NOT 反转条件的含义，返回不满足条件的问题。             |
| EMPTY    | 字段没有值   | 用于检查字段是否为空（没有值）。                            |
| NULL     | 字段没有值   | 与 EMPTY 同义，用于检查字段是否为空。                       |
| ORDER BY | 按某字段排序 | 用于按指定字段的值对问题进行升序（ASC）或降序（DESC）排序。 |

## 4、function 方法

| 函数            | 含义                           |
| --------------- | ------------------------------ |
| currentUser()   | 返回当前登录用户。             |
| endOfDay()      | 返回当天的日终时间。           |
| endOfWeek()     | 返回当前周的周终时间。         |
| endOfMonth()    | 返回当前月的月终时间。         |
| endOfYear()     | 返回当前年的年终时间。         |
| futureSprints() | 返回还未开始的 sprint。        |
| myApproval()    | 返回当前用户作为审批人的问题。 |
| myPending()     | 返回等待当前用户审批的问题。   |
| now()           | 返回当前时间。                 |
| openSprints()   | 返回正在进行中的 sprint。      |
| pending()       | 返回等待审批的问题。           |
| startOfDay()    | 返回当天的日初时间。           |
| startOfWeek()   | 返回当前周的周初时间。         |
| startOfMonth()  | 返回当前月的月初时间。         |
| startOfYear()   | 返回当前年的年初时间。         |

## 5、jql语法案例

下面就可以写一些条件组合了，假设我们有一个名为 Test 的项目

1）当前项目的所有未解决 Issue 且按优先级倒排：`project = Test AND resolution = Unresolved ORDER BY priority DESC`

2）看看老大张麻子现在还有哪些未解决的 Issue： `project = Test AND resolution = Unresolved AND assignee = 张麻子 ORDER BY priority DESC`

3）昨天到今天又新建了多少 Issue： `project = Test AND created >= startOfDay(-1) ORDER BY priority DESC`

4）看看消息中间件通信模块今天有哪些 Issue 到期了： `project = Test AND due = endOfDay() AND component = 消息中间件通信模块 ORDER BY priority DESC`

5）我的待办任务： `project = Test AND resolution = Unresolved AND assignee = currentUser() ORDER BY priority DESC`



# 三、使用Jira库操作Jira API

## 1、安装jira库

在调用Jira API的时候，我们需要先下载第三方库jira

```py
# 安装jira
pip install jira
```

![image-20231025092456396](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231025092456396-16988066691749.png)

## 2、认证登录

Jira的访问是有权限的，在访问Jira项目时首先要进行认证，Jira Python库提供了3种认证方式：

1. 通过Cookis方式认证（用户名，密码）
2. 通过Basic Auth方式认证（用户名，密码）
3. 通过OAuth方式认证

认证方式只需要选择一种即可，以下代码为使用Cookies方式认证。

```python
form jira import JIRA

jira = JIRA('http://jira.***.com/', auth=(username, password))
```

返回的jira对象便可以对Jira进行操作。

## 3、项目操作（Project）

- jira.projects(): 查看所有项目列表
- jira.project("项目的Key"): 查看单个项目

项目对象的主要属性及方法如下：

- key: 项目的Key
- name: 项目名称
- description: 项目描述
- lead: 项目负责人
- projectCategory: 项目分类
- components: 项目组件
- versions: 项目中的版本
- raw: 项目的原始API数据

示例

```python
print(jira.projects())  # 打印所有你有权限访问的项目列表

project = jira.project('某个项目的Key')

print(project.key, project.name, project.lead) 
```

## 4、问题操作（Issue）

Issue是Jira的核心，Jira中的任务，用户Story，Bug实质上都是一个Issue。
单个问题对象可以通过jira.issue("问题的Key")得到，问题的主要属性和方法如下：

- id: 问题的id
- key: 问题的Key
- permalink(): 获取问题连接
- fields: 问题的描述，创建时间等所有的配置域
- raw: 问题的原始API数据

issue中的id和key属性有什么区别：

> 在Jira中，Issue的ID和Key是两个不同的标识符，它们用于唯一标识和引用不同的工作项（Issue）。
>
> Issue ID:
>
> - Issue ID是jira内部唯一的数字标识符，通常自动生成，每个新创建的Issue都会被分配一个唯一的Issue ID，即使Issue的Key被修改或Issue被移动到不同的项目也不会更改
> - ssue ID 对于Jira的用户来说没有特别大的意义，它主要用于Jira数据库内部的唯一标识。
>
> Issue key：
>
> - Issue Key 是一个包含项目前缀和数字的短标识符，通常采用大写字母。如：COSSOW-10075。其中 "COSSOW" 是项目前缀，而 "10075" 是唯一的数字。
> - Issue Key通常用于在Jira中引用和搜索Issue。==它是用户友好的标识符，易于识别和记忆。==



## 5、配置域操作（Fields）

fields配置与包含了大部分事件（issue)基本的属性，问题的fields中的属性分为固定属性和自定义属性，自定义属性格式一般为类似customfield_10012这种。

常用的问题的Fields属性有：

- assignee：经办人
- created: 创建时间
- creator: 创建人
- labels: 标签
- priorit: 优先级
- progress:
- project: 所示项目
- reporter: 报告人
- status: 状态
- summary: 问题描述
- worklog: 活动日志
- updated: 更新时间
- watches: 关注者
- comments: 评论
- resolution: 解决方案
- subtasks: 子任务
- issuelinks: 连接问题
- lastViewed: 最近查看时间
- attachment

示例如下：

```python
issue = jira.issue('JRA-1330')
print(issue.key, issue.fields.summary, issue.fields.status)
```

## 6、issue字段映射表

> issue字段：
>
> id:表示Jira问题的唯一标识符。每个问题都有一个唯一的ID，可用于在Jira中标识问题
> key:问题的唯一键，通常是由项目和数字组成的标识符，用于快速定位问题。
> expend:用于在api请求中指定要获取的域issue相关的附加信息（如获取issue下的评论comments详细信息）
> 	comments:获取评论信息
> 	transitions：获取问题从一个状态改变为另一个状态以反映其在工作流中的进展（issue状态："待办"、"进行中"、"已解决" 等）
> 	operations：获取与问题关联的操作信息
> 	schema：获取关于字段的架构信息，包括字段的类型、系统字段、自定义字段等。
> fields：包含与Jira问题相关的字段数据
> 	摘要（Summary）： 问题的简要描述，通常用于快速了解问题内容。
> 描述（Description）： 问题的详细描述，包括更多的信息和上下文。
> 报告人（Reporter）： 报告问题的用户或团队。
> 受理人（Assignee）： 负责解决问题的用户或团队。
> 优先级（Priority）： 问题的紧急程度，通常分为高、中、低等级别。
> 状态（Status）： 问题的当前状态，如待办、进行中、已解决等。
> 解决版本（Fix Version）： 问题解决后所属的版本。
> 影响版本（Affects Version）： 受问题影响的版本。
> 标签（Labels）： 用于对问题进行分类和标记的关键词。
> 组件（Components）： 问题所属的组件或模块。
> 自定义字段（Custom Fields）： 根据项目需求自定义的字段，可以包括文本字段、选择列表、日期字段等。
> 报告日期（Created）： 问题创建的日期和时间。
> 更新日期（Updated）： 问题最后更新的日期和时间。
> 截止日期（Due Date）： 问题的截止日期，如果适用。
> 工作日志（Worklogs）： 记录与问题相关的工作日志，包括工作时间和描述。
> 附件（Attachments）： 与问题相关的附件，如截图、文档等。
> self:资源的URL链接，允许直接访问该资源的详细信息

| issue字段      | 描述/子字段                                                  | 子字段描述                                                   |
| :------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| id             | Jira中唯一且不变的问题标识符，只起到标识一个问题的作用       |                                                              |
| key            | 通常是由项目和数字组成的标识符，比较通俗易懂。会随着问题的移动改变 |                                                              |
| expend（拓展） | 用于在api请求中指定要获取的域issue相关的附加信息             |                                                              |
|                | comments                                                     | 评论信息                                                     |
|                | transitions                                                  | 获取问题从一个状态改变为另一个状态以反映其在工作流中的进展   |
|                | operations                                                   | 获取与问题关联的操作信息                                     |
|                | schema                                                       | 获取关于字段的架构信息，包括字段的类型、系统字段、自定义字段等。 |
| fields         | 包含与Jira问题相关的字段数据                                 |                                                              |
|                | 摘要（Summary）                                              | 问题的简要描述，通常用于快速了解问题内容。                   |
|                | 描述（Description）                                          | 问题的详细描述，包括更多的信息和上下文。                     |
|                | 报告人（Reporter）                                           | 报告问题的用户或团队。                                       |
|                | 受理人（Assignee）                                           | 负责解决问题的用户或团队                                     |
|                | 优先级（Priority）                                           | 问题的紧急程度，通常分为高、中、低等级别。                   |
|                | 状态（Status）                                               | 问题的当前状态，如待办、进行中、已解决等。                   |
|                | 解决版本（Fix Version）                                      | 问题解决后所属的版本。                                       |
|                | 影响版本（Affects Version）                                  | 受问题影响的版本。                                           |
|                | 标签（Labels）                                               | 用于对问题进行分类和标记的关键词。                           |
|                | 组件（Components）                                           | 问题所属的组件或模块。                                       |
|                | 自定义字段（Custom Fields）                                  | 根据项目需求自定义的字段，可以包括文本字段、选择列表、日期字段等。 |
|                | 报告日期（Created）                                          | 问题创建的日期和时间。                                       |
|                | 更新日期（Updated）                                          | 问题最后更新的日期和时间。                                   |
|                | 截止日期（Due Date）                                         | 问题的截止日期，如果适用。                                   |
|                | 工作日志（Worklogs）                                         | 记录与问题相关的工作日志，包括工作时间和描述。               |
|                | 附件（Attachments）                                          | 与问题相关的附件，如截图、文档等。                           |
| self（链接）   | 资源的URL链接，允许直接访问该资源的详细信息                  |                                                              |

## 7、实例

首先我们创建一个Jira对象

```py
from jira import JIRA

jira = JIRA(server='http://jira.jaguarmicro.com', basic_auth=('username', 'password'))  # 传入用户名密码
```

调用对象的projects()方法，可以输出公司所有项目：

```py
projects = jira.projects()  # 获取公司所有的项目
print(projects) # 输出公司的所有项目列表
```

编写JQL语句，调用对象方法search_issues返回jql语法查询到的问题（issue）对象列表

```py
jql = 'project = "Corsica-SW"'  # 查找项目名称为Corsica-SW的项目，并将语句存入变量jql
issues = jira.search_issues(jql, fields='')  # 使用search_issues()方法返回jql搜索的所有issue，fields参数是Jira的search_issues方法的一个参数，用于指定在搜索Jira时要返回fields内的哪个属性。为空表示返回全部属性
```

遍历输出获取到的issue对象名称和属性内容

```py
for i in issues:  
     print(i)  # 输出issue的名称
     print(dir(i))  # dir()方法，返回issue的所有属性和方法
     print(dir(i.fields)) # 这个fields和上面的fields不是一个概念，i.fields返回表示issue的fields属性，dir(i.fields)返回i的fields属性的属性
```

输出内容：

![image-20231025095249212](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231025095249212.png)

综合代码：

```py
from jira import JIRA

jira = JIRA(server='http://jira.jaguarmicro.com', basic_auth=('username', 'password'))

projects = jira.projects()  # 获取公司所有的项目
print(projects) # 输出公司的所有项目列表

jql = 'project = "Corsica-SW"'  # 查找项目名称为Corsica-SW的项目，并将语句存入变量jql
issues = jira.search_issues(jql, fields='')  # 使用search_issues()方法返回jql搜索的所有issue，fields参数是Jira的search_issues方法的一个参数，用于指定在搜索Jira时要返回fields内的哪个属性。为空表示返回全部属性
for i in issues:  
     print(i)  # 输出issue的名称
     print(dir(i))  # dir()方法，返回issue的所有属性和方法
     print(dir(i.fields)) # 这个fields和上面的fields不是一个概念，i.fields返回表示issue的fields属性，dir(i.fields)返回i的fields属性的属性
```

当然我们也可以使用issue的fields属性方法获取到issue的详细信息

```py
issue.fields.assignee 获取当前issue的处理者
issue.fields.attachment 取当前issue的附件
issue.fields.comment 获取当前issue的评论
issue.fields.created 获取当前issue的创建时间
issue.fields.description 获取当前issue的描述
issue.fields.duedate 获取当前issue的截止时间
issue.fields.issuelinks 获取当前issue的链接ID(注意：此处获取的并不是相关联的issue 而是链接的ID)
issue.fields.issuetype 获取当前issue的类型
issue.fields.labels 获取当前issue的标签
issue.fields.priority 获取当前issue的优先级
issue.fields.project 获取当前issue所属项目名
issue.fields.reporter 获取当前issue的报告者
issue.fields.resolution 获取当前issue的解决状态(Unresolved, closed...)
issue.fields.status 获取当前issue的状态(Open, Resolved, Tested, Decision, Implementing, Analyzing, Testing, Review, Closed)
issue.fields.summary 获取当前issue的标题
issue.fields.versions 获取当前issue的版本
issue.fields.votes 获取当前issue的票数
issue.fields.watchers 获取当前issue的关注者
issue.fields.worklog 获取当前issue的日志
```

有了如上方法，我们可以创建一个简单的Jira操作类，方便我们获取到issue的信息

```py
class JiraOperate():
    def __int__(self, user_name, password, server_link, issue):  # python的类属性直接在init方法里面创建即可
        self.jira = JIRA(server=server_link, basic_auth=(user_name, password))  # 初始化登录jira
        self.issue = self.jira.issue(issue)  # 初始化操作的事件issue

    def get_summay(self):  # 获取issue的标题
        issue_summary = self.issue.fields.summary
        return issue_summary

    def get_description(self):  # 获取issue的描述
        issue_description = self.issue.fields.description
        return issue_description

    def get_reporter(self):  # 获取issue的提交人
        bug_tester = str(self.issue.fields.reporter)
        blank_pos = bug_tester.find(" ")
        issue_reporter = bug_tester[:blank_pos]
        return issue_reporter

    def get_issuetype(self):  # 获取issue的类型
        issue_type = str(self.issue.fields.issuetype)
        return issue_type
```

到这里，你们可能会问，为什么issue的信息都需要调用field属性才可以获得呢，这里我们讲一下issue的fields属性

issue的主要信息都包含在fields属性内，如

- assignee：经办人
- created: 创建时间
- creator: 创建人
- labels: 标签
- priorit: 优先级
- progress:
- project: 所示项目
- reporter: 报告人
- status: 状态
- summary: 问题描述
- worklog: 活动日志
- updated: 更新时间
- watches: 关注者
- comments: 评论
- resolution: 解决方案
- subtasks: 子任务
- issuelinks: 连接问题
- lastViewed: 最近查看时间
- attachment

因此直接使用fields调用属性就可以获得它的信息

```py
issue = jira.issue('JRA-1330')
print(issue.key, issue.fields.summary, issue.fields.status)
```

# 四、使用requests库调用Jira API

Jira Clould Rest API文档：[The Jira Cloud platform REST API (atlassian.com)](https://developer.atlassian.com/cloud/jira/platform/rest/v2/api-group-issue-fields/#api-rest-api-2-field-get)

Confluence Rest API文档：[REST API - TestRay for Jira DC/Server (formerly synapseRT) - Confluence (atlassian.net)](https://goldfingerholdings.atlassian.net/wiki/spaces/SRT/pages/25985678/REST+API)

## 1、Jira API结构

JIRA的REST API通过URI路径提供对资源（数据实体）的访问。  JIRA REST API使用JSON作为其通信格式，以及标准的HTTP方法，如GET，PUT，POST和DELETE。

```json
http://host:port/context/rest/api-name/api-version/resource-name
```

- host:port/context : JIRA的访问URL
- rest : 固定的rest
- api-name 有两个值
  - auth：对于与认证相关的操作
  - api: 其他api操作，通常情况下都是用的 api
- api-version : 目前的版本是2, 也可以用latest

## 2、Requests方式鉴权

首先导入requests库

使用HTTPBasicAuth方法将username和password转码成http可识别的字符，传入到auth鉴权

使用requests方法传入要访问的url地址和auth鉴权码

```py
import requests
from requests.auth import HTTPBasicAuth

api_url = "https://your-domain.atlassian.net/rest/api/2/issue"  # 根据官方文档查询到的对应功能的url
username = 'xxx'
password = 'xxx'

base_auth = HTTPBasicAuth(username, password)  # auth认证

response = requests.get(api_url, auth=base_auth)  # 请求jira url
```

## 3、实例

以下是使用requests调用jira api 创建issue的案例

```py
import requests
from requests.auth import HTTPBasicAuth
import json

url = "https://your-domain.atlassian.net/rest/api/2/issue"  # 根据官方文档查询到的对应功能的url
username = 'xxx'
password = 'xxx'

auth = HTTPBasicAuth(username, password)  # auth认证

headers = {  # 设置请求头
  "Accept": "application/json",
  "Content-Type": "application/json"
}

payload = json.dumps( {  # 请求json格式内容
  "fields": {
    "assignee": {
      "id": "5b109f2e9729b51b54dc274d"
    },
    "components": [
      {
        "id": "10000"
      }
    ],
    "customfield_10000": "09/Jun/19",
    "customfield_20000": "06/Jul/19 3:25 PM",
    "customfield_30000": [
      "10000",
      "10002"
    ],
    "customfield_40000": "Occurs on all orders",
    "customfield_50000": "Could impact day-to-day work.",
    "customfield_60000": "jira-software-users",
    "customfield_70000": [
      "jira-administrators",
      "jira-software-users"
    ],
    "customfield_80000": {
      "value": "red"
    },
    "description": "Order entry fails when selecting supplier.",
    "duedate": "2019-03-11",
    "environment": "UAT",
    "fixVersions": [
      {
        "id": "10001"
      }
    ],
    "issuetype": {
      "id": "10000"
    },
    "labels": [
      "bugfix",
      "blitz_test"
    ],
    "parent": {
      "key": "PROJ-123"
    },
    "priority": {
      "id": "20000"
    },
    "project": {
      "id": "10000"
    },
    "reporter": {
      "id": "5b10a2844c20165700ede21g"
    },
    "security": {
      "id": "10000"
    },
    "summary": "Main order flow broken",
    "timetracking": {
      "originalEstimate": "10",
      "remainingEstimate": "5"
    },
    "versions": [
      {
        "id": "10000"
      }
    ]
  },
  "update": {
    "worklog": [
      {
        "add": {
          "started": "2019-07-05T11:05:00.000+0000",
          "timeSpent": "60m"
        }
      }
    ]
  }
} )

response = requests.request(  # 传入拼接好的请求内容，获取相应
   "POST",
   url,
   data=payload,
   headers=headers,
   auth=auth
)

print(json.dumps(json.loads(response.text), sort_keys=True, indent=4, separators=(",", ": ")))  # 将获取到的响应内容转化为可读的json格式并输出
```

# 五、使用JQL和Requests方式调用Jira API

jira 提供了一个搜索url,用于jql语句的搜索

```py
{base_url}/rest/api/2/search?jql={jql语句}
```

我们可以将需要搜索的jql语法拼接在url语句后，使用requests获取搜索到的内容，如下

编写jql语句获取项目名下的所有测试计划类型的issue

```py
jql=project={project_name} AND issuetype = "测试计划"'
```

将其拼接在jira api通过的url后，如下：

```py
api_url = f'{base_url}/rest/api/2/search?jql=project={project_name} AND issuetype = "测试计划"'
```

使用requests获取到响应,并打印出来

```py
auth = HTTPBasicAuth(username, password)
response = requests.get(api_url, auth=auth)  # 传入url和鉴权auth
print(response.text)  # 以文本的形式打印响应内容
```

获取到的项目下的测试计划：

```json
{
	"expand": "operations,versionedRepresentations,editmeta,changelog,renderedFields",
	"fields": {
		"aggregateprogress": {
			"progress": 0,
			"total": 0
		},
		"aggregatetimeestimate": null,
		"aggregatetimeoriginalestimate": null,
		"aggregatetimespent": null,
		"assignee": {
			"active": true,
			"avatarUrls": {
				"16x16": "http://jira.jaguarmicro.com/secure/useravatar?size=xsmall&ownerId=JIRAUSER10615&avatarId=10900",
				"24x24": "http://jira.jaguarmicro.com/secure/useravatar?size=small&ownerId=JIRAUSER10615&avatarId=10900",
				"32x32": "http://jira.jaguarmicro.com/secure/useravatar?size=medium&ownerId=JIRAUSER10615&avatarId=10900",
				"48x48": "http://jira.jaguarmicro.com/secure/useravatar?ownerId=JIRAUSER10615&avatarId=10900"
			},
			"displayName": "Larry Cheng",
			"emailAddress": "larry.cheng@jaguarmicro.com",
			"key": "JIRAUSER10615",
			"name": "larry.cheng",
			"self": "http://jira.jaguarmicro.com/rest/api/2/user?username=larry.cheng",
			"timeZone": "Asia/Shanghai"
		},
		"components": [{
			"description": "Corsica芯片RDMA组/RDMA相关开发任务",
			"id": "10155",
			"name": "RDMA",
			"self": "http://jira.jaguarmicro.com/rest/api/2/component/10155"
		}],
		"created": "2023-03-10T09:53:33.000+0800",
		"creator": {
			"active": true,
			"avatarUrls": {
				"16x16": "http://jira.jaguarmicro.com/secure/useravatar?size=xsmall&avatarId=10122",
				"24x24": "http://jira.jaguarmicro.com/secure/useravatar?size=small&avatarId=10122",
				"32x32": "http://jira.jaguarmicro.com/secure/useravatar?size=medium&avatarId=10122",
				"48x48": "http://jira.jaguarmicro.com/secure/useravatar?avatarId=10122"
			},
			"displayName": "Chozy Zhao",
			"emailAddress": "chozy.zhao@jaguarmicro.com",
			"key": "JIRAUSER10806",
			"name": "chozy.zhao",
			"self": "http://jira.jaguarmicro.com/rest/api/2/user?username=chozy.zhao",
			"timeZone": "Asia/Shanghai"
		},
		"customfield_10000": "{summaryBean=com.atlassian.jira.plugin.devstatus.rest.SummaryBean@206d8fea[summary={pullrequest=com.atlassian.jira.plugin.devstatus.rest.SummaryItemBean@53e4fd11[overall=PullRequestOverallBean{stateCount=0, state='OPEN', details=PullRequestOverallDetails{openCount=0, mergedCount=0, declinedCount=0}},byInstanceType={}], build=com.atlassian.jira.plugin.devstatus.rest.SummaryItemBean@582bcf1a[overall=com.atlassian.jira.plugin.devstatus.summary.beans.BuildOverallBean@2cd3ad7b[failedBuildCount=0,successfulBuildCount=0,unknownBuildCount=0,count=0,lastUpdated=<null>,lastUpdatedTimestamp=<null>],byInstanceType={}], review=com.atlassian.jira.plugin.devstatus.rest.SummaryItemBean@323e1f74[overall=com.atlassian.jira.plugin.devstatus.summary.beans.ReviewsOverallBean@e72710a[stateCount=0,state=<null>,dueDate=<null>,overDue=false,count=0,lastUpdated=<null>,lastUpdatedTimestamp=<null>],byInstanceType={}], deployment-environment=com.atlassian.jira.plugin.devstatus.rest.SummaryItemBean@8a73cc1[overall=com.atlassian.jira.plugin.devstatus.summary.beans.DeploymentOverallBean@39aee0db[topEnvironments=[],showProjects=false,successfulCount=0,count=0,lastUpdated=<null>,lastUpdatedTimestamp=<null>],byInstanceType={}], repository=com.atlassian.jira.plugin.devstatus.rest.SummaryItemBean@7b299d25[overall=com.atlassian.jira.plugin.devstatus.summary.beans.CommitOverallBean@664e496b[count=0,lastUpdated=<null>,lastUpdatedTimestamp=<null>],byInstanceType={}], branch=com.atlassian.jira.plugin.devstatus.rest.SummaryItemBean@57be2057[overall=com.atlassian.jira.plugin.devstatus.summary.beans.BranchOverallBean@1958ba43[count=0,lastUpdated=<null>,lastUpdatedTimestamp=<null>],byInstanceType={}]},errors=[],configErrors=[]], devSummaryJson={\"cachedValue\":{\"errors\":[],\"configErrors\":[],\"summary\":{\"pullrequest\":{\"overall\":{\"count\":0,\"lastUpdated\":null,\"stateCount\":0,\"state\":\"OPEN\",\"details\":{\"openCount\":0,\"mergedCount\":0,\"declinedCount\":0,\"total\":0},\"open\":true},\"byInstanceType\":{}},\"build\":{\"overall\":{\"count\":0,\"lastUpdated\":null,\"failedBuildCount\":0,\"successfulBuildCount\":0,\"unknownBuildCount\":0},\"byInstanceType\":{}},\"review\":{\"overall\":{\"count\":0,\"lastUpdated\":null,\"stateCount\":0,\"state\":null,\"dueDate\":null,\"overDue\":false,\"completed\":false},\"byInstanceType\":{}},\"deployment-environment\":{\"overall\":{\"count\":0,\"lastUpdated\":null,\"topEnvironments\":[],\"showProjects\":false,\"successfulCount\":0},\"byInstanceType\":{}},\"repository\":{\"overall\":{\"count\":0,\"lastUpdated\":null},\"byInstanceType\":{}},\"branch\":{\"overall\":{\"count\":0,\"lastUpdated\":null},\"byInstanceType\":{}}}},\"isStale\":true}}",
		"customfield_10100": "0|i072db:",
		"customfield_10101": null,
		"customfield_10102": null,
		"customfield_10317": null,
		"customfield_10320": null,
		"customfield_10322": null,
		"customfield_10323": null,
		"customfield_10325": null,
		"customfield_10327": {
			"disabled": false,
			"id": "10111",
			"self": "http://jira.jaguarmicro.com/rest/api/2/customFieldOption/10111",
			"value": "需求变更"
		},
		"customfield_10332": null,
		"customfield_10340": null,
		"customfield_10341": null,
		"customfield_10342": {
			"disabled": false,
			"id": "10149",
			"self": "http://jira.jaguarmicro.com/rest/api/2/customFieldOption/10149",
			"value": "芯片概要设计文档"
		},
		"customfield_10405": null,
		"customfield_10406": null,
		"customfield_10407": null,
		"customfield_10408": null,
		"customfield_10409": null,
		"customfield_10410": null,
		"customfield_10411": null,
		"customfield_10412": null,
		"customfield_10413": null,
		"customfield_10414": null,
		"customfield_10415": null,
		"customfield_10416": null,
		"customfield_10431": null,
		"customfield_10435": null,
		"customfield_10442": null,
		"customfield_10464": null,
		"customfield_10465": null,
		"customfield_10466": null,
		"customfield_10484": null,
		"customfield_10485": null,
		"customfield_10486": null,
		"customfield_10499": null,
		"customfield_10500": "保留既往jira信息显示，无需编辑，内容编辑请添加到<描述>一栏当中",
		"customfield_10502": "保留既往jira信息显示，无需编辑，内容编辑请添加到<描述>一栏当中",
		"customfield_10600": {
			"disabled": false,
			"id": "10647",
			"self": "http://jira.jaguarmicro.com/rest/api/2/customFieldOption/10647",
			"value": "未定义"
		},
		"customfield_10605": {
			"disabled": false,
			"id": "10649",
			"self": "http://jira.jaguarmicro.com/rest/api/2/customFieldOption/10649",
			"value": "未定义"
		},
		"customfield_10606": {
			"disabled": false,
			"id": "10645",
			"self": "http://jira.jaguarmicro.com/rest/api/2/customFieldOption/10645",
			"value": "未定义"
		},
		"customfield_10607": {
			"disabled": false,
			"id": "10646",
			"self": "http://jira.jaguarmicro.com/rest/api/2/customFieldOption/10646",
			"value": "未定义"
		},
		"customfield_10609": null,
		"customfield_10610": null,
		"customfield_10611": null,
		"customfield_10612": "http://null",
		"customfield_10613": "http://null",
		"customfield_10614": "http://null",
		"customfield_10615": "http://null",
		"customfield_10700": null,
		"customfield_10701": null,
		"customfield_10702": null,
		"customfield_10703": null,
		"customfield_10704": null,
		"customfield_10800": null,
		"customfield_10900": null,
		"description": null,
		"duedate": null,
		"environment": null,
		"fixVersions": [],
		"issuelinks": [],
		"issuetype": {
			"description": "For Go2Group SYNAPSE Test Plan issue type",
			"iconUrl": "http://jira.jaguarmicro.com/download/resources/com.go2group.jira.plugin.synapse/synapse/images/icon-testplan.png",
			"id": "10424",
			"name": "测试计划",
			"self": "http://jira.jaguarmicro.com/rest/api/2/issuetype/10424",
			"subtask": false
		},
		"labels": [],
		"lastViewed": null,
		"priority": {
			"iconUrl": "http://jira.jaguarmicro.com/images/icons/priorities/highest.svg",
			"id": "1",
			"name": "Highest",
			"self": "http://jira.jaguarmicro.com/rest/api/2/priority/1"
		},
		"progress": {
			"progress": 0,
			"total": 0
		},
		"project": {
			"avatarUrls": {
				"16x16": "http://jira.jaguarmicro.com/secure/projectavatar?size=xsmall&avatarId=10324",
				"24x24": "http://jira.jaguarmicro.com/secure/projectavatar?size=small&avatarId=10324",
				"32x32": "http://jira.jaguarmicro.com/secure/projectavatar?size=medium&avatarId=10324",
				"48x48": "http://jira.jaguarmicro.com/secure/projectavatar?avatarId=10324"
			},
			"id": "10208",
			"key": "COSSW",
			"name": "Corsica-SW",
			"projectTypeKey": "software",
			"self": "http://jira.jaguarmicro.com/rest/api/2/project/10208"
		},
		"reporter": {
			"active": true,
			"avatarUrls": {
				"16x16": "http://jira.jaguarmicro.com/secure/useravatar?size=xsmall&avatarId=10122",
				"24x24": "http://jira.jaguarmicro.com/secure/useravatar?size=small&avatarId=10122",
				"32x32": "http://jira.jaguarmicro.com/secure/useravatar?size=medium&avatarId=10122",
				"48x48": "http://jira.jaguarmicro.com/secure/useravatar?avatarId=10122"
			},
			"displayName": "Chozy Zhao",
			"emailAddress": "chozy.zhao@jaguarmicro.com",
			"key": "JIRAUSER10806",
			"name": "chozy.zhao",
			"self": "http://jira.jaguarmicro.com/rest/api/2/user?username=chozy.zhao",
			"timeZone": "Asia/Shanghai"
		},
		"resolution": null,
		"resolutiondate": null,
		"status": {
			"description": "",
			"iconUrl": "http://jira.jaguarmicro.com/images/icons/statuses/open.png",
			"id": "1",
			"name": "新建",
			"self": "http://jira.jaguarmicro.com/rest/api/2/status/1",
			"statusCategory": {
				"colorName": "blue-gray",
				"id": 2,
				"key": "new",
				"name": "待办",
				"self": "http://jira.jaguarmicro.com/rest/api/2/statuscategory/2"
			}
		},
		"subtasks": [],
		"summary": "RDMA基础用例测试",
		"timeestimate": null,
		"timeoriginalestimate": null,
		"timespent": null,
		"updated": "2023-03-10T09:53:33.000+0800",
		"versions": [],
		"votes": {
			"hasVoted": false,
			"self": "http://jira.jaguarmicro.com/rest/api/2/issue/COSSW-27773/votes",
			"votes": 0
		},
		"watches": {
			"isWatching": false,
			"self": "http://jira.jaguarmicro.com/rest/api/2/issue/COSSW-27773/watchers",
			"watchCount": 1
		},
		"workratio": -1
	},
	"id": "51613",
	"key": "COSSW-27773",
	"self": "http://jira.jaguarmicro.com/rest/api/2/issue/51613"
}
```



## 1、案例：通过项目名称获取所有的测试计划

```py
api_url = f'{base_url}/rest/api/2/search?jql=project={project_name} AND issuetype = "测试计划"'
```

编写一个函数使用Requests获取到项目名称下的所有测试计划

```py
def get_all_testplan_by_project_name(base_url, auth, project_name):  # 通过项目名称以json的格式返回项目对应的所有的测试计划
    api_url = f'{base_url}/rest/api/2/search?jql=project={project_name} AND issuetype = "测试计划"'

    try:
        response = requests.get(api_url, auth=auth)

        if response.status_code == 200:

            testplan_infos = json.dumps(json.loads(response.text), sort_keys=True, indent=4,
                                        ensure_ascii=False)  # 转化为json换行的可读形式
            return testplan_infos

        else:  # 请求失败，返回错误码
            print(f"请求失败，HTTP状态码: {response.status_code}")
            return None

    except requests.exceptions.RequestException as e:  # 抛出错误信息
        print(f"请求发生错误: {e}")
        return None
base_url = "xxx"
username = "xxx"
password = "xxx"
auth = HTTPBasicAuth(username, password)

print(get_all_testplan_by_project_name(base_url, auth, project_name))
```

## 2、案例：通过测试计划获取里面的测试用例

```py
api_url = f"{self.base_url}/rest/api/2/search?jql=summary ~ {testplan_name}"
```

编写一个函数使用Requests获取到项目名称下的所有测试计划

```py
def get_all_issue_by_testplan_name(base_url, auth, testplan_name):  # 通过测试计划名称获取里面的测试用例

    api_url = f"{base_url}/rest/api/2/search?jql=summary ~ {testplan_name}"

    try:
        # 发送 GET 请求到 Jira API
        response = requests.get(api_url, auth=auth)

        # 检查响应状态码
        if response.status_code == 200:
            # 解析 JSON 响应
            project_info = json.loads(response.text)  # 将结果转化为json的形式，但不会换行

            formatted_project_info = json.dumps(project_info, indent=4,
                                                ensure_ascii=False)  # 转化为json可读的格式，ensure_ascii=False表示不转义汉字
            return formatted_project_info
        else:
            print(f"请求失败，HTTP状态码: {response.status_code}")
            return None
    except requests.exceptions.RequestException as e:
        print(f"请求发生错误: {e}")
        return None

base_url = "xxx"
username = "xxx"
password = "xxx"
auth = HTTPBasicAuth(username, password)

print(get_all_issue_by_testplan_name(base_url, auth, testplan_name))

```

由于以上两个案例具有一定的相似性，我们可以创建一个jiraopreation类以便更好的调用以上方法

```py
import requests
from requests.auth import HTTPBasicAuth
import json

#  创建类JiraOperater
class JiraOperater:
    base_url = None
    project_key = None
    username = None
    password = None
    headers = None
    auth = None

    def __init__(self, base_url, username, password):  # 初始化属性值
        self.base_url = base_url
        self.username = username
        self.password = password
        self.auth = HTTPBasicAuth(username, password)
        self.headers = {
            "Accept": "application/json"
        }

    def get_all_testplan_by_project_name_new(self, project_name):  # 通过项目名称以集合的形式返回项目对应的所有的测试计划
        api_url = f'{self.base_url}/rest/api/2/search?jql=project={project_name} AND issuetype = "测试计划"'

        try:
            response = requests.get(api_url, auth=self.auth)

            if response.status_code == 200:

                testplan_infos = json.dumps(json.loads(response.text), sort_keys=True, indent=4,
                                            separators=(",", ": "))  # 转化为json换行的可读形式
                json_data = testplan_infos

                data_dict = json.loads(json_data)  # 将测试计划信息转化为json格式

                issues = data_dict['issues']  # 获取测试计划信息内所有issues的value值

                results = {}  # 创建一个字典由于接收 key:summary
                for issue in issues:  # 将issues遍历输出，提取内部的summary

                    summary_value = issue['fields']['summary']  # 获取fieldss属性内的summary
                    key_value = issue['key']
                    results[key_value] = summary_value  # 将summary和key重新拼接成json格式放在results字典内

                return results  # 返回字典
                # return testplan_infos
            else:  # 请求失败，返回错误码
                print(f"请求失败，HTTP状态码: {response.status_code}")
                return None

        except requests.exceptions.RequestException as e:  # 抛出错误信息
            print(f"请求发生错误: {e}")
            return None

    def get_all_testplan_by_project_name(self, project_name):  # 通过项目名称以json的格式返回项目对应的所有的测试计划
        api_url = f'{self.base_url}/rest/api/2/search?jql=project={project_name} AND issuetype = "测试计划"'

        try:
            response = requests.get(api_url, auth=self.auth)

            if response.status_code == 200:

                testplan_infos = json.dumps(json.loads(response.text), sort_keys=True, indent=4,
                                            ensure_ascii=False)  # 转化为json换行的可读形式
                return testplan_infos

            else:  # 请求失败，返回错误码
                print(f"请求失败，HTTP状态码: {response.status_code}")
                return None

        except requests.exceptions.RequestException as e:  # 抛出错误信息
            print(f"请求发生错误: {e}")
            return None

    def get_all_issue_by_testplan_name(self, testplan_name):  # 通过测试计划名称获取里面的测试用例

        api_url = f"{self.base_url}/rest/api/2/search?jql=summary ~ {testplan_name}"

        try:
            # 发送 GET 请求到 Jira API
            response = requests.get(api_url, auth=self.auth)

            # 检查响应状态码
            if response.status_code == 200:
                # 解析 JSON 响应
                project_info = json.loads(response.text)  # 将结果转化为json的形式，但不会换行

                formatted_project_info = json.dumps(project_info, indent=4,
                                                    ensure_ascii=False)  # 转化为json可读的格式，ensure_ascii=False表示不转义汉字
                return formatted_project_info
            else:
                print(f"请求失败，HTTP状态码: {response.status_code}")
                return None
        except requests.exceptions.RequestException as e:
            print(f"请求发生错误: {e}")
            return None

    def get_issue_in_cycle(self, cycle_name):  # 通过项目名称获取测试计划里的测试用例

        api_url = f"{self.base_url}/rest/api/2/search?jql=issueType='测试周期' AND summary ~ {cycle_name}"

        try:
            # 发送 GET 请求到 Jira API
            response = requests.get(api_url, auth=self.auth)

            # 检查响应状态码
            if response.status_code == 200:
                # 解析 JSON 响应

                project_info = json.dumps(json.loads(response.text), sort_keys=True, indent=4,
                                          ensure_ascii=False)  # 转化为json换行的可读形式

                return project_info
            else:
                print(f"请求失败，HTTP状态码: {response.status_code}")
                return None
        except requests.exceptions.RequestException as e:
            print(f"请求发生错误: {e}")
            return None

#  测试方法main()
if __name__ == '__main__':
    base_url = "xxx"
    username = "xxx"
    password = "xxx"
    auth = HTTPBasicAuth(username, password)
    jira = JiraOperater(base_url, username, password)
    issues = jira.get_all_issue_by_testplan_name("autoTestPlanDemo")  # 调用get_jira_issue_in_testplan_by_project_name()方法获取jira的测试计划内的用例
    testplans = jira.get_all_testplan_by_project_name("Corsica-SW")
    testplans1 = jira.get_all_testplan_by_project_name_new("Corsica-SW")  # 以json格式输出testplan，更具美观性
    print(f"测试用例：{issues}")
    print(f"测试计划：{testplans}")
    print(f"测试计划：{testplans1}")


```

## 3、更新JIra里面指定ID的测试用例

jira更新用例状态status

| 选项           | status |
| -------------- | ------ |
| TO前功能未交付 | 6      |
| 不可测         | 7      |
| TO后功能未交付 | 8      |
| 通过           | 11     |
| 失败           | 2      |
| 锁定           | 3      |
| 未执行         | 4      |
| 不适用         | 5      |

![image-20231109115916702](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109115916702.png)

```py
import requests
from requests.auth import HTTPBasicAuth
import json


class JiraLibrary():

    def __init__(self, base_url, username, password):  # 初始化属性值
        self.base_url = base_url
        self.username = username
        self.password = password
        self.auth = HTTPBasicAuth(username, password)

    # 通过测试用例的run_id更新用例状态
    def update_testrun_by_run_id(self, run_id, status_id):

        api_url = f"{self.base_url}/rest/synapse/1.0/testRun/updateTestRunStatus?runId={run_id}&status={status_id}"
        headers = {

            "Content-Type": "application/json"
        }
        try:
            response = requests.put(api_url, headers=headers, auth=self.auth)

            if response.status_code == 200:
                # 解析 JSON 响应
                project_info = "用例更新成功！"
                return project_info
            else:
                print(f"请求失败，HTTP状态码: {response.status_code}")
                return None
        except requests.exceptions.RequestException as e:
            print(f"请求发生错误: {e}")
            return None

if __name__ == '__main__':
   username = "wenxuan.qiu"
     password = "Njpj#04125617"
     base_url = "http://jira.jaguarmicro.com"
     run_id = "264499"

     result = jira.update_testrun_by_run_id(run_id, status_id=7)  # 传入test_run,和更新后的status，更新测试用例
     print(result)

```

