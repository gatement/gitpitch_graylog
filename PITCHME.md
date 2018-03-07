@title[Title]
## <span class="gold">Graylog</span>
##### Enterprise Log Management 

---

@title[Outline]
#### Outline
- Introduction |
- Architecture |
- Data Ingesting |
- Inputs |
- Streams |
- Dashboards |
- Alerts |

---
@title[Introduction1]
#### Application Data Types
![Image-Absolute](assets/images/application_data_types.png)

---

@title[Introduction2]
#### Graylog is a <span class="gold">centralized logging system</span>
- Storing
- Exploration
- Alerting
- Reporting
<br/>
<span class="aside">and more...</span>

---

@title[Architecture]
#### Components
- <span class="gold">MongoDB</span>: storing meta information and configuration data
- <span class="gold">Elasticsearch</span>: storing log data
- <span class="gold">Graylog-Server</span>: input/output/processing/Web UI

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

@title[Data Ingesting1]
#### Data Ingesting Methods
- Formats: Raw/Plaintext, Syslog, CEF, GELF, ...
- Transport methods: TCP, UDP, Kafka, AMQP, ...

+++

#### [<span class="gold">GELF</span>](http://docs.graylog.org/en/2.4/pages/gelf.html) (<span class="gold">G</span>raylog <span class="gold">E</span>xtended <span class="gold">L</span>og <span class="gold">F</span>ormat) is a log format.
- Structured
- Chunking
- Compression (GZIP/GLIB)

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
<span class="aside">We will put the application and the graylog on the same LAN</span>

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

@title[Input]
## <span class="gold">Inputs</span>
##### Define the input endpoints
![Image-Absolute](assets/images/inputs.png)

+++ 

##### Demo
![Image-Absolute](assets/images/inputs_screenshot).png)

---

@title[Stream]
## <span class="gold">Stream</span>
##### Filter messages into different channels
![Image-Absolute](assets/images/streams.png)

+++ 

##### Demo
![Image-Absolute](assets/images/streams_screenshot).png)



