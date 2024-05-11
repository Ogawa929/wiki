# Gradleの基本知識

## 参考
* https://zenn.dev/phd/articles/c4a2f26b2ff103
* https://zenn.dev/loglass/articles/6c449ab8a750f2
---
## Gradleの基本
Gradleはオープンソースで開発されているビルド自動化ツール。  
JVM上で実行されるため、JVM系言語のビルドツールとしてしばしば利用される。  
TypeScriptやGoもプラグイン利用することでビルド可能。

実態は依存関係に基づくタスクランナー。コンパイル、ビルドなどは個々のタスクでしかない。  

DSLはGroovy、Kotlin、Kotlinで書く場合は拡張子に`.kts`をつける。


## Project
Gradleのビルド対象のこと。  
build.gradleがあるディレクトリ＝1プロジェクト。  
ネストして、サブプロジェクトの作成したり、依存関係を定義することが可能。

## Task
Taskを定義すると、`gradle タスク名`として呼び出しが可能。
```
tasks.register('sample') {
    doLast {
        println 'sample1 hello!'
    }
    doLast {
        println 'sample2 hello!'
    }
}
```
また、登録したタスクは直接タスク名で呼び出すことが可能
```
sample {
    doLast{
        println 'hoge'
    }
}
```
### doLast
Taskはactionsという名前のプロパティを持っている。
doLastは引数として与えられたクロージャをActionのオブジェクトに変換し、actionsの末尾に追加する。
コードは上から下に流れていく。
### doFirst
actionsの先頭に処理を追加する。
### 依存関係
```
sample {
    doLast{
        println 'hoge'
    }
}

test {
    dependsOn 'sample'
    doLast {
        println 'fuga'
    }
}
```
上記の用に記述することで、sample -> test の順で実行される用に依存関係を定義可能。
依存関係の中で重複するタスクは排除されるため、タスクの実行は1回のみとなる。
### 組み込みタスク
すでに存在するタスクを継承することで記述を減らせる。  
```
#Copy Task
tasks.register('copy', Copy) {
    from './from'
    into './into'
}
#Exec Task
tasks.register('exec', ExecSpec) {
    environment [ 'test1': 'aaa' , 'test2': 'bbb', 'test3': 'ccc' ]
    commandLine 'sh', '-c', 'echo $test1 $test2 $test3'
}
#Zip,Tar Task
tasks.register('zip', Zip) {
    from './from'
    archiveFileName.set('test.zip')
    destinationDirectory.set(layout.projectDirectory) // build.gradleがあるディレクトリ
}
tasks.register('tar', Tar) {
    from './from'
    archiveFileName.set('test.tar.gz')
    destinationDirectory.set(layout.projectDirectory) // build.gradleがあるディレクトリ
}
```

## Configuration
Configurationはファイルやライブラリの依存関係をグループ化する仕組みです。
### ローカルのファイルシステムからファイルの取得
```
configurations {
    conf
}

dependencies {
    conf files('./a.txt')
}

copy {
    from configurations.conf
    into './dir'
}
```

### セントラルリポジトリからjarの取得
### 推移的依存関係の解決
repositoriesにリポジトリを指定した場合、jarは依存関係を辿った全jarが含まれる（重複は排除される）  
`gradle dependencies`
```
configurations {
    conf
}

dependencies {
    conf 'org.slf4j:slf4j-api:1.7.25'
}

repositories {
    mavenCentral()
}

copy {
    from configurations.conf
    into './lib'
}
```
### 推移的依存関係解決の無効化
```
configurations {
    conf {
        #無効化
        transitive false
        #部分的に無効化...除外しているだけ？
        exclude module: 'log4j-core'
    }
}
```

## ビルドライフサイクル

### Initialization phase（初期化フェーズ）
設定ファイル（settings.gradleなど）を検出して評価
ビルドするプロジェクトを決定、各プロジェクトに対してProjectインスタンスを作成するといったことを行います。

設定ファイルを含むディレクトリでGradleを実行すると、Gradleは設定ファイルを使用してビルド初期を行う。
### Configuration phase（設定フェーズ）
ビルドに参加している全てのプロジェクトの評価と、リクエストされたタスクに対してタスクグラフの作成を行う。  
タスクグラフとは、Gradleの依存関係を示したグラフでこのグラフをもとにGradleのタスクを依存関係の順序で実行されることを保証する。  

Configuration cacheを利用することで、ビルド速度を向上させることも可能。  
Configuration cacheを利用することで、ビルド構成に影響する変更が加わっていない場合Configuration phaseをスキップできる。  
Configuration cacheはConfiguration phaseの出力を保存。  
build cacheはExecution phaseの出力を保存する。  

Project#configurations  
↓  
Project#dependencies  
↓  
Project#repositories  
↓  
Tasksのプロパティ初期化、Task追加  
↓  
プラグインの登録（Projectオブジェクトを受取、任意の操作を行う）  
### Execution phase（実行フェーズ）
各タスクを依存関係のある順にスケジューリングして実行。  
Configuration phaseで作成されたタスクグラフを元に依存関係の順に実行するタスクを決定して実行。  
ライブラリのDL、コードのコンパイル、入力の読み込み、出力の書き込みなどビルドに関連する作業が含まれる。

## プラグインによるProjectの拡張
GradleはPluginを通して機能を拡張する。  
プラグイン内部でProjectオブジェクトを操作することによって、さまざまなTaskやConfigurationを事前に用意可能。  
Javaプロジェクトに必要な機能もすべてGradle本体から切り離し可能な形で実装されており、それがJavaプラグイン、applicationプラグインとして提供されている。  
Gradleの中心はあくまでTaskとConfigurationをはじめとした抽象的な仕組みにあり、用途に応じたプラグインを組み込むことによりビルドツールとして様々なユースケースに答えるフレームワークになっている。  

### 拡張の対象
* Task
* Configuration
* Extention

## マルチプロジェクト
* Multi-Project Build
  一つのルートプロジェクトの下に複数のサブプロジェクトが存在する。
  ```
    rootProject/
    ├── settings.gradle
    ├── build.gradle
    ├── projectA/
    │   ├── build.gradle
    │   └── src/
    └── projectB/
        ├── build.gradle
        └── src/
  ```
* Composite Build
  各プロジェクトは独立したディレクトリを持つ。
  ```
    rootProject/
    ├── settings.gradle
    ├── projectA/
    │   ├── build.gradle
    │   └── src/
    └── projectB/
        ├── build.gradle
        └── src/  
  ```

## 現在では非推奨な書き方
* プラグイン導入に`apply plugin`を使う  
  レガシーな記法であり、現在は以下の記法。
  ```
  plugins {
    id("com.jfrog.bintray") version "1.8.5"
  }
  ```
* マルチモジュール構成で、subprojects{}、allprojects{}を使わない  
  公式としては非推奨らしい、規模が大きいと複雑となり管理やビルドパフォーマンスの低下が懸念されるよう。  
  * 共通の設定は`buildSrc`というディレクトリを作成しそこに記載する。
  * 各プロジェクトはその設定をプラグインで明示的に読み込む