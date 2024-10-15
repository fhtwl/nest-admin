# 环境搭建

## NestJs CLI安装

使用**NestJs**需要确保当前**Node.js**版本>=16

```bash
$node -v
v20.15.0
$ npm -v
8.x.x

```

全局安装**NestJs CLI**

```bash
npm i -g @nestjs/cli

# 如果因为网络问题无法安装，可以考虑使用淘宝源
npm i -g @nestjs/cli --registry https://registry.npm.taobao.org
```

此时，我们可以通过查询本地的`NestJs CLI`版本号来判断是否安装成功

```sh
nest -v
# 10.4.5
```

我们还可以通过`nest info`或者`nest i`来查询当前`NestJs`的版本号

```sh
[System Information]
OS Version     : Windows 10.0.19045
NodeJS Version : v20.15.1
PNPM Version    : 9.12.1

[Nest CLI]
Nest CLI Version : 10.4.5

[Nest Platform Information]
platform-express version : 10.4.4
schematics version       : 10.1.4
testing version          : 10.4.4
common version           : 10.4.4
core version             : 10.4.4
cli version              : 10.4.5
```

以下是一些比较常用的指令：

|   指令   | 别名 |         作用         |
| :------: | :--: | :------------------: |
|   new    |  n   |   生成Nest应用程序   |
|   info   |  i   | 显示Nest项目详细信息 |
|  start   |      |   运行Nest应用程序   |
|  build   |      |   编译Nest应用程序   |
| generate |  g   |       生成资源       |

更多指令可以执行`nest --help`, 这里不再赘述

## 使用`NestJs CLI`创建一个新项目

```bash
nest new nest-admin --strict
```

`nest-admin` 是项目名称，`--strict` 标志会传递给 `nest new` 命令 , 新项目启用所有 `strict mode` 系列选项 , `strict mode` 支持广泛的类型检查行为，从而更有力地保证程序的正确性. 笔者强烈建议您开启这个选项。

`nest new` 命令执行时您可以选择您熟悉的包管理工具。笔者这里将会选择 **pnpm**，并建议您也这样选择

如果顺利，`Nest CLI`会创建目录，安装 `node modules`、配置文件和一些其他样板文件, 以及根模块，也就是`Nest`程序的入口文件

> 如果因为网络问题安装失败，您可以使用 `pnpm i --registry https://registry.npm.taobao.org`命令重新安装。后续网络问题均如此，不在赘述。

## `NestJs CLI`配置文件

使用`NestJs CLI`创建项目，项目根目录下会自动生成一个`nest-cli.json`

```json
{
  "$schema": "https://json.schemastore.org/nest-cli",
  "collection": "@nestjs/schematics",
  "root": "src"
}
```

|      属性       |                                   作用                                    |
| :-------------: | :-----------------------------------------------------------------------: |
|   collection    |               指向用于生成组件的原理图集合；（最好不要改）                |
|   sourceRoot    | 指向标准模式结构中单个项目的源代码根目录，或monorepo 模式结构中的默认项目 |
| compilerOptions |                            编译相关的配置选项                             |
| generateOptions |                          全局生成相关的配置选项                           |
|    monorepo     |          （仅限 monorepo）对于 monorepo 模式结构，该值始终为true          |
|      root       |                 （仅限 monorepo）指向默认项目的项目根目录                 |

这里面很多属性暂时我们用不到，先略过，后续章节会详细介绍

## 开发目录

```
src
  ├── app.controller.spec.ts
  ├── app.controller.ts
  ├── app.module.ts
  ├── app.service.ts
  ├── main.ts
```

|          文件          |                            作用                             |
| :--------------------: | :---------------------------------------------------------: |
|   app.controller.ts    |                带有单个路由的基本控制器示例                 |
| app.controller.spec.ts |                对于基本控制器的单元测试样例                 |
|     app.module.ts      |                      应用程序的根模块                       |
|     app.service.ts     |                   带有单个方法的基本服务                    |
|        main.ts         | 应用程序入口文件。它使用 NestFactory 用来创建 Nest 应用实例 |

## 处理`prettier`和`eslint`报错

`NestJs`已经配置好了`prettier`和`eslint`， 因为不同平台回车符不同，如果您的项目遇到如下错误：

```sh
Delete `␍`eslintprettier/prettier
```

可以编辑`.prettierrc`文件：

```json
{
  ...
  "endOfLine": "auto"
}

```

然后重启vscode

## 运行项目

```sh
# 运行项目
pnpm run start
# 运行项目并监听文件改动自动重启
pnpm run start:dev

```

运行`pnpm run start`打开浏览器访问`http://localhost:3000`, 我们能看到`Hello World`

这就是我们用`nest`写的**第一个接口**!

入口文件`main.ts`中, 我们定义异步函数`bootstrap`并执行, 通过`NestFactory.create`方法创建了一个Nest应用程序实例，这个方法需要传入一个**模块**(在这里是`AppModule`)。然后通过`await app.listen(3000);`，监听了`3000`端口

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
