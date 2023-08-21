# github 操作

## github リモート設定

Ref. https://prettytabby.com/github-tips3/

1. 上記を参考に`access token`を生成して、トークンをコピー<br/>
   トークンを生成する際、[こちら](https://ios-docs.dev/20210813support-for-password/)を参考に権限を付与。いくつか付与する必要あり。
2. git のリモートURLに取得したアクセストークンを追加する
   ```agsl
   git remote set-url origin https://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@github.com/jyokosaw/rhdg8-embedded.git
   ```

## github にローカルの変更を反映する方法

```agsl
jyokosaw@jyokosaw-mac rhdg8-embedded-jyokosaw % git add .
jyokosaw@jyokosaw-mac rhdg8-embedded-jyokosaw % git commit -m "add debug categories"
[main 61f8eab] add debug categories
 3 files changed, 9 insertions(+), 4 deletions(-)
jyokosaw@jyokosaw-mac rhdg8-embedded-jyokosaw % git diff                            
jyokosaw@jyokosaw-mac rhdg8-embedded-jyokosaw % git push -u origin main             
Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 12 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (8/8), 865 bytes | 865.00 KiB/s, done.
Total 8 (delta 4), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
To https://github.com/jyokosaw/rhdg8-embedded.git
   df68b04..61f8eab  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

## 差分確認

```agsl
jyokosaw@jyokosaw-mac rhdg8-embedded-jyokosaw % git diff
diff --git a/README.adoc b/README.adoc
index 4334366..e2c6877 100644
--- a/README.adoc
+++ b/README.adoc
@@ -63,8 +63,8 @@ Deploying your client application on OCP requires to create several Openshift ob
 [source, bash]
 ----
 export APP_NAME=rhdg
-export NAMESPACE=rhdg8-embedded
-export GIT_REPO=https://github.com/alvarolop/rhdg8-embedded.git
+export NAMESPACE=rhdg8-embedded-jy
+export GIT_REPO=https://github.com/jyokosaw/rhdg8-embedded.git
 ----
 
 First, create a project to host the application:
diff --git a/pom.xml b/pom.xml
index 05af5e0..94e35f4 100644
--- a/pom.xml
+++ b/pom.xml
@@ -10,12 +10,14 @@
     <properties>
         <version.spring.boot>2.3.4.RELEASE</version.spring.boot>
         <version.spring.session>2.3.0.RELEASE</version.spring.session>
+        <!-- RHDG 8.1.1 -->
+        <version.infinispan>11.0.9.Final-redhat-00001</version.infinispan>
         <!-- RHDG 8.2.0 -->
 <!--        <version.infinispan>12.1.3.Final-redhat-00001</version.infinispan>-->
         <!-- RHDG 8.2.1 -->
 <!--        <version.infinispan>12.1.7.Final-redhat-00001</version.infinispan>-->
         <!-- RHDG 8.3.0 -->
-        <version.infinispan>13.0.0.Final</version.infinispan>
+        <!--<version.infinispan>13.0.0.Final</version.infinispan>-->
         <lombok.version>1.18.18</lombok.version>
         <java.version>11</java.version>
         <maven.compiler.source>${java.version}</maven.compiler.source>
diff --git a/src/main/resources/logback-spring-k8s.xml b/src/main/resources/logback-spring-k8s.xml
index 78a41b7..bfc31a4 100644
--- a/src/main/resources/logback-spring-k8s.xml
+++ b/src/main/resources/logback-spring-k8s.xml
@@ -15,7 +15,7 @@
 <!--    </appender>-->
 
     <!-- LOG everything at INFO level -->
-    <root level="INFO">
+    <root level="TRACE">
         <appender-ref ref="CONSOLE" />
     </root>
 
@@ -24,4 +24,7 @@
 
     <!-- LOG "org.infinispan*" at TRACE level -->
     <logger name="org.infinispan" level="info"/>
+    <Logger name="org.infinispan.CLUSTER" level="trace"/>
+    <Logger name="org.infinispan.CONFIG" level="trace"/>
+    <Logger name="org.jgroups" level="trace"/>
 </configuration>
```