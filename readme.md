### Собрать плагин:
```shell script
./gralew clean build
```
### Опубликовать плагин в локальный maven-репозиторий
```shell script
./gradlew publishToMavenLocal
```

# Подключение плагина в проект из  локального maven репозитория
### Файл settings.gradle
```groovy
pluginManagement {
    repositories {
        mavenLocal()
        gradlePluginPortal()
    }
}
rootProject.name = 'gradle-plugin-test'
```
### Файл build.gradle
```groovy
buildscript{
	repositories {
		mavenLocal()
		dependencies {
			classpath 'org.pva:GreetingPlugin:1.0-SNAPSHOT'
		}
	}
}

plugins {
	id 'org.springframework.boot' version '2.3.1.RELEASE'
	id 'io.spring.dependency-management' version '1.0.9.RELEASE'
	id 'java'
}

group = 'org.pva'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenLocal()
	mavenCentral()
}

apply plugin: org.pva.GreetingPlugin

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter'
	compileOnly 'org.projectlombok:lombok'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

test {
	useJUnitPlatform()
}
```