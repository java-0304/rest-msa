# 실습순서
- Spring 기반 개발 환경 구성
- 기본 REST API 구현
- API 문서화
- HATEOAS 구현

## 1. Spring 기반 개발 환경 구성
- Open JDK 17
- Intellij IDEA 2013
- Git
- 실습 Spring 스펙은 build.gradle 파일의 dependencies 참고
  
### build.gradle(rest-msa)
```
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.2.3'
	id 'io.spring.dependency-management' version '1.1.4'
	id 'org.asciidoctor.jvm.convert' version '3.3.2'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'

java {
	sourceCompatibility = '17'
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

ext {
	set('snippetsDir', file("build/generated-snippets"))
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-actuator'
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-hateoas'
	implementation 'org.springframework.boot:spring-boot-starter-validation'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.data:spring-data-rest-hal-explorer'
	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
}

tasks.named('test') {
	outputs.dir snippetsDir
	useJUnitPlatform()
}

tasks.named('asciidoctor') {
	inputs.dir snippetsDir
	dependsOn test
}

```
