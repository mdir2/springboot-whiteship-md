# __스프링부트 자동설정 만들기__
## __POM.xml Setting__
* 의존성 추가
```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-autoconfigure</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-autoconfigure-processor</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```
* 의존성 관리 추가
```
<dependencyManagement>
    <dependencies>
        <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.0.3.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
<br>

---

<br>

## __자동설정 코드 작성__
### __Package Module__
* Holoman
``` java
public class Holoman {
	String name;
	int howLong;

	public String getName() {
		return name;
	}

	public void setName(final String name) {
		this.name = name;
	}

	public int getHowLong() {
		return howLong;
	}

	public void setHowLong(final int howLong) {
		this.howLong = howLong;
	}

	@Override
	public String toString() {
		return "Holoman{" +
				"name='" + name + '\'' +
				", howLong=" + howLong +
				'}';
	}
}
```

* HolomanConfiguration
``` java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class HolomanConfiguration {

	@Bean
	public Holoman holoman() {
		Holoman holoman = new Holoman();
		holoman.setHowLong(33);
		holoman.setName("wook");
		return holoman;
	}
}
```

* /resources/META_INF/spring.factories
``` properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  me.wook.HolomanConfiguration
```

### __Package Module 사용__
* HolomanRunner
``` java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

@Component
public class HolomanRunner implements ApplicationRunner {

	@Autowired
	Holoman holoman;

	@Override
	public void run(final ApplicationArguments args) throws Exception {
		System.out.println(holoman);
	}
}
```

<br>

---

<br>

## __프러퍼티 사용 버전__
* HolomanProperties
``` java
import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties("holoman")
public class HolomanProperties {
	private String name;
	private int howLong;

	public String getName() {
		return name;
	}

	public void setName(final String name) {
		this.name = name;
	}

	public int getHowLong() {
		return howLong;
	}

	public void setHowLong(final int howLong) {
		this.howLong = howLong;
	}
}

```

* HolomanConfiguration ; 프러퍼티 설정 추가
``` java
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableConfigurationProperties(HolomanProperties.class)
public class HolomanConfiguration {

	@Bean
	@ConditionalOnMissingBean
	public Holoman holoman(HolomanProperties properties) {
		Holoman holoman = new Holoman();
		holoman.setHowLong(properties.getHowLong());
		holoman.setName(properties.getName());
		return holoman;
	}
}

```

> _``"나는 굳이 Bean등록을 하면서 쓰고싶지 않다." 라면, 프러퍼티 설정으로...``_