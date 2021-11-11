# 参考

[gradle 编译 Spring 源码](https://blog.csdn.net/yy_diego/article/details/116381958)

# 阿里云仓库

[阿里云](https://developer.aliyun.com/mvn/view)

# 具体步骤

> fork代码

> clone代码，并添加upstream

```bash
git remote -v
git remote add upstream https://github.com/selfteaching/the-craft-of-selfteaching.git
```

> 切换代码到需要的分支

> 手动安装 gradle 版本

```
/gradle/wrapper/gradle-wrapper.properties 可查看gradle版本

下载的zip包放入： `~/.gradle/wrapper/dists/版本/序列码/`
```

> 修改gradle配置

```bash
# build.gradle
repositories {
			maven { url 'https://maven.aliyun.com/nexus/content/groups/public/' }
			maven { url 'https://maven.aliyun.com/nexus/content/repositories/jcenter'}
			maven { url 'https://maven.aliyun.com/repository/gradle-plugin'}
			mavenCentral()
			maven { url "https://repo.spring.io/libs-spring-framework-build" }
		}
		
# grade.properties
# spring 源码的版本，不是 gradle 的，别瞎改。
version=5.2.19.BUILD-SNAPSHOT
org.gradle.jvmargs=-Xmx2048M
## 开启 Gradle 缓存
org.gradle.caching=true
## 开启并行编译
org.gradle.parallel=true
## 启用新的孵化模式
org.gradle.configureondemand=true
## 开启守护进程 通过开启守护进程，下一次构建的时候，将会连接这个守护进程进行构建，而不是重新fork一个gradle构建进程
org.gradle.daemon=true

# settings.gradle
pluginManagement {
	repositories {
		maven { url "https://maven.aliyun.com/repository/public" }
		gradlePluginPortal()
		maven { url 'https://repo.spring.io/plugins-release' }
	}
}
```