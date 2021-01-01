

# Java

外部APIを叩くときにJavaベースのクラスライブラリを利用した開発が要求されることがある(あった)。世の中はRESTやwebsocketばかりではない。

あまりテクニカルなことをしなければJava言語は素直な言語なので書ける。しかしjavaのコンパイラやリンカのAPIとか考え方がよくわからないのでまとめる。

## 拡張子

```sh
.java # ソースファイル
.class # クラスファイル
META-INF/MANIFEST.MF # マニフェストファイル jarアーカイブのメタファイル
.jar # jarファイル zip形式でマニフェストファイルと複数のクラスファイルを持つ
```

## java_home

インストールされているjavaのバージョンを列挙する。(rbenvみたいなもの)

```sh
/usr/libexec/java_home -V # mac
archlinux-java status # archlinux
```

## java

mavenで.jarを作って、java -jar x.jar を使えばいい。本当はX optionも必要だけど省略

```sh
java -help
java -version # バージョン確認
java HelloWorld # HelloWorld.classの実行 classpathは実行ディレクトリ
java -classpath classes/x/ HelloWorld # 省略形 -cp classpath指定しての実行
java HelloWorld arg1 arg2 ... # コマンドライン引数付で実行
java HelloWorld -Dsystem.language=jp # システムプロパティ付の実行
java -jar x-with-dependencies.jar # fat jarファイルの実行
```

## javac

mavenを使えばあまり縁はない。

```sh
javac greetings/*.java # 複数ファイルをまとめてコンパイル
-classpath classes/x/:b HelloWorld # クラスパス複数指定
-sourcepath a:b HelloWorld # ソースパス複数指定
-cp classes/x/:b HelloWorld # クラスパス複数指定
-d target # 出力ディレクトリを指定
-encoding UTF-8 # エンコーディング
-g # デバッグ情報を生成
```

## maven

とりあえずmvn installとmvn cleanだけでいい

```sh
mvn --version # バージョン確認
mvn archetype:generate #対話形式でmavenプロジェクト作成
mvn validate # mavenプロジェクトがコンパイル可能か検証
mvn compile # mavenプロジェクトのコンパイル
mvn clean # ビルドしたファイルのクリーン
mvn test # テスト実行
mvn package # jarパッケージの作成?
mvn install # インストール？
mvn dependency:tree
mvn assembly:assembly -DdescriptorId=jar-with-dependencies # jar-with-dependenciesの作成
```

## maven project

```sh
src # a.javaなどをおく
pom.xml # 依存関係とかを書く
rc #リソースファイル(コンフィグファイルとか)をおく
```

## pom.xml

```xml
<!-- 多分XMLのテンプレ、ボイラープレート -->
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- プロジェクトの情報を入れる -->
    <groupId>組織名など</groupId>
    <artifactId>プロジェクト名など</artifactId>
    <version>バージョン</version>
    <!-- 依存ライブラリをDLする上でmaven公式以外のレポジトリに接続する場合登録する -->
    <repositories>
        <repository>
            <id>接続先レポジトリのID</id>
            <name>接続先レポジトリの名前</name>
            <url>接続先レポジトリのURL</url>
        </repository>
    </repositories>
    <!-- 依存ライブラリを記述する -->
    <dependencies>
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.2.18</version>
        </dependency>
        <dependency></dependency>
    </dependencies>
    <!-- ビルドを定義する -->
    <build>
        <!-- ソースディレクトリを定義する -->
        <sourceDirectory>src</sourceDirectory>
        <!-- リソースディレクトリを定義する -->
        <resources>
            <resource>
                <directory>rc</directory>
            </resource>
        </resources>
        <!-- ビルドプラグインを定義する -->
        <plugins>
            <plugin>
                <!-- Javaのバージョンを固定するために使った -->
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <inherited>true</inherited>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <fork>true</fork>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <!-- 依存性を含めたjar(fat jar)をmvn installで作成するために使った -->
                <artifactId>maven-assembly-plugin</artifactId>
                <version>3.3.0</version>
                <configuration>
                    <!-- finalName.jarにコンパイルされる -->
                    <finalName>name-${project.version}</finalName>
                    <!-- falseにしないとwith-dependenciesというpostfixがついてしまう-->
                    <appendAssemblyId>false</appendAssemblyId>
                    <archive>
                        <manifest>
                            <!-- Main関数を持つクラスを指示しないと実行可能にはならない -->
                            <mainClass>singlejartest.Main</mainClass>
                        </manifest>
                    </archive>
                    <descriptorRefs>
                        <!-- 依存性を含めたjarにパッケージする -->
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

## vscodeの設定

extension packは重すぎなので、redhatのやつだけ使う。

```json
{
    "java.configuration.updateBuildConfiguration": "automatic",
    "java.dependency.syncWithFolderExplorer": true,
    "java.format.enabled": true,
    "java.format.settings.url": "https://raw.githubusercontent.com/google/styleguide/gh-pages/eclipse-java-google-style.xml",
    "java.format.settings.profile": "GoogleStyle",
    "[java]": {
        "editor.defaultFormatter": "redhat.java"
    }
}
```

