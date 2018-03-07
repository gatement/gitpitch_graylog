@title[Title]
## <span class="gold">Graylog</span>
##### Enterprise Log Management 

---

@title[Outline]
#### Outline
- Introduction |
- Architecture |
- Data Ingesting |
- Streams |
- Search |
- Dashboards |
- Alerts |
- Storing Policy |

---

@title[Introduction1 - App Data Types]
#### Application Data Types
![Image-Absolute](assets/images/drawing_app_data_types.png)

---

@title[Introduction - User Pain Point]
#### User <span class="gold">pain points</span>
- Decentralized
- Hard to Explore
- Hard to Analyse
- Hard to Manage

---

@title[Introduction3]
#### Graylog is a <span class="gold">centralized logging system</span>
- Storing
- Exploration
- Alerting
- Reporting
- API
<br/>
<span class="aside">and more...</span>

---

@title[Architecture]
#### Components
- <span class="gold">MongoDB</span><span class="aside">: storing meta information and configuration data</span>
- <span class="gold">Elasticsearch</span><span class="aside">: storing log data</span>
- <span class="gold">Graylog-Server</span><span class="aside">: input/output/processing/Web UI</span>

+++

@title[Small Setup]
<span style="color:gray; font-size:0.7em">Small setup</span>
<br/>
![Image-Absolute](assets/images/architec_small_setup.png)

+++

@title[Multi-node Setup]
<span style="color:gray; font-size:0.7em">Multi-node setup</span>
<br/>
![Image-Absolute](assets/images/architec_bigger_setup.png)

---

@title[Data Ingesting]
#### Data <span class="gold">Ingesting</span>
![Image-Absolute](assets/images/drawing_inputs.png)
<span class="aside">Define the input endpoints</span>
</span>
+++ 

#### Data Ingesting <span class="gold">Methods</span>
- Message Formats: <span class="aside">Raw/Plaintext, Syslog, CEF, <span class="gold">GELF</span>, ...</span>
- Transport Methods: <span class="aside">TCP, <span class="gold">UDP</span>, Kafka, AMQP, ...</span>

+++

#### [<span class="gold">GELF</span>](http://docs.graylog.org/en/2.4/pages/gelf.html) (<span class="gold">G</span>raylog <span class="gold">E</span>xtended <span class="gold">L</span>og <span class="gold">F</span>ormat) is a log format.
- Structured: <span class="aside">easy analyze</span>
- Chunking: <span class="aside">large size</span>
- Compression (GZIP/GLIB): <span class="aside">network friendly</span>

+++

<p><span class="menu-title slide-title">GELF message</span></p>
```javascript
{
  "version": "1.1",
  "host": "example.org",
  "short_message": "A short message that helps you identify what is going on",
  "full_message": "Backtrace here\n\nmore stuff",
  "timestamp": 1385053862.3072,
  "level": 1,
  "_user_id": 9001,
  "_some_info": "foo",
  "_some_env_var": "bar"
}
```

+++

#### <span class="gold">GELF</span> + <span class="gold">UDP</span>
#### <span class="gold">UDP</span> is not reliable, but it is <span class="gold">non-blocked</span>.
<span class="aside">We will put the application and the Graylog on the same LAN</span>

+++ 

<span style="color:gray; font-size:0.7em">Graylog inputs</span>
![Image-Absolute](assets/images/screenshot_inputs.png)

+++

<p><span class="menu-title slide-title">pom.xml</span></p>
```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-log4j2</artifactId>
        </dependency>
        <dependency>
            <groupId>org.graylog2.log4j2</groupId>
            <artifactId>log4j2-gelf</artifactId>
            <version>1.3.1</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

@[16-20](Maven dependency)

+++

<p><span class="menu-title slide-title">log4j2-spring.xml</span></p>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="warn" name="MyApp" packages="">
	<Appenders>
		<Console name="Console" target="SYSTEM_OUT" ignoreExceptions="false">
			<PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} [%t] (%F:%L)  - %m%n" />
		</Console>
		<GELF name="gelfAppender" server="www.johnsonlau.net" port="12201"
			hostName="appserver01.example.com" protocol="UDP">
			<KeyValuePair key="environment" value="DEV" />
			<KeyValuePair key="application" value="demo" />
		</GELF>
	</Appenders>
	<Loggers>
		<Root level="info">
			<AppenderRef ref="Console" />
			<AppenderRef ref="gelfAppender" />
		</Root>
	</Loggers>
</Configuration>
```
@[7-11](GELF Appender)
@[16-16](GELF Logger)

+++

<p><span class="menu-title slide-title">Java code</span></p>
```java
@RestController
@Slf4j
public class HelloController {

	@GetMapping("/hello")
	public String hello() {
		log.info("GET /hello");
		return "hello world, graylog2";
	}

}

```

@[7-7](log)

+++ 

<span style="color:gray; font-size:0.7em">Log in Graylog</span>
<br/>
![Image-Absolute](assets/images/log-hello.png)

---

@title[Stream]
#### <span class="gold">Streams</span>
![Image-Absolute](assets/images/drawing_streams.png)
<span class="aside">Define the input endpoints</span>

+++ 

##### Demo
![Image-Absolute](assets/images/screenshot_streams.png)

---

@title[Search]
#### <span class="gold">Search</span> [query syntax](http://docs.graylog.org/en/2.4/pages/queries.html)
- ssh
- ssh login
- "ssh login"
- type:ssh
- type:"ssh login"
- type:(ssh login)
- (ssh OR login) AND NOT source:example.org
- source:*.org
- source:exam?le.org
- http_response_code:[0 TO 64}
- http_response_code:>=400
- http_response_code:(>=400 AND <500)

+++ 

#### Reports
- Chart
- Quick values
- Statistics

+++

##### demo
![Image-Absolute](assets/images/screenshot_reports.png)

---

@title[Dashboards]
#### <span class="gold">Dashboards</span>


